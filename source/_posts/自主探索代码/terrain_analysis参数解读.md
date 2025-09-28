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

`scanVoxelSize` 点云下采样的分辨率。**室外环境可以调大一些，如`0.1`**

`decayTime` 点云衰减时间阈值，即点云数据在多长时间后会被视为过时并从地图中移除。**动态场景减少数值**

`noDecayDis`  ​豁免点云衰减的距离阈值参数，保护车辆周围一定范围内的点云数据，即使这些数据的时间戳超过了阈值也不会被移除。**动态场景减少数值**

`clearingDis` 手动触发点云清除的距离阈值，​允许用户或外部指令清除车辆周围指定范围内的所有点云数据

`useSorting` 决定是否使用​排序分位数法来估计每个平面体素的地面高度。启用后抗噪声能力强，但计算量略大。**结构化平坦道路可开启**

`quantileZ` 地形高程估计的保守程度，决定了从排序后的点云高度数据中选择哪个分位数的值作为地面高度的估计。需要`useSorting`设为`True`才会发挥作用。

`considerDrop` 它决定是否将点云相对于地面的高度差的 ​绝对值用于障碍物判断，设为`True`同时考虑​高于地面​的障碍物和​低于地面的凹陷，否则只关注​高于地面的障碍物。**地面存在凹陷时开启**

`limitGroundLift` 是否限制地面高程估计的最大抬升幅度（相对于该体素内的最低点），避免因噪声或离群点导致的地面高估。

`maxGroundLift` 体素内最大抬升幅度。需要`limitGroundLift`设为`True`才会发挥作用

`clearDyObs`是否启用动态障碍物点云清除。**存在动态障碍物时开启**

`minDyObsDis`这个距离内的点云会被直接标记为动态障碍物。**根据车辆自身部件遮挡设置**

`minDyObsRelZ`用于计算斜率角的基线高度`angle = atan2(pointZ - minDyObsRelZ, dis)`

`minDyObsAngle`动态障碍物的最小仰角阈值（度），高于该角度的点才可能被视为动态障碍物

`absDyObsRelZThre`把点旋转到传感器坐标后的垂直高度小于该阈值才可能被认为是动态

`minDyObsVFOV / maxDyObsVFOV`：在旋转到传感器坐标后计算垂直视场角（angle4），在这个范围内才可能被认为是动态

`minDyObsPointNum`标记为动态障碍物的最小点数

`noDataObstacle`将无数据区域标记为障碍物。**建议关闭**

`vehicleHeight`用于最终判定 disZ（点相对估计地面高度）是否属于“对车辆有影响”的高度区间：条件是 `disZ >= 0 && disZ < vehicleHeight`

`voxelPointUpdateThre / voxelTimeUpdateThre`体素更新的点数阈值和时间阈值。**小的阈值 → 更频繁更新，响应快但计算多；大的阈值 → 更稳定但滞后。**

`minRelZ / maxRelZ / disRatioZ`筛选点云时的竖直方向坐标范围，接收点云的范围为`point.z - vehicleZ > minRelZ - disRatioZ * dis` 以及 `point.z - vehicleZ < maxRelZ + disRatioZ * dis`   **`vehicleZ`为激光雷达的位置**
