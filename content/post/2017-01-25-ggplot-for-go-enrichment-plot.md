---
title: 'R语言画GO/KEGG的分类图'
author: 波比
date: '2017-01-25'
slug: R语言画GO/KEGG的分类图
categories:
  - 生物信息学
tags:
  - ggplot2
  - GO分类图
  - GO富集画图,
  - R语言
  - 生物信息学
---

这个标题我也不知道描述清楚了没，大意就是用ggplot画GO enrichment analysis results

```R
suppressWarnings(require(ggplot2)) # 载入画图函数包

attach(pathway) #加载数据
```

Pathway的数据结构如下：

![](http://www.tanboyu.com/wp-content/uploads/2017/01/img_5888dc1919b92.png)

关键用到的columns：term, Pvalue, logPvalue…… 好下面开始上代码

```R
windowsFonts(TNR=windowsFont("TT Times New Roman")) # 更改字体为Times New Roman
p <- ggplot(pathway,aes(x=reorder(Term,LogPvalue),y=LogPvalue)) + geom_bar(stat = "identity",fill="black",width = 0.65)+coord_flip()
p + theme(panel.grid.major =element_blank(), 
 panel.grid.minor = element_blank(),
 panel.background = element_blank(),
 axis.title = element_text(family = "TNR"),
 axis.text = element_text(family = "TNR",size=14),
 axis.line.x = element_line(colour = "black"),
 axis.title.x = element_text(size = 14, family = "TNR", face = "bold"),
 axis.title.y = element_text(size = 14, family = "TNR", face = "bold"),
 axis.ticks.y = element_blank())+scale_y_continuous(expand = c(0, 0))+ xlab("Terms")

# 注释讲下几个地方：reorder对数据重新排序，fill填充颜色，panel* 就是去网格线、背景色等
```



![](http://www.tanboyu.com/wp-content/uploads/2017/01/img_5888dd34df9e4.png)