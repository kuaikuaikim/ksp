---
title: Mathematical Derivation of RSA Encryption Algorithm
date: 2017-05-25 21:56:43
tags: 加密算法
mathjax: true
---
###RSA算法数学推导

p和q都是质数
$$
n = pq
$$
取n欧拉函数$\phi(n)$
$$
\phi(n) = (p-1)(q-1)
$$

取 $e,d < \phi(n)$ ,e,d与$\phi(n)$互质,且满足:
$$  
ed \equiv 1 \pmod {\phi(n)} \tag{1}
$$

所以:
$$
ed = k\phi(n)+1 \tag{2}
$$

公钥就是 (n,e), 私钥就是(n,d)。  
假设明文内容: m, 密文内容: c。  
加密过程:
$$
m^e = c \pmod{n}
$$
解密过程:
$$
c^d = m \pmod{n}
$$


以上RSA加解密等同于证明:
$$
m^{ed} \equiv m \pmod {n} \tag{3}
$$

把2代入3，则:
$$
m^{k\phi(n)+1} \equiv m \pmod{n} \tag{4}
$$

即:
$$
m^{k\phi(n)}m \equiv m \pmod{n} \tag{5}
$$

当m和n互质时
$$
m^{\phi(n)} \equiv 1 \pmod{n} \\
$$
$$
(m^{\phi(n)})^k \equiv 1 \pmod{n} \tag{7}
$$

$$
m \equiv m \pmod{n} \tag{8}
$$

7和8根据同余式相乘法则，即可证明式5，4,最终证明3
$$
m^{ed} \equiv m \pmod {n} \tag{3}
$$
