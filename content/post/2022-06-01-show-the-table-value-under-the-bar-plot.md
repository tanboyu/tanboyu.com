---
title: 画带表格的条形图
author: 波叔
date: '2022-06-01'
slug: show_the_table_value_under_the_bar_plot
categories:
  - R语言
tags:
  - 画图
draft: no
mathjax: no
type: post
---

最近工作需要画一种带表格内容的条形图。以前只是画过条形图没尝试过如何把表格与图整合到一个图中。

要完成这样的画图大致思路是将表格与图进行拼合应该就可以了。网上搜索了一下，还真看到有这种需求的。把代码搬到这里供有需者参考。

```r
library(grid)
library(gridExtra)
library(ggplot2)
library(reshape)

# demo data
df <-
  structure(
    list(
      year = 2002:2005,
      work = c(1L, 2L, 3L, 2L),
      confid = c(8L,
                 5L, 0L, 6L),
      jrs = c(0L, 3L, 4L, 5L)
    ),
    .Names = c("year", "work",
               "confid", "jrs"),
    class = "data.frame",
    row.names = c(NA,-4L)
  )

md <- melt(df, id=(c("year")))

# Function to extract legend
# https://stackoverflow.com/a/13650878/496488
g_legend <- function(a.gplot){
  tmp <- ggplot_gtable(ggplot_build(a.gplot))
  leg <- which(sapply(tmp$grobs, function(x) x$name) == "guide-box")
  legend <- tmp$grobs[[leg]]
  return(legend)}

p = ggplot(data=md, aes(x=year, y=value, fill=variable) ) + 
  geom_bar(stat="identity")+ 
  #theme(axis.text.x=element_text(angle=90, vjust=0.5, hjust=0.5))+ 
  ggtitle("Score Distribution") +
  labs(fill="")

# Extract the legend as a separate grob
leg = g_legend(p)

# Create a table grob
tab = t(df)
tab = tableGrob(tab, rows=NULL)
tab$widths <- unit(rep(1/ncol(tab), ncol(tab)), "npc")

# Lay out plot, legend, and table grob
grid.arrange(arrangeGrob(nullGrob(), 
                         p + guides(fill=FALSE) + 
                           theme(axis.text.x=element_blank(),
                                 axis.title.x=element_blank(),
                                 axis.ticks.x=element_blank()),
                         widths=c(1,8)), 
             arrangeGrob(arrangeGrob(nullGrob(),leg,heights=c(1,10)),
                         tab, nullGrob(), widths=c(6,20,1)),
             heights=c(4,1))

```

效果图如下：

![](http://m.qpic.cn/psc?/V516nacF3U4jO51xJMmC0a7T7z28f6KI/bqQfVz5yrrGYSXMvKr.cqSyk9pEwJ6Zwu7tAY7M5qsLh7a6Qqb4SypwnkbXHW5U32PQOsGBtwmcdmaIGiVpKxqm*R4sscSByCtLlqNx7w3A!/b&bo=EAImAgAAAAADBxQ!&rf=viewer_4)
