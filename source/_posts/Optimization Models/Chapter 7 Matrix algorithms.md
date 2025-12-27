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
3. 对此特征向量左乘$\bm{A}$便可以得到$\lambda _1 \bm{u}_1$，再左乘$\bm{u}^\dagger$，便可以得到标量$\lambda _1$

幂迭代的一个主要优点是该算法主要依赖于矩阵与向量的乘法，因此可以利用$\bm{A}$的任何特殊结构，例如稀疏性。幂迭代方法的两个主要缺点是

1. 它只能求出一个特征值（模最大的那个）及其对应的特征向量
2. 它的收敛速度取决于$\lvert \lambda _2 \rvert / \lvert \lambda _1 \rvert$，因此当该比值接近$1$时，性能可能会很差。克服这些问题的一种方法是对矩阵 A 的适当移位版本应用幂迭代算法，后续将进行讨论

### 1.2 平移-反幂法

给定一个复标量$\sigma$，以及$\bm{A} \in \mathbb{R} ^{n,n}$可对角化，考虑矩阵
$$
\bm{B}_{\sigma } = (\bm{A} - \sigma \bm{I})^{-1}
$$
根据谱映射定理，见Section 3.7.2 ，$\bm{B}_{\sigma }$与$\bm{A}$有相同的特征向量，且$\bm{B}_{\sigma }$的特征值为$\mu _i = (\lambda _i - \sigma )^{-1}$，其中$\lambda _i,i=1,\cdots ,n$是$\bm{A}$的特征值。$\bm{B}_{\sigma }$的最大模特征值 $\mu _{\max}$现在对应于在复平面上最接近$\sigma$的$\lambda _i$。将幂法应用于$\bm{B}_{\sigma }$，我们因此可以得到最接近所选$\sigma$的特征值$\lambda _i$以及相应的特征向量。移位-反幂法如下所示

![algorithm2.png](https://minio.wblv66.top/optimization-models/algorithm2.png)
