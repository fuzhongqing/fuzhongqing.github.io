---
title:  "RSA算法及其数学原理"
tags: "RSA"
---

RSA算法是一种用于在网络上安全地传输消息的非对称加密算法。它的安全性基于**将大数相乘容易，但大数因式分解非常困难的**。 例如，很容易计算 31 乘以 37 是否等于 1147，但试图找到 1147 的因数是一个很难得过程。 

## 概述

介绍RSA前，先弄懂什么是非对称加密，用下面的例子来说明：假设 Alice 希望向 Bob 发送一颗珍贵的钻石，但传输过程很容易被偷窃。 Alice 和 Bob 都有各种各样的锁，但他们不拥有相同的锁，这意味着他们的钥匙无法打开对方的锁。

 > Alice是如何将钻石寄给 Bob 的？

 方案：

 > - Bob 首先向 Alice 发送一个未上锁的挂锁。请注意，这个未上锁的挂锁是公开透明的， Bob 可以给任何人一个未上锁的挂锁，因为挂锁的唯一用途是可以让其他人向 Bob 发送一些东西。
 > - Alice 将钻石放到箱子里，并且用Bob的锁锁上。
 > - 当Bob收到箱子时，用钥匙打开锁。

尽管这个概念实际上略有不同但是这个例子展示了非对称加密背后的思想。 Alice 使用 Bob 的公钥加密她的消息，Alice 将加密的消息发送给Bob，Bob 用私钥解密接收到的加密信息。

## 基本数学概念

### 素数

质数（Prime number），又称素数，指在大于1的自然数中，除了1和该数自身外，无法被其他自然数整除的数（也可定义为只有1与该数本身两个正因数的数）。前1000个素数:

>2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101, 103, 107, 109, 113, 127, 131, 137, 139, 149, 151, 157, 163, 167, 173, 179, 181, 191, 193, 197, 199, 211, 223, 227, 229, 233, 239, 241, 251, 257, 263, 269, 271, 277, 281, 283, 293, 307, 311, 313, 317, 331, 337, 347, 349, 353, 359, 367, 373, 379, 383, 389, 397, 401, 409, 419, 421, 431, 433, 439, 443, 449, 457, 461, 463, 467, 479, 487, 491, 499, 503, 509, 521, 523, 541, 547, 557, 563, 569, 571, 577, 587, 593, 599, 601, 607, 613, 617, 619, 631, 641, 643, 647, 653, 659, 661, 673, 677, 683, 691, 701, 709, 719, 727, 733, 739, 743, 751, 757, 761, 769, 773, 787, 797, 809, 811, 821, 823, 827, 829, 839, 853, 857, 859, 863, 877, 881, 883, 887, 907, 911, 919, 929, 937, 941, 947, 953, 967, 971, 977, 983, 991, 997, ...

### 最大公约数

能够整除多个整数的最大正整数。而多个整数不能都为零。例如8和12的最大公因数为4。

$$gcd(8, 12)=4$$


### 互质

互质（英文：Coprime，符号：⊥，又称互素）。在数论中，如果两个或两个以上的整数的最大公约数是1，则称它们为互质。

$$gcd(a, b)=1 \Leftrightarrow a \perp b$$

### 同余

两个整数$$a$$，$$b$$，若它们除以正整数$$m$$所得的余数相等，则称$$a$$，$$b$$对于$$m$$同余,记作

$$ a\equiv b (\mod m) $$

读作a同余于b模m，或读作a与b关于模m同余。

比如$$ 26 \equiv 14 (\mod 12)$$。

### 欧拉函数

在数论中，对正整数n，欧拉函数 $$\phi(n)$$ 是小于等于n的正整数中与n互质的数的数目。

例如$$\phi(8) = 4$$，因为1,3,5,7均和8互质。

- 如果 $$p$$ 是一个素数，则  $$\phi(p)=p-1$$ 。
- 若$$m \perp n$$ ，则$$\phi(mn)=\phi(m)\phi(n)$$ （积性）。

### 欧几里得算法 (辗转相除法)

两个整数的最大公约数等于其中较小的数和两数余数的最大公约数。

$$gcd(a, b)=gcd(b,a \mod b),  a > b > 0 $$

例如:

$$ gcd(34, 10) = gcd(10, 34\mod10) = gcd(10, 4) = gcd(4, 2) = gcd(2, 0) $$

所以 34 和 10 的最大公约数是2。

### 扩展欧几里得算法

给定二个整数$$a$$、$$b$$，必存在整数$$x$$、$$y$$使得$$ax + by = gcd(a,b)$$。

### 欧拉定理


有整数$$a$$, $$p$$，且 $$a \perp p$$，那么 $$a^{\phi(p)} \equiv 1(\mod p)$$。

### 模逆元



## RSA 算法

RSA 的实现大量使用了模运算、欧拉定理和欧拉函数。算法的每一步只涉及乘法，因此计算机运行算法会很快：

- 首先，接收者选择两个大素数$$p$$, $$q$$，他们的乘积 $$n = pq$$, 这个n就是公钥的一半内容。
- 接着，接受者计算 $$\phi(pq) = (p-1)(q-1)$$，并且选择一个和$$phi(pq)$$互质的数 $$e$$。通常，$$e$$选为 $$2^16+1=65537$$，有些情况下他可以小到 $$3$$，$$e$$将会是公钥的另一半内容。
- 接收者计算 $$e \mod \phi(n)$$的模逆元 $$d$$，换句话说，求 d 满足 $$ de \equiv 1 (\mod \phi(n)) $$，d就是私钥。
- 接受者发布公钥 n 和 e，将私钥d保密。


既然已经生成了公钥和私钥，就可以根据需要多次重复使用它们。发送者按照一下步骤：

- 首先，发件人将他的消息转换成一个数字m。一种常见的转换过程使用 ASCII 字母表：

Ascii 'H' => 72
Ascii 'E' => 69
Ascii 'L' => 76
Ascii 'O' => 79

例如，消息“HELLO”将被编码为 7269767679。m<n 很重要，否则在取模 n 时消息会丢失，因此如果 n 小于 m ，可以把消息m分片发送。

- 然后发送方计算 $$ c \equiv m^e \pmod{n} $$。 c 是密文或说加密消息，另外还有公钥 (n, e)，这是攻击者能够窃取的唯一信息。

- 接收方计算 $$ c^d \equiv m \pmod nc $$，从而获得原始数 m

