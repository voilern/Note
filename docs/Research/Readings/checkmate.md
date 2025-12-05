---
comments: true
---

# Checkmate: Breaking the Memory Wall with Optimal Tensor Rematerialization

!!! info "说明"
    本文参考了 [GoPoux 的笔记本 - Checkmate](https://note.gopoux.cc/research/rc4ml/checkmate/){target="_blank"}

## Introduction

深度学习训练需要大量的高带宽内存，这些训练内存主要由反向传播所需的中间激活值 (activation tensors) 主导。高带宽内存的有限可用性造成了模型训练的内存墙 (memory wall)。为了降低内存开销，在前向传递过程中可以丢弃部分选定张量，在反向传播时需要时重新计算 (rematerilization)，即以计算换内存。在论文 [voilern's note - Training Deep Nets with Sublinear Memory Cost](https://note.voilern.cn/Research/Readings/checkpoint/) 中将这个问题称为 Checkpointing，而他们所提出的方法无法普通应用到非线性 DNN 中，并依赖于图中所有节点具有相同 cost 的强假设。

本篇论文将 tensor rematerialization 形式化为约束优化问题，也即 MILP (mixed integer linear program, 混合整数线性规划) 问题，提出一种基于两阶段确定性线性规划舍入的近似算法，并给出一个在 TensorFlow 中实现的可用于模型训练的系统 Checkmate。

## Motivation

训练期间的内存占用主要包括大小取决于输入维度的中间特征 / 激活值和大小取决于权重维度的参数及其梯度。鉴于输入通常比 kernels 大，内存主要被特征所占用。诸如 TensorFlow 和 Pytorch 在前向传递时存储所有中间激活值，带来巨大的内存开销。通过引入 rematerialization 可显著降低内存占用。

本文的工作对于神经网络图几乎没有假设，允许每个节点具有多个输入和输出的任意图、跨层的可变内存开销和每层的可变计算成本。仅将解约束在 correct 和 within the RAM budget 两个条件下，即节点的依赖关系必须在其被评估前被确定，并且在执行期间的任意时刻，驻留的张量可以存放在内存中。

在这些约束条件下，将调度投影到时间与空间，可将目标表示为线性表达式，从而可使用诸如 Gurobi、GLPK、COIN-OR Branch-and-Cut 等混合整数线性规划求解器进行求解。其最优解将在内存预算内最小化额外的计算成本。

## Related Work

相关工作主要可分为 checkpointing and rematerialization、reversible networks、distributed computation 和 activation compression。

## Optimal Rematerialization

在本节中，本文开发了一个最优求解器，用于在评估通用数据流图（包括神经网络训练中所使用的）时调度计算并进行垃圾回收。将 rematerialization 问题建模为可使用线性规划器求解的 MILP 问题。 

### Problem definition

计算图或数据流图 $G = (V, E)$ 为包含 $n$ 个节点 $V = \{ v_1, \dots, v_n \}$ 的有向无环图，其中每个节点 $v$ 表示产生值（例如张量）的运算，边 $e = (v_i, v_j)$ 表示运算之间的依赖关系。节点按照拓扑序编号，使得运算 $v_j$ 仅依赖运算 $v_{i<j}$ 的结果。每个运算的结果需要 $M_v$ 的内存来存储，使用其输入对输出进行重计算需要 $C_v$ 的成本。我们希望在内存预算 $M_{\text{budget}}$ 下找到具有最大内存开销和最小计算成本的终端节点 (terminal node) $v_n$。

### Representing a schedule

将 schedule 策略表示为一系列被保存或重计算的节点。将网络的执行展开为 $T$ 个阶段，并且每个阶段只允许一个节点重计算一次，定义：

- $S_{t, i} \in \{0, 1\}$ 表示运算 $i$ 的结果从阶段 $t - 1$ 到阶段 $t$ 都应被保存在内存中
- $R_{t, i} \in \{0, 1\}$ 表示运算 $i$ 在阶段 $t$ 是否进行重计算

该表示对 checkpointing 进行了推广，即值可被保留和释放多次，但代价是 $O(Tn)$ 的决策变量。为权衡决策变量数与调度灵活性，令 $T = n$，即 $O(n^2)$ 的运算和在线性图中常量级的内存开销。

### Scheduling with ample memory

首先考虑内存充足时的情况。即使没有内存约束，仍需要保证运算时其所有依赖均驻留在内存中。则目前的优化问题可表述为：

$$
\begin{align}
\arg \min_{R, S} & \quad \sum_{t=1}^{n} \sum_{i=1}^{t} C_i R_{t, i} \tag{1a} \\
\text{subject to} & \notag \\
R_{t, j} & \leq R_{t, i} + S_{t, i}, && \forall t \; \forall(v_i, v_j) \in E, \tag{1b} \\
S_{t, i} & \leq R_{t-1, i} + S_{t-1, i}, && \forall t \geq 2 \; \forall i, \tag{1c} \\
\sum_{i} S_{1, i} &= 0, \tag{1d} \\
\sum_{t} R_{t, n} & \leq 1, \tag{1e} \\
R_{t, i}, S_{t, i} & \in \{0, 1\}, && \forall t \; \forall i \tag{1f}
\end{align}
$$

- （1a）：优化目标，最小化计算成本。
- （1b）：若运算 $j$ 在阶段 $t$ 被重新计算，则其所有依赖 $i$ 必须在阶段 $t$ 可用，即在阶段 $t$ 被重新计算或在阶段 $t$ 及之前已经保存在内存中。
- （1c）：若运算 $i$ 在阶段 $t-1$ 到阶段 $t$ 期间被保存在内存中，则运算 $i$ 在应在阶段 $t-1$ 被重新计算，或在阶段 $t-1$ 及之前已经保存在内存中。
- （1d）：初始阶段不保存任何运算结果，内存中没有任何值。
- （1e）：最终的运算必须在某个阶段被计算，确保训练过程的完整性。
- （1f）：决策变量 $R, S$ 的定义。

### Constraining memory utilization

在上文的基础上，为约束内存使用，引入内存计算变量 $U_{t, k} \in \mathbb{R}_+$，表示在阶段 $t$ 中，运算 $v_k$ 完成后所使用的内存。$U_{t, k}$ 借助二元变量 $\text{FREE}_{t, i, k} \in \{0, 1\}, (v_i, v_k) \in E$ 递归定义，$\text{FREE}_{t, i, k}$ 表示在阶段 $t$ 中 $v_k$ 完成后是否可以释放 $v_i$ 占用的内存。

我们假设有：

- 网络的输入和参数始终驻留在内存中。
- 有足够的空间存放参数梯度，参数梯度所占内存和参数所占内存相同。
- 在阶段开始时，所有 checkpointed value 都驻留在内存中。

因此，可以写出初始时的表达式 $U_{t, 0}$：

$$
U_{t, 0} = \underbrace{M_{\text{input}} + 2M_{\text{param}}}_{\text{Constant overhead}} + \sum_{i=1}^{n} \underbrace{M_i S_{t, i}}_{\text{Checkpoints}} \tag{2}
$$

即每个阶段开始前，所用内存为输入所占内存、参数和参数梯度所占内存、以及所有前一阶段保存在 checkpoint 中的运算结果所占内存之和。在计算节点 $v_{k+1}$ 前，$v_k$ 及其依赖节点若以后不再使用，则可以释放其占用的内存。则递归定义为

$$
U_{t, k+1} = U_{t, k} - \text{mem\_freed}_t(v_k) + R_{t, k+1}M_{k+1} \tag{3}
$$

其中，$\text{mem\_freed}_t(v_k)$ 表示在阶段 $t$ 中，节点 $v_k$ 及其父节点可被释放的内存量。令

$$
\begin{align}
\text{DEPS}[k] &= \{i \ : \ (v_i, v_k) \in E\} \\
\text{USERS}[i] &= \{j \ : \ (v_i, v_j) \in E\}
\end{align}
$$

分别表示节点的父节点和子节点，根据辅助变量 $\text{FREE}_{t, i, k}$，对于 $(v_i, v_k) \in E$，

$$
\begin{align}
\text{mem\_freed}_t(v_k) &= \sum_{i \in \text{DEPS}[k] \cup \{k\}} M_i \times \text{FREE}_{t, i, k} \tag{4} \\
\text{FREE}_{t, i, k} &= R_{t, k} \times \underbrace{(1 - S_{t+1, i})}_{\text{Not checkpoint}} \prod_{j \in \text{USERS}[i], j > k} \underbrace{(1 - R_{t, j})}_{\text{Not dep.}} \tag{5}
\end{align}
$$

式（5）确保：当 $v_i$ 需要驻留在内存中时，不对其进行释放；当当前阶段存在 $v_i$ 的子节点需要重计算时，不释放 $v_i$ 的内存；当 $v_k$ 不进行重计算时，不能保证 $v_i$ 能被释放，由于每个阶段只有一个节点进行重计算，所以也保证了不会重复释放内存。

### Linear reformulation of memory constraint

由于 $\text{FREE}_{t, i, k}$ 的表达式不是线性的，不能放入 ILP 模型中，需要将其变换成线性形式。引入两个引理

!!! note "Lemma 4.1 Linear Reformulation of Binary Polynomial"

    若 $x_1, \dots, x_n \in \{0, 1\}$，则

    $$
    \prod_{i=1}^n x_i =
    \begin{cases}
    1 & \quad \sum_{i=1}^n (1-x_i) = 0 \\
    0 & \quad otherwise
    \end{cases}
    $$


!!! note "Lemma 4.2 Linear Reformulation of Indicator Constraints"

    整数 $y$，存在常量上界 $\kappa$，即 $0 \leq y \leq \kappa$，则

    $$
    x =
    \begin{cases}
    1 & \quad y = 0 \\
    0 & \quad otherwise
    \end{cases}
    $$

    当且仅当 $x \in \{0, 1\}, (1 - x) \leq y \leq \kappa(1 - x)$ 时成立。

令 $\text{num\_hazards}(t, i, k)$ 表示约束 (5) 中右侧为零的因子个数，则有

$$
\text{num\_hazards}(t, i, k) = (1 - R_{t, k}) + S_{t+1, i} + \sum_{j \in \text{USERS}[i], j > k} R_{t, j}
$$

应用 Lemma 4.1 得

$$
\text{FREE}_{t, i, k} =
\begin{cases}
1 & \quad \text{num\_hazards}(t, i, k) = 0 \\
0 & \quad \text{otherwise}
\end{cases}
\tag{6}
$$

设 $\kappa$ 为 $\text{num\_hazards}(t, i, k)$ 的上界，应用 Lemma 4.2 得

$$
\begin{align}
\text{FREE}_{t, i, k} &\in \{0, 1\} \tag{7a} \\
1 - \text{FREE}_{t, i, k} &\leq \text{num\_hazards}(t, i, k) \tag{7b} \\
\kappa (1 - \text{FREE}_{t, i, k}) &\geq \text{num\_hazards}(t, i, k) \tag{7c}
\end{align}
$$

### Tractability via frontier-advancing stages

给定计算图的一个执行顺序（拓扑序），将节点划分为 frontier-advancing stages，使得 $v_i$ 在阶段 $i$ 中首次被求值。用约束 (8a - 8c) 替换 (1d, 1e)：

$$
\begin{align}
R_{i, i} &= 1, \quad \forall i \tag{8a} \\
\sum_{i \geq t} S_{t, i} &= 0 \tag{8b} \\
\sum_{i > t} R_{t, i} &= 0 \tag{8c}
\end{align}
$$

其中：

- (8a) 确保每个节点在其对应的阶段被计算。
- (8b, 8c) 确保节点 $v_i$ 在阶段 $i$ 之前未被计算过。

这种约束更加严格，减少了可行集的大小，限制了搜索空间，缩短运行时间。

### Complete Integer Linear Program formulation

完整的 MILP 如下，具有 $O(|V||E|)$ 个变量和约束。

$$
\begin{align}
\arg\min_{R, S, U, FREE} & \quad \sum_{t = 1}^n \sum_{i = 1}^t C_i R_{t, i} \\
\text{subject to} & \notag \\
R_{t, j} & \leq R_{t, i} + S_{t, i}, \quad \forall t, \forall(v_i, v_j) \in E \tag{1b} \\
S_{t, i} & \leq R_{t-1, i} + S_{t-1, i}, \quad \forall t \geq 2, \forall i \tag{1c} \\
R_{t, i}, S_{t, i} & \in \{0, 1\}, \quad \forall t, i \tag{1f} \\
U_{t, 0} &= M_{\text{input}} + 2M_{\text{param}} + \sum_{i=1}^{n} M_i S_{t, i} \tag{2} \\
U_{t, k+1} &= U_{t, k} - \text{mem\_freed}_t(v_k) + R_{t, k+1}M_{k+1} \tag{3} \\
\text{FREE}_{t, i, k} &\in \{0, 1\} \tag{7a} \\
1 - \text{FREE}_{t, i, k} &\leq \text{num\_hazards}(t, i, k) \tag{7b} \\
\kappa (1 - \text{FREE}_{t, i, k}) &\geq \text{num\_hazards}(t, i, k) \tag{7c} \\
R_{i, i} &= 1, \quad \forall i \tag{8a} \\
\sum_{i \geq t} S_{t, i} &= 0 \tag{8b} \\
\sum_{i > t} R_{t, i} &= 0 \tag{8c} \\
U_{t, k} &\leq M_{\text{budget}}, \quad \forall t, k \notag
\end{align}
$$

### Constraints implied by optimality

注意到 $\text{FREE}_{t, k, k} = 1$ 当且仅当 $v_k$ 被计算后没有被使用，那么直接不计算 $v_k$ 也是可行的，即 $R_{t, k} = 0$。因此直接固定 $\text{FREE}_{t, k, k} = 0$，此时式 (4) 可以改为

$$
\text{mem\_freed}_t(v_k) = \sum_{i \in \text{DEPS}[k]} M_i \times \text{FREE}_{t, i, k}
$$

这样减少了 $|V|^2$ 个变量。

### Generating an execution plan

根据计算图 $G=(V, E)$ 和 MILP 的解 $(R, S, FREE)$，生成执行计划 $P=(s_1, \dots, s_k)$，其中 $s_i$ 代表计算操作或者释放内存。

![](./img/checkmate_1.png){.center}

### Cost model

计算成本 $C$ 在构建 MILP 之前确定，方法是在目标硬件上使用一系列随机输入对网络层进行性能分析。并且由于输入和输出大小已知，计算图中每个值的内存消耗都可以静态计算，计算出的内存消耗 $M$ 用于构建内存约束。

## Approximation

由于求解 ILP 是 NP-hard 的，对于大规模的神经网络，对 ILP 进行精确求解不可能。论文采用的方法是：

1. 先采用在多项式时间内生成分数解的线性规划
2. 采用两阶段舍入算法：利用分数解先舍入一部分变量为整数解，再计算剩余变量的最优解。

### Relaxing integrality constraints

通过放宽变量的取值范围，将整数约束转化为线性约束，从而可以在多项式时间内求解，即令 $R, S, FREE \in [0, 1]$。

求得分数解 $R^*, S^*$ 之后，可以使用随机舍入或者确定性舍入得出整数解。但是直接舍入后可能导致解违反了约束条件。

### A two-phase rounding strategy

采用两阶段舍入策略：

![](./img/checkmate_2.png){.center}

### Memory budget feasibility

算法 2 满足了条件 (1b, 1c, 1f, 8a)，但是可能不满足内存约束。解决方法是在求解 ILP 时，令 $U_{t, k} \leq (1 - \epsilon)M_{\text{budget}}$，预留总内存预算的余量。

### Evaluation

研究以下问题：

1. 使用重计算策略时，如何权衡内存占用和计算开销之间的关系？
2. 使用重计算策略处理大规模输入是否可行？
3. 如何才能逼近最优重计算策略？

将 checkmate 和启发式算法 baseline 进行对比，在所有内存预算下，最优重计算都能显著降低 baseline 的计算开销，并实现比以往更低的内存占用。

### Baselines and generalizations

理想情况下，Checkpoint all 保存所有中间结果，不进行重计算。

线性图中，可以直接使用 Griewank 等人提出的 $\log n$ 算法和 Chen 等人提出的 $\sqrt{n}$ 启发式算法和贪心算法。

对于非线性图，对 $\sqrt{n}$ 启发式算法和贪心算法进行扩展，扩展得到 AP $\sqrt{n}$ 算法、AP 贪心算法、线性化 $\sqrt{n}$ 算法和线性化贪心算法。

### Evaluation setup

Checkmate 使用 TensorFlow 实现，接受 Keras 模型作为输入。提取正向和反向计算图，使用 Gurobi 求解器求解优化问题 (9)。最后生成执行计划，构建静态训练图。

### What is the trade-off between memory usage and computational overhead?

Checkmate 线性网络（VGG16、MobileNet）上以最低的计算成本生成内存限制内的解决方案，并显著降低复杂架构（U-Net）上的内存消耗和计算成本。

### Are large inputs practical with rematerialization?

为 ILP 问题加入变量 $B$ 表示 batchsize，添加约束再次求解，得出最大 batchsize。经过测试，checkmate 的最大 batchsize 在 U-Net、FCN8、SegNet、VGG19、ResNet50、MobileNet 几种模型中均高于 baseline，在部分模型（U-Net、MoblieNet）上提升显著。

### How well can we approximate the optimal rematerialization policy?

测量了近似解与最优解的计算开销的比值，即最优解相对于近似解的加速比。对于所有测试模型，加速比最多为 $1.06\times$，说明近似解已经很接近最优解。

### Conclusion

本文提出的 Checkmate 重计算策略，允许在有限的内存上训练大型神经网络。并且不需要对神经网络结构作出强假设，能够支持残差网络等复杂结构，并通过一个硬件感知、基于配置文件的 cost model 来评估整个计算图中非均匀内存使用和计算成本的影响。最佳的重计算策略在各种内存限制下都具有最小的计算开销，并且基于 Checkmate 能够训练具有显著更大批量大小的高分辨率模型。此外，论文还提出了一个两阶段舍入策略对 ILP 的最优解进行了近似，能够在多项式时间内获得接近最优的解。