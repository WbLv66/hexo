---
title: terrain_analysis参数解读
date: 2025-06-29 21:51:54
# updated:
# tags:
#     - 
categories: 自主探索代码
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

## 1. terrain_analysis

scanVoxelSize 点云下采样的分辨率

decayTime 点云衰减时间阈值，即点云数据在多长时间后会被视为过时并从地图中移除

noDecayDis  ​豁免点云衰减的距离阈值参数，保护车辆周围一定范围内的点云数据，即使这些数据的时间戳超过了 阈值也不会被移除

clearingDis 手动触发点云清除的距离阈值，​允许用户或外部指令清除车辆周围指定范围内的所有点云数据

useSorting 决定是否使用​排序分位数法来估计每个平面体素的地面高度。启用后抗噪声能力强，但计算量略大

quantileZ 地形高程估计的保守程度，决定了从排序后的点云高度数据中选择哪个分位数的值作为地面高度的估计。需要`useSorting`设为`True`才会发挥作用

considerDrop 它决定是否将点云相对于地面的高度差的 ​绝对值用于障碍物判断，设为`True`同时考虑​高于地面​ 的障碍物和​低于地面的凹陷，否则只关注​高于地面的障碍物

limitGroundLift 是否限制地面高程估计的最大抬升幅度（相对于该体素内的最低点），避免因噪声或离群点导致的地面高估

maxGroundLift 体素内最大抬升幅度。需要`limitGroundLift`设为`True`才会发挥作用

clearDyObs
