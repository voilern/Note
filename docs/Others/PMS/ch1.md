# 第一章：概率论的基本概念

## 样本空间、随机事件

### 样本空间与随机事件

对随机现象进行观察、记录或试验，称为随机试验。随机试验具有以下特点：

- 可以在相同条件下重复进行
- 每次试验可能出现的结果是不确定的，但能事先知道试验的所有可能结果
- 每次试验完成前不能预知哪一个结果会发生

称随机试验的所有可能结果构成的集合为样本空间，常用字母 $S$ 或 $\Omega$ 表示。样本空间 $S$ 中的每一个元素，即试验的每一个结果称为样本点，样本空间的任一子集称为随机事件，简称事件，常用大写字母表示。特别地，只含有一个样本点的事件称为基本事件。

在一次试验完成时，当试验所出现的结果（即样本点）属于某一事件，即这一事件所包含的一个样本点恰好为此次试验出现的结果时，称该事件发生，否则称该事件没有发生（或该事件不发生）。特别地，若将样本空间 $S$ 视为一事件，则 $S$ 一定会发生，称 $S$ 为必然事件。与之相对应，常称空集 $\emptyset$ 为不可能事件。

### 事件的相互关系及运算

假设所考虑的随机试验 $E$ 的样本空间为 $S$，且下述所提及的事件均为其子集。

- 事件的包含与相等
    - 包含 / 包含于：若事件 $B$ 发生一定导致事件 $A$ 发生，则称事件 $B$ 包含于事件 $A$，或称事件 $A$ 包含事件 $B$，亦称事件 $B$ 为事件 $A$ 的子事件，记为 $B \subset A$ 或 $A \supset B$。

    - 相等：若 $B \subset A$ 且 $A \supset B$，则称事件 $A$ 与事件 $B$ 相等，记为 $A = B$。

- 事件的运算
    - 和事件：由事件 $A$ 与事件 $B$ 的所有样本点构成的集合为 $A$ 与 $B$ 的和事件，记为 $A \cup B$，即

    $$ A \cup B = \lbrace x: x \in A \vee x \in B \rbrace $$

    当且仅当 $A$ 与 $B$ 至少有一个发生时，和事件 $A \cup B$ 发生。类似地，记 $\bigcup_{i=1}^{n}A_i$ 为 $n$ 个事件 $A_1, A_2, \cdots, A_n (n \geq 2)$ 的和事件，记 $\bigcup_{i=1}^{+\infty}A_i$ 为可列个事件 $A_1, A_2, \cdots$ 的和事件。

    - 积事件：由事件 $A$ 与事件 $B$ 的共同样本点构成的集合为 $A$ 与 $B$ 的积事件，记为 $A \cap B$，或 $AB$，或 $A \cdot B$，即

    $$ A \cap B = AB = A \cdot B = \lbrace x: x \in A \wedge x \in B \rbrace $$

    当且仅当 $A$ 与 $B$ 同时发生时，积事件 $A \cap B$ 发生。类似地，记 $\bigcap_{i=1}^{n}A_i$ 为 $n$ 个事件 $A_1, A_2, \cdots, A_n (n \geq 2)$ 的积事件，记 $\bigcap_{i=1}^{+\infty}A_i$ 为可列个事件 $A_1, A_2, \cdots$ 的积事件。

    特别地，当积事件 $A \cap B$ 为不可能事件时，有如下定义：
        - 互斥事件：设 $A, B$ 为两随机事件，当 $A \cap B = \emptyset$ 时，称 $A$ 与 $B$ 互不相容（或互斥）。

    - 逆事件 / 对立事件：若 $A \cup B = S$ 且 $A \cap B = \emptyset$，则称事件 $A$ 与事件 $B$ 互为逆事件或对立事件，常记 $\overline{A}$ 或 $A^c$ 为事件 $A$ 的逆事件，即

    $$ \overline{A} = \lbrace x: x \notin A \rbrace $$

    - 差事件：称由事件 $A$ 与事件 $\overline{B}$ 的共同样本点构成的集合为 $A$ 对 $B$ 的差事件，记为 $A - B$ 或 $A \cap \overline{B}$，即

    $$ A - B = A \cap \overline{B} = \lbrace x: x \in A \wedge x \notin B \rbrace $$

    当且仅事件 $A$ 发生且事件 $\overline{B}$ 不发生时，$A$ 对 $B$ 的差事件 $A - B$ 发生。

