---
title: local_planner参数解读
date: 2025-06-29 19:38:07
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

## 1. localPlanner中的参数

`pathFolder` 存放离线待选路径的位置

`vehicleLength` 机器人车体长度

`vehicleWidth` 机器人车体宽度

`sensorOffsetX` 传感器相对于车辆中心的 X 轴偏移量​，右手坐标系

`sensorOffsetY` 传感器相对于车辆中心的 Y 轴偏移量​

`twoWayDrive` 能否倒车行驶

`laserVoxelSize` 激光雷达点云的降采样体素网格大小

`terrainVoxelSize` 地形点云的降采样体素网格大小

`useTerrainAnalysis` 是否启用地形分析，决定了是使用​原始激光雷达点云​还是​预处理后的地形点云​进行障碍物检测和路径决策

`checkObstacle` 是否启用障碍物检测，决定了是使用​原始激光雷达点云​还是​预处理后的地形点云​进行障碍物检测和路径决策

`checkRotObstacle` 是否检测车辆旋转时车身与障碍物的碰撞风险

`adjacentRange` 路径规划时考虑障碍物的最大范围和路径搜索范围

`obstacleHeightThre` 障碍物高度阈值，当点云的intensity即地形高度​超过该阈值时，视为障碍物

`groundHeightThre` 地面高度阈值，当地形点云的intensity即地形高度​低于此阈值时，会被视为地面点

`costHeightThre` 高度成本阈值，当地形点云的intensity即地形高度超过此阈值会计算通行成本。需要`useCost`设为`True`才会发挥作用

`costScore` 地形起伏对路径选择的惩罚系数

`useCost` 是否启用地形成本分析

`pointPerPathThre` 有效路径周围障碍物点云的最大数量，一条候选路径周围的点云数量需要小于这个阈值才能被视为可行路径

`minRelZ` 高度过滤参数，剔除高度低于`vehicleZ + minRelZ` 的点。需要`useTerrainAnalysis`设为`False`才会发挥作用

`maxRelZ` 高度过滤参数，剔除高度高于`vehicleZ + maxRelZ` 的点。需要`useTerrainAnalysis`设为`False`才会发挥作用

`maxSpeed` 机器人行驶最大速度

`dirWeight` 路径方向与目标方向的一致性在路径评分中的权重系数

`dirThre` 路径方向与目标方向之间的最大允许偏差

`dirToVehicle` 决定路径方向与目标方向的偏差是相对于 ​车辆当前航向（车体坐标系）​​还是​全局坐标系​进行判断，为`True`则基于车体坐标系，会受航向漂移影响

`pathScale` ​路径缩放因子​，动态调整候选路径的几何尺寸

`minPathScale` 路径缩放因子的下限参数，​限制路径缩放的最小比例

`pathScaleStep` 路径缩放因子的调整步长​，当初始路径因障碍物无法通过时，算法按这个步长逐步缩小路径尺寸

`pathScaleBySpeed` 是否根据速度缩放路径尺寸

`minPathRange` 限制路径搜索的最小空间范围

`pathRangeStep` 当初始搜索路径范围内无可行路径时，按 这个步长逐步扩大检测范围

`pathRangeBySpeed` 是否根据速度调整路径搜索范围

`pathCropByGoal` 是否根据目标点位置裁剪候选路径，避免不必要的延伸

`autonomyMode` 初始是否为自主模式

`autonomySpeed` 自主模式行驶速度

`joyToSpeedDelay` 手柄参数，可删去

`joyToCheckObstacleDelay` 手柄参数，可删去

`goalClearRange` 路径会裁剪到​目标点扩展范围内。需要`pathCropByGoal`设为`True`才会发挥作用

`goalX` 初始目标的位置，一般设为0

`goalY` 初始目标的位置，一般设为0

## 2. pathFollower中的参数

`sensorOffsetX` 传感器相对于车辆中心的 X 轴偏移量​，右手坐标系

`sensorOffsetY` 传感器相对于车辆中心的 Y 轴偏移量​

`pubSkipNum` 发布消息时跳过的循环次数，若为1则每2次循环发布一次

`twoWayDrive` 能否倒车行驶

`lookAheadDis` 前瞻点距离，会向前看多远的目标点作为当前跟踪点

`yawRateGain` ​转向控制增益参数

`stopYawRateGain` 低速转向增益参数

`maxYawRate` ​最大转向速率​

`maxSpeed` 机器人行驶最大速度

`maxAccel` 机器人行驶最大加速度

`switchTimeThre` ​双向行驶模式下的​前进/后退方向切换时间阈值。需要`twoWayDrive`设为`True`才会发挥作用

`dirDiffThre` ​方向偏差的阈值，当前航向与目标方向偏差大于阈值将减速

`stopDisThre` 停止距离阈值，距离目标点小于这个阈值则停止转向并刹车

`slowDwnDisThre` 减速距离阈值，距离目标点小于这个阈值则减速

`useInclRateToSlow` 是否启用倾斜角速度过高自动触发减速

`inclRateThre` 滚转或俯仰的角速度超过这个阈值则减速。需要`useInclRateToSlow`设为`True`才会发挥作用

`slowRate1` 倾斜角速度过高触发减速时第一阶段减速比例系数

`slowRate2` 倾斜角速度过高触发减速时第二阶段减速比例系数

`slowTime1` 倾斜角速度过高触发减速时第一阶段减速时间

`slowTime2` 倾斜角速度过高触发减速时第二阶段减速时间

`useInclToStop`  是否启用倾斜角度过高自动触发停车

`inclThre`  滚转或俯仰的角度超过这个阈值则停车。需要`useInclToStop`设为`True`才会发挥作用

`stopTime`  倾斜角度过高触发停车时持续时间

`noRotAtStop` 决定机器人在​完全停止时是否允许转向

`noRotAtGoal` 决定机器人接近路径终点时是否允许转向

`autonomyMode` 初始是否为自主模式

`autonomySpeed` 自主模式行驶速度

`joyToSpeedDelay` 手柄参数，可删去
