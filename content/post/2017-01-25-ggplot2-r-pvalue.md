---
title: 'R画图注释Pvalue'
author: 波比
date: '2017-01-25'
slug: R画图注释Pvalue
categories:
  - R语言
tags:
  - ggplot2
  - R语言
  - 画图
  - 统计
---

这种画图在科研出图的时候经常会用到，今天见识了如何利用ggplot2话这样的图。

```R
suppressPackageStartupMessages(require(ggplot2))
a<-matrix(nrow=100,ncol=3,data=runif(300,max=2))
b<-matrix(nrow=100,ncol=3,data=runif(300,max=1))
colnames(a)<-c("case 1","case 2","case 3")
colnames(b)<-c("case 1","case 2","case 3")
n <- 3
xpos <- 0:(n-1)*3+1.5
ypos.a <- apply(a, 2, max)
ypos.b <- apply(b, 2, max)
pvalue <- c(0.5, 0.05, 0.001)
mark <- symnum(pvalue, cutpoints=c(0, 0.05, 1), symbols=c("*", NA))
dist <- max(range(a,b))/20
ylim <- range(a, b)
ylim[2] <- ylim[2]+dist
boxplot(a, at=0:(n-1)*3 + 1, xlim=c(0,n*3), ylim=ylim, xaxt="n", col="yellow")
boxplot(b, at=0:(n-1)*3+2, xaxt="n", add=TRUE, col="red")
axis(1, at = 0:(n-1)*3 + 1.5, labels = colnames(a), tick = TRUE)
for(i in 1:length(mark)){
  if(!is.na(mark[i])){
    segments(xpos[i]-.5, ypos.a[i]+dist/2, xpos[i]-.5,max(ypos.a[i], ypos.b[i])+dist)
    segments(xpos[i]+.5, ypos.b[i]+dist/2, xpos[i]+.5, max(ypos.a[i], ypos.b[i])+dist)
    segments(xpos[i]-.5, max(ypos.a[i], ypos.b[i])+dist, xpos[i]-0.1, max(ypos.a[i], ypos.b[i])+dist)
    segments(xpos[i]+.5, max(ypos.a[i], ypos.b[i])+dist, xpos[i]+0.1, max(ypos.a[i], ypos.b[i])+dist)
    text(x=xpos[i], y=max(ypos.a[i], ypos.b[i])+dist, label=mark[i], col="red")
  }
}
```

![](http://www.tanboyu.com/wp-content/uploads/2017/01/img_588853cf3587b.png)