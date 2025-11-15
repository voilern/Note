---
comments: true
---

# Lecture 8: Dynamic Programming

## 基本步骤

我们通常按如下 4 个步骤设计一个动态规划算法：

1. 刻画一个最优解的结构特征，也即定义基本的状态；
2. 递归地定义最优解的值，通常会得到一个递推表达式（也称为状态转移方程）、base case 和其它边界条件；
3. 计算最优解的值，通常采用自底向上的方法，并使用迭代法计算；
4. 重构一个相应的最优解。

其中步骤 1 - 3 是动态规划算法求解问题的基础。步骤 3 中，通常我们使用迭代进行计算，但实际上也可以利用递归法。将中间结果存入数组 / 哈希表中自顶向下计算，若表中有对应结果则直接返回，若没有则进行计算。这种方法称为记忆化搜索 ([OI Wiki - 记忆化搜索](https://oi-wiki.org/dp/memo/))。第 4 步并不是必要的，如果需要构造出的解本身而进行步骤 4，通常需要在步骤 3 的过程中维护一些额外信息用于构造最优解。

## 动态规划的应用

### 斐波那契数列

即 $F_N = F_{N-1} + F_{N - 2}$，要得到 $F_i$，只需 $F_{i-1}$ 和 $F_{i-2}$
的值。因此可以在计算过程中仅保留 $F_{i-1}$ 和 $F_{i-2}$，用于计算 $F_i$
后更新并进入下一轮计算即可。相较于没有做记忆化处理的尾递归实现，这种方式是 $O(N)$ 的。

```c
int Fibonacci (int N) {
    int i, Last, NextToLast, Answer;
    if (N <= 1) return 1;
    Last = NextToLast = 1;              // F(0) = F(1) = 1

    for (i = 2; i <= N; i++) {
        Answer = Last + NextToLast;     // F(i) = F(i - 1) + F(i - 2)
        NextToLast = Last;
        Last = Answer;                  // update F(i - 1) and F(i - 2)
    }

    return Answer;
}
```

### 矩阵乘法排序问题

对于多个矩阵相乘，其乘法顺序会显著影响运算次数。考虑穷举，令 $b_n$ 为计算矩阵乘法 $M_1 \cdot M_2 \cdot \cdots \cdot M_n$ 的顺序数，可知有 $b_2 = 1, b_3 = 2, b_4 = 5, \dots$。令 $M_{ij} = M_i \cdot \cdots \cdot M_j$，则 $M_{1n} = M_1 \cdot \cdots \cdot M_n = M_{1i} \cdot M_{i+1 \; n}$，则有 $b_n = \sum_{i=1}^{n-1}{b_i b_{n-i}}$，其中 $n > 1$ 且 $b_1 = 1$。推导可得 $b_n = O(\frac{4^n}{n\sqrt{n}})$，即 $b_n$ 为卡塔兰数。显然我们并不满足于如此大的时间复杂度。

假设 $M_i$ 是规模为 $r_{i-1} \times r_i$ 的矩阵，令矩阵乘法 $M_i \cdot \cdots \cdot M_j$ 的最优成本为 $m_{ij}$，我们有如下的递推表达式：

$$
m_{ij} = \left\{
    \begin{aligned}
    & 0 & \text{if} \; i = j \\
    & min_{i \leq l < j}\{{m_{il} + m_{l+1 \; j} + r_{i-1} r_l r_j}\} & \text{if} \; j > i
    \end{aligned}
\right.
$$

```c
// r contains number of columns for each of the N matrices.
// r[0] is the number of rows in matrix 1.
// Minimum number of multiplications is left in M[1][N] 
void OptMatrix(const long r[], int N, TwoDimArray M) {
    int i, j, k, L;
    long thisM;

    for (i = 1; i <= N; i++) M[i][i] = 0;
    for (k = 1; k < N; k++) {
        for (i = 1; i <= N - k; i++) {
            j = i + k;
            M[i][j] = INF;
            for (L = i; L < j; L++) {
                thisM = M[i][L] + M[L + 1][j] + r[i - 1] * r[L] * r[j];
                if (thisM < M[i][j])
                    M[i][j] = thisM;
            }
        }
    }
}
```

### 最优二叉查找树

最优二叉查找树 (Optimal Binary Search Trees, OBST) 问题

解法与矩阵乘最优顺序类似，令 

- $T_{ij}$：由单词 $w_i \dots w_j$ 构成的 OBST
- $c_{ij}$：$T_{ij}$ 的成本，$c_{ii} = p_i$
- $r_{ij}$：$T_{ij}$ 的根节点
- $w_{ij}$：$T_{ij}$ 的权重，即为 $\sum_{k = i}^{j}{p_k}$，其中 $w_{ii} = p_i$

令 $w_k = r_{ij}$，则其成本为

$$
\begin{aligned}
c_{ij}
& = p_k + \text{cost}(L) + \text{cost}(R) + \text{weight}(L) + \text{weight}(R) \\
& = p_k + c_{i, k-1} + c_{k+1, j} + w_{i, k-1} + w_{k+1, j} \\
& = w_{ij} + c_{i, k-1} + c_{k+1, j}
\end{aligned}
$$

若 $T_{ij}$ 最优，则应满足 $c_{ij} = w_{ij} + \min_{i < l \leq j}\{{c_{i, l-1} + c_{l + 1, j}}\}$。

### Floyd 算法

Floyd 算法 (Floyd Shortest Path Algorithm)

有递推关系 $ D^k[i][j] = \min\\{D^{k-1}[i][j], D^{k-1}[i][k] + D^{k-1}[k][j]\\} $。代码实现如下：

```c
// A[] contains the adjacency matric with A[i][i] = 0
// D[] contains the values of the shortest path
// N is the number of vertices
// A negative cycle exists iff D[i][i] < 0
void AllPairs(TwoDimArray A, TwoDimArray D, int N) {
    int i, j, k;
    for (i = 0; i < N; i++)
        for (j = 0; j < N; j++)
        D[i][j] = A[i][j];

    for (k = 0; k < N; k++)
        for (i = 0; i < N; i++)
            for (j = 0; j < N; j++)
                if (D[i][k] + D[k][j] < D[i][j])
                    D[i][j] = D[i][k] + D[k][j];
}
```

### 产品组装

产品组装 (Product Assembly) 问题

### 背包问题

背包问题 (Knapsack Problem)

## 失效情况

动态规划并不适用于所有问题，当存在以下两种情况时我们无法应用动态规划：

1. 没有最优子结构，例如存在 history-dependency
2. 子问题间没有 overlapping