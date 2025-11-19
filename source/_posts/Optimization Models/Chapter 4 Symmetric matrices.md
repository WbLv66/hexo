---
title: Chapter 4 Symmetric matrices
date: 2025-10-12 21:27:29
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

## 1. 基础

### 1.1 定义和例子

当方阵$\bm{A} \in \mathbb{R} ^{n,n}$满足$\bm{A} = \bm{A}^\top$的时候则称这个矩阵为对称(symmetric)矩阵。$n \times n$的对称矩阵组成的集合是$\mathbb{R} ^{n,n}$的子空间，记作$\mathcal{S}^{n}$

样本协方差矩阵(sample covariance matrix)是对称矩阵，给定$m$个点$\bm{x}^{(1)},\cdots,\bm{x}^{(m)} \in \mathbb{R}^n$，则样本协方差矩阵写为

$$
\Sigma \coloneqq \frac{1}{m} \sum_{i=1}^{m}(\bm{x}^{(i)} -  \hat{\bm{x}})(\bm{x}^{(i)} -  \hat{\bm{x}})^\top
$$

其中$\hat{\bm{x}}$是点的样本均值

$$
\hat{\bm{x}} \coloneqq \frac{1}{m} \sum_{i=1}^{m}\bm{x}^{(i)}
$$

协方差矩阵$\Sigma$很明显是一个对称矩阵，当计算标量积(scalar product)的样本方差(sample variance)时会出现。比如定义$s_i \coloneqq w^\top \bm{x}^{(i)},i = 1,\cdots,m$，那么向量$\bm{x}$的样本均值为

$$
\hat{\bm{s}} = \frac{1}{m}(s_1+\cdots+s_m)= \bm{w}^\top \hat{\bm{x}}
$$

样本方差为

$$
\sigma ^2 = \sum_{i=1}^{m}(w^\top \bm{x}^{(i)} - \hat{\bm{s}})^2 = \sum_{i=1}^{m}\big(\bm{w}^\top (\bm{x}^{(i)} - \hat{\bm{x}})\big)^2 = \bm{w}^\top \Sigma \bm{w}
$$

一个二阶可微的函数$f:\mathbb{R} ^n \rightarrow \mathbb{R}$在点$x \in \operatorname{dom} f$处的海森(Hessian)矩阵是包含该点函数二阶导数的矩阵。海森矩阵的元素为

$$ \bm{H}_{ij} = \frac{\partial^2 f(x)}{\partial x_i \partial x_j},  1 \leq i,j \leq n $$

海森矩阵也经常写为$\nabla^2f(x)$。由于二阶导数与求导的顺序无关，因此对于每一对$(i, j)$，都有$\bm{H}_{ij} = \bm{H}_{ji}$，因此海森矩阵总是对称矩阵

考虑一个二次函数(quadratic function)（多项式函数的单项最高次数为二）

$$ q(x) = x_1^2 + 2x_1x_2+3x_2^2+4x_1+5x_2+6 $$

它的海森矩阵可以写为

$$ \bm{H} = \left[ \frac{\partial^2 q(x)}{\partial x_i \partial x_j} \right]_{1 \leq i,j \leq 2} =
\begin{bmatrix}
    \frac{\partial^2 q(x)}{\partial x_1^2 } & \frac{\partial^2 q(x)}{\partial x_1  \partial x_1} \\
    \frac{\partial^2 q(x)}{\partial x_2x_1 } & \frac{\partial^2 q(x)}{\partial x_2^2 }
\end{bmatrix} =
\begin{bmatrix}
    2 & 2 \\
    2 & 6
\end{bmatrix}$$

对于二次函数来说，海森矩阵是一个常数，与$x$点的取值无关。函数$q(x)$的二次项也可以写为

$$ x_1^2 + 2x_1x_2+3x_2^2 = \frac{1}{2} \bm{x}^\top \bm{H} \bm{x} $$

因此**二次函数可以写为包含海森矩阵的二次项和仿射项的和**

$$ q(x) = \frac{1}{2} \bm{x}^\top \bm{H} \bm{x} + \bm{c}^\top \bm{x} + d,\quad  \bm{c}^\top = [ 4 \quad 5 ],\quad d = 6 $$

