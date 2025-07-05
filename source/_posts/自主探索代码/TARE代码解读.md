---
title: TARE代码解读
date: 2025-06-27 21:49:25
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

本代码主只是用来做自主探索时的上层路径规划和重定位

1. exploration_path
2. keypose_graph
3. navigation_boundary_publisher
4. rolling_grid
5. tare_planner_node
6. utils
7. graph
8. lidar_model
9. planning_env
10. rolling_occupancy_grid
11. tare_visualizer
12. viewpoint
13. grid_world
14. local_coverage_planner
15. pointcloud_manager
16. sensor_coverage_planner
17. tsp_solver
18. viewpoint_manager

## 1. exploration_path

实现探索路径的类

## 2. keypose_graph

## 3. navigation_boundary_publisher

在Matterport3D 仿真环境时，从`src/tare_planner/data/boundary.ply`文件读取探索边界信息并发送出去

输入为空

输出为`/navigation_boundary`

## 4. rolling_grid

实现滚动3D网格的类

## 5. tare_planner_node

tare planner主程序入口

## 6. utils

杂项功能函数

## 7. graph

实现图的类

## 8. lidar_model

实现激光雷达模型的类，应该是用来计算viewpoint的回报的时候用的

## 9. planning_env

管理使用点云的世界表示的类

## 10. rolling_occupancy_grid

实现滚动占据栅格的类

## 11. tare_visualizer

可视化规划过程的类

## 12. viewpoint

实现viewpoint的类

## 13. grid_world

实现网格世界的类

## 14. local_coverage_planner

确保覆盖机器人周围的类

## 15. pointcloud_manager

实现点云网格的类

## 16. sensor_coverage_planner

输入为

`/start_exploration` 开始探索

`/terrain_map`

`/terrain_map_ext`

`/state_estimation_at_scan`

`/registered_scan`

`/sensor_coverage_planner/coverage_boundary`

`/navigation_boundary` 作为viewpoint_boundary

`/sensor_coverage_planner/nogo_boundary` 禁止区域边界

`/joy` 手柄，可删去

`/reset_waypoint`

输出为

`global_path_full`

`global_path`

`old_global_path`

`to_nearest_global_subspace_path`

`local_path`

`exploration_path`

`/way_point`

`exploration_finish`

`runtime_breakdown`

`/runtime`

`momentum_activation_count`

`pointcloud_manager_neighbor_cells_origin`

## 17. tsp_solver

实现了tsp求解器的封装,在tsp规划的时候调用

## 18. viewpoint_manager

管理局部规划范围内viewpoint的类

参数解读

`kAutoStart` 是否自动开始，如果设为`True`，则`/start_exploration`话题不起作用。见`sensor_coverage_planner_ground.cpp`的1276行

`kRushHome` 接近起点时是否快速回到起点，当探索结束、接近起点、快速回到起点设为`True`都满足时，会直接把起点发送为目标点。见`sensor_coverage_planner_ground.cpp`的1155行

`kUseTerrainHeight` 是否考虑地形高度，如果设为`True`，在设置ViewPoint时会根据地形调整高度，见`sensor_coverage_planner_ground.cpp`的491行；会将`/terrain_map_ext`话题里的消息放进地形点云里，见`sensor_coverage_planner_ground.cpp`的335行

`kCheckTerrainCollision` 是否考虑地形碰撞

`kExtendWayPoint` 是否要延长路径点，如果设为`True`，当机器人与前瞻点距离小于延长阈值时会适当延长路径点，见`sensor_coverage_planner_ground.cpp`的1168行

`kUseLineOfSightLookAheadPoint` ​前瞻点的选择是否考虑视线的范围和遮挡

`kNoExplorationReturnHome` 探索结束后是否返回起点

`kExtendWayPointDistanceBig` 无障碍物遮挡时的路径点延长阈值，此时移动更加激进，阈值较大。需要`kExtendWayPoint`设为`True`才会发挥作用

`kExtendWayPointDistanceSmall` 有障碍物遮挡时的路径点延长阈值，此时移动更加保守，阈值较小。需要`kExtendWayPoint`设为`True`才会发挥作用

`kKeyposeCloudDwzFilterLeafSize` 关键帧点云的下采样粒度

`kRushHomeDist` 回到起点时，当与起点的距离小于这个阈值判断为接近起点，若是`kRushHome`设为`True`，此时会快速回到起点

`kAtHomeDistThreshold` 当与起点的距离小于这个阈值判断为已经到达起点

`kTerrainCollisionThreshold` 地形碰撞检测的阈值，`/terrain_map`中每个点的intensity（点云强度）字段表示​地形高度或危险程度，当intensity > 这个阈值，则该点被视为​障碍物，会被加入地形碰撞检测点云`terrain_collision_cloud_`

`kLookAheadDistance` 前瞻点距离，机器人会在路径上搜索距离机器人当前位置​最远不超过此距离的点作为前瞻点

`kUseMomentum` 是否启用​动量机制，如果设为`True`并且机器人激活了动量机制，机器人在选择前瞻点时，会​优先选择与当前运动方向一致的点

`kDirectionChangeCounterThr` 方向变化次数的阈值，若方向频繁反转超过此阈值，则激活动量机制

`kDirectionNoChangeCounterThr` 方向保持不变次数的阈值，若方向稳定，不变化的次数超过此阈值，则激活动量机制

`kResetWaypointJoystickAxesID` 使用手柄重设路径点的按键ID号，可删去
