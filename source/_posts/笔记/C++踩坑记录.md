---
title: C++踩坑记录
date: 2025-07-30 23:28:35
# updated:
tags:
    - C++
categories: 笔记
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

## 1. 数组智能指针

在创建指向数组的智能指针时需要注意传入模板的类型需要加中括号，例如创建八个元素的整数数组指针`std::unique_ptr<int[]> ptr = std::make_unique<int[]>(8)`
