---
title: Optimization-Based Collision Avoidance
date: 2026-04-20 16:20:30
# updated:
# tags:
#     - 
categories: 论文解读
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

障碍物用如下公式表示

$$
\mathbb{O}^{(m)} = \left\{ \bm{y} \in \mathbb{R} ^n \mid \bm{A}^{(m)} \bm{y} \preceq _{\mathcal{K}} \bm{b}^{(m)}\right\}  
$$

上标$^{(m)}$表示在时间步为$m$时的障碍物位置；$\bm{A}^{(m)} \bm{y} \preceq _{\mathcal{K}} \bm{b}^{(m)}$可以展开为$\bm{b}^{(m)} - \bm{A}^{(m)} \bm{y} \in \mathcal{K}$。若令$\mathcal{K} = \mathbb{R} ^l_+$即非负象限锥，则此广义不等式变为逐元素不等式$\leq$，此时障碍物由不同的半空间组成，是一个多面体；若令锥为二阶锥，此时障碍物是一个椭球

被控物体用符号$\mathbb{E}(\bm{x}_k)$表示，其中$\bm{x}_k$表示被控物体在时间步为$k$时的状态

当被控物体被视为质点时，它可以表示为

$$
\mathbb{E}(\bm{x}_k) = \bm{p}_k
$$

$\bm{p}_k$表示质点在时间步为$k$时的位置

当被控物体被视为全维度物体时，即形状不可忽略，它可以表示为

$$
\mathbb{E}(\bm{x}_k) = \bm{R}(\bm{x}_k) \mathbb{B} + \bm{t}(\bm{x}_k) , \mathbb{B} \coloneqq \left\{ \bm{y} \mid \bm{Gy} \preceq _{\bar{\mathcal{K}}} \bm{g}\right\}
$$

可以视为对一个已知的锥$\mathbb{B}$进行旋转$\bm{G}$和平移$\bm{g}$

可以构造一个具备避障功能的模型预测控制

$$
\begin{align*}
  \min_{\bm{x},\bm{u},\bm{\lambda}} \ &\sum_{k=0}^{N}  \ell(\bm{x}_k, \bm{u}_k)  \\
  \text{s.t. } \ & \bm{x}_0 = \bm{x}_S, \quad \bm{x}_{N+1} = \bm{x}_F \\
  & \bm{x}_{k+1} = f(\bm{x}_k,\bm{u}_k), \quad h(\bm{x}_k, \bm{u}_k) \leq 0 \\
  & \mathbb{E}(\bm{x}_k) \cap \mathbb{O}^{(m)} = \emptyset
 \end{align*}
$$

其中$\ell(\cdot)$为代价函数