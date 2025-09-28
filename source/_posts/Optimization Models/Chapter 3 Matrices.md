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

$$\left[\bm{A}^\top\right]_{ij} = \left[\bm{A}\right]_{ji}$$

符号$\left[\bm{A}\right]_{ij}$(有时候简写为$\bm{A}_{ij}$)代表矩阵第$i$行$j$列的元素。$\mathbb{R}^{m,n}$中的零矩阵表示为$\bm{0}_{m,n}$，或者直接简写为$\bm{0}$

矩阵的数乘(multiplication by a scalar)被定义为矩阵中的每个元素都与标量相乘；矩阵的加法(两个矩阵大小相同)被定义为矩阵对应位置的元素相加。定义了这些运算后，我们可以将$\mathbb{R}^{m,n}$看作一个向量空间

### 1.2 矩阵乘积

如果两个矩阵的尺寸符合，他们才可以相乘。i.e. $\bm{A} \in \mathbb{R}^{m,n}, \bm{B} \in \mathbb{R}^{n,p}$，矩阵的乘法$\bm{AB} \in \mathbb{R}^{m,p}$被定义为

$$ \left[\bm{AB}\right]_{ij} = \sum^n_{k=1}\bm{A}_{ik}\bm{B}_{kj} $$

矩阵乘法是非交换的(non-commutative)，这意味着一般情况下$\bm{AB} \neq \bm{BA}$

$n \times n$单位矩阵(identity matrix)表达为$\bm{I}_n$(也可以简写为$\bm{I}$)，它是对角(diagonal)元素为$1$其他元素为$0$的矩阵。单位矩阵满足$\bm{AI}_n=\bm{A}$，$\bm{A}$有$n$行；$\bm{I}_n\bm{B}=\bm{B}$，$\bm{B}$有$n$列

矩阵可以看成一组行向量的组合，也可以看成一组列向量的组合

$$
\bm{A} = \left[ \bm{a}_1 \quad \bm{a}_2 \quad \cdots \quad \bm{a}_n \right],\text{or }
\bm{A} = \left[
\begin{array}{c}
\bm{\alpha}_{1}^\top \\
\bm{\alpha}_{2}^\top  \\
\vdots  \\
\bm{\alpha}_{m}^\top
\end{array}
\right]
$$

其中$\bm{a}_1,\cdots,\bm{a}_n \in \mathbb{R}^m$表示$\bm{A}$的列；$\bm{\alpha}_1^\top,\cdots,\bm{\alpha}_n^\top \in \mathbb{R}^n$表示$\bm{A}$的行

因此矩阵乘积可以写为

$$ \bm{AB} = \bm{A}
\left[ \bm{b}_1 \quad \cdots \quad \bm{b}_p \right]  = \left[ \bm{Ab}_1 \quad \cdots \quad \bm{Ab}_p \right]$$

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

矩阵乘积的转置满足

$$ \left( \bm{A}_1 \bm{A}_2 \cdots \bm{A}_p \right)^\top = \bm{A}_p^\top \cdots \bm{A}_2^\top  \bm{A}_1^\top $$

### 1.3 块矩阵乘积

只要保证块(block)大小一致，矩阵代数可以推广到块。首先考虑矩阵$\bm{A}$与向量$\bm{x}$的乘积，其中矩阵和向量都是分块的

$$
\begin{gather*}

\bm{A} = \left[ \bm{A}_1 \quad \bm{A}_2 \right],\bm{x} = \left[
\begin{array}{c}
\bm{x}_1 \\
\bm{x}_2
\end{array}
\right] \\

\bm{Ax}= \bm{A}_1\bm{x}_1 + \bm{A}_2\bm{x}_2

\end{gather*}
$$

从符号上看这就像是行向量与列向量的内积。矩阵与矩阵相乘也可以进行类似展开