- 运算规则
    - 交换律：$A \cup B = B \cup A, \; A \cap B = B \cap A$

    - 结合律：$A \cup (B \cup C) = (A \cup B) \cup C, \; A \cap (B \cap C) = (A \cap B) \cap C$

    - 分配律：$A \cap (B \cup C) = (A \cap B) \cup (A \cap C), \; (A \cap B) \cup C = (A \cup C) \cap (B \cup C)$
    
    - De Morgan's Law：$$ \overline{\bigcup_{j=1}^{n}A_j} = \bigcap_{j=1}^{n}\overline{A_j}, \; \overline{\bigcap_{j=1}^{n}A_j} = \bigcup_{j=1}^{n}\overline{A_j} $$

概率论中常有以下定义：由 $n$ 个元件组成一个系统，若有一个元件损坏，系统就损坏，则称该系统为串联系统；若只要有一个元件不损坏，系统就不损坏，则称该系统为并联系统。

## 频率与概率

### 频率

在相同条件下进行 $n (n \geq 1)$ 次重复试验，若事件 $A$ 在这 $n$ 次重复试验中发生 $n_A$ 次（称 $n_A$ 为 $A$ 在这 $n$ 次实验中发生的频数，$0 \leq n_A \leq n$），则称比值 $\frac{n_A}{n}$ 为事件 $A$ 在这 $n$ 次试验中发生的频率，记为 $f_n(A)$，即

$$ f_n(A) = \frac{n_A}{n} = \frac{\text{A 发生的次数}}{\text{总的试验次数}} $$

由定义，易知其具有以下性质：

- 对任一事件 $A$，$0 \leq f_n(A) \leq 1$
- $f_n(S) = 1$
- 若 $A$ 与 $B$ 互不相容，则 
$$ f_n(A \cup B) = f_n(A) + f_n(B) $$
该性质可以推广至多个两两互不相容的事件，即 
$$ f_n(\bigcup_{j=1}^{k}A_j) = \sum_{j=1}^{k}f_n(A_j) $$

### 概率

设某一随机试验所对应的样本空间为 $S$，对 $S$ 中的任一事件 $A$，当总的试验次数充分大时，$A$ 的频率 $f_n(A)$ 的稳定值 $p$ 定义为事件 $A$ 的概率，记为 $P(A) = p$。该说法常称为概率的统计性定义。接下来介绍概率的公理性定义：

设某一随机试验所对应的样本空间为 $S$，对其中的任一事件 $A$，定义一个实数 $P(A)$，若其满足以下三条公理：

- 非负性：$P(A) \geq 0$
- 规范性：$P(S) = 1$
- 可列可加性：对 $S$ 中的可列个两两不相容的事件 $A_1, A_2, \cdots, A_n, \cdots$ 即（$A_i A_j = \emptyset, i \neq j, i, j = 1, 2, \cdots$），有
$$ P(\bigcup_{j=1}^{+\infty}A_j) = \sum_{j=1}^{+\infty}P(A_j) $$
则称 P(A) 为 $A$ 发生的概率。

根据概率的公理化定义，可以得到以下性质：

- 有限可加性：对于有限个两两互不相容事件的和事件，有
$$ P(\bigcup_{j=1}^{n}A_j) = \sum_{j=1}^{n}P(A_j) $$
- $P(A) = 1 - P(\overline{A})$
- 当 $A \supset B$ 时，$P(A - B) = P(A) - P(B)$，则 $P(A) \geq P(B)$
- 概率的加法公式：
$$ P(A \cup B) = P(A) + P(B) - P(AB) $$
该性质可以推广：

$$
\begin{aligned}
P(\bigcup_{j=1}^{n}A_j) 
& = \sum_{j=1}^{n}P(A_j) - \sum_{i<j}P(A_i A_j) + \sum_{i<j<k}P(A_i A_j A_k) - \cdots \\
& + (-1)^{n-1}P(A_1 A_2 \cdots A_n), n \geq 1
\end{aligned}
$$

## 等可能概型

若一个随机试验满足以下两个条件：

- 有限性：样本空间中样本点数有限
- 等可能性：出现每一个样本点的概率相等

则称这个试验为等可能概型，又称古典概型。在等可能概型中，任一事件 $A$ 的概率为

$$ P(A) = \frac{\text{A 所包含的样本点数}}{\text{S 中样本点总数}} $$

