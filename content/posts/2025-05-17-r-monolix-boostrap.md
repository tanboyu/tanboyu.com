---
title: 基于R语言进行Monolix建模后的Boostrap验证
author:
  - 波叔
date: '2025-05-17'
slug: r-monolix-boostrap
categories:
  - 定量药理学
tags:
  - R语言
  - 定量药理学
  - tech
lastmod: '2025-05-17T14:40:03+08:00'
keywords:
  - R语言
  - boostrap
  
description: '用R语言进行药动学模型boostrap验证'
summary: ''
weight: ~
draft: no
comments: yes
showToc: yes
TocOpen: yes
autonumbering: yes
hidemeta: no
disableShare: yes
searchHidden: no
showbreadcrumbs: yes
mermaid: yes
cover:
  image: './postimg/boosting.png'
  caption: ''
  alt: 'boostrap验证'
  relative: no
---

Monolix建模后进行Boostrap验证可以基于GUI界面进行，如果会R语言的话，非常简单。

```r
library(Rsmlx)
library(RJSONIO)
library(lixoftConnectors)

# 检查是否正常运行
initializeLixoftConnectors(software = "monolix")
loadProject("./***.mlxtran") # 载入模型
runScenario()
getEstimatedPopulationParameters()
# 一旦本代码验证过后，上述代码可以删除


# Final boosting
initializeLixoftConnectors(software = "monolix")

project <- "./project_file_path/***.mlxtran" # 修改对应的文件路径
boostdat <- bootmlx(project, nboot = 1000, parametric = T, settings = list(plot =T, level = 0.95))

print(boostdat)

dat <- read.table("./project_file_path/boostrap/parametric/PopulationParameters.txt",header = T, sep = ",") # 修改对应的文件路径

head(dat)
colMeans(dat)
median(dat$v_pop)


```