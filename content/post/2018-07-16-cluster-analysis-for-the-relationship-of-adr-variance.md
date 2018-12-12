---
title: '基于聚类分析药物不良反应相关指标的相关性'
author: 波比
date: '2017-07-16'
slug: 基于聚类分析药物不良反应相关指标的相关性
categories:
  - R语言
tags:
  - R语言
  - 统计学方法
  - 聚类
  - 数据挖掘
---

在临床用药过程中，常遇到出现药物不良反应（ADR）的情况，如何判断这类药物与哪些因素有关？通常的做法就是填写ADR的表单，然后结合患者用药的前后过程，时间顺序、临床用药、类似反应以及临床操作处置等因素综合分析，然而这种综合分析大多时候都是推测，说白了和猜没多少区别。如何做到能让数据说话呢？既然学着玩数据这么长久了，能否有算法来实现呢？ 纵观常用的相关的算法，基本就是聚类、关联规则之类的了。今天就以聚类来分析下ADR的情况。

## 读入数据

```R
adrdata <- read.xlsx("./data/adrdata.xlsx") # 读入数据
```

## 数据字段格式 

年龄 给药途径 性别 结局 适应症 联合药物 症状 ……

```R
# 预处理 将age等数值型变量暂时分离，然后将分类变量数据框转矩阵
my_matrix = as.data.frame(do.call(cbind, lapply(adrdata, function(x) table(1:nrow(adrdata), x))))

# 预处理
preproc = preProcess(my_matrix) 

# 标化数据
my_matrixNorm = as.matrix(predict(preproc, my_matrix))

# 计算距离然后聚类
distances = dist(my_matrixNorm, method = "euclidean")
clusterdrug = hclust(distances, method = "ward.D")

# 画图
plot(clusterdrug, cex=0.5, labels = FALSE,cex=0.5,xlab = "", sub = "",cex=1.2)

```



![](https://ws1.sinaimg.cn/large/8f5e6680gy1fnijc0uzafj20r60cddh1.jpg)