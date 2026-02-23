---
title: TARE代码解读
date: 2026-02-21 17:53:54
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

## 1. 代码结构

### 1.1 `lidar_model`

这里的体素索引依靠的是水平角度索引+垂直角度索引，二者的分辨率都是按每2度递增。其中垂直角度索引可以看成行索引，水平角度索引可以看成列索引

水平角度是以`map`坐标系下的x方向为零，在代码中将-x方向为索引的开始，逆时针为正；垂直角度是以`map`坐标系下的z方向为零，但是此方向一般为激光雷达的盲区，在代码中将视场角的上边作为索引的开始，通过`kVerticalAngleOffset`实现偏移

`GetHorizontalNeighborNum`和`GetVerticalNeighborNum`像是经验公式，即射线的邻居数量与距离成反比（距离越远，点云越稀疏）与分辨率成反比（分辨率越大，体素越稀疏）

### 1.2 `grid`

维护了一组栅格，并实现了线性索引、向量索引、维度下标索引和坐标值间的互相转换

### 1.3 `rolling_grid`

滚动栅格

栅格存储的数据反向滚动

### 1.4 `rolling_occupancy_grid`

