---
title: 'latex单元格内换行设置'
author: 波比
date: '2016-10-16'
slug: latex单元格内换行设置
categories:
  - 网络技术
tags:
  - latex
  - 网络技术
  - 写作
  - 单元格
  - 换行
---

步骤如下：

1.  在导言区将下面一行的命令复制过去； \\newcommand{\\tabincell}\[2\]{\\begin{tabular}{@{}#1@{}}#2\\end{tabular}}
2.  在需要进行换行的单元格使用\\tabincell{c}{单元格文字内容，使用“\\\\”进行换行设置\\\\这样就可以实现换行了。}