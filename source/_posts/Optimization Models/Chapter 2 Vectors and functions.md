---
title: Chapter 2 Vectors and functions
date: 2025-09-13 10:39:05
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

## 1. 向量基础

### 1.1 向量

**向量可以被视为数字的集合(collection)**，通常写为列排列

$x_i$被称为向量$\mathbf{x}$的第$i$个元素(element)/条目(entry)/分量(component)，元素的数量为$\mathbf{x}$的维度

向量中的元素为实数(real)时，i.e. $x_i \in \mathbb{R}$，向量为实数向量，i.e. $\mathbf{x} \in \mathbb{R}^n$；若向量中的元素为复数(complex)时，i.e. $x_i \in \mathbb{C}$，向量为复数向量，i.e. $\mathbf{x} \in \mathbb{C}^n$

当我们不在乎向量是行向量(row)还是列向量(column)时，可以直接使用$\mathbf{x}=(x_1,x_2 \cdots, x_n)$来表示向量

### 1.2 向量空间

**向量可以被视为空间中的点**  

向量空间(vector space),$\mathcal{X}$是通过为向量配备加法和标量乘法的操作而获得的，最常见的向量空间为$\mathcal{X} = \mathbb{R}^n$

如果$\mathcal{V}$是向量空间$\mathcal{X}$的一个非空子集，并且$\mathcal{V}$在加法和标量乘法下是**封闭的**(closed)，那么$\mathcal{V}$是$\mathcal{X}$的子空间(subspace)。

加法和标量乘法下的**封闭性**可以表述为：对于任意标量$\alpha,\beta$有

$$x,y \in \mathcal{V} \Rightarrow \alpha x + \beta y \in \mathcal{V}$$

> 子空间必须包含原点

线性组合(linear combination)的形式如下

$$\alpha_1 \mathbf{x}^{(1)} + \alpha_2 \mathbf{x}^{(2)} + \alpha_3 \mathbf{x}^{(3)} + \cdots \alpha_m \mathbf{x}^{(m)}, \alpha_i \in \mathbb{R}$$

向量集合$\mathcal{S}$中的向量组成的所有线性组合会形成一个子空间，称为由$\mathcal{S}$生成的子空间，或者称为$\mathcal{S}$的张成，记为$span(\mathcal{S})$

1. 由单向量$\mathcal{S}=\{ x^{(1)} \}$生成的子空间是一条过原点的直线
2. 由不共线的两个向量$\mathcal{S}=\{ x^{(1)},x^{(2)} \}$生成的子空间是一个过原点的平面

当两个子空间的交集只有零向量的时候，i.e. $\mathcal{X}\cap \mathcal{Y} = \mathbf{0}$，那么这两个子空间的和称为直和(direct sum)，定义为$\mathcal{X} \oplus \mathcal{Y}$

如果向量集合中的任何一个向量都无法表示为其他向量的线性组合，那么这个集合是线性无关的(linearly independent)，充要条件为

$$\sum_{i=1}^m\alpha_i \mathbf{x}_i = \mathbf{0}\Longrightarrow \alpha = 0$$

包含$m$个元素的向量集合$\mathcal{S} = \{\mathbf{x}_1, \cdots, \mathbf{x}_m \}$，它可以生成一个子空间$span(\mathcal{S})$。假设最后一个元素可以写成集合中其他元素的线性组合，那么去掉这个元素后生成的子空间是一样的，i.e. $span(\mathcal{S}) = span(\mathcal{S} \setminus \mathbf{x}_m)$。重复这个步骤直到集合中的向量都是线性无关的，得到的集合为$\mathcal{B} = \{\mathbf{x}_1, \cdots , \mathbf{x}_d \}, d \le m$，这样的**集合**称为$span(\mathcal{S})$的基(basis)，元素数量$d$称为$span(\mathcal{S})$的维数(dimension)

一个子空间可以有无限个不同的基，但任何基中的元素数量是固定的，并且等于子空间的维度。如果我们得到了子空间的基，那么我们可以用基中元素的线性组合表达子空间中的所有向量。$\mathbb{R}^n$中的标准基写为$\{ \mathbf{e}_1, \cdots, \mathbf{e}_n  \}$，$\mathbf{e}_i$中第$i$个元素为$1$其余全为$0$

仿射集(affine sets)被定义为子空间的平移

$$ \mathcal{A} = \{ \mathbf{x} | \mathbf{x} = \mathbf{v} + \mathbf{y}, \mathbf{v} \in \mathcal{V} \} $$

其中$\mathbf{y}$是给定的点，$\mathcal{V}$是给定的子空间.仿射集必定经过$\mathbf{y}$

一个直线可以由两个元素描述：一个是直线上的点和一个表示直线方向的向量

$$ \mathcal{L} = \{ \mathbf{x} | \mathbf{x} = \mathbf{x}_0 + \mathbf{v} \} $$

## 2. 范数和内积
