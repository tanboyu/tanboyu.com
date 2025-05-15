---
title: 'edgeR入门'
author: 波比
date: '2016-04-26'
slug: EdgeR入门
categories:
  - 数据分析
tags:
  - edgeR
  - 使用手册
  - 指南
  - 生物信息学
  - research
---

edgeR包主要是用于利用来自不同技术平台的read数（包括RNA-seq，SAGE或者ChIP-seq等）来鉴别差异表达或者差异标记（ChIP-seq）。主要是利用了多组实验的精确统计模型或者适用于多因素复杂实验的广义线性模型。所以有时作者也把前者叫做“经典edgeR”， 后者叫做”广义线性模型 edgeR“。这里定义的read数是可以指基因水平、外显子水平、转录本水平或者标签水平等，这个由用户根据自己数据分析的实际需要而定。这里作者也列举了一些差异表达鉴定方面的文献：包括edgeR刚发布时的文献–“edgeR: a Bioconductor package for differential expression analysis of digital gene expression data”以及后来的一些改进文章。

    source("http://bioconductor.org/biocLite.R")
    biocLite("edgeR")

## 快速开始

对一个软件不是很熟悉时，一般都可以看看它的快速简介，就能进行运算了。edgeR的基本计算步骤就是： 经典edgeR操作步骤：

1.  先读取read数（比如说RNA-seq数据，可以利用Bowtie、Tophat及htseq-counts这么个流程获取），一个行为基因（或者其它特征）列为样本元素为read数的矩阵；
2.  构建分组变量，主要是对样本进行分组（分为处理组和对照组，利用factor函数）
3.  建立基因表达列表，利用DEGList函数， 参数counts为read数矩阵，group为上一步的分组变量，还有其它参数，后面有介绍
4.  估计与计算各种其它参数，计算各样本内的标准化因子（calcNormFactors函数）（由于RNA-seq测序过程中技术因素可能会产生样本特异性效应，所以标准化一般是在这种情况下才使用的，也主要是测序深度即和文库大小有关，可以通过DEGList函数的lib.size=colSums(counts)来设置。），估计生物学变异系数（BCV，Biological cofficient of variation， 在经典edgeR中，BCV就是负二项分布中的离散度的平方根，所以估计BCV也是估计离散度，edgeR先认为所有的基因有同样的离散度，estimateCommonDisp算得，在此基础上计算基因间范围内的离散度，estimateTagwiseDisp算得）
5.  通过检验来计算鉴别差异表达基因

```R
    x <- read.delim("fileofcounts.txt",row.names="Symbol") # 读取reads count文件
    group <- factor(c(1,1,2,2)) #分组变量 前两个为一组， 后一个为一组， 每个有两个重复
    y <- DGEList(counts=x,group=group) # 构建基因表达列表
    y <- calcNormFactors(y) # 计算样本内标准化因子
    y <- estimateCommonDisp(y) #计算普通的离散度
    y <- estimateTagwiseDisp(y) #计算基因间范围内的离散度
    et <- exactTest(y) # 进行精确检验
    topTags(et) # 输出排名靠前的差异表达基因信息
```
    

#### 广义线性模型 edgeR操作步骤：

1.  也是要读取raw read count 数据（同上）；
2.  构建实验设计变量 也就是一个矩阵；
3.  构建基因表达列表并进行标准化处理
4.  利用实验设计变量及基因表达列表来估计估计生物变异的离散度（dispersion）、基因间离散度估算
5.  拟合广义线性模型并利用似然比检验来鉴别差异表达基因

```R
    rawdata <- read.delim("TableS1.txt", check.names=FALSE, stringsAsFactors=FALSE);  #读取原始文件
    y <- DGEList(counts=rawdata[,4:9], genes=rawdata[,1:3]);  # 建立基因表达列表
    y$samples$lib.size <- colSums(y$counts);  #设置各个样本的库大小
    y <- calcNormFactors(y);  #利用库的大小来进行标准化TMM
    Patient <- factor(c(8,8,33,33,51,51));  #因素1
    Tissue <- factor(c("N","T","N","T","N","T")); #因素2 
    data.frame(Sample=colnames(y),Patient,Tissue); 
    design <- model.matrix(~Patient+Tissue);  #建立分组变量
    rownames(design) <- colnames(y); 
    y <- estimateGLMCommonDisp(y, design, verbose=TRUE);  #估计离散度
    y <- estimateGLMTrendedDisp(y, design);  
    y <- estimateGLMTagwiseDisp(y, design); 
    fit <- glmFit(y, design);  #拟合广义线性模型
    lrt <- glmLRT(fit); topTags(lrt);  #利用似然比检验来检验差异表达基因
    topTags(lrt);    #输出靠前的差异表达基因
```

#### 实例：

下面通过文档中提供的一个RNA-seq的例子来具体说下edgeR的使用流程（经典edgeR），GLM方法的就不讲了，更复杂的见edgeR的文档。 数据： [pnas\_expression](http://www.iwhgao.com/wp-content/uploads/2014/06/pnas_expression.txt) 文本数据

```R
  #加载软件包
library("edgeR",verbose=0);
  
# 1. 载入数据 读取read count数
data <-  read.delim("pnas_expression.txt", row.names=1, stringsAsFactors=FALSE);
head(data);

#输出
# lane1 lane2 lane3 lane4 lane5 lane6 lane8  len
# ENSG00000215696     0     0     0     0     0     0     0  330
# ENSG00000215700     0     0     0     0     0     0     0 2370
# ENSG00000215699     0     0     0     0     0     0     0 1842
# ENSG00000215784     0     0     0     0     0     0     0 2393
# ENSG00000212914     0     0     0     0     0     0     0  384
# ENSG00000212042     0     0     0     0     0     0     0   92
    
dim(data);
# [1] 37435     8
    
#2. 构建分组变量
#分为 Control组和DHT组  分别为4个和3个重复
targets <- data.frame(Lane = c(1:6,8), Treatment = c(rep("Control",4),rep("DHT",3)),
                        Label = c(paste("Con", 1:4, sep=""), paste("DHT", 1:3, sep="")));
    
targets

#输出
#   Lane Treatement Label
# 1    1    Control  Con1
# 2    2    Control  Con2
# 3    3    Control  Con3
# 4    4    Control  Con4
# 5    5        DHT  DHT1
# 6    6        DHT  DHT2
# 7    8        DHT  DHT3
    
#3. 创建基因表达列表 进行标准化因子计算
y <- DGEList(counts=data[,1:7], group=targets$Treatment, genes=data.frame(Length=data[,8]));
colnames(y) <- targets$Label;
dim(y);

# [1] 37435     7

#过滤表达量偏低的基因
#基因在至少3个样本中得count per million（cpm）要大于1
keep <- rowSums(cpm(y)>1) >= 3;
y <- y[keep,];
dim(y)
# [1] 16494 7
#重新计算库大小
y$samples$lib.size <- colSums(y$counts);

#3. 进行标准化因子计算 默认使用TMM方法
y <- calcNormFactors(y);

#输出
# An object of class "DGEList"
# $counts
# Con1 Con2 Con3 Con4 DHT1 DHT2 DHT3
# ENSG00000124208  478  619  628  744  483  716  240
# ENSG00000182463   27   20   27   26   48   55   24
# ENSG00000124201  180  218  293  275  373  301   88
# ENSG00000124207   76   80   85   97   80   81   37
# ENSG00000125835  132  200  200  228  280  204   52
# 16489 more rows ...

# $samples
# group lib.size norm.factors
# Con1     1   976847    1.0296636
# Con2     1  1154746    1.0372521
# Con3     1  1439393    1.0362662
# Con4     1  1482652    1.0378383
# DHT1     1  1820628    0.9537095
# DHT2     1  1831553    0.9525624
# DHT3     1   680798    0.9583181

# $genes
# [1] 2131 5453 4060 3264  945
# 16489 more rows ...

#这里主要是通过图形的方式来展示实验组与对照组样本是否能明显的分开
#以及同一组内样本是否能聚的比较近 即重复样本是否具有一致性
plotMDS(y);

#4. 估计离散度
y <- estimateCommonDisp(y, verbose=TRUE)
# Disp = 0.02002 , BCV = 0.1415 
y <- estimateTagwiseDisp(y);

plotBCV(y);

#5. 通过检验来鉴别差异表达基因
et <- exactTest(y);
top <- topTags(et);
top

#输出
# Comparison of groups:  DHT-Control 
# Length    logFC    logCPM        PValue           FDR
# ENSG00000151503   5605 5.816156  9.716866  0.000000e+00  0.000000e+00
# ENSG00000096060   4093 5.004454  9.950606  0.000000e+00  0.000000e+00
# ENSG00000166451   1556 4.683425  8.850401 2.297717e-249 1.263285e-245
# ENSG00000127954   3919 8.120955  7.216393 1.534440e-229 6.327264e-226
# ENSG00000162772   1377 3.316701  9.743797 7.975243e-216 2.630873e-212
# ENSG00000115648   2920 2.598440 11.474677 6.984860e-180 1.920138e-176
# ENSG00000116133   4286 3.244446  8.791930 1.290432e-174 3.040627e-171
# ENSG00000113594  10078 4.111120  8.055613 3.115276e-161 6.422921e-158
# ENSG00000130066    868 2.609899  9.989778 6.009018e-155 1.101253e-151
# ENSG00000116285   3076 4.201846  7.361640 6.299060e-149 1.038967e-145

​    
#6. 定义差异表达基因与基本统计
summary(de <- decideTestsDGE(et)); # 默认选取FDR = 0.05为阈值
    
#输出
# [,1] 
# -1  2094   #显著下调
# 0  12060   #没有显著差异
# 1   2340   #显著上调
    
#图形展示检验结果
detags <- rownames(y)[as.logical(de)];
plotSmear(et, de.tags=detags);
abline(h=c(-1, 1), col="blue");
```

![图1](http://www.iwhgao.com/wp-content/uploads/2014/06/1.png) ![图2](http://www.iwhgao.com/wp-content/uploads/2014/06/3.png)

#### 关于样本没有重复时该怎么做

当然最好还是有重复，实在没有重复的话，edgeR的文档也有提到，那就是选择一个认为合理的离散度的值。 通常如果是实验控制的好的人类数据，那么选择BCV=0.4，比较好的模式生物选择BCV=0.1，技术重复的话选择BCV=0.1。 这里作者举了个简单的模拟数据的例子：

```R
bcv <- 0.2
counts <- matrix( rnbinom(40,size=1/bcv^2,mu=10), 20,2)
y <- DGEList(counts=counts, group=1:2)
et <- exactTest(y, dispersion=bcv^2)
```

via：[rabbit gao's blog](http://www.iwhgao.com/?p=98)