---
comments: true
---

# 2025 - 2026 秋冬 期中回忆卷

往年卷：

- [【学习天地】2022 - 23 春夏 计算机组成 马德班 期中考回忆卷]( https://www.cc98.org/topic/5600097){target="_blank"}

## 单选题

> 共 10 题，每题 2 分，共计 20 分。

1-1 冯诺依曼架构中描述不正确的一项是：

A. 指令与数据保存在同一个存储器  
D. 指令与数据通过格式区分
	
1-2 以下哪项是 CPU 最小时间单元：

A. Instruction execution time  
C. Clock cycle time  
D. I/O time

1-3 处理器的时钟频率为 2.4 GHz，计算混合 CPI 指令的 MIPS

| Instruction | Percent | CPI |
| :---------: | :-----: | :-: |
|      A      |   0.4   |  2  |
|      B      |  0.25   |  3  |
|      C      |   0.2   |  4  |
|      D      |  0.15   |  5  |  

A. 700  
B. 800  
C. 900  
D. 1000

1-4 下列哪项数值最大：

A. $(42)_{10}$  
B. $(52)_8$  
C. $(C2)_{16}$  
D. $(101011)_2$  

1-5 计算 $\text{0xFFFFFFE7} - \text{0x00000019}$ 的值

1-6 $x10 = \text{0x?}$ 分别执行 `srli x10, x10, 2` 与 `srai x10, x10, 2` 的结果

1-7 两个等长的浮点数，一个 `exp` 较长，一个 `frac` 较长，问哪个范围大、哪个精度高

1-8 以下属于系统软件的是：

- Operating System
- Compiler
- Browser
- Device Driver

## 填空题

> 共 10 空，每空 2 分，共计 20 分。

2-1 将 $(2025)_{16}$ 转为八进制  

2-2 将单精度浮点数 $-11.53125$ 按照 IEEE-754 标准转为二进制表示，分三空分别填入 $S$、$\text{Biased Exponent}$、$\text{Fraction}$  

2-3 已知 $PC = \text{0x20000000}$，求 `B-type` 指令可跳转的地址范围

2-4 已知 $x1 = \text{0x7FFFFFFF}$、$x2 = \text{0x00000001}$、$x3 = \text{0x12345678}$，若 `add` 结果发生溢出则不保存结果，跳过该指令。在执行 `add x3, x1, x2` 后 $x3$ 中的值是多少

2-5 $-6.75$ 的单精度二进制表示

2-6 `n` 的值为

```c
int m = -32767;
unsigned int n = m;
```

2-7 已知 $x10 = \text{0x}?$，求执行 `add x10, x10, x11` 后能使得结果发生溢出的 $x11$ 的范围

## 汇编与机器码

> 共 6 题，每题 5 分，共计 30 分。

3-1 RISC-V - 机器码

1. `add x1, x2, x3`  
2. `sw x9, 120(x10)`
3. `beq x7, x8, 100`

3-2 机器码 - RISC-V
	
1. `0x0700006F`  
2. `0x01498933`
3. `0xFFDEOCE3`  

## 汇编与反汇编

> 共 2 题，每题 5 分，共计 10 分。

4-1 反汇编

```c
int find_max(int arr[], int n) {
	int max = arr[0];
	for (int i = 1; i < n; i++) {
		if (arr[i] > max) {
			max = arr[i];
		} 
	}
}
```

4-2 汇编

```asm
mystery:
	
recurse:

return:
```

## Datapath 设计

> 共 20 分，各问分值分别为 5、5、6、4。

5-1 在单周期 CPU Datapath 的基础上新增指令 `mw rd, rs1, rs2` 即 `Reg[rd] = Mem[Reg[rs1]] + Reg[rs2]`，问：

1. 所需新增元件  
2. 修改已有元件  
3. 绘制 Datapath  
4. 所需控制信号  