---
title: 使用GitHub
date: 2025-05-21 16:59:57
# updated:
tags:
    - Git
    - Linux
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

## 1. SSH配置

请参考{% post_link 技术文档/使用SSH  使用SSH博客%}

配置config文件

```bash
vim ~/.ssh/config
```

在里面添加

```ini
Host github.com
  HostName github.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/xxx
  # 这里xxx换成你命名的私钥名称
```

## 2. 添加SSH key到GitHub

拷贝xxx.pub文件的内容

```bash
vim ~/.ssh/xxx.pub
```

登录GitHub账号，从右上角的设置进入，然后点击菜单栏的 SSH key 进入页面添加 SSH key

## 3. 设置Git信息

```bash
git config --global user.name '你的名字' 
git config --global user.email '你的邮箱'
```

`git config --list`可以查看信息
