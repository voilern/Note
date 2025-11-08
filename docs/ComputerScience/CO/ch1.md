---
comments: true
---

# Chapter 1: Computer Abstractions and Technology

## Eight Great Ideas in Computer Architecture

1. 面向摩尔定律的设计（Design for Moore's Law）：摩尔定律指出单芯片上所集成的晶体管资源每 18 至 24 个月翻一番；  
2. 使用抽象简化设计（Use abstraction to simplify design）：提高硬件和软件生产率的主要技术之一是使用抽象来表示不同的设计层次，通过隐藏低层细节，为高层提供一个更简单的模型；  
3. 加速经常性事件（Make the common case fast）：加速经常性事件比优化罕见情形能够更好地提升性能，并且通常情况下经常性事件比罕见情形更加简单，也更易于优化；  
4. 通过并行提高性能（Performance via Parallelism）：包括不同层次的并行，如数据级并行、指令级并行、线程级并行与进程级并行；
5. 通过流水线提高性能（Performance via Pipelining）：并行性的一种特殊场景，由于其在计算机体系结构中十分普遍，因此它有专有名称：流水线；
6. 通过预测提高性能（Performance via Prediction）：假设从预测错误中恢复的代价并不高，且预测相对准确，则平均来说进行预测并开始工作可能比等到明确结果后再执行更快；
7. 存储层次（Hierarchy of Memories）：通过存储层次处理内存成本与性能间的冲突；
8. 通过冗余提高可靠性（Dependability via redundancy）：通过引入冗余组件来使系统更加可靠，该组件在发生故障时可以替代失效组件并帮助检测故障。

## Below Program

下图是一个从高级编程语言到汇编语言再到机器语言的典型过程：

```mermaid
graph LR
    A[High-level language program]
    B[Assembly language program]
    C[Binary machine language program]
    A -- Complier --> B
    B -- Assembler --> C
```

## Components of Computer

任何一台计算机的基础硬件都要完成相同的基本功能：输入、输出、处理和存储数据。因而，计算机由 5 个经典部件构成，分别是输入、输出、存储器、数据通路（也称作运算器）和控制器，其中后两个部件通常合称为处理器。我们也将处理器称作中央处理单元（Central Processor Unit），即 CPU。

内存（Memory）是程序运行时的存储空间，同时也用于存储程序运行时所使用的数据。内存由 DRAM 芯片组成（Dynamic Random Access Memory，动态随机访问存储器），DRAM 可随机访问任何地址的内存。

在存储器内部使用另一种存储器：高速缓存（Cache Memory）。高速缓存是一种小而快的存储器，一般作为大而慢的存储器的缓冲。其采用 SRAM 组成（Static Random Access Memory，静态随机访问存储器），速度相较于 DRAM 更快，集成度更低。

硬件与底层软件之间的接口的抽象被命名为计算机指令系统体系结构（Instruction Set Architecture，ISA），或简称体系结构，包含了需要编写正确运行的机器语言程序所需要的全部信息，包括指令、寄存器、存储器访问和 I/O 等。提供给应用程序员的基本指令集和操作系统接口合称为应用二进制接口（Application Binary Interface, ABI）。

对于易失性存储与非易失性存储，我们将前者称为主存储或主要存储（Main Memory or Primary Memory），将后者称为辅助存储（Secondary Memory）。

## Technologies for Building Processors and Memory

集成电路的成本可以用下面 3 个简单公式来表示：

$$
\text{每晶片的价格} = \frac{\text{每晶圆的价格}}{\text{每晶圆的晶片数} \times \text{良率}} \\[1.5em]
\text{每晶圆的晶片数} \approx \frac{\text{晶圆面积}}{\text{晶片面积}} \\[1.5em]
\text{工艺良率} = \frac{1}{(1 + (\text{单位面积的缺陷数} \times \text{芯片面积} / 2))^2}
$$

## Performance

### Definition

为衡量计算机的性能，我们有若干不同方式进行定义，此处我们引入两个概念：

- 响应时间（response time）：也称为执行时间，指计算机完成某任务所需的总时间；
- 吞吐率（throughput）：也称为带宽（bandwidth），指单位时间内完成的任务数量。

一般来说，降低响应时间几乎总是可以增加吞吐率，而增加吞吐率则不一定能够降低响应时间。为了使性能最大化，我们希望任务的响应时间最小化。对于某个计算机 X，我们可将性能与执行时间的关系表达为：

$$
\text{性能}_X = \frac{1}{\text{执行时间}_X}
$$

我们将使用 “X 的执行速度是 Y 的 n 倍” 的表述方式，即

$$
\frac{\text{性能}_X}{\text{性能}_Y} = n
$$

如果 X 的执行速度是 Y 的 n 倍，那么在 Y 的执行时间是在 X 上的执行时间的 n 倍，即

$$
\frac{\text{性能}_X}{\text{性能}_Y} = \frac{\text{执行时间}_Y}{\text{执行时间}_X} = n
$$

### Measurement

程序的执行时间一般以秒为单位，然而时间也可以用不同的方式定义，这取决于我们所计数的内容。对时间最直接的定义为挂钟时间（wall clock time），也叫响应时间、运行时间（elapsed time）等，这些术语均表示完成某项任务所需的总时间，包括了磁盘访问、内存访问、I/O 活动和操作系统开销等一切时间。

而一个处理器通常需要同时运行多个程序，此时系统可能更侧重于优化吞吐率。因此我们往往要将运行自己任务的时间与一般的运行时间区别开来。可以使用 CPU 执行时间（CPU execution time）来进行区别，简称为 CPU 时间，只表示在 CPU 上花费的时间。其还可以进一步分为：

- 用户 CPU 时间（user CPU time）：程序本身所花费的 CPU 时间
- 系统 CPU 时间（system CPU time）：为执行程序而花费在操作系统上的时间

要精确区分这两种 CPU 时间是困难的。为了一致，我们保持区分基于响应时间的性能和基于 CPU 执行时间的性能。用系统性能（system performance）表示空载系统的响应时间，用 CPU 性能（CPU performance）表示用户 CPU 时间。

由于几乎所有计算机的构建都需要基于时钟，用于确定各类事件在硬件中的发生时刻。这些离散时间间隔被称为时钟周期数（clock cycle，也称为滴答数、时钟数等），即计算机一个时钟周期的时间，时钟周期的倒数称为时钟频率。

### CPU Performance and its Factors

在有了以上的定义后，我们可以提出如下的性能公式：

$$
\text{CPU 时间} = \text{CPU 时钟周期数} \times \text{时钟周期长度} \\[1.5em]
\text{CPU 时间} = \frac{\text{CPU 时钟周期数}}{\text{时钟频率}}
$$

### Instruction Performance

上述性能公式并未涉及程序所需的指令数，考虑指令数（instruction count），我们有：

$$
\text{CPU 时钟周期数} = \text{指令数} \times \text{CPI}
$$

指令平均时钟周期数（clock cycle per instruction，CPI）表示执行每条指令所需的时钟周期平均数。显然有：

$$
\text{CPI} = \frac{\text{CPU 时钟周期数}}{\text{指令数}}
$$

### Classic CPU Performance Equation

综上，我们有基本性能公式：

$$
\text{CPU 时间} = \text{指令数} \times \text{CPI} \times \text{时钟周期长度} \\[1.5em]
\text{CPU 时间} = \frac{\text{指令数} \times \text{CPI}}{\text{时钟频率}}
$$

需要铭记于心的是，时间是唯一对计算机性能进行测量的完整而可靠的指标，我们不能用性能公式的一个真子集去度量性能。

## The Power Wall

当前在集成电路技术中占统治地位的是 CMOS，其主要能耗来源是动态能耗，即在晶体管开关过程中产生的能耗，即晶体管的状态经过一次翻转的能量。动态能耗取决于每个晶体管的负载电容和工作电压：

$$
\text{能耗} \propto \text{负载电容} \times \text{电压}^2
$$

这个等式表示的是一次 $1 \rightarrow 0 \rightarrow 1$ 的逻辑转换过程中消耗的能量，一个晶体管消耗的能量为：

$$
\text{能耗} \propto 1/2 \times \text{负载电容} \times \text{电压}^2
$$

每个晶体管需要的功耗是一次翻转需要的能耗和开关频率的乘积：

$$
\text{功耗} \propto 1/2 \times \text{负载电容} \times \text{电压}^2 \times{开关频率}
$$

其中开关频率是时钟频率的函数，负载电容是连接到输出上的晶体管数量（称为扇出）和工艺的函数，该函数决定导线和晶体管的电容。

## Others

### Amdahl's Law

改进后的程序执行时间可用下面的 Amdahl 定律计算：

$$
\text{改进后的执行时间} = \frac{\text{受改进影响的执行时间}}{\text{改进量}} + \text{不受影响的执行时间}
$$

该定律阐述了 “对于特定改进的性能提升可能由所使用的改进特征的数量所限制” 的规则，可以视作边际效益递减定律的量化版本。

### MIPS

一种取代时间以度量性能的尺度是每秒百万条指令数，即 MIPS。对于一个给定的程序，MIPS 可表示为：

$$
\begin{aligned}
\text{MIPS} 
&= \frac{\text{指令数}}{\text{执行时间} \times 10^6} \\[1.5em]
&= \frac{\text{指令数}}{\frac{\text{指令数} \times \text{CPI}}{\text{时钟频率}} \times 10^6} \\[1.5em]
&= \frac{\text{时钟频率}}{\text{CPI} \times 10^6}
\end{aligned}
$$

MIPS 是指令执行的速率，它规定性能与执行时间成反比，越快的计算机具有越高的 MIPS 值。然而用 MIPS 作为性能度量指标存在以下问题：无法比较具有不同指令系统的计算机、同一计算机上的不同程序具有不同的 MIPS、MIPS 可能独立于性能而发生变化。