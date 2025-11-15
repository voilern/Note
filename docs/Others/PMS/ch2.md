# 第二章：随机变量及其概率分布

## 随机变量

设随机试验的样本空间为 $S$，若 $X = X(e)$ 为定义在样本空间 $S$ 上的实值单值函数，$e \in S$，则称 $X = X(e) 为随机变量。

## 离散型随机变量

若随机变量的所有可能取值为有限个或可列个，则称此随机变量为离散型随机变量。离散型随机变量的统计规律通常用概率分布律来描述。设 $X$ 为离散型随机变量，若其可能取值为 $x_1, x_2, \cdots, x_k, \cdots$，则称

$$ P \lbrace X = x_k \rbrace = p_k, k = 1, 2, \cdots$$

为 $X$ 的概率分布律或概率分布列，简称为 $X$ 的分布律或分布列。概率分布律需满足以下两条性质：

- $p_k \geq 0, k = 1, 2, \cdots$
- $\sum_{k = 1}^{+\infty} p_k = 1$

### 0-1 分布

若随机变量 $X$ 的概率分布律为

$$ P\lbrace X = 0 \rbrace = 1 - p, P\lbrace X = 1 \rbrace = p, 0 < p < 1 $$

则称 $X$ 服从参数为 $p$ 的 0-1 分布，也称作两点分布，记作 $X \sim 0 - 1(p)$ 或 $X \sim B(1, p)$。其概率分布律也可以写成

$$ P \lbrace X = k \rbrace = p^k (1-p)^{1-k}, k = 0, 1. $$

### 二项分布

若随机变量 $X$ 的概率分布律为

$$ P \lbrace X = k \rbrace = C_{n}^{k} p^{k} (1 - p)^{n - k}, k = 0, 1, 2, \cdots, n, 0 < p < 1, n \geq 1 $$

则称 $X$ 服从参数为 $(n, p)$ 的二项分布，记作 $X \sim B(n, p)$。

设在 $n$ 次独立重复试验中，每个试验都只有两个结果：$A, \overline{A}$，且每次试验中 $A$ 发生的概率不变，记 $P(A) = p, 0 < p < 1$，称这一系列试验为 $n$ 重伯努利试验。设 $X$ 为在 $n$ 次试验中 $A$ 发生的次数，则 $X \sim B(n, p)$。

### 泊松分布

若随机变量 $X$ 的概率分布律为

$$ P \lbrace X = k \rbrace = \frac{e^{- \lambda} \lambda^{k}}{k!}, k = 0, 1, 2, \cdots, \lambda > 0 $$

则称 $X$ 服从参数为 $\lambda$ 的泊松分布，记作 $X \sim P(\lambda)$。

当 $n$ 充分大，$p$ 充分小（一般要求 $p < 0.1$），且 $np$ 保持适当大小时，参数为 $(n, p)$ 的二项分布也可用泊松分布近似描述。也就是说，当 $n$ 充分大，$p$ 足够小时，有

$$ C_{n}^{k} p^{k} (1 - p)^{n - k} \approx \frac{e^{- \lambda} \lambda^{k}}{k!} $$

### 超几何分布

若随机变量 $X$ 的概率分布律为

$$ P \lbrace X = k \rbrace = \frac{C_{a}^{k} C_{b}^{n - k}}{C_{N}^{n}}, k = l_{1}, l_{1} + 1, \cdots, l_{2}, l_{1} = \max \lbrace 0, n - b \rbrace, l_{2} = \min \lbrace a, n \rbrace $$

则称 $X$ 服从参数为 $(n, a, N)$ 的超几何分布，记作 $X \sim H(n, a, N)$。

### 几何分布

若随机变量 $X$ 的概率分布律为

$$ P\lbrace X = k \rbrace = p (1 - p)^{k - 1}, k = 1, 2, \cdots $$

则称 $X$ 服从参数为 $p$ 的几何分布。

### 帕斯卡分布

若随机变量 $X$ 的概率分布律为

$$ P\lbrace X = k \rbrace = C_{k - 1}^{r - 1} p^{r} (1 - p)^{k - r}, k = r, r + 1, r + 2, \cdots $$

则称 $X$ 服从参数为 $(r, p)$ 的帕斯卡分布，也称作负二项分布，记作 $X \sim NB(r, p)$。

## 随机变量的概率分布函数

设 $X$ 为一随机变量，$x$ 为任意实数，函数

$$ F(x) = P \lbrace X \leq x \rbrace $$

称为随机变量 $X$ 的概率分布函数，简称分布函数。

对任意的实数 $x_1, x_2 (x_1 < x_2)$，有

$$ P \lbrace x_1 < X \leq x_2 \rbrace = P \lbrace X \leq x_2 \rbrace - P \lbrace X \leq x_1 \rbrace = F(x_2) - F(x_1)$$

即 $X$ 落在区间 $\left (x_1, x_2 \right ]$ 的概率为两端点处分布函数值之差。

分布函数具有以下性质：

- $F(x)$ 单调不减。
- $0 \leq F(x) \leq 1$，且有 $\lim_{a \to -\infty} F(a) = 0, \lim_{b \to +\infty} F(b) = 1$，简记为 $F(-\infty) = 0, F(+\infty) = 1$。
- $F(x + 0) = F(x)$，即 $F(x)$ 是右连续函数。

只要函数 $F(x)$ 满足以上三条性质，其就可以作为某随机变量的分布函数。

## 连续型随机变量

对于随机变量 $X$，其分布函数为 $F(x)$，若存在一个非负的实值函数 $f(x), -\infty < x < +\infty$，使得对于任意实数 $x$ 有

$$ F(x) = \int_{-\infty}^{x} f(t) \mathrm{d} t $$

则称 $X$ 为连续型随机变量，称 $f(x)$ 为 $X$ 的概率密度函数，简称密度函数。

可知连续型随机变量的分布函数是连续的，其密度函数 $f(x)$ 具有以下性质：

- $f(x) \geq 0$
- $\int_{-\infty}^{+\infty} f(x) \mathrm{d} x = 1$
- $\forall x_{1}, x_{2} \in \mathbb{R}, x_{1} < x_{2}, P\lbrace x_{1} < X \leq x_{2} \rbrace = F(x_{2}) - F(x_{1}) = \int_{x_{1}}^{x_{2}} f(t) \mathrm{d} t$
- 在 $f(x)$ 的连续点 $x$ 处，$F'(x) = f(x)$

### 均匀分布

若随机变量 $X$ 具有密度函数

$$
f(x) = 
\begin{cases}
\frac{1}{b - a}, & x \in (a, b) \\
0, & \text{else}
\end{cases}
$$

则称 $X$ 服从区间 $(a, b)$ 上的均匀分布，记作 $X \sim U(a, b)$。其对应的分布函数为

$$
F(x) = 
\begin{cases}
0, & x < a \\
\frac{x - a}{b - a}, & a \leq x < b \\
1, & x \geq b
\end{cases}
$$

### 正态分布

若随机变量 $X$ 具有密度函数

$$ f(x) = \frac{1}{\sqrt{2 \pi} \sigma} e^{-\frac{(x - \mu)^{2}}{2 \sigma^{2}}}, -\infty < x < +\infty, -\infty < \mu < +\infty, \sigma > 0 $$

则称 $X$ 服从参数为 $(\mu, \sigma)$ 的正态分布，$X$ 为正态变量，记作 $X \sim N(\mu, \sigma^{2})$。其对应的分布函数为

$$ F(x) = \int_{-\infty}^{x} \frac{1}{\sqrt{2 \pi} \sigma} e^{-\frac{(t - \mu)^{2}}{2 \sigma^{2}}} \mathrm{d} t $$

正态分布中常用积分变量代换 $\frac{x - \mu}{\sigma} = t$。

正态变量 $X$ 的密度函数 $f(x)$ 具有以下性质：

- $f(x)$ 关于 $x = \mu$ 对称。
- $\max_{-\infty < x < +\infty} f(x) = f(\mu) = \frac{1}{\sqrt{2 \pi} \sigma}$
- $\lim_{|x - \mu| \to +\infty} f(x) = 0$

常称参数 $\mu$ 为位置参数，参数 $\sigma$ 为尺度参数。特别地，当 $\mu = 0, \sigma = 1$ 时，称 $Z \sim N(0, 1)$ 中的 $Z$ 服从标准正态分布，其密度函数为 

$$ \phi(x) = \frac{1}{\sqrt{2 \pi}} e^{-\frac{x^{2}}{2}}, -\infty < x < +\infty $$

其相应的分布函数为 

$$ \Phi(x) = \int_{-\infty}^{x} \frac{1}{\sqrt{2 \pi}} e^{-\frac{t^{2}}{2}} \mathrm{d} t $$

对于标准正态分布，有 $\Phi(x) + \Phi(-x) = 1$。

通过变量代换，可以得到当 $X \sim N(\mu, \sigma^{2})$ 时，有 

$$ P \lbrace a < X < b \rbrace = \Phi(\frac{b - \mu}{\sigma}) - \Phi(\frac{a - \mu}{\sigma}) $$

### 指数分布

若随机变量 $X$ 具有密度函数

$$
f(x) = 
\begin{cases}
\lambda e^{-\lambda x}, & x > 0 \\
0, & x \leq 0
\end{cases}
$$

其中 $\lambda > 0$，则称 $X$ 服从参数为 $\lambda$ 的指数分布，记作 $X \sim E(\lambda)$。其对应的分布函数为

$$
F(x) = 
\begin{cases}
1 - e^{-\lambda x}, & x > 0 \\
0, & x \leq 0
\end{cases}
$$

当 $X \sim E(\lambda)$ 时，对任意的 $t > 0, t_0 > 0$，有

$$ P\lbrace X > t_{0} + t \rbrace = P\lbrace X > t_{0} \rbrace \cdot P\lbrace X > t \rbrace $$

这被称作指数分布的无记忆性。

### $\Gamma$ 分布

若随机变量 $X$ 具有密度函数

$$
f(x) = 
\begin{cases}
\frac{\lambda e^{-\lambda x} (\lambda x)^{\alpha - 1}}{\Gamma(\alpha)}, & x > 0 \\
0, & x \leq 0
\end{cases}
$$

其中参数 $\alpha > 0, \lambda > 0$，则称 $X$ 服从参数为 $(\alpha, \lambda)$ 的$\Gamma$ 分布，记为 $X \sim \Gamma (\alpha, \lambda)$。

式中的 $\Gamma (\alpha) = \int_{0}^{+\infty} e^{-x} x^{\alpha - 1} \mathrm{d} x$，且有 $\Gamma(\alpha) = (\alpha - 1) \Gamma (\alpha - 1)$。特别地，当 $n$ 为正整数时，有 $\Gamma (n) = (n - 1)!$。

### 二参数威布尔分布

若随机变量 $X$ 具有密度函数

$$
f(x) = 
\begin{cases}
\frac{\gamma}{\alpha} (\frac{x}{\alpha})^{\gamma - 1} e^{-(\frac{x}{\alpha})^{\gamma}}, & x > 0 \\
0, & x \leq 0
\end{cases}
$$

其中参数 $\alpha > 0, \lambda > 0$，则称 $X$ 服从参数为 $(\alpha, \lambda)$ 的二参数威布尔分布。

### $\beta$ 分布

若随机变量 $X$ 具有密度函数

$$
f(x) = 
\begin{cases}
\frac{\Gamma (a + b)}{\Gamma (a) \Gamma (b)} x^{\alpha - 1} (1 - x)^{b - 1}, & 0 < x < 1 \\
0, & \text{else}
\end{cases}
$$

其中参数 $a > 0, b > 0$，则称 $X$ 服从参数为 $(a, b)$ 的 $\beta$ 分布，记作 $X \sim \beta (\alpha, \lambda)$。

## 随机变量函数的分布

设 $X$ 为一连续型随机变量，其密度函数为 $f_{X} (x)$，随机变量 $Y = g(X)$。若函数 $y = g(x)$ 为一处处可导的严格单调函数，记 $y = g(x)$ 的反函数为 $x = h(y)$，则 $Y$ 的密度函数为

$$
f_{Y} (y) = 
\begin{cases}
f_{X} (h(y)) \cdot |h'(y)|, & y \in D \\
0, & y \notin D
\end{cases}
$$

其中 $D$ 为函数 $y = g(x)$ 的值域。
