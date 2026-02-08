---
title: DSVP代码解读
date: 2026-02-03 11:28:36
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

## 1. 代码依赖

- `volumetric_mapping`：[项目地址](https://github.com/ethz-asl/volumetric_mapping.git)
  - `minkindr`：[项目地址](https://github.com/ethz-asl/minkindr.git)
  - `minkindr_ros`：[项目地址](https://github.com/ethz-asl/minkindr_ros.git)

## 2. 代码结构

### 2.1 `dsvplanner`包

入口文件为`drrtp_node`，此文件会启动`drrtp`

`drrtp`会启动`/drrtPlannerSrv`服务的服务端和`/cleanFrontierSrv`服务的服务端

  

### 2.2 `kdtree`包

使用`kdtree`数据接口来管理`drrt`的节点，从而加速最近邻搜索

这个包使用的是[开源项目](https://github.com/jtsiomb/kdtree)

---
参考文章

[代码解析：DSVP: Dual-Stage Viewpoint Planner for Rapid Exploration by Dynamic Expansion](https://www.guyuehome.com/detail?id=1825490290073202690)
