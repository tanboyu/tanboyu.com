---
title: TCGA数据分析工具
author: 波比
date: '2018-12-14'
slug: TCGA数据分析工具
categories:
  - 生物信息学
tags:
  - TCGA
  - 生物信息学
---

<img src="https://pbs.twimg.com/media/CZHENHjWQAQGTaf.jpg",style="max-width:65%" alt="TCGA" />

今天看文献的时候恰好浏览看到一篇文献，[CrossHub: a tool for multi-way analysis of The Cancer Genome Atlas (TCGA) in the context of gene expression regulation mechanisms](https://academic.oup.com/nar/article/44/7/e62/2467801),大致看了下这个软件，感觉很不错。CrossHub可以用于预测或验证的结合位点的mRNA-miRNA对（TargetScan，mirSVR，PicTar，DIANA microT，miRTarBase）和强阴性表达的情况，改天好好研究下。

![](https://ws1.sinaimg.cn/large/8f5e6680gy1fy6696hdc4j225f1htnpf.jpg)
CrossHub workflow. Complementation of ENCODE ChIP-Seq data and Jaspar predictions with TCGA expression correlation analysis allows the user to outline interactions with potential functional impacts to a specific cancer subtype. Similarly, combining miRNA target predictions with gene–miRNA expression correlation profiling (based on TCGA expression data) highlights gene–miRNA interactions, which likely take place for a particular tumor type. Expression-methylation correlation analysis allow identification of hypermethylated CpG sites or regions within promoters or enhancers (annotated with ENCODE) having the greatest potential impact on gene expression. In addition, CrossHub enables conventional differential expression (DE) and methylation analysis.