$$ \bm{AB} = \left[ \bm{A}_1 \quad \bm{A}_2 \right] \left[
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

$$ \langle \bm{A},\bm{B} \rangle = \text{trace} \left( \bm{A}^\top \bm{B} \right) $$

其中$\text{trace}\left(\bm{X} \right)$是方阵的迹(trace)，定义为方阵主对角线上元素的和。这个内积引出了所谓的Frobenius范数

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

我们可以将矩阵解释为从输入空间到输出空间的作用的线性映射（向量值函数，即输出为向量）或者操作。我们回顾一下线性映射：当任意点$\bm{x},\bm{z} \in \mathcal{X}$和任意标量$\lambda,\mu \in \mathcal{Y}$满足$f\left( \lambda \bm{x} + \mu \bm{z} \right) = \lambda f\left(\bm{x}\right) + \mu f\left(\bm{z}\right)$那么映射$f:\mathcal{X}\rightarrow \mathcal{Y}$为线性。任意线性映射$f:\mathbb{R}^n\rightarrow \mathbb{R}^m$都可以用一个矩阵$\bm{A}\in\mathbb{R}^{m,n}$表示

![3.3](https://minio.wblv66.top/optimization-models/3.3 "3.3")

放射映射就是简单地在线性方程上加一个常数项，因此任意放射映射$f:\mathbb{R}^n\rightarrow \mathbb{R}^m$都可以表示为

$$ f\left( \bm{x} \right) = \bm{A}\bm{x} + \bm{b}  $$

其中$\bm{A} \in \mathbb{R}^{m,n},\bm{b} \in \mathbb{R}^{m}$

将向量的每个元素按某个标量因子 进行缩放的线性映射，可以用对角矩阵来描述

### 2.2 非线性方程的近似

一个非线性映射（在该点可微）在给定点$\bm{x}_0$的邻域内(neighborhood)可以被近似为一个仿射映射

$$ f \left( \bm{x} \right) = f \left( \bm{x}_0 \right) + J_f \left( \bm{x}_0 \right) \left( \bm{x}-\bm{x}_0 + o \left( \lVert \bm{x} - \bm{x}_0 \rVert \right)  \right) $$

当$\bm{x} \rightarrow \bm{x}_0$时$o \left( \lVert \bm{x} - \bm{x}_0 \rVert \right)$比一阶(first order)收敛更快，$J_f$是雅可比矩阵，定义为

$$ J_f \left( \bm{x}_0 \right) \coloneqq \left[
\begin{array}{ccc}
    \frac{\partial f_1 }{\partial x_1} & \cdots & \frac{\partial f_1}{\partial x_n} \\
    \vdots & \ddots & \vdots \\
    \frac{\partial f_m }{\partial x_1} & \cdots & \frac{\partial f_m}{\partial x_n}
\end{array} \right]_{ \bm{x} = \bm{x}_0 } $$

因此对于接近$\bm{x}_0$的$\bm{x}$，变分$\delta_f \left( \bm{x} \right) \coloneqq f \left( \bm{x} \right) - f \left( \bm{x}_0 \right)$可以用雅可比矩阵定义的线性映射来一阶近似

一个在$\bm{x}_0$处二阶可微的标量值函数（即输出为标量）可以使用梯度和二阶导数矩阵（海森矩阵）进行二阶局部近似

$$ f \approx f \left( \bm{x}_0 \right) + \Delta f \left( \bm{x}_0 \right)^\top \left( \bm{x} -\bm{x}_0 \right) + \frac{1}{2}  \left( \bm{x} -\bm{x}_0 \right)^\top \Delta^2 f \left( \bm{x}_0 \right) \left( \bm{x} -\bm{x}_0 \right) $$

其中$\Delta^2 f \left( \bm{x}_0 \right)$是海森矩阵(Hessian)定义为

$$ \Delta^2 f \left( \bm{x}_0 \right) \coloneqq \left[
\begin{array}{ccc}
    \frac{\partial ^2 f}{\partial  x_1^2} & \cdots & \frac{\partial ^2 f}{\partial  x_1 \partial x_n} \\
    \vdots & \ddots & \vdots \\
    \frac{\partial ^2 f}{\partial  x_n \partial x_1} & \cdots & \frac{\partial ^2 f}{\partial x_n^2}
\end{array}
 \right]_{\bm{x}= \bm{x}_0} $$

在这种情况下，$f$在局部通过由Hessian矩阵定义的二次函数进行近似

### 2.3 值域，秩和零空间

考虑一个矩阵$\bm{A}$对它的列向量进行线性组合，得到的集合称为$\bm{A}$的值域(range)或者为列空间，被写为$\mathcal{R} \left( \bm{A} \right)$

$$ \mathcal{R} \left( \bm{A} \right) = \left\{ \bm{A} \bm{x} \vert  \bm{x} \in \mathbb{R}^n \right\} $$

列空间是一个子空间。$\mathcal{R} \left( \bm{A} \right)$的维数称为$\bm{A}$的秩(rank)，记作$\text{rank}\left( \bm{A} \right)$根据定义，秩表示$\bm{A}$线性无关的列数量，根据前人证明秩也等于线性无关的行数量。因此矩阵的秩等于它转置的秩

$$ \text{rank} \left( \bm{A} \right)  = \text{rank} \left( \bm{A}^\top \right)  $$

因此我们可以提出一个约束

$$ 0 \leq \text{rank} \left( \bm{A} \right)   \leq \min \left( m,n \right)  $$

矩阵$\bm{A}$的零空间(nullspace)是输入空间中被映射到$\bm{0}$的向量组成的集合，记作$\mathcal{N} \left( \bm{A} \right)$

$$ \mathcal{N} \left( \bm{A} \right) = \left\{ \bm{x} \vert \bm{A} \bm{x} = \bm{0} \right\} $$

零空间也是一个子空间

---
未完待续