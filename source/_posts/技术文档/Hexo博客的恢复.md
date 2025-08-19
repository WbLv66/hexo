---
title: Hexo博客的恢复
date: 2025-08-12 15:39:15
# updated:
tags:
    - Hexo
    - Git
categories: 技术文档
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

## 1. 安装Nodejs

从[官网](https://deb.nodesource.com/)下载安装包

```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo bash -

sudo apt-get install -y nodejs
```

## 2. 安装Hexo

```bash
sudo npm install hexo-cli -g

sudo npm install hexo -g
```

## 3. 克隆项目

将`github`项目下载到本地并拉取子模块

```bash
git clone git@github.com:WbLv66/hexo.git

cd hexo

git submodule update --init
```

## 4. 恢复博客

```bash
npm install

npm install hexo-deployer-git
```

---
参考
[Hexo博客的备份与恢复](https://blog.csdn.net/muzihuaner/article/details/113880440)
