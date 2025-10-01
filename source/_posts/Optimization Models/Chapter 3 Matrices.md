---
title: Chapter 3 Matrices
date: 2025-09-28 10:43:55
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

## 1. 矩阵基础

### 1.1 将矩阵视为数字的数组

矩阵(Matrix)是数组的矩形数组，形式为

$$ \bm{A}=\left[
\begin{array}{cccc}
a_{11} & a_{12} & \cdots & a_{1n} \\
a_{21} & a_{22} & \cdots & a_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
a_{m1} & a_{m2} & \cdots & a_{mn}
\end{array}\right] $$

这个矩阵有$m$行(rows)$n$列(columns)，若是元素为实数，我们可以说$\bm{A} \in \mathbb{R}^{m,n}$；若是元素为复数，我们可以说$\bm{A} \in \mathbb{C}^{m,n}$。矩阵的每一行都是行向量，每一列都是列向量。矩阵的转置(transposition)操作是将矩阵的行列交换

$$[\bm{A}^\top]_{ij} = [\bm{A}]_{ji}$$

符号$[\bm{A}]_{ij}$(有时候简写为$\bm{A}_{ij}$)代表矩阵第$i$行$j$列的元素。$\mathbb{R}^{m,n}$中的零矩阵表示为$\bm{0}_{m,n}$，或者直接简写为$\bm{0}$

矩阵的数乘(multiplication by a scalar)被定义为矩阵中的每个元素都与标量相乘；矩阵的加法(两个矩阵大小相同)被定义为矩阵对应位置的元素相加。定义了这些运算后，我们可以将$\mathbb{R}^{m,n}$看作一个向量空间

### 1.2 矩阵乘积

如果两个矩阵的尺寸符合，他们才可以相乘。i.e. $\bm{A} \in \mathbb{R}^{m,n}, \bm{B} \in \mathbb{R}^{n,p}$，矩阵的乘法$\bm{AB} \in \mathbb{R}^{m,p}$被定义为

$$ [\bm{AB}]_{ij} = \sum^n_{k=1}\bm{A}_{ik}\bm{B}_{kj} $$

矩阵乘法是非交换的(non-commutative)，这意味着一般情况下$\bm{AB} \neq \bm{BA}$

$n \times n$单位矩阵(identity matrix)表达为$\bm{I}_n$(也可以简写为$\bm{I}$)，它是对角(diagonal)元素为$1$其他元素为$0$的矩阵。单位矩阵满足$\bm{AI}_n=\bm{A}$，$\bm{A}$有$n$行；$\bm{I}_n\bm{B}=\bm{B}$，$\bm{B}$有$n$列

矩阵可以看成一组行向量的组合，也可以看成一组列向量的组合

$$
\bm{A} = [ \bm{a}_1 \quad \bm{a}_2 \quad \cdots \quad \bm{a}_n ],\text{or }
\bm{A} = \left[
\begin{array}{c}
\bm{\alpha}_{1}^\top \\
\bm{\alpha}_{2}^\top  \\
\vdots  \\
\bm{\alpha}_{m}^\top
\end{array}
\right]
$$

其中$\bm{a}_1,\cdots,\bm{a}_n \in \mathbb{R}^m$表示$\bm{A}$的列，即**列向量**；$\bm{\alpha}_1^\top,\cdots,\bm{\alpha}_n^\top \in \mathbb{R}^n$表示$\bm{A}$的行，即**行向量**

因此矩阵乘积可以写为

$$ \bm{AB} = \bm{A}
[ \bm{b}_1 \quad \cdots \quad \bm{b}_p ]  = [ \bm{Ab}_1 \quad \cdots \quad \bm{Ab}_p ]$$

$$ \bm{AB} = \left[
\begin{array}{c}
\bm{\alpha}_{1}^\top \\
\vdots  \\
\bm{\alpha}_{m}^\top
\end{array}
\right] \bm{B}  = \left[
\begin{array}{c}
\bm{\alpha}_{1}^\top \bm{B}\\
\vdots  \\
\bm{\alpha}_{m}^\top\bm{B}
\end{array}
\right] $$

最终矩阵的乘积可以被解释为二元矩阵(dyadic matrices)

$$ \bm{AB} =  \sum_{i=1}^n \bm{a}_i \bm{\beta}_i^\top$$

矩阵的乘积定义同样使用矩阵与向量的乘积

$$ \bm{Ab} = \sum_{k=1}^n \bm{a}_k b_k$$

$\bm{Ab}$的结果是一个向量，可以看成是对矩阵$\bm{A}$中的列向量进行线性组合，系数为$\bm{b}$中的元素。同样地，我们可以定义向量左乘矩阵

$$ \bm{c}^\top \bm{A} = \sum_{k=1}^m  c_k \bm{\alpha}_k^\top$$

定义$\bm{C}=\bm{AB}$，根据矩阵与向量乘积的定义，$\bm{C}=[\bm{Ab}_1 \quad \cdots \quad \bm{Ab}_p]$可以进一步拆分

$$ \bm{C}=[\bm{Ab}_1 \quad \cdots \quad \bm{Ab}_p] = [ \sum_{k=1}^n \bm{a}_k b_{1k} \quad \cdots \quad  \sum_{k=1}^n \bm{a}_k b_{pk}] $$

其中$b_{ij}$为向量$\bm{b}_i$的第$j$个元素，**因此矩阵$\bm{C}$的每一列都可以看成是对$\bm{A}$的列向量进行线性组合得到的**。同样地，$\bm{C}=[\bm{\alpha}_{1}^\top \bm{B} \quad \cdots \quad \bm{\alpha}_{m}^\top\bm{B} ]^\top$可以进一步拆分

$$ \bm{C} = \left[
\begin{array}{c}
\bm{\alpha}_{1}^\top \bm{B}\\
\vdots  \\
\bm{\alpha}_{m}^\top\bm{B}
\end{array}
\right] = \left[
\begin{array}{c}
\sum_{k=1}^m  \alpha_{1k} \bm{\beta}_k^\top\\
\vdots  \\
\sum_{k=1}^m  \alpha_{mk} \bm{\beta}_k^\top
\end{array}
\right] $$

其中$\alpha_{ij}$为向量$\bm{\alpha}_{i}^\top$的第$j$个元素，**因此矩阵$\bm{C}$的每一行都可以看成是对$\bm{B}$的行向量进行线性组合得到的**。

矩阵乘积的转置满足

$$ ( \bm{A}_1 \bm{A}_2 \cdots \bm{A}_p )^\top = \bm{A}_p^\top \cdots \bm{A}_2^\top  \bm{A}_1^\top $$

### 1.3 块矩阵乘积

只要保证块(block)大小一致，矩阵代数可以推广到块。首先考虑矩阵$\bm{A}$与向量$\bm{x}$的乘积，其中矩阵和向量都是分块的

$$
\begin{gather*}

\bm{A} = [ \bm{A}_1 \quad \bm{A}_2 ],\bm{x} = \left[
\begin{array}{c}
\bm{x}_1 \\
\bm{x}_2
\end{array}
\right] \\

\bm{Ax}= \bm{A}_1\bm{x}_1 + \bm{A}_2\bm{x}_2

\end{gather*}
$$

从符号上看这就像是行向量与列向量的内积。矩阵与矩阵相乘也可以进行类似展开

$$ \bm{AB} = [ \bm{A}_1 \quad \bm{A}_2] \left[
\begin{array}{c}
\bm{B}_1 \\
\bm{B}_2
\end{array}
\right] = \bm{A}_1\bm{B}_1 + \bm{A}_2\bm{B}_2 $$

<!-- 最终我们讨论外积(outer product)。当两个矩阵分别被看成列向量和行向量，矩阵乘法可以写成

$$ \bm{AB} =  \left[
\begin{array}{c}
\bm{A}_1 \\
\bm{A}_2
\end{array}
\right] \left[ \bm{B}_1 \quad \bm{B}_2 \right]=
\left[
\begin{array}{cc}
\bm{A}_1\bm{B}_1 & \bm{A}_1\bm{B}_2 \\
\bm{A}_2\bm{B}_1 & \bm{A}_2\bm{B}_2
\end{array}
\right] $$

当矩阵AB变成真的列向量和行向量时 -->

### 1.4 矩阵空间和内积

对于向量空间$\mathbb{R}^{m,n}$，可以赋予一个标准内积

$$ \langle \bm{A},\bm{B} \rangle = \text{trace} ( \bm{A}^\top \bm{B} ) $$

其中$\text{trace}(\bm{X})$是方阵的迹(trace)，定义为方阵主对角线上元素的和。这个内积引出了所谓的Frobenius范数

$$ \sqrt{ \langle \bm{A} , \bm{A} \rangle} = \sqrt{ \text{trace}\bm{AA}^\top} = \lVert\bm{A}\rVert_F \coloneqq \sqrt{\sum_{ij} a_{ij}^2}  $$

我们的选择与向量情况下的选择是一致的。实际上，上述内积表示的是通过将矩阵$\bm{A},\bm{B}$的所有列依次首尾相连展开得到的两个向量之间的标量积；因此，Frobenius范数就是矩阵向量化形式的欧几里得范数。

迹运算符是一个线性运算符，同时还有许多性质

$$
\begin{gather*}

\text{trace}  \bm{A}  = \text{trace} \bm{A}^\top \\

\text{trace} \bm{AB} = \text{trace} \bm{BA}
\end{gather*}
$$

## 2. 矩阵和线性映射

### 2.1 矩阵，线性和仿射映射

我们可以将矩阵解释为从输入空间到输出空间的作用的线性映射（向量值函数，即输出为向量）或者操作。我们回顾一下线性映射：当任意点$\bm{x},\bm{z} \in \mathcal{X}$和任意标量$\lambda,\mu \in \mathcal{Y}$满足$f( \lambda \bm{x} + \mu \bm{z} ) = \lambda f(\bm{x}) + \mu f(\bm{z})$那么映射$f:\mathcal{X}\rightarrow \mathcal{Y}$为线性。任意线性映射$f:\mathbb{R}^n\rightarrow \mathbb{R}^m$都可以用一个矩阵$\bm{A}\in\mathbb{R}^{m,n}$表示

