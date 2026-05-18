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

1. 求直线向量$\bm{v} = \bm{p}_2 - \bm{p}_1 = (x_2 - x_1 , y_2 - y_1)$，向量指向逆时针方向
2. 令$\bm{v}$与$\bm{a}$的叉乘结果为负，可以求连线右侧的法向向量。注意，对于凸多边形，由于表达式是$\leq$，因此法向量必须指向多边形外侧
3. 用$\bm{a}^\top \bm{p}_1$求解$\bm{b}$

`def cone_para_array(
        self, array: cp.Variable, cone_flag: cp.Variable)-> cp.constraints.nonpos.Inequality`

此函数的作用是构建约束$\bm{\lambda}_{t,m} \succeq _{\mathcal{O}^*_m} 0$

此函数将非负象限锥和二阶锥的情况都放进一个约束中，因为优化问题是提前建立好的，在运动前无法确定第$n$个障碍物到底是哪种类型，因此都需要考虑进去

## 2. 优化问题构建

**加粗代表变量**

### 2.1 `construct_su_prob`

将$s,u,d$视为变量

优化问题为
$$
\begin{align*}
\min \ & (\operatorname{cost}_{nav} + \operatorname{cost}_{su}) \\
\text{s.t.} \ & \operatorname{constraints}_{nav} \cap \operatorname{constraints}_{su}
\end{align*}
$$

1. `nav_cost_cons`

约束包含
$$
\begin{align*}
& \operatorname{constraints}_{nav}：\\
& \bm{s}_{t+1} = A_t \bm{s}_t + B_t \bm{u}_t + C_t, \forall t \\
& \lVert \bm{v}_{t+1} - \bm{v}_t \rVert \leq  a_{\max}, \forall t \\
& \lVert \bm{u}_t \rVert \leq  u_{\max},\forall t \\
& \bm{s}_0 = s_0
\end{align*}
$$

代价函数为

$$
\begin{align*}
& \operatorname{cost}_{nav} : \\
& Q_t \sum_{t=0}^{N} \lVert \bm{s}_t - s'_t \rVert_2^2 + P_t \sum_{t=0}^{N} (\bm{v}_t - v'_t) ^2
\end{align*}
$$

1. `update_su_cost_cons`

约束包含
$$
\begin{align*}
& \operatorname{constraints}_{su}：\\
& R'_t - (J' \phi')_t + J'_t \bm{\theta}_t  - \bm{R}_t =0, \forall t \\
& \bm{d}_t \in [d_{\min }, d_{\max}], \forall t \\
& \bm{I}_{t,m} = (\lambda D)_{t,m}\bm{s}_t-(\lambda b)_{t,m} - \mu _{t,m}^\top h - \bm{d}_t - z_{t,m} + \zeta _{t,m},\forall t,m \\
& \bm{H}_{t,m} = \mu _{t,m}^\top G +(\lambda D)\bm{R}_t+ \xi _{t,m} ,\forall t,m
\end{align*}
$$

代价函数为

$$
\begin{align*}
& \operatorname{cost}_{su} : \\
&-\eta \sum_{t=0}^{N}\bm{d}_t +\frac{\rho_1}{2} \sum_{t=0}^{N} \sum_{m=0}^{M} \lVert \min(\bm{I}_{t,m} , 0)  \rVert_2^2 + \frac{\rho_2}{2} \sum_{t=0}^{N} \sum_{m=0}^{M} \lVert \bm{H}_{t,m}  \rVert_2^2
\end{align*}
$$
这里在计算$I_{t,m}$的代价函数时使用了$\min(\bm{I}_{t,m} , 0)$，这是为了惩罚大于零的情况，本质上是将约束从$=0$放松为$\geq 0$，因为安全距离适当变大是可接受的，这样可以加快计算速度

### 2.2 `construct_LamMuZ_prob`

将$\lambda ,\mu ,z$视为变量

优化问题为
$$
\begin{align*}
\forall m \\
\min \ & \operatorname{cost}_m \\
\text{s.t.} \ & \operatorname{constraints}_m
\end{align*}
$$

`LamMuZ_cost_cons`

约束包含
$$
\begin{align*}
& \operatorname{constraints}_{m}：\\
& \lVert D_{t,m}^\top \bm{\lambda}_{t,m}  \rVert_* \leq 1, \forall t,m \\
& \bm{\lambda}_{t,m} \succeq _{\mathcal{O}^*_m} 0, \forall t \\
& \bm{\mu}_{t,m} \succeq _{\mathcal{K}^*_r} 0, \forall t \\
& \bm{I}_{t,m} = \bm{\lambda}_{t,m}^\top  (D p)_{t,m}-\bm{\lambda}_{t,m}^\top b_{t,m} - \bm{\mu} _{t,m}^\top h - d_t - \bm{z}_{t,m} + \zeta _{t,m},\forall t,m \\
& \bm{H}_{t,m} = \bm{\mu} _{t,m}^\top G +\bm{\lambda}_{t,m}^\top(D R)_t+ \xi _{t,m} ,\forall t,m
\end{align*}
$$

代价函数为

$$
\begin{align*}
& \operatorname{cost}_m : \\
& \frac{\rho_1}{2} \sum_{t=0}^{N} \lVert \min(\bm{I}_{t,m} , 0)  \rVert_2^2 + \frac{\rho_2}{2} \sum_{t=0}^{N} \lVert \bm{H}_{t,m}  \rVert_2^2
\end{align*}
$$
