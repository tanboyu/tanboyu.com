---
title: 'ggplot2画火山图'
author: 波比
date: '2017-04-17'
slug: ggplot2画火山图
categories:
  - 生物信息学
tags:
  - ggplot2
  - 可视化
  - 火山图
  - 生物信息学
---

用ggplot2画火山图看代码似乎还是很简单，但是真的来画了，细节还是蛮多的。 把代码写在这里，以便以后用。

```R
library(ggplot2)
library（Cairo）
# 读取数据
data <- read.delim("./data/g1_VS_g3.txt",header = T,sep="\t",na.strings = "")
# 设置颜色域

datathreshold = as.factor(datapvalues < 0.05 & abs(log2(data$foldchange)) >=1.5)

Cairo(file="volcan_PNG_300_dpi.png", 
      type="png",
      units="in",
      bg="white",
      width=8, 
      height=6, 
      pointsize=12, 
      dpi=300)
ggplot(data=data, 
            aes(x=log2(foldchange), y =-log10(pvalues), 
                colour=threshold)) +
  geom_point(alpha=0.4, size=1.75) +
  xlim(c(-4, 4)) +
  xlab("log2 fold change") + ylab("-log10 p-value") +
  ggtitle("Volcano picture of DEG")+
  theme_gray() +
  geom_vline(xintercept=c(-1.5,1.5),lty=4,col="grey",lwd=0.5)+ 
  geom_hline(yintercept = -log10(0.05),lty=4,col="grey",lwd=0.5)+ 
  theme(legend.position="none")
# 在x轴-1.5与1.5的位置画两根竖线, 在p-value 0.05的位置画一根横线
dev.off()
```

这只是一个粗略的图，要细致，还需要调整网格，坐标轴，颜色，字体大小。

**我们能不能将下调，上调的分别改变颜色体现呢？**

那就来试试吧，思路其实就是颜色的映射到不同的 见前代码 **colour=threshold **，之前的设置只设定了2个水平，显著or不显著。那其实只要把显著与不显著改成上调、下调、无显著改变就可以了。见代码：

```R
datathreshold <- as.factor(ifelse(datapvalues < 0.05 & abs(log2(datafoldchange)) >=1.5,ifelse(log2(datafoldchange) > 1.5 ,'Up','Down'),'Not'))
```

建立好了分层，我们还需要做的一点是，分配颜色。

```R
scale_color_manual(values=c("blue", "grey","red"))
```

这样就搞定了。但是，但是，强迫症估计还要加上图历，映射不同颜色是吧，呵呵。那就看下面的代码吧

```R
theme(legend.position="right")
```

这样通过**legend.position** 指定显示的位置，或许你还想更改图例的标题，%……%……这需要你自己动手了，Google or bing去寻求学习。 下面放出完整代码：

```R
library(ggplot2)
library(ggthemes)
library(Cairo)

data <- read.delim("./data/g1_VS_g3.txt",header = T,sep="\t",na.strings = "")

datathreshold <- as.factor(ifelse(datapvalues < 0.05 & abs(log2(datafoldchange)) >=1.5,ifelse(log2(datafoldchange) > 1.5 ,'Up','Down'),'Not'))

# Construct the plot object
# with legend

Cairo(file="volcan_PNG_300_lengend_dpi.png", 
      type="png",
      units="in",
      bg="white",
      width=5.5, 
      height=5, 
      pointsize=12, 
      dpi=300)
ggplot(data=data, 
            aes(x=log2(foldchange), y =-log10(pvalues), 
                colour=threshold,fill=threshold)) +
  scale_color_manual(values=c("blue", "grey","red"))+
  geom_point(alpha=0.4, size=1.2) +
  xlim(c(-4, 4)) +
  theme_bw(base_size = 12, base_family = "Times") +
  geom_vline(xintercept=c(-1.5,1.5),lty=4,col="grey",lwd=0.6)+
  geom_hline(yintercept = -log10(0.05),lty=4,col="grey",lwd=0.6)+
  theme(legend.position="right",
        panel.grid=element_blank(),
        legend.title = element_blank(),
        legend.text= element_text(face="bold", color="black",family = "Times", size=8),
        plot.title = element_text(hjust = 0.5),
        axis.text.x = element_text(face="bold", color="black", size=12),
        axis.text.y = element_text(face="bold",  color="black", size=12),
        axis.title.x = element_text(face="bold", color="black", size=12),
        axis.title.y = element_text(face="bold",color="black", size=12))+
  labs(x="log2 (fold change)",y="-log10 (p-value)",title="Volcano picture of DEG")
dev.off()

# without legend

Cairo(file="volcan_PNG_300_dpi.png", 
      type="png",
      units="in",
      bg="white",
      width=6, 
      height=5, 
      pointsize=12, 
      dpi=300)
ggplot(data=data, 
       aes(x=log2(foldchange), y =-log10(pvalues), 
           colour=threshold,fill=threshold)) +
  scale_color_manual(values=c("blue", "grey","red"))+
  geom_point(alpha=0.4, size=1.2) +
  xlim(c(-4, 4)) +
  theme_bw(base_size = 12, base_family = "Times") +
  geom_vline(xintercept=c(-1.5,1.5),lty=4,col="grey",lwd=0.6)+
  geom_hline(yintercept = -log10(0.05),lty=4,col="grey",lwd=0.6)+
  theme(legend.position="none",
        panel.grid=element_blank(),
        # legend.title = element_blank(),
        # legend.text= element_text(face="bold", color="black",family = "Times", size=8),
        plot.title = element_text(hjust = 0.5),
        axis.text.x = element_text(face="bold", color="black", size=12),
        axis.text.y = element_text(face="bold",  color="black", size=12),
        axis.title.x = element_text(face="bold", color="black", size=12),
        axis.title.y = element_text(face="bold",color="black", size=12))+
  labs(x="log2 (fold change)",y="-log10 (p-value)",title="Volcano picture of DEG")
dev.off()
```
图略。