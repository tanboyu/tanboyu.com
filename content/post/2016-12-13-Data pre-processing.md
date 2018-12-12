---
title: 'Data pre-processing'
author: 波比
date: '2016-12-13'
slug: Data pre-processing
categories:
  - 数据分析
tags:
  - centering
  - data pre-processing
  - pareto scala
  - 代谢组学
  - 数据预处理
  - 标化数据
  - 生物信息学
  - 统计学方法
---

##### Center data

calculate the average of each variable (column) and substract it from each value.

![](http://www.tanboyu.com/wp-content/uploads/2016/12/img_58500681d2f4a.png)

#####  Unit variance (UV) scaling

*   if variables are measured in different units, data are scaled to give each variable equal chance to influence  the model.
*   divide each variable by its standard deviation, variance of scaled variables =1

![](http://www.tanboyu.com/wp-content/uploads/2016/12/img_5850063a327a3.png)

##### Pareto(PAR) scaling

What happens if big features dominate, but we know medium features are also important?

 Ctr (mean-centering only)

 -- RISK: Medium peaks masked by large peaks

UV (mean-centering and unit variance)

\-- RISK: Baseline noise may be inflated

##### The alternative is Pareto scaling

*   Divide each variable by the square root of its SD
*   Intermediate between no scaling (Ctr) and UV
*   Weights up medium features without inflating baseline noise
*   Recommended option (NMR & MS metabonomics, Gene chip & proteomics data)

![](http://www.tanboyu.com/wp-content/uploads/2016/12/img_585007513c5de.png)