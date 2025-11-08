---
comments: true
---

# Chapter 3: Arithmetic for Computers

## Addition and Subtraction

二进制的加法与十进制类似，数字从右到左逐位相加，并将进位 (carry) 传递至高位。减法利用加法实现，减去一个二进制数相当于加上该数的补码。

硬件规模总是有限的，当运算结果超出限制时就会发生溢出 (overflow)。对于有符号数的加法，当操作数符号不同时，显然不会发生溢出；在减法中则相反。在其它条件下则会发生溢出，我们有以下的溢出条件与溢出结果：

![](./img/ch3_1.png){.center}

