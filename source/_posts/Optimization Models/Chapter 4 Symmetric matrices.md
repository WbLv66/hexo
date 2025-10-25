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

一个二阶可微的函数$f:\mathbb{R} ^n \rightarrow \mathbb{R}$在点$x \in \operatorname{dom} f$处的海森(Hessian)矩阵是包含该点函数二阶导数的矩阵。海森矩阵的元素为

$$ \bm{H}_{ij} = \frac{\partial^2 f(x)}{\partial x_i \partial x_j} 1 \leq i,j \leq n $$

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

因此**二次函数可以写为包含海森矩阵的二次项和放射项的和**

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

\frac{z_i^2(Z-z_i)^2}{Z^4} & \frac{z_i^2z_j(Z-z_i)}{Z^4} \\

\end{bmatrix}

\frac{1}{Z^2}() $$
