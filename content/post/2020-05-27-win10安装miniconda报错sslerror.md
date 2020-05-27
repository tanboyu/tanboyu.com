---
title: Win10安装miniconda报错SSLError
author: 波比
date: '2020-05-27'
slug: install_miniconda_reported_sslerror
categories:
  - 网络技术
tags: []
---

工作站（Win10）重新安装了miniconda之后，运行**conda update conda** 老提示SSLError错误。由于修改了国内镜像，以为是镜像url出了问题，点击发现镜像url可以访问，百思不得解。一顿试错操作后发现如下为正解。

1.执行
```bash
conda config --set show_channel_urls yes
```

2. 在“c:\users\<用户名>” 对应用户名的目录下会有一个“.condarc”文件，修改如下：

```bash
ssl_verify: true
show_channel_urls: true
channel_alias: https://mirrors.tuna.tsinghua.edu.cn/anaconda
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/pro
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

3. 将miniconda3的这几个目录添加进环境变量“PATH”中,目录路径因人而已，自己需查找正确的目录路径。我的是如下

```bash
C:\ProgramData\Miniconda3
C:\ProgramData\Miniconda3\Library\bin
C:\ProgramData\Miniconda3\Scripts
```

如此，再运行conda命令操作不再报错。