![3.3](https://minio.wblv66.top/optimization-models/3.3 "3.3")

放射映射就是简单地在线性方程上加一个常数项，因此任意放射映射$f:\mathbb{R}^n\rightarrow \mathbb{R}^m$都可以表示为

$$ f( \bm{x} ) = \bm{A}\bm{x} + \bm{b}  $$

其中$\bm{A} \in \mathbb{R}^{m,n},\bm{b} \in \mathbb{R}^{m}$

将向量的每个元素按某个标量因子 进行缩放的线性映射，可以用对角矩阵来描述

### 2.2 非线性方程的近似

一个非线性映射（在该点可微）在给定点$\bm{x}_0$的邻域内(neighborhood)可以被近似为一个仿射映射

$$ f ( \bm{x} ) = f ( \bm{x}_0 ) + J_f ( \bm{x}_0 ) ( \bm{x}-\bm{x}_0 + o ( \lVert \bm{x} - \bm{x}_0 \rVert )  ) $$

当$\bm{x} \rightarrow \bm{x}_0$时$o ( \lVert \bm{x} - \bm{x}_0 \rVert )$比一阶(first order)收敛更快，$J_f$是雅可比矩阵，定义为

$$ J_f ( \bm{x}_0 ) \coloneqq \left[
\begin{array}{ccc}
    \frac{\partial f_1 }{\partial x_1} & \cdots & \frac{\partial f_1}{\partial x_n} \\
    \vdots & \ddots & \vdots \\
    \frac{\partial f_m }{\partial x_1} & \cdots & \frac{\partial f_m}{\partial x_n}
\end{array} \right]_{ \bm{x} = \bm{x}_0 } $$

因此对于接近$\bm{x}_0$的$\bm{x}$，变分$\delta_f ( \bm{x} ) \coloneqq f ( \bm{x} ) - f ( \bm{x}_0 )$可以用雅可比矩阵定义的线性映射来一阶近似

一个在$\bm{x}_0$处二阶可微的标量值函数（即输出为标量）可以使用梯度和二阶导数矩阵（海森矩阵）进行二阶局部近似

$$ f \approx f ( \bm{x}_0 ) + \Delta f ( \bm{x}_0 )^\top ( \bm{x} -\bm{x}_0 ) + \frac{1}{2}  ( \bm{x} -\bm{x}_0 )^\top \Delta^2 f ( \bm{x}_0 ) ( \bm{x} -\bm{x}_0 ) $$

其中$\Delta^2 f ( \bm{x}_0 )$是海森矩阵(Hessian)定义为

$$ \Delta^2 f ( \bm{x}_0 ) \coloneqq \left[
\begin{array}{ccc}
    \frac{\partial ^2 f}{\partial  x_1^2} & \cdots & \frac{\partial ^2 f}{\partial  x_1 \partial x_n} \\
    \vdots & \ddots & \vdots \\
    \frac{\partial ^2 f}{\partial  x_n \partial x_1} & \cdots & \frac{\partial ^2 f}{\partial x_n^2}
\end{array}
 \right]_{\bm{x}= \bm{x}_0} $$

在这种情况下，$f$在局部通过由Hessian矩阵定义的二次函数进行近似

### 2.3 值域，秩和零空间

考虑一个矩阵$\bm{A}$对它的列向量进行线性组合，得到的集合称为$\bm{A}$的值域(range)或者为列空间，被写为$\mathcal{R} ( \bm{A} )$

$$ \mathcal{R} ( \bm{A} ) = \{ \bm{A} \bm{x} \mid  \bm{x} \in \mathbb{R}^n \} $$

列空间是一个子空间。$\mathcal{R} ( \bm{A} )$的维数称为$\bm{A}$的秩(rank)，记作$\text{rank}( \bm{A} )$根据定义，秩表示$\bm{A}$线性无关的列数量，根据证明秩也等于线性无关的行数量。因此矩阵的秩等于它转置的秩

$$ \text{rank} ( \bm{A} )  = \text{rank} ( \bm{A}^\top )  $$

**证明**

假设$\bm{A} \in \mathbb{R}^{m,n}$的列秩为$c$，行秩为$r$，尝试将它拆分为$\bm{BC}$两个矩阵，其中$\bm{B}$矩阵由$\bm{A}$中线性独立的列向量组成，因为$\bm{A}$的每一列都可以通过$\bm{B}$中的列向量线性组合得到，根据矩阵乘积的定义得知拆分是合理的。此时$\bm{B}\in\mathbb{R}^{m,c},\bm{C}\in\mathbb{R}^{c,n}$。同时$\bm{A}$的每一行都可以看成由$\bm{C}$矩阵中的行向量线性组合得到的，因此$\bm{A}$的行空间维数不大于$\bm{C}$的行数，i.e. $r \leq c$

用同样的方式对$\bm{A}$的转置进行推导，可以得到$\bm{A}$转置的行空间维数$c$不大于列空间维数$r$，i.e. $c \leq r$。将两个结论对比只能取$r=c$。行秩等于列秩得到了证明

因此我们可以提出一个约束

$$ 0 \leq \text{rank} ( \bm{A} )   \leq \min ( m,n )  $$

矩阵$\bm{A}$的零空间(nullspace)是输入空间中被映射到$\bm{0}$的向量组成的集合，记作$\mathcal{N} ( \bm{A} )$

$$ \mathcal{N} ( \bm{A} ) = \{ \bm{x} \mid  \bm{A} \bm{x} = \bm{0} \} $$

零空间也是一个子空间

### 2.4 线性代数的基本理论

线性代数基本定理建立了矩阵的零空间与其转置的值域之间的重要联系。首先我们可以发现$\bm{A}^\top$值域中的任意向量都和$\bm{A}$零空间的任意向量正交，i.e. $\bm{x}^\top \bm{z} = 0,\forall \bm{x} \in \mathcal{R} ( \bm{A}^\top ), \forall \bm{z}\in \mathcal{N} ( \bm{A} )$。根据值域的定义，$\mathcal{R} ( \bm{A}^\top )$中的所有向量都可以写为$\bm{A}$中的行向量的线性组合，因此

$$ \bm{x}^\top \bm{z} = ( \bm{A}^\top \bm{y} )^\top \bm{z} = \bm{y}^\top \bm{A} \bm{z} = ( \bm{y}^\top \bm{A} ) \bm{z} = 0 $$

因此$\mathcal{R} ( \bm{A}^\top )$和$\mathcal{N} ( \bm{A} )$是正交子空间，i.e. $\mathcal{N}(\bm{A}) \perp \mathcal{R}(\bm{A}^\top)$或者写为$\mathcal{N}(\bm{A}) = \mathcal{R}(\bm{A}^\top)^\perp$。回顾第二章2.3节，子空间和其正交补的直和等于整个空间

$$ \mathbb{R}^n = \mathcal{N}(\bm{A}) \oplus \mathcal{N}(\bm{A})^\perp = \mathcal{N}(\bm{A}) \oplus \mathcal{R}(\bm{A}^\top) $$

同样的我们可以证明$\bm{z}^\top \bm{x} = 0,\forall \bm{x} \in \mathcal{R} (\bm{A}), \forall \bm{z}\in \mathcal{N} (\bm{A}^\top)$，因此$\mathcal{N}(\bm{A}^\top) \perp \mathcal{R}(\bm{A})$，输出空间可以分解为

$$ \mathbb{R}^m = \mathcal{R}(\bm{A}) \oplus \mathcal{R}(\bm{A})^\perp = \mathcal{R}(\bm{A}) \oplus \mathcal{N}(\bm{A}^\top) $$

**线性代数基本定理**：对于任意矩阵$\bm{A} \in \mathbb{R}^{m,n}$，有$\mathcal{N}(\bm{A}^\top) \perp \mathcal{R}(\bm{A}),\mathcal{N}(\bm{A}) \perp \mathcal{R}(\bm{A}^\top)$，因此

$$
\begin{gather*}
\mathcal{R}(\bm{A}^\top) \oplus \mathcal{N}(\bm{A}) = \mathbb{R}^n \\
\mathcal{R}(\bm{A}) \oplus \mathcal{N}(\bm{A}^\top) = \mathbb{R}^m
\end{gather*}
$$

并且

$$
\begin{gather*}
\dim\mathcal{N}(\bm{A}) + \text{rank}{A} = n \\
\dim\mathcal{N}(\bm{A}^\top) + \text{rank}{A} = m
\end{gather*}
$$

因此，我们可以将任意向量$\bm{x}$分解为两个互相正交的向量的和，一个在$\bm{A}^\top$的值域中，另一个在$\bm{A}$的零空间中：

$$ \bm{x}  = \bm{A}^\top\bm{y} + \bm{z},\bm{z} \in \mathcal{N}(\bm{A}) $$

类似地，我们可以将任意向量$\bm{x}$分解为两个互相正交的向量的和，一个在$\bm{A}$的值域中，另一个在$\bm{A}^\top$的零空间中：

$$ \bm{x}  = \bm{A}\bm{\phi } + \bm{\zeta},\bm{\zeta} \in \mathcal{N}(\bm{A}^\top) $$

![3.4](https://minio.wblv66.top/optimization-models/3.4 "3.4")

## 3. 行列式、特征值和特征向量

### 3.1 矩阵对直线的作用

我们首先讨论，一个线性映射$\bm{A}$如何作用于通过原点的直线（一维子空间）。考虑一个非零向量$\bm{u}\in \mathbb{R}^n$以及从原点出发原点并经过$\bm{u}$的直线，即集合$\mathcal{L} = \{ \bm{x}\mid  \bm{x} = \alpha\bm{u}, \alpha \in \mathbb{R}\}$。当矩阵作用于属于直线上的向量时，它会将该点旋转(rotate)一个固定角度$\theta_u$，并将它的长度按固定量$\gamma_u$放大或缩小(shrink/amplify)。旋转角度$\theta_u$和长度增益$\gamma_u$对于直线上的每个点都是恒定值

$$ \lVert \bm{y} \rVert_2 = \lVert \bm{Ax} \rVert_2 = \lvert \alpha \rvert \lVert \bm{Au} \rVert_2 = \frac{\lVert\bm{Au} \rVert_2}{\lVert \bm{u} \rVert_2} \lvert \alpha \rvert \lVert \bm{u} \rVert = \frac{\lVert \bm{Au}\rVert_2}{\lVert \bm{u} \rVert_2} \lVert \bm{x} \rVert_2 $$

长度增益为$\gamma_u=\tfrac{\lVert \bm{Au}\rVert_2}{\lVert \bm{u} \rVert_2}$，对于旋转角度

$$ \cos \theta_u = \frac{\bm{y}^\top \bm{x}}{\lVert \bm{x} \rVert_2 \lVert \bm{y} \rVert_2} = \frac{\bm{x}^\top \bm{A}^\top \bm{x}}{\lVert \bm{x} \rVert_2 \lVert \bm{y} \rVert_2} = \frac{\alpha^2 \bm{u}^\top \bm{A}^\top \bm{u}}{\gamma_u \alpha^2 \lVert \bm{u} \rVert^2_2} = \frac{ \bm{u}^\top \bm{A}^\top \bm{u}}{\gamma_u \lVert \bm{u} \rVert^2_2} $$

这二者都仅仅取决于直线的方向$\bm{u}$，而不取决于直线上的实际点

当$\lVert \bm{x} \rVert_2$保持不变且方向$\bm{u}$扫描所有可能的方向时，$\bm{x}$会沿圆周移动，而图中显示了相应的$\bm{y}$的轨迹

![3.5](https://minio.wblv66.top/optimization-models/3.5 "3.5")

通过数值实验可以发现，在这个例子中存在两个输入方向$\bm{u}(1)$、$\bm{u}(2)$，它们在由$\bm{A}$定义的映射下是角度不变的即角度$\theta_u$为零（或 $\pm 180^\circ$），此时$\bm{A}$在这些直线上表现为标量乘法

![3.6](https://minio.wblv66.top/optimization-models/3.6 "3.6")

### 3.2 行列式和单位立方体的变化

对于一个$2 \times 2$的矩阵

$$ \bm{A} = \left[
\begin{array}{cc}
    a_{11} & a_{12} \\
    a_{21} & a_{22}
\end{array}
\right]  $$

这个矩阵的行列式(determinant)被定义为

$$ \det \bm{A} \coloneqq a_{11}a_{22} - a_{12}a_{21}  $$

要求解一般矩阵的行列式，首先要定义标量$a$的行列式$\det a = a$，然后应用拉普拉斯行列式展开(Laplace’s determinant expansion)来计算

$$ \det (\bm{A}) = \sum_{j=1}^n(-1)^{i+j}a_{ij}\det \bm{A}_{(i,j)}  $$

其中$i$是任意一行，$\bm{A}_{(i,j)}$表示通过删除$\bm{A}$的第$i$行和第$j$列得到的一个$(n−1) \times (n−1)$子矩阵

假设我们将线性映射$\bm{y} = \bm{Ax}$应用于$\mathbb{R}^2$中单位正方形顶点的四个向量，变换后的点构成一个平行四边形的顶点

![3.6](https://minio.wblv66.top/optimization-models/3.6 "3.6")

单位正方形的面积为一。通过验证可以知道变换后的四边形（即平行四边形）的面积等于矩阵行列式的绝对值。可以证明，在一般维数$n$中，**矩阵$\bm{A}$的行列式的绝对值仍然描述了单位超立方体通过$\bm{A}$变换得到的平行多面体的体积**

行列式是定义在**方阵**上的实值函数，矩阵的行列式有以下性质

1. 交换矩阵的两行或者两列会改变行列式的符号
2. 行列式在矩阵的每一行/列上都是线性的
3. 单位矩阵的行列式为1

当变换后的立方体体积为零时，也就是行列式为零时，此时矩阵为奇异矩阵(singular)。此时矩阵某一行（或某一列）是另一行（或另一列）的倍数，列（和行）不再是线性无关的，并且矩阵具有非平凡的零空间（零空间不只有原点）。这意味着存在输入空间中的方向，沿着这些方向所有输入向量都被$\bm{A}$映射为零，可以证明

$$ \bm{A} \in \mathbb{R}^{n,n}\text{ is singular }\Leftrightarrow\det \bm{A}=0\boldsymbol{\Leftrightarrow}\mathcal{N}(\bm{A})\text{ is not equal to }\{0\}. $$

对于任意方阵$\bm{A},\bm{B}\in \mathbb{R}^{n,n}$有如下性质

$$ \begin{gather*}
    \det \bm{A} = \det \bm{A}^\top \\
    \det \bm{AB} = \det \bm{BA} = \det \bm{A} \det \bm{B} \\
    \det \alpha\bm{A} = \alpha^n\det \bm{A}
\end{gather*} $$

对于分块上三角(upper block-triangular)矩阵

$$ \bm{X} = \left[ \begin{array}{cc}
    \bm{X}_{11} & \bm{X}_{12} \\
    \bm{X}_{21} & \bm{X}_{22}
\end{array}
 \right]  $$

有如下结论

$$ \det \bm{X} =  \det \bm{X}_{11} + \det \bm{X}_{22}  $$

对于分块下三角(lower block-triangular)矩阵也有类似结论

### 3.3 矩阵的逆

对于一个非奇异矩阵$\bm{A}$，我们定义它的逆矩阵(inverse matrix)$\bm{A}^{-1}$定义为满足以下条件的唯一矩阵

$$ \bm{AA}^{-1} = \bm{A}^{-1}\bm{A} = \bm{I}_n $$

矩阵求逆有以下性质

$$ \begin{gather*}
    (\bm{AB})^{-1}=\bm{B}^{-1}\bm{A}^{-1} \\
    (\bm{A}^\top)^{-1} = (\bm{A}^{-1})^\top \\
    \det \bm{A} = \det \bm{A}^\top = \frac{1}{\det \bm{A}^{-1}}
\end{gather*}
 $$

对于非方阵或是奇异方阵不存在常规意义的逆矩阵，但是可以定义广义逆矩阵(generalized inverse)/伪逆矩阵(pseudoinverse)。对于一般矩阵$\bm{A}\in \mathbb{R}^{m,n}$，如果满足

$$\begin{gather*}
 \bm{A}^{li}\bm{A} = \bm{I}_n,m \geq n \\
 \bm{A}\bm{A}^{ri} = \bm{I}_m,n \geq m \\
\end{gather*}$$

则$\bm{A}^{li}$被称为$\bm{A}$的左逆,$\bm{A}^{ri}$被称为$\bm{A}$的右逆

如果$\bm{AA}^{pi} \bm{A} = \bm{A}$，则称矩阵$\bm{A}^{pi}$为$\bm{A}$的伪逆。左逆、右逆和伪逆将在Chapter 5中进一步讨论

### 3.4 相似矩阵

如果存在一个非奇异矩阵$\bm{P}\in \mathbb{R}^{n,n}$，使得两个矩阵$\bm{A},\bm{B}\in \mathbb{R}^{n,n}$满足如下条件，则称它们是相似(similar)的

$$ \bm{B} = \bm{P}^{-1}\bm{AP} $$

相似矩阵是同一线性映射在不同空间基的不同表现。考虑原空间的线性映射

$$ \bm{y} = \bm{Ax} $$

由于$\bm{P}$是非奇异的，其列向量是线性无关的，因此它们代表了$\mathbb{R}^{n,n}$的一组基。向量$\bm{y}$和$\bm{x}$可以在该基下表示为$\bm{P}$列向量的线性组合

$$ \begin{gather*}
    \bm{A}\tilde{\bm{y}} = \bm{y} \\
    \bm{A}\tilde{\bm{x}} = \bm{x}
\end{gather*}  $$

线性映射在新基下表达为

$$ \bm{y} = \bm{Ax} \quad \Rightarrow \quad \tilde{\bm{y}}= \bm{P}^{-1}\bm{AP} \tilde{\bm{x}} = \bm{B} \tilde{\bm{x}} $$

### 3.5 特征向量和特征值

我们前面在研究矩阵对直线的作用时提到过，矩阵会对对直线上的点（向量）进行旋转和放缩，我们现在将视角从$\mathbb{R}^n$扩大到$\mathbb{C}^n$。特征向量(eigenvector)只是$\mathbb{C}^n$中在矩阵作用下角度不变的方向，特征值(eigenvalue)是对点放缩的系数。更准确地说，如果存在$\lambda \in \mathbb{C}$是矩阵$\bm{A} \in \mathbb{R}^{n,n}$的特征值，且$\bm{u} \in \mathbb{C}^n$是对应的特征向量，则下式成立

$$ \bm{Au}  = \lambda \bm{u}, \bm{u} \neq \bm{0} $$

或者等价形式

$$ (\lambda \bm{I}_n - \bm{A}) \bm{u} = \bm{0}, \bm{u} \neq \bm{0} $$

方程表明为了使$(\lambda , \bm{u})$成为特征值/特征向量对，必须满足以下条件：
1. $\lambda$的取值要使矩阵$\lambda \bm{I}_n - \bm{A}$奇异
2. $\bm{u}$位于$\lambda \bm{I}_n - \bm{A}$的零空间中

由于$\lambda \bm{I}_n - \bm{A}$当且仅当其行列式为零时是奇异的，因此特征值可以很容易地被描述为满足下述方程的实数或复数

$$ \det (\lambda \bm{I}_n - \bm{A}) = 0 $$

$p(\lambda ) \coloneqq \det (\lambda \bm{I}_n - \bm{A})$是关于$\lambda$的$n$次多项式，被称为矩阵$\bm{A}$的特征多项式(characteristic polynomial)。因此，矩阵的特征值就是特征多项式的根。其中一些特征值确实可以是特征多项式的“重根”。此外，一些特征值可能是复数，具有非零的虚部，在这种情况下，它们成共轭复数对出现。下列定理成立

**代数学基本定理**：任意矩阵$\bm{A} \in \mathbb{R}^{n,n}$都有$n$个特征值，按重数计算。

我们称不考虑重数的特征值为互异特征值(distinct eigenvalues)，每个互异特征值都有一个对应的代数重数(algebraic multiplicity)$\mu_i \geq 1$，定义为该特征值作为特征多项式根出现的次数。因此$\sum_{i=1}^{k} \mu_i = n$

对于每个互异特征值都对应一个由与该特征值相关的特征向量组成的整个子空间$\mathcal{\phi}_i \coloneqq \mathcal{N}(\lambda_i \bm{I}_n − \bm{A})$，称为特征空间。属于不同特征空间的特征向量是线性无关的

**定理**：设 λi, i = 1, …, k ≤ n 是矩阵 A ∈ Rn,n 的不同特征值。设 φi = N (λi In − A)，并且令 u(i) 为任意非零向量，使得 u(i) ∈ φi, i = 1, …, k。则这些 u(i) 线性无关。

---
未完待续