## 条件概率

### 条件概率

若 $P(B) > 0$，那么在 $B$ 发生的条件下 $A$ 发生的条件概率为

$$ P(A | B) = \frac{P(AB)}{P(B)} $$

条件概率满足概率的定义和性质。例如，当 $P(C) \neq 0$ 时，有

- $P(A | C) \geq 0$
- $P(S | C) = 1$
- $P(B | C) = 1 - P(\overline{B} | C)$
- 当 $A \subset B$ 时，$P(A | C) \geq P(B|C)$
- $P(A \cup B | C) = P(A | C) + P(B | C) - P(AB | C)$，特别地，若 $AB = \emptyset$，则 $P(A \cup B | C) = P(A | C) + P(B | C)$

### 乘法公式

当 $P(A) \neq 0, P(B) \neq 0$ 时，

$$ P(AB) = P(A) \cdot P(B | A) = P(B) \cdot P(A | B) $$

称此等式为概率的乘法公式。该公式可推广至多个事件的情形，一般地，当 $P(A_1 A_2 \cdots A_{n-1}) \neq 0 (n \geq 3)$ 时，有

$$ P(A_1 A_2 \cdots A_n) = P(A_1) P(A_2 | A_1) P (A_3 | A_1 A_2) \cdots P(A_n | A_1 A_2 \cdots A_{n-1}) $$

此外，当 $ P(AC) \neq 0$ 时，有

$$ P(AB | C) = P(A | C) P(B | AC) $$

### 全概率公式、贝叶斯公式

设 $S$ 为某一随机试验的样本空间，$B_1, B_2, \cdots, B_n$ 为该试验的一组事件，且满足：

- $B_i B_j = \emptyset, i, j = 1, 2, \cdots, n, i \neq j$
- $B_1 \cup B_2 \cup \cdots \cup B_n = S$

则称 $B_1, B_2, \cdots, B_n$ 为 $S$ 的一个划分，或称 $S$ 的一个完备事件组。

设 $S$ 为某一试验的样本空间，若 $B_1, B_2, \cdots, B_n$ 是 $S$ 的一个划分，且 $P(B_j) > 0, j = 1, 2, \cdots, n$，则对任一事件 $A$ 有

$$ P(A) = \sum_{j=1}^{n}P(B_j)P(A | B_j) $$

称上式为概率的全概率公式。

设 $S$ 为某一试验的样本空间，若 $B_1, B_2, \cdots, B_n$ 是 $S$ 的一个划分，且 $P(B_j) > 0, j = 1, 2, \cdots, n$，则对任一事件 $A, P(A) \neq 0$ 有

$$ P(B_k | A) = \frac{P(B_k A)}{P(A)} = \frac{P(B_k) P(A | B_k)}{\sum_{j=1}^{n}P(B_j)P(A | B_j)}, k = 1, 2, \cdots, n $$

称上式为概率的贝叶斯公式，或逆概公式。常称 $P(B_j)$ 为先验概率，$P(B_j | A)$ 为后验概率。

## 事件的独立性与独立试验

设 $A, B$ 为两随机事件，当

$$ P(AB) = P(A) \cdot P(B) $$

时，称事件 $A, B$ 相互独立。

当 $P(A) \cdot P(B) \neq 0$ 时，事件 $A, B$ 相互独立等价于 “条件概率等于无条件概率”，即

$$ P(B | A) = P(B)$$

当事件 $A, B$ 相互独立时，$A$ 与 $\overline{B}$，$\overline{A}$ 与 $B$，$\overline{A}$ 与 $\overline{B}$ 均相互独立。

设 $n$ 个事件 $A_1, A_2, \cdots, A_n (n \geq 2)$，若对其中任意 $k$ 个事件 $A_{i_1}, A_{i_2}, \cdots, A_{i_k} (2 \leq k \leq n)$，都有

$$ P(A_{i_1} A_{i_2} \cdots A_{i_k}) = \prod_{j=1}^{k}P(A_{i_j}) $$

成立，则称事件 $A_1, A_2, \cdots, A_n$ 相互独立。

对于可列个事件，若其中任意有限个事件相互独立，则称这可列个事件相互独立。

结果互不影响的一系列事件称为独立试验，若各个子实验在相同条件下进行，称为重复试验。

概率很小的事件在一次试验中几乎不发生。这称为实际推断原理。