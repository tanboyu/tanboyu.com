---
title: 根据最佳证据优化新生儿万古霉素暴露
author:
  - 波叔
date: '2025-05-17'
slug: vancomycin_poppk
categories:
  - 定量药理学
tags:
  - tech
  - 定量药理学
lastmod: '2025-05-17T15:03:20+08:00'
keywords:
  - word 1
  - word 2
description: ''
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
  image: 'https://ars.els-cdn.com/content/image/1-s2.0-S1043661819302178-ga1_lrg.jpg'
  caption: ''
  alt: ''
  relative: no
---

这是一篇2019年的研究，基于大型新生儿队列的数据开发了新生儿万古霉素的群体药动学模型，同时还与已发表的多个新生儿万古霉素群体药动学模型进行了性能比较。对于开展新生儿万古霉素精准给药，这是一篇值得精读的文章。

**Optimisation of vancomycin exposure in neonates based on the best level of evidence**

Kim Dao, Monia Guidi,Pascal André, Eric Giannoni, Sébastien Basterrechea, Wei Zhao, Aline Fuchs, Marc Pfister, Thierry Buclin, Chantal Csajka

[full-text](https://doi.org/10.1016/j.phrs.2019.104278)


万古霉素是治疗新生儿感染的重要抗生素，但其在足月或早产新生儿中的最佳剂量尚未达成共识。现有的剂量推荐通常基于年龄、肾功能和/或体重，但缺乏前瞻性验证。本研究在大型新生儿队列中开发万古霉素的综合群体 PK 模型，使用新开发的模型模拟评估、比较当前给药方法在目标实现方面的性能。


### 研究背景与模型开发

研究回顾了22个新生儿万古霉素PK模型，并基于洛桑大学医院10年数据（2006-2016年）开发了一个新的模型。该模型利用NONMEM®（版本7.3）进行非线性混合效应建模，测试了一室和二室模型，考虑体重（WT）、血清肌酐（SCr）、胎龄（GA）、出生后时间（PNA）和胎龄后时间（PMA）等协变量。

### 排除的对比模型
17个现有模型因以下原因被排除：

- 仅针对早产儿（7个模型）、较大儿童（3个模型）或持续输注人群（2个模型）。
- 涉及体外膜肺氧合（ECMO）患者（1个模型）。
- 仅使用体重作为协变量（1个模型）或缺乏详细描述（5个模型）。
- 采用异常建模方法（如在协变量分析后添加个体间变异，1个模型）。
- 这些排除反映了现有模型在人群覆盖、数据完整性或方法论上的局限性，与本研究的大样本、多变量分析形成对比。


### 新模型特点

在从 409 名患者收集的 1947 个万古霉素浓度中，排除了 116 份样本和 4 名患者。因此，共有 1831 个万古霉素浓度和 405 名患者被纳入分析以开发群体 PK 模型。

研究分析了1831个万古霉素浓度（平均每例4.5个），平均胎龄29周（18%足月，82%早产），中位剂量为13.7 mg/kg（范围2.0-222.2 mg/kg）。

**新开发的模型相关参数**

| **Parameter** | **Estimate** | **RSE (%)** | **BSV (%)** | **RSE (%) (Bootstrap)** | **Bootstrap Estimate** | **CI95% (Bootstrap)** | **BSV (%) (Bootstrap)** | **CI95% (Bootstrap, BSV)** |
|---------------|--------------|-------------|-------------|--------------------------|------------------------|-----------------------|--------------------------|-----------------------------|
| CL (L/h)      | 0.273        | 17          | 22.6        | 8                        | 0.271                  | (0.20; 0.38)          | 22.2                     | (18.7; 26.0)               |
| V (L)         | 0.628        | 2           | -           | -                        | 0.628                  | (0.61; 0.65)          | -                        | -                          |
| θWT           | 0.438        | 18          | -           | -                        | 0.442                  | (0.28; 0.58)          | -                        | -                          |
| T50           | 46.4         | -           | -           | -                        | 46.4                   | -                     | -                        | -                          |
| Hill          | 3.54         | 14          | -           | -                        | 3.52                   | (2.58; 4.44)          | -                        | -                          |
| θSCr          | 0.473        | 15          | -           | -                        | 0.47                   | (0.34; 0.61)          | -                        | -                          |
| σprop (CV%)   | 0.236        | 6           | -           | -                        | 0.23                   | (0.21; 0.26)          | -                        | -                          |
| σadd (CV%)    | 1.98         | 12          | -           | -                        | 1.97                   | (1.38; 2.39)          | -                        | -                          |

**Abbreviations and Definitions**
- **CL**: Clearance (L/h)  
- **V**: Volume of distribution (L) for a patient of 1.0 kg, the rounded mean population body weight (WTmedian)  
- **σprop**: Exponential residual error (CV%)  
- **σadd**: Additive residual error (CV%)  
- **θWT**: Effect of body weight, expressed as (WT/WTmedian)<sup>θWT</sup>  
- **θSCr**: Effect of serum creatinine (SCr), expressed as (SCrmedian/SCr)<sup>θSCr</sup>, where SCrmedian = 45 µmol/L  
- **T50**: Postmenstrual age (PMA) when 50% of maturation of CL has been reached  
- **Hill**: Slope of the sigmoid maturation function, MF = (PMA<sup>Hill</sup>)/(PMA<sup>Hill</sup> + T50<sup>Hill</sup>)  
- **RSE**: Relative standard error of the estimate (SE estimate/estimate, expressed as a percentage)  
- **BSV**: Between-subject variability (%)  
- **CI95%**: 95% confidence interval  

**最终模型:**

<math><mtext is="true">C</mtext><mtext is="true">L</mtext><mo is="true">=</mo><msub is="true"><mtext is="true">θ</mtext><mrow is="true"><mtext is="true">C</mtext><mtext is="true">L</mtext></mrow></msub><mo is="true">·</mo><msup is="true"><mrow is="true"><mfenced close=")" open="(" is="true"><mrow is="true"><mfrac is="true"><mrow is="true"><mtext is="true">W</mtext><mtext is="true">T</mtext></mrow><mrow is="true"><msub is="true"><mrow is="true"><mtext is="true">W</mtext><mtext is="true">T</mtext></mrow><mrow is="true"><mtext is="true">m</mtext><mtext is="true">e</mtext><mtext is="true">d</mtext><mtext is="true">i</mtext><mtext is="true">a</mtext><mtext is="true">n</mtext></mrow></msub></mrow></mfrac></mrow></mfenced></mrow><mrow is="true"><msub is="true"><mtext is="true">θ</mtext><mrow is="true"><mtext is="true">W</mtext><mtext is="true">T</mtext></mrow></msub></mrow></msup><mo is="true">·</mo><mfenced close="]" open="[" is="true"><mrow is="true"><msup is="true"><mrow is="true"><mfenced close=")" open="(" is="true"><mrow is="true"><mfrac is="true"><mrow is="true"><msub is="true"><mrow is="true"><mtext is="true">S</mtext><mtext is="true">C</mtext><mtext is="true">r</mtext></mrow><mrow is="true"><mtext is="true">m</mtext><mtext is="true">e</mtext><mtext is="true">d</mtext><mtext is="true">i</mtext><mtext is="true">a</mtext><mtext is="true">n</mtext></mrow></msub></mrow><mrow is="true"><mtext is="true">S</mtext><mtext is="true">C</mtext><mtext is="true">r</mtext></mrow></mfrac></mrow></mfenced></mrow><mrow is="true"><msub is="true"><mtext is="true">θ</mtext><mrow is="true"><mtext is="true">S</mtext><mtext is="true">C</mtext><mtext is="true">r</mtext></mrow></msub></mrow></msup></mrow></mfenced><mo is="true">·</mo><mfenced close="]" open="[" is="true"><mrow is="true"><mfenced close=")" open="(" is="true"><mrow is="true"><mfrac is="true"><mrow is="true"><msup is="true"><mrow is="true"><mtext is="true">P</mtext><mtext is="true">M</mtext><mtext is="true">A</mtext></mrow><mrow is="true"><mtext is="true">H</mtext><mtext is="true">i</mtext><mtext is="true">l</mtext><mtext is="true">l</mtext></mrow></msup></mrow><mrow is="true"><msup is="true"><mrow is="true"><mtext is="true">P</mtext><mtext is="true">M</mtext><mtext is="true">A</mtext></mrow><mrow is="true"><mtext is="true">H</mtext><mtext is="true">i</mtext><mtext is="true">l</mtext><mtext is="true">l</mtext></mrow></msup><mo is="true">+</mo><mtext is="true"></mtext><msup is="true"><mrow is="true"><mtext is="true">T</mtext><mn is="true">50</mn></mrow><mrow is="true"><mtext is="true">H</mtext><mtext is="true">i</mtext><mtext is="true">l</mtext><mtext is="true">l</mtext></mrow></msup></mrow></mfrac></mrow></mfenced></mrow></mfenced></math>


<math><msub is="true"><mrow is="true"><mtext is="true">V</mtext><mo is="true">=</mo><mtext is="true">θ</mtext></mrow><mtext is="true">V</mtext></msub><mo is="true">·</mo><msup is="true"><mrow is="true"><mfenced close=")" open="(" is="true"><mrow is="true"><mfrac is="true"><mrow is="true"><mtext is="true">W</mtext><mtext is="true">T</mtext></mrow><mrow is="true"><msub is="true"><mrow is="true"><mtext is="true">W</mtext><mtext is="true">T</mtext></mrow><mrow is="true"><mtext is="true">m</mtext><mtext is="true">e</mtext><mtext is="true">d</mtext><mtext is="true">i</mtext><mtext is="true">a</mtext><mtext is="true">n</mtext></mrow></msub></mrow></mfrac></mrow></mfenced></mrow><mn is="true">1</mn></msup></math>

即某患者体重2.0 kg，PMA 34周，SCr 40 μmol/L，万古霉素CL为1.78 L/min。例如，体重增加1 kg对CL的影响为：+0.34 L/min（CL：2.12 L/min），PMA增加1周对CL的影响为：+0.14 L/min（CL：1.91 L/min），SCr增加50 μmol/L对CL的影响为：-0.57 L/min（CL：1.21 L/min）。




### 剂量方案性能评估

通过模拟评估了20种剂量方案的AUC<sub>24</sub>（400-700 mg·h/L）和C<sub>min</sub>（10-20 mg/L）靶点达标率：

- 低剂量方案表现不佳，尤其在对抗耐甲氧西林金黄色葡萄球菌（MR-CoNS）中心，靶点达成率低，建议弃用。
- 复杂分层方案（多层次剂量调整）因临床操作难度大，实用性有限。
- NNF7或Neofax高剂量方案（含加载剂量）显著改善早期暴露，适合重症感染。
- SmPC方案在早期靶点达成率上表现接近，值得推荐。
- 模型驱动方法允许首次剂量个体化，结合第四剂前的治疗药物监测（TDM），可应对残余变异性。

## 洞察


### 新模型与已发布模型的参数比较

| Iterms | **Grimsley (n=59)** | **Mehrorta (n=134)** | **Capparelli (n=374)** | **Frymoyer (n=249)** | **Kimura (n=19)** | **Our model (n=405)** |
| --- | --- | --- | --- | --- | --- | --- |
| **Postnatal age (weeks)** | 4.1 | 3.8 | 3.9 | 2.7 | - | 2.4 |
| **Gestational age (weeks)** | 29 | 32.7 | 33.5 | 34 | 24.1-41.3 | 29 (24 - 42) |
| **Postmenstrual age (weeks)** | - | 36.5 | - | 39 | 27.1-50.4 | 32 (24 - 65) |
| **Birth weight (kg)** | - | - | - | 2 | - | 1.05 |
| **Weight (kg)** | 1.52 | 2.5 | 2 | 2.9 | 0.71-5.2 | 1.1 |
| **Median SCr** | 49 (µmol/L) | 0.6 (mg/dL) | 0.7 (mg/dL) | 35.4 (µmol/L) | 0.2-0.9 (mg/dL) | 54 (µmol/L) |
| **Covariates on CL** | WT，SCr (µmol/L) | WT, PMA，SCr (mg/dL) | WT，SCr (mg/dL) | WT, PMA，Scr (mg/dL) | WT，SCr (mg/dL) | WT, PMA，SCr (µmol/L) |
| **Covariates on V** | WT |  |  |  |  |  |
| **Model for CL** | θ1 · WT / SCr | CL = θ1 · (WT/2.5)0.75 · (0.42/SCr)θ2 · (PMA/37)θ3 | CL = WT · (θ1/SCr + θ2 · PNA · [if SCr < 0.7]  + θ3 · [if GA > 28] ) | CL = θ1 · (WT/2.9)0.75 · MF · (1/SCr)θ2 
MF = 1/(1+[PMA/T50]-Hill) | PMA ≥ 36 : CL = θ1 · WT / SCr   
PMA < 36 : CL = θ2 · WT / SCr | CL = θ1 · (WT/1000)θ2 · (SCr/60)θ3 · MF
MF = PMAHill / (PMAHill + T50Hill) |
| **Model for V** | θ2 · WT | V = θ4 · WT / 2.5 | V1 = θ4 · WT + θ5 | V = θ3 · (WT/2.9) | V = θ3 · WT | V = θ4 · WT |
| **Model for Q** |  |  | Q = θ6 * WT |  |  |  |
| **Model for V2** |  |  | V2 = θ7 * V1 |  |  |  |
| **Final PK parameters** | θ1 = 3.56
θ2 = 0.669 | θ1 = 0.18
θ2 = 0.7
θ3 = 1.4
θ4 = 1.7 | θ1 = 0.028
θ2 = 0.000127 
θ3 = 0.0123 
θ4 = 0.793
θ5 = 0.01
θ6 = 0.0334
θ7 = 0.666 | θ1 = 0.345
θ2 = 0.267
θ3 = 1.75
Hill = 4.53
T50 = 34.8 | θ1 = 0.0344
θ2 = 0.025
θ3 = 0.61 | θ1 = 0.268
θ2 = 0.438
θ3 = 0.483
θ4 = 0.629
Hill = 3.57
T50 = 46 |
| **Residual error (type & value)** | add: 4.53 | add: 5 | prop: 14%, add : 3,4 | prop: 20.5%, add: 1.3 | add: 3.38 | prop: 22.8%, add: 2.2 |
| **IIV CL, V values (%)** | 22 | 25.3 | 32 | 21.6 | 25.8 | 22.6 |
| **External validation** | - | - | Validation group (n = 67) | Validation group (n=243) | - | - |
| **Mean prediction error (MPE)** | -0.29 (IC95%:-0.20;-0.38) | 0.01 (IC95%: -0.03; 0.04) | -0.10 (IC95%: -0.04;-0.17) | -0.05 (IC95%: -0.10;-0.01) | 0.06 (IC95%: 0.00;0.13) | 0.01 (IC95%: -0.05;0.07) |
| **Root mean squared error (precision)** | 0.563 (76%) | 0.178 (19%) | 0.369 (45%) | 0.257 (29%) | 0.339 (40%) | 0.315 (37%) |



作者团队还提供了一个excel表格构建的[新生儿万古霉素剂量计算器](https://view.officeapps.live.com/op/view.aspx?src=https%3A%2F%2Fars.els-cdn.com%2Fcontent%2Fimage%2F1-s2.0-S1043661819302178-mmc2.xlsx&wdOrigin=BROWSELINK)，可以参考学习。

