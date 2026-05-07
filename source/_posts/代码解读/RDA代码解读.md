---
title: RDA代码解读
date: 2026-04-29 20:47:01
# updated:
# tags:
#     - 
categories: 代码解读
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

## 1. 重要函数

`def is_convex_and_ordered(self, points: NDArray[np.float64]) -> tuple[bool, str]`

这个函数的作用是判断按照顺序排列的顶点是否构成凸集，并判断点是顺时针排列"CW"还是逆时针排列"CCW"

实现思路：按顺序取三个点$o,a,b$，判断$\overrightarrow{oa}$和$\overrightarrow{ob}$的叉积，如果所有叉积同号则说明$a$点在$\overrightarrow{ob}$的外侧，那么所有内角都为凸角，同时根据叉积的正负还可以判断点是顺时针排列（负）还是逆时针排列（正）

`def gen_inequal_global(self, vertex: NDArray[np.float64])-> tuple[NDArray[np.float64], NDArray[np.float64]]`

此函数的作用是根据顶点生成半空间不等式$\bm{Ax} \leq \bm{b}$

实现思路：先判断顶点的凸性和排列顺序，统一为逆时针排列。随后按顺序遍历相邻的两个顶点，计算半空间表达式

每个超平面可以表达为$\bm{a}^\top (\bm{x} - \bm{x}_0) = 0$，即与$\bm{x}_0$的连线与$\bm{a}$垂直的点，因此$\bm{a}$为法向量；$\bm{x}_0$为超平面上的点。变换后为$\bm{a}^\top \bm{x}  =\bm{b}$，变为不等号即得到半空间

计算半空间的过程

1. 求直线向量$\bm{v} = \bm{p}_2 - \bm{p}_1 = (x_2 - x_1 , y_2 - y_1)$，向量指向顺时针方向
2. 令$\bm{v}$与$\bm{a}$的叉乘结果为负，可以求连线左侧的法向向量。注意，对于凸多边形，由于表达式是$\leq$，因此法向量必须指向多边形外侧
3. 用$\bm{a}^\top \bm{p}_1$求解$\bm{b}$


差速运动学模型

$$
\bm{s}_{t+1} = \bm{A}_t \bm{s}_{t} + \bm{B}_t \bm{u}_{t} + \bm{c}_t \\

\bm{s}_t = ( x_t , y_t , \theta_t ) \\

\bm{u}_t = (v_t , \psi_t)

$$

$$
x_{t+1} = x_t - v \sin (\theta) dt \theta _t + \cos (\theta) dt v_t + \theta_t v \sin (\theta) dt \\


[1, 0, -v * dt * sin(phi)], [0, 1, v * dt * cos(phi)], [0, 0, 1]
$$