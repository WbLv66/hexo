---
title: 使用uv
date: 2025-06-24 19:01:21
# updated:
tags:
    - python
    - uv
    - 包管理
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

## 1. uv安装

直接使用Python自带的pip安装，兼容性最佳

```bash
pip install uv
```

随后将pip安装的包导入到环境变量中，在.bashrc或者.zshrc中添加

```bash
export PATH="$HOME/.local/bin:$PATH"
```

使用`source ~/.zshrc`刷新

## 2. 基础用法

### 2.1 创建项目

首先设定python版本

```bash
uv python pin 3.13
uv init
```

### 2.2 添加依赖

添加`numpy`库

```bash
uv add numpy
```

添加指定版本的`numpy`库

```bash
uv add numpy>=2.0.2
```

### 2.3 移除依赖

移除`numpy`库

```bash
uv remove numpy
```

### 2.4 查看项目的依赖树

```bash
uv tree
```

### 2.5 创建虚拟环境

创建一个虚拟环境，并指定`Python`版本

```bash
uv venv my-name --python 3.11
```

### 2.6 激活虚拟环境

```bash
source .venv/bin/activate
```

### 2.7 退出虚拟环境

```bash
deactivate
```

### 2.8 在uv中使用pip

```bash
uv pip install
```

## 3. 换源

`uv`换源包括三个方面一个是依赖包的源一个是`python`的源还有一个是`pip`的源

在`.zshrc`中更换`python`源

```bash
export UV_PYTHON_INSTALL_MIRROR=https://ghproxy.cn/https://github.com/indygreg/python-build-standalone/releases/download
```

在项目中的`pyproject.toml`文件中更换依赖包源

```toml
[tool.uv]
index-url = "https://pypi.tuna.tsinghua.edu.cn/simple/"
```

创建文件`~/.pip/pip.conf`

```bash
mkdir ~/.pip

touch ~/.pip/pip.conf
```

打开配置文件，修改如下

```toml
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host = https://pypi.tuna.tsinghua.edu.cn
```

查看 镜像地址：

```bash
pip3 config list
```

## 4. 安装pytorch

这里以安装`pytorch-cu124`版本为例

在`pyproject.toml`文件中添加如下内容

```toml
[project]
...
dependencies = [
     "torch>=2.4.0",
     "torchvision>=0.22.0",
     ...
]
[tool.uv.sources]
torch = [
  { index = "pytorch-cu124" },
]
torchvision = [
    { index = "pytorch-cu124" },
]
[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple"
explicit = true
```

然后同步项目依赖

```bash
uv sync
```

## 5. 使用Jupyter

使用如下指令即可运行Jupyter

```bash
uv run --with jupyter jupyter lab
```

---

[官方文档](https://docs.astral.sh/uv/)
