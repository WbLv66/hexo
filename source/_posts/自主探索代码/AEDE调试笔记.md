---
title: AEDE调试笔记
date: 2025-08-08 16:08:09
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

1. 在fast_lio项目中的`laserMapping.cpp`的`"camera_init"`改为`"map"`,`"body"`改为`"sensor"`
2. 设置AEDE项目中的`loam_interface/launch/loam_interface.launch`文件`stateEstimationTopic =/Odometry, registeredScanTopic = /cloud_registered, flipStateEstimation = false, flipRegisteredScan = false`
3. 设置AEDE项目中的`local_planner/launch/local_planner.launch`文件`maxSpeed`改为`0.5`
4. `far_planner`配置文件中将`is_static_env`设为`false`以清除动态障碍物