---
title: 将Hexo部署到远程服务器
date: 2025-05-16 20:09:23
# updated:
tags:
    - Git
    - Hexo
    - 网站
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

## 1. 服务器部署

## 1.1 Git用户设置

首先在远程服务器中创建Git用户

```bash
adduser git
```

增加sudo权限

```bash
sudo vim /etc/sudoers
```

在文件中找到如下命令

```bash
## Allow root to run any commands anywhere
root    ALL=(ALL)     ALL
```

添加以下内容

```bash
git     ALL=(ALL)     ALL
```

然后按`Esc`键，然后按`w!`回车和`q!`回车，强制写入

在git用户下创建.ssh并将公钥复制到`authorized_keys`文件中

## 1.2 创建仓库目录

在var目录下创建repo作为Git仓库目录

```bash
sudo mkdir /var/repo
```

赋予权限

```bash
sudo chown -R git:git /var/repo
sudo chmod -R 755 /var/repo
```

接下来创建Hexo目录作为网站根目录，并赋予权限

```bash
sudo mkdir /var/hexo
sudo chown -R git:git /var/hexo
sudo chmod -R 755 /var/hexo
```

接下来创建一个空白的Git仓库

```bash
cd /var/repo
sudo git init --bare hexo.git
```

创建一个新的 Git 钩子，用于自动部署.

在`/var/repo/hexo.git`下，有一个自动生成的 hooks文件夹。我们需要在里边新建一个新的钩子文件 `post-receive`

```bash
sudo vim /var/repo/hexo.git/hooks/post-receive
```

写入如下代码

```bash
#!/bin/bash
git --work-tree=/var/hexo --git-dir=/var/repo/hexo.git checkout -f
```

修改权限

```bash
sudo chown -R git:git /var/repo/hexo.git/hooks/post-receive
sudo chmod +x /var/repo/hexo.git/hooks/post-receive
```

## 2. 配置Nginx

使用宝塔快速搭建Nginx，先安装一下[宝塔](https://www.bt.cn/bbs/thread-19376-1-1.html)

```bash
wget -O install.sh https://download.bt.cn/install/install-ubuntu_6.0.sh && sudo bash install.sh 12f2c1d72
```

去阿里云轻量服务器控制台中的防火墙中，添加宝塔面板相应的端口

在宝塔面板的软件商店中安装Nginx，部署完成之后，点击网站，添加站点，填写域名，随后点击配置文件

```nginx
server
{
    listen 80;
    # server_name填写你自己的域名，没有的话填ip
    server_name wblv66.top;
    index index.php index.html index.htm default.php default.htm default.html;
    # 这里root填写自己的网站根目录，修改为/var/hexo
    root /var/hexo;
```

## 3. 修改本地配置

修改`.ssh/config`的设置，增加如下

```ssh-config
Host wblv66.top
  HostName wblv66.top
  User git
  IdentityFile  /home/lwb/.ssh/私钥
  IdentitiesOnly yes
```

进入本地电脑Hexo博客的根目录，编辑站点配置文件 `_config.yml`，找到`deploy`，修改成以下

```yml
deploy:
  type: git
  #repo改为repo: git@你的域名:/var/repo/hexo.git
  repo: git@wblv66.top:/var/repo/hexo.git
  branch: master
```

输入以下命令部署

```bash
hexo clean

hexo d -g
```

---
参考文章

[将Hexo部署到阿里云轻量服务器（保姆级教程）](https://blog.laoda.de/archives/hexo-building#7-%E9%85%8D%E7%BD%AEnginx)
