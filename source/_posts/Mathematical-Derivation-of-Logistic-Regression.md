---
title: Mathematical Derivation of Logistic Regression
date: 2017-05-26 21:20:16
tags: 机器学习
mathjax: true
---
###Logistic回归数学推导

设训练输入数据$x$, 各权重值为$w_0,w_1,w_2$...$w_n$ 则
$$ \kappa  (x)=w_0+w_1x_1+w_2x_2...+w_nx_n \tag{1} $$


这里 $x_1,x_2...x_n$是数据$x$的n维特征。
最终以sigmod函数输出$f(x)$, 则
$$ f(x)=\frac{1}{1+e^{-x}} \tag{2} $$
结合1和2，
$$ f(x)=\frac{1}{1+e^{-\kappa (x)}} \tag{3} $$

sigmod函数是个S型曲线。  
![sigmod](/img/logistic_curve_svg.png)

Logistic回归结果是个二项分布，不是1就是0.  
设在$(x;w)$的条件下，输出结果为1的概率:
$$
P(y=1|x;w) =\mu(x) = \frac{1}{1+e^{-\kappa (x)}} \tag{4}
$$
那么结果为0的概率
$$
P(y=0|x;w) = 1-\mu(x) = \frac{1}{1+e^{\kappa (x)}} \tag{5}
$$

设有m个独立的训练数据 $x=(x_1,x_2...x_m)$，我们的目的是找到一组n维特征权重参数 $w=(w_1,w_2...w_n)$，使得这m个数据的输出结果的概率最有可能发生(极大似然估计)。由于这m个数据相互独立，而且满足二项分布，得到极大似然函数
$$ L(x) = \prod_{i=1}^{m}(\mu(x_i))^{y_i}(1-\mu(x_i))^{1-y_i} \tag{6} $$

左右两边取对数,
$$
\ln L(x) =  \sum_{i=1}^{m} (y_i\ln \mu(x_i)+(1-y_i)\ln(1-\mu(x_i))) \tag{7}
$$

对每个参数$w$求偏导数，例如对$w_k$求偏导数（注意:累加的导数等于各项导数的累加）

$$
\begin{align} \notag
\frac{\partial  \ln L(x)}{\partial  w_k}  &=  \sum_{i=1}^{m}\left[\frac{y_i}{\mu(x_i)}\mu(x_i)'-\frac{1-y_i}{1-\mu(x_i)}\mu(x_i)'\right]  \tag{8}\\  
&= \sum_{i=1}^{m}\left[(\frac{y_i}{\mu(x_i)}-\frac{1-y_i}{1-\mu(x_i)})\mu(x_i)'\right] \tag{9} \\
&= \sum_{i=1}^{m}\left[(\frac{y_i}{\mu(x_i)}-\frac{1-y_i}{1-\mu(x_i)})[\mu(x)(1-\mu(x))]\kappa(x)' \right] \tag{10} \\
&= \sum_{i=1}^{m}\left[ (y_i-\mu(x_i))\kappa(x)' \right] \tag{11} \\
&= \sum_{i=1}^{m}\left[ (y_i-\mu(x_i))x_{ik} \right ] \tag{12}
\end{align}
$$

这里$y_i$表示对应训练数据$x_i$的标记结果(0或1)，$\mu(x_i)$表示对应$x_i$的logistic输出结果，$x_{ik}$表示对应$x_i$的第k维参数。  
当$\frac{\partial  \ln L(x)}{\partial  w_k}=0$时，$\ln
L(x)$取极大值，n个参数就有n个偏导数方程。  
直接求方程有一定的难度，计算机一般利用梯度下降法来求最优参数$w_k$，按照梯度的反方向我们总能找到极小值。(注意：此处求极大值，因此是梯度上升的方向)

$$w_k=w_k+\lambda\frac{\partial  \ln L(x)}{\partial  w_k} $$

这里$\lambda$表示步长。