考虑指数之和的对数函数(log-sum-exp) $\operatorname{lse}:\mathbb{R} ^n \rightarrow \mathbb{R}$

$$ \operatorname{lse}(\bm{x}) = \ln \sum_{i=1}^{n} \mathrm{e}^{x_i} $$

首先定义$\bm{z} = [\mathrm{e}^{x_1} \cdots \mathrm{e}^{x_n}],\quad Z = \sum_{i=1}^{n} z_i$，那么我们可以确定$x$点处的梯度$\nabla \operatorname{lse}(\bm{x}) = [\tfrac{\partial f(x)}{\partial x_1} \cdots \tfrac{\partial f(x)}{\partial x_n}]^\top$，定义$g_i(\bm{x})$为梯度的第$i$项

$$ \nabla \operatorname{lse}(\bm{x}) =  \frac{1}{Z} \bm{z} $$

$$ g_i(\bm{x}) = \frac{\partial f(x)}{\partial x_i} = \frac{\partial \ln Z}{\partial z_i} \frac{\partial  z_i}{\partial x_i} = \frac{z_i}{Z} $$

再次求梯度

$$ \frac{\partial g_i(\bm{x})}{\partial x_i} = \frac{z_i}{Z} - \frac{z_i^2}{Z^2}$$

对于$i \neq j$

$$ \frac{\partial g_i(\bm{x})}{\partial x_j} = - \frac{z_iz_j}{Z^2} $$

因此

$$ \nabla ^2 \operatorname{lse}(x) =
\begin{bmatrix}

\frac{Zz_1-z_1^2}{Z^2} & \frac{-z_1z_2}{Z^2} \\

\frac{-z_2z_1}{Z^2} & \frac{Zz_2-z_2^2}{Z^2}

\end{bmatrix}

= \frac{1}{Z^2}\big( Z\operatorname{diag}(z)-zz^\top \big) $$

假设给出$d$个线性无关的向量$\bm{x}^{(1)},\cdots,\bm{x}^{(d)} \in \mathbb{R}^n$和另一个向量$\bm{x} \in \mathbb{R}$，我们计算$\bm{x}$向$\bm{x}^{(1)},\cdots,\bm{x}^{(d)}$张成子空间的投影$\bm{x}^*$，根据Section 2.3.2.3，投影可以写为

$$
\bm{x}^* =\sum_{i=1}^{d} \alpha_i \bm{x}^{(i)}  = \bm{X} \bm{\alpha},\bm{X}=[\bm{x}^{(1)},\cdots,\bm{x}^{(d)}]
$$

其中$\bm{\alpha}$是一个系数向量，必须满足$\left\langle \bm{x},\bm{x}^{(k)} \right\rangle = \left\langle  \bm{x}^*,\bm{x}^{(k)}\right\rangle$,可以写为所谓的Gram线性方程

$$
\begin{bmatrix}
\bm{x}^{(1) \top} \bm{x}^{(1)} & \cdots &  \bm{x}^{(1) \top} \bm{x}^{(d)} \\
\vdots  & \ddots  &  \vdots  \\
\bm{x}^{(d) \top} \bm{x}^{(1)} & \cdots &  \bm{x}^{(d) \top} \bm{x}^{(d)}
\end{bmatrix}

\begin{bmatrix}
\alpha_1 \\
\vdots \\
\alpha_d
\end{bmatrix}

=

\begin{bmatrix}
\bm{x}^{(1) \top} \bm{x} \\
\vdots \\
\bm{x}^{(d) \top} \bm{x}
\end{bmatrix}

$$

左侧的系数矩阵是一个对称矩阵，被称为Gram矩阵，满足$\bm{G} = \bm{X}^\top \bm{X} \in \mathcal{S}^{n}$

### 1.2 二次函数

二次函数$q : \mathbb{R} \rightarrow \mathbb{R}$是关于$x$的二阶多元多项式，包含所有不超过二次的单项式的线性组合的函数。因此，这样的函数可以表示为

$$
q(x) = \sum_{i=1}^{n} \sum_{j=1}^{n} a_{ij}x_i x_j + \sum_{i=1}^{n}c_i x_i + d
$$

