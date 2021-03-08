---
title: Rmarkdown自循环生成结果
author: 波比
date: '2019-01-10'
slug: rmarkdown自循环生成结果
categories:
  - R语言
tags:
  - Rmarkdown
---

最近遇到一个头痛的事，有185个项目的内容需要整理成简单的会议记录。想着R应该是可以搞成的，所以试了下，效果不错。
代码分享给需要的人：
用Rmarkdown格式的文件写的，{r echo=FALSE, message=FALSE, warning=FALSE, results="asis"} 就不放在代码段中了，自己设置。

``` r
library(dplyr)
data <- readxl::read_excel("e:/文件名.xlsx",sheet=1)%>%filter(分类=='分类筛选变量名')
colnames(data) <- c('yq','zy','lcks','jsfl','xmmc','xmfzr','pjh','scjg','yjsm','zswy','cate')

temp <- "
%s\n
院区：%s\n
专业:%s项目\n
临床科室：%s\n
技术分类：%s\n
项目名称：%s\n
项目负责人:%s\n
项目由%s委员主审，主审委员%s\n
最终结果为:%s\n\n
"
for(i in seq(nrow(data))){
  wd <- data[i,]
  wd[,9] <- apply(wd[,9], 2, function(x){ifelse(is.na(x),'无意见,其他委员无异议。',paste0('认为：',x))}) 
  
  cat(sprintf(temp,i,wd[1],wd[2],wd[3],wd[4],wd[5],wd[6],wd[10],wd[9],wd[8]))
}

```