---
title: Chapter 7 Matrix algorithms
date: 2025-12-23 16:34:30
# updated:
# tags:
#     - 
categories: Optimization Models
# keywords:
# description:
top_img: transparent
# comments:
# cover:
# toc:
# toc_number:
# toc_style_simple:
# copyright:
# copyright_author:
# copyright_author_href:
# copyright_url:
# copyright_info:
# mathjax:
# katex:
# aplayer:
# highlight_shrink:
# aside:
# abcjs:
# noticeOutdate:
---

## 1. 计算特征值和特征向量

### 1.1 幂迭代法(The power iteration method)

在本节中，我们概述了一种计算可对角化矩阵的特征值和特征向量的技术。幂迭代(Power Iteration，PI)方法可能是计算矩阵一个特征值/特征向量对的最简单方法。它的收敛速度相对较慢，并且存在一些局限性。然而，我们在这里介绍它，因为它构成了许多其他更精细的特征值计算算法的基础。还有许多其他计算特征值和特征向量的技术，其中一些是为具有特殊结构的矩阵设计的，例如稀疏矩阵、带状矩阵或对称矩阵

设$\bm{A} \in \mathbb{R} ^{n,n}$，假设$\bm{A}$可对角化，并记$\lambda _1 ,\cdots , \lambda _n$为$\bm{A}$的特征值，按模递减顺序排列，即$\lvert \lambda _1 \rvert > \lvert \lambda _2 \rvert \geq  \cdots  \geq \lvert \lambda _n \rvert$（注意我们假设$\lvert \lambda _1 \rvert$严格大于$\lvert \lambda _2 \rvert$，即$\bm{A}$有一个主导特征值）。由于$\bm{A}$可对角化，我们可以将其写为$\bm{A} = \bm{U \Lambda U}^{-1}$，其中我们可以在不失一般性的情况下假设构成$\bm{U}$列的特征向量$\bm{u}_1 , \cdots , \bm{u}_n$已归一化，使得$\lVert \bm{u}_i \rVert_2 = 1$。我们有$\bm{A}^k = \bm{U} \bm{\Lambda}^k \bm{U}^{-1}$，那么
$$
\bm{A}^k \bm{U} = \bm{U\Lambda}^k
$$
现在令$\bm{x} \in \mathbb{C} ^n$为随机选择的试验向量，且$\lVert \bm{x} \rVert ^2 =1$。由于$\bm{U}$的列彼此线性无关，它们可以张成整个$\mathbb{C} ^{n}$。可以定义$\bm{x} = \bm{Uw}$，并考虑
$$
\bm{A}^k \bm{x} = \bm{A}^k\bm{Uw} = \bm{U\Lambda}^k \bm{w} = \sum_{i=1}^{n} w_i \lambda_i^k \bm{u}_i
$$
请注意，如果随机选择$\bm{x}$，那么$\bm{w}$的第一个元素$w_1$以概率$1$非零。将前面的表达式乘以和除以$\lambda _1^k$，我们可以得到
$$
\bm{A}^k \bm{x} = \lambda _1^k \sum_{i=1}^{n} w_i \left(  \frac{\lambda _i}{\lambda _1} \right)^k \bm{u}_i = w_1 \lambda _1^k \left( \bm{u}_1 + \sum_{i=2}^{n} \frac{w_i}{w_1} \left( \frac{\lambda _i}{\lambda _1} \right)^k  \bm{u}_i \right)
$$
也就是说，$\bm{A}^k \bm{x}$在$\bm{u}_1$的方向上有一个分量$\alpha _k \bm{u}_1$，并且在$\bm{u}_2,\cdots ,\bm{u}_n$的方向上有一个分量$\alpha _k \bm{z}$，即
$$
\bm{A}^k \bm{x} = \alpha _k \bm{u}_1 + \alpha _k \bm{z}, \alpha _k = w_1 \lambda _1^k \in \mathbb{C} , \bm{z} = \sum_{i=2}^{n} \frac{w_i}{w_1} \left( \frac{\lambda _i}{\lambda _1} \right)^k  \bm{u}_i
$$
对于$\bm{z}$分量的大小，设$\beta _i = w_i / w_1$，我们有
$$
\begin{aligned}
\lVert \bm{z} \rVert_2 =& \left \lVert \sum_{i=2}^{n} \beta _i \left( \frac{\lambda _i}{\lambda _1} \right)^k  \bm{u}_i \right \rVert_2 \leq \sum_{i=2}^{n} \left \lVert  \beta _i \left( \frac{\lambda _i}{\lambda _1} \right)^k  \bm{u}_i \right \rVert_2 \\