其中$a_{ij}$是二次单项式的系数，$c_i$是一次项单项式的系数，$d$是常数项，上面表达式在矩阵形式下有更加紧凑的表达

$$
q(x) = \bm{x}^\top \bm{A} \bm{x} + \bm{c}^\top \bm{x} + d
$$

注意，$\bm{x}^\top \bm{A} \bm{x}$是标量，所以它等于自身的转置，即$\bm{x}^\top \bm{A} \bm{x} = \bm{x}^\top \bm{A}^\top  \bm{x}$，因此

$$
\bm{x}^\top \bm{A} \bm{x} = \frac{1}{2}\bm{x}^\top (\bm{A} + \bm{A}^\top) \bm{x}
$$

其中$\bm{H} = \bm{A} + \bm{A}^\top$是一个对称矩阵，更一般的表达可以写为

$$
q(x) = \frac{1}{2}\bm{x}^\top \bm{H} \bm{x} + \bm{c}^\top \bm{x} + d = \frac{1}{2}
\begin{bmatrix}
  \bm{x} \\
  1
\end{bmatrix}^\top

\begin{bmatrix}
  \bm{H} & \bm{c}\\
  \bm{c}^\top & 2d
\end{bmatrix}

\begin{bmatrix}
  \bm{x} \\
  1
\end{bmatrix}
$$

二次型(quadratic form)是没有线性项和常数项的二次函数

$$
q(x) = \frac{1}{2}\bm{x}^\top \bm{H} \bm{x},\bm{H} \in \mathcal{S}^n
$$

注意二次函数的海森矩阵是常数，$\nabla^2q(\bm{x}) = \bm{H}$

一个一般的、二阶可微的函数可以通过泰勒级数展开，在点$\bm{x}_0$的邻域内用一个二次函数进行局部近似，详见第Section3.2.2

$$
f(\bm{x}) \approx q(\bm{x}) = f(\bm{x}_0) + \nabla f(\bm{x}_0)^\top (\bm{x}-\bm{x}_0) + \frac{1}{2}(\bm{x}-\bm{x}_0)^\top \nabla^2 f(\bm{x}_0)(\bm{x}-\bm{x}_0)
$$

通过参数替换可以将上式写成标准的二次函数形式

有两种特殊情况：对角矩阵和并矢矩阵。对角矩阵是对称矩阵的一种特殊形式，与对角矩阵$\operatorname{diag}(\bm{a})$相关的二次项为

$$
q(\bm{x}) = \bm{x}^\top  \operatorname{diag}(\bm{a}) \bm{x} = \sum_{i=1}^{n}a_i x_i^2
$$

也就是说，$q(\bm{x})$是纯平方的线性组合。即在和中不出现 $x_i x_j$类型的交叉乘积项

另一类重要的对称矩阵是对称并矢矩阵，即由以下形式的向量积形成的矩阵

$$
\bm{A} = \bm{a}\bm{a}^\top =
\begin{bmatrix}
    a_1^2 & a_1a_2 &  \cdots & a_1a_n \\
    a_2a_1 & a_2^2 &  \cdots & a_2a_n \\
    \vdots  & \vdots &  \ddots & \vdots \\
    a_na_1 & \cdots &  \cdots & a_n^2
\end{bmatrix}
$$

并矢是秩为一的矩阵，与并矢相关的二次型具有如下形式

$$
q(\bm{x}) = \bm{x}^\top  \bm{a} \bm{a}^\top  \bm{x} = (\bm{x}^\top  \bm{a}) (\bm{a}^\top  \bm{x}) = (\bm{a}^\top  \bm{x}) (\bm{a}^\top  \bm{x}) = (\bm{a}^\top  \bm{x})^2
$$

也就是说，它是关于$\bm{x}$的线性形式的平方。因此，与一个并矢相关的二次型总是非负的

## 2. 谱定理(The spectral theorem)

### 2.1 对称矩阵的特征值分解

我们回顾一下Section3.3中方阵特征值和特征向量的定义。设$\bm{A}$为一个$n \times n$矩阵。如果存在一个非零向量$\bm{u}$使得

$$
\bm{Au} = \lambda \bm{u}
$$

向量$\bm{u}$被称为与特征值$λ$相关的特征向量。如果$\lVert \bm{u} \rVert_2=1$，那么特征向量被称为已归一化。在这种情况下，我们可以得到$\bm{u}^\dagger\bm{Au} = \lambda \bm{u}^\dagger\bm{u}=\lambda$。这里的$^\dagger$代表厄米共轭(Hermitian conjugate)，即先转置再取共轭

$\bm{u}$的解释是它定义了一个方向，在此方向上，由$\bm{A}$定义的线性映射表现得就像标量乘法一样。缩放的量由$\lambda$给出

$\bm{A}$的特征值是特征多项式的根

$$
p_{\bm{A}}(\lambda) = \det (\lambda \bm{I}-\bm{A})
$$

因此对于一般的矩阵，特征值可以是实数或复数（以复共轭对出现），同样，相应的特征向量可以是实数或复数。然而，对于对称矩阵来说，特征值和特征向量均为实数。而且对于每个不同的特征值$\lambda _i$，特征空间$\mathcal{\phi}_i = \mathcal{N}(\lambda_i \bm{I}_n − \bm{A})$的维数与该特征值的代数重数相同

> **定理4.1（对称矩阵的特征值分解）**：设$\bm{A} \in \mathbb{R}^{n,n}$是对称矩阵。令$\lambda _i,i=1, \cdots,k \leq n$是$\bm{A}$的互异特征值。进一步记$\mu _i,i = 1, \cdots, k$，表示$\lambda _i$的代数重数，并记$\mathcal{\phi}_i = \mathcal{N}(\lambda_i \bm{I}_n − \bm{A})$。那么，对所有$i = 1, \cdots , k$ 有
> 1. $\lambda _i \in \mathbb{R}$
> 2. $\mathcal{\phi}_i \perp \mathcal{\phi}_j,i \neq j$
> 3.  $\dim \mathcal{\phi}_i = \mu _i$

> **证明**
>
> **第一部分**
> 让$\lambda ,\bm{u}$为$\bm{A}$的任意特征值和特征向量对。则
> $$ \bm{Au} = \lambda \bm{u} $$
> 对两边取厄米共轭
> $$ \bm{u}^\dagger \bm{A}^\dagger = \lambda ^\dagger \bm{u}^\dagger $$
> 对第一个方程左乘$\bm{u}^\dagger$，对第二个方程右乘$\bm{u}$可以得到
> $$\begin{gather*}
> \bm{u}^\dagger\bm{Au} = \lambda \bm{u}^\dagger\bm{u} \\
> \bm{u}^\dagger \bm{A}^\dagger \bm{u} = \lambda ^\dagger \bm{u}^\dagger \bm{u}
> \end{gather*}
> $$
> 已知$\bm{u}^\dagger\bm{u} = \lVert \bm{u} \rVert^2_2 \neq 0$，因为$\bm{A}$为实数那么$\bm{A}^\dagger = \bm{A}^\top$，将上式相减可以得到
> $$ \bm{u}^\dagger (\bm{A}-\bm{A}^\top ) \bm{u} = (\lambda - \lambda ^\dagger )\lVert \bm{u} \rVert^2_2 $$
> 因为$\bm{A}$是对称矩阵，所以$\bm{A} - \bm{A}^\top = \bm{0}$，可以得到
> $$ \lambda - \lambda ^\dagger =0 $$
> 这意味着$\lambda$一定为实数。注意，相关的特征向量$\bm{u}$也总可以选择为实向量。如果一个复向量$\bm{u}$满足\bm{Au} = \lambda \bm{u}，且$\bm{A},\lambda$为实数，那么我们也有$\operatorname{Re}(\bm{Au}) = \bm{A}\operatorname{Re}(\bm{u}) = \lambda \operatorname{Re}(\bm{u})$，这意味着$\operatorname{Re}(\bm{u})$是与$\lambda$相关联的特征向量
>
> **第二部分**
> 令$\bm{v}_i \in \mathcal{\phi}_i,\bm{v}_j \in \mathcal{\phi}_j,i \neq j$因为$\bm{Av}_i = \lambda _i \bm{v}_i,\bm{Av}_j = \lambda _j \bm{v}_j$，我们可以得到