=& \sum_{i=2}^{n}  \lvert  \beta _i \rvert \left \lvert \frac{\lambda _i}{\lambda _1} \right \rvert^k \lVert \bm{u}_i \rVert_2 = \sum_{i=2}^{n}  \lvert  \beta _i \rvert \left \lvert \frac{\lambda _i}{\lambda _1} \right \rvert^k \\

\leq & \left \lvert \frac{\lambda _2}{\lambda _1} \right \rvert^k \sum_{i=2}^{n}\lvert  \beta _i \rvert
\end{aligned}
$$
最后的不等式是由特征值模的大小顺序得出的。由于$\lvert \lambda _2 / \lambda _1  \rvert < 1$，我们有$\bm{z}$分量的大小在$k \to \infty$时趋于零，收敛速率由比值$\lvert \lambda _2 \rvert / \lvert \lambda _1 \rvert$决定。因此$\bm{A}^k \bm{x} \to \alpha _k \bm{u}_1$，这意味着随着$k \to \infty$，$\bm{A}^k \bm{x}$趋向于与$\bm{u}_1$平行。因此，通过对向量$\bm{A}^k \bm{x}$进行归一化，我们得到
$$
\lim_{k \to \infty} \frac{\bm{A}^k \bm{x}}{\lVert \bm{A}^k \bm{x} \rVert_2} = \bm{u}_1
$$
定义
$$
x(k) = \frac{\bm{A}^k \bm{x}}{\lVert \bm{A}^k \bm{x} \rVert_2}
$$
并且还注意到$x(k) \to \bm{u}_1$意味着$\bm{A}x(k) \to \bm{Au}_1 = \lambda _1 \bm{u}_1$，因此$\bm{x}^\dagger(k)\bm{A} x(k) \to \lambda _1 \bm{u}^\dagger _1 \bm{u}_1$（$\dagger$表示厄米共轭，因为$\bm{u}_i$向量可以是复数值的）。因此，回想一下$\bm{u}^\dagger _1 \bm{u}_1 = \lVert \bm{u}_1 \rVert_2^2 = 1$，我们有
$$
\lim_{k \to \infty}  \bm{x}^\dagger(k)\bm{A} x(k) = \lambda _1
$$
也就是说，乘积$\bm{x}^\dagger(k)\bm{A} x(k)$会收敛到$\bm{A}$的模最大的特征值
$$
\begin{aligned}
    &x^\dagger (k)\bm{A} x(k) \\
    =& \frac{(\bm{A}^k \bm{x})^\dagger\bm{A} (\bm{A}^k \bm{x})}{ \lVert \bm{A}^k \bm{x} \rVert_2^2} \\
    =& \frac{(\alpha _k \bm{u}_1 + \alpha _k \bm{z})^\dagger \bm{A} (\alpha _k \bm{u}_1 + \alpha _k \bm{z})}{(\alpha _k \bm{u}_1 + \alpha _k \bm{z})(\alpha _k \bm{u}_1 + \alpha _k \bm{z})} \\
    =& \frac{( \bm{u}_1 +  \bm{z})^\dagger \bm{A} (\bm{u}_1 +  \bm{z})}{( \bm{u}_1 +  \bm{z})( \bm{u}_1 +  \bm{z})}  \\
    =& \frac{\bm{u}^\dagger _1 \bm{A} \bm{u}_1 + \bm{u}^\dagger _1 \bm{A} \bm{z} + \bm{z}^\dagger \bm{A} \bm{u}_1 + \bm{z}^\dagger \bm{A} \bm{z}}{\bm{u}^\dagger _1  \bm{u}_1 + \bm{u}^\dagger _1  \bm{z} + \bm{z}^\dagger  \bm{u}_1 + \bm{z}^\dagger \bm{z}}  \\
    =& \frac{\bm{u}^\dagger _1 \lambda _1 \bm{u}_1 + \bm{u}^\dagger _1 \bm{A} \bm{z} + \bm{z}^\dagger \lambda _1 \bm{u}_1 + \bm{z}^\dagger \bm{A} \bm{z}}{1 + \bm{u}^\dagger _1  \bm{z} + \bm{z}^\dagger  \bm{u}_1 + \bm{z}^\dagger \bm{z}}  \\
    =& \frac{ \lambda _1 + \bm{u}^\dagger _1 \bm{A} \bm{z} + \bm{z}^\dagger \lambda _1 \bm{u}_1 + \bm{z}^\dagger \bm{A} \bm{z}}{1 + \bm{u}^\dagger _1  \bm{z} + \bm{z}^\dagger  \bm{u}_1 + \bm{z}^\dagger \bm{z}}
\end{aligned}
$$
其他项中$\bm{u}_1$和$\bm{A}$均不会随$k$而变化，而$\bm{z}$中含有$\left( \lambda _i / \lambda _1 \right)^k$，因此$k \to \infty$时$\bm{z} \to \bm{0}$。因此$x^\dagger (k)\bm{A} x(k)$会收敛到$\lambda _1$，收敛速度由$\lvert \lambda _2 \rvert / \lvert \lambda _1 \rvert$比例决定，并且以线性速度收敛

以上推理提出了以下迭代算法

![algorithm1.png](https://minio.wblv66.top/optimization-models/algorithm1.png)

算法总结：

1. $\bm{x}$可以任取，只需要满足$\lVert \bm{x} \rVert_2=1$便可以
2. 不断对$\bm{x}$左乘$\bm{A}$并归一化便可以趋近于特征向量$\bm{u} _1$
3. 在迭代的过程中$x^\dagger \bm{A} x$也会趋近于标量$\lambda _1$
4. 这里在求解$\lambda _1$时不需要再次对向量归一化，因为$\lambda _1$并不参与迭代，它的取值不会影响迭代速度

幂迭代的一个主要优点是该算法主要依赖于矩阵与向量的乘法，因此可以利用$\bm{A}$的任何特殊结构，例如稀疏性。幂迭代方法的两个主要缺点是

1. 它只能求出一个特征值（模最大的那个）及其对应的特征向量
2. 它的收敛速度取决于$\lvert \lambda _2 \rvert / \lvert \lambda _1 \rvert$，因此当该比值接近$1$时，性能可能会很差。克服这些问题的一种方法是对矩阵 A 的适当移位版本应用幂迭代算法，后续将进行讨论

### 1.2 移位-逆幂法

给定一个复标量$\sigma$，以及$\bm{A} \in \mathbb{R} ^{n,n}$可对角化，考虑矩阵
$$
\bm{B}_{\sigma } = (\bm{A} - \sigma \bm{I})^{-1}
$$
根据谱映射定理，见Section 3.7.2 ，$\bm{B}_{\sigma }$与$\bm{A}$有相同的特征向量，且$\bm{B}_{\sigma }$的特征值为$\mu _i = (\lambda _i - \sigma )^{-1}$，其中$\lambda _i,i=1,\cdots ,n$是$\bm{A}$的特征值。$\bm{B}_{\sigma }$的最大模特征值 $\mu _{\max}$现在对应于在复平面上最接近$\sigma$的$\lambda _i$。将幂法应用于$\bm{B}_{\sigma }$，我们因此可以得到最接近所选$\sigma$的特征值$\lambda _i$以及相应的特征向量。移位-逆幂法如下所示

![algorithm2.png](https://minio.wblv66.top/optimization-models/algorithm2.png)

算法总结：

1. $\sigma$选取要尽可能接近目标特征值，这样经过移位$\lambda -\sigma$后，数值变为最小，再取逆后数值变为最大
2. $\bm{B}_{\sigma }$与$\bm{A}$有相同的特征向量，因此不断左乘$\bm{B}_{\sigma }$得到的特征向量也是$\bm{A}$的特征向量
3. 由于最终要求解的是$\bm{A}$的特征值，因此对特征向量左乘的矩阵是$\bm{A}$

移位-逆幂法相对于幂迭代法的优势在于，我们现在可以快速（但仍然是线性速度）收敛到任意所需的特征值，只需选择一个足够接近目标特征值的移位$\sigma$。然而，移位-逆幂法要求预先已知目标特征值的一个较好的近似值。如果事先不知道这样的良好近似值，该方法的一个变体是先用一个粗略的近似值$\sigma$启动算法，然后在某个时刻，当获得了特征向量的合理近似后，动态修改移位$\sigma$，重复这个过程，不断迭代地改进$\sigma$。这个思想将在下一段中讨论

### 1.3 瑞利商(Rayleigh quotient)迭代

假设在移位-逆幂算法的某一步中，我们有一个近似特征向量$\bm{x}(k) \neq \bm{0}$。那么，我们寻找某个近似特征值$\sigma _k$，即一个近似满足特征值/特征向量方程的标量
$$
\bm{x}(k) \sigma _k \approx \bm{Ax}(k)
$$
这里所谓近似是指我们寻找$\sigma _k$，使得方程残差的平方范数最小，即$\min \lVert \bm{x}(k) \sigma _k - \bm{Ax}(k) \rVert$。通过要求该函数对$\sigma _k$的导数为零，我们得到
$$
\begin{aligned}
   \frac{\partial \big(\bm{x}(k) \sigma _k - \bm{Ax}(k))^\dagger (\bm{x}(k) \sigma _k - \bm{Ax}(k)\big)}{\partial \sigma_k} &= 0 \\
   \frac{\partial \big(\sigma _k \bm{x}^\dagger (k) \bm{x}(k) \sigma _k   - 2\bm{x}^\dagger (k)\bm{A} \bm{x}(k) \sigma _k \big)}{\partial \sigma_k} &= 0 \\
   \frac{\bm{x}^\dagger (k)\bm{A} \bm{x}(k)}{\bm{x}^\dagger (k) \bm{x}(k)} &= \sigma _k
\end{aligned}
$$
这被称为瑞利商(Rayleigh quotient)，参见Section第4.3.1节。如果我们按照在移位-逆幂算法中自适应地选择移位，就得到了所谓的瑞利商迭代法，如下所示。与幂迭代方法不同，瑞利商迭代法可以被证明**具有局部二次收敛性**，也就是说，在经过一定次数迭代后，第$k+1$次迭代中解的收敛差距与第$k$次迭代中解的差距的平方成正比

算法总结：

1. 瑞丽商迭代法需要先使用幂迭代算法或者移位-逆幂算法迭代一定次数，以得到近似的特征向量值
2. 这里在求解$\sigma _k$时需要再次对向量归一化，因为浮点数运算是有精度误差。如果归一化，这些微小的误差会在不断迭代中不断放大

### 1.4 使用幂迭代计算特征值分解

矩阵$\bm{A} \in \mathbb{R} ^{m,n}$的奇异值分解的因子可以通过计算两个对称矩阵$\bm{AA}^\top$和$\bm{A}^\top \bm{A}$的谱分解来获得。事实上，我们在Section 5定理 5.1 的证明中已经看到，$\bm{V}$因子是来自$\bm{A}^\top \bm{A}$谱分解的特征向量矩阵
$$
\bm{A}^\top \bm{A} = \bm{V \Lambda}_n \bm{V}^\top
$$
并且$\bm{U}$因子的列是$\bm{AA}^\top$的特征向量矩阵
$$
\bm{AA}^\top = \bm{U \Lambda}_m \bm{U}^\top
$$
$\bm{\Lambda }_n$和$\bm{\Lambda }_m$是对角矩阵，其前$r$个对角元素是平方奇异值$\sigma_i^2,i=1,\cdots ,r$其余对角元素为零

接下来，我们将概述如何使用幂迭代法来确定与矩阵最大奇异值对应的左奇异向量和右奇异向量。基本思路是对对称矩阵$\bm{A}^\top \bm{A}$和$\bm{AA}^\top$应用幂迭代，但以隐式方式进行，从而绕过对该矩阵的显式计算，因为该矩阵通常是稠密的。

$\bm{v}(k)$的序列对应于对$\bm{A}^\top \bm{A}$应用幂迭代；同样，$\bm{u}(k)$的序列对应于对$\bm{AA}^\top$应用幂迭代。因此，下面的算法计算矩阵$\bm{A}$的最大奇异值$\sigma _1$，以及相关的左奇异向量$\bm{u}_1$和右奇异向量$\bm{v}_1$($\sigma = \bm{u}_1^\top \bm{A} \bm{v}_1$)，前提是有占优特征值

然后，这种技术可以递归地应用于矩阵A的压缩版本，以确定其他奇异值及其对应的左奇异向量和右奇异向量。更准确地说，我们定义矩阵
