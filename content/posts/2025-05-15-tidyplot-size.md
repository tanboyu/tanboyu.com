---
title: Tidyplot绘图调整画布
author:
  - Bobby
date: '2025-05-15'
slug: tidyplot_size
categories:
  - R语言
tags:
  - R语言
  - tidyplot
  - tech
lastmod: '2025-05-15T23:34:00+08:00'
keywords:
  - word 1
  - word 2
description: 'tidyplots 中的默认绘图区域大小为 50 毫米 x 50 毫米。默认设置下图不是很好看，而且可能很难看清。有几种方法可以解决这个问题。'
summary: 'tidyplots 中的默认绘图区域大小为 50 毫米 x 50 毫米。默认设置下图不是很好看，而且可能很难看清。有几种方法可以解决这个问题。'
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
  image: ''
  caption: ''
  alt: ''
  relative: no
---

与 ggplot2 不同，tidyplots 使用毫米 (mm) 的绝对尺寸来表示绘图区域大小。绘图区域是两个轴所包围的区域。这意味着刻度、标题、说明和图例不计入绘图区域大小。

tidyplots 中的默认绘图区域大小为 50 毫米 x 50 毫米。根据您的设置，这可能很难看清。有几种方法可以解决这个问题。

在 RStudio 中，您可以增加缩放比例来放大图表。View> Zoom In。或者Ctrl+ +。但是，这也会增加 RStudio 中的字体大小。可以通过tidyplot()或adjust_size()函数提供替代方案width，height（以毫米为单位）来增加 tidyplot 的绝对尺寸。

```r
# 1.using the tidyplot() function

study %>%
  tidyplot(x = treatment, y = score, color = treatment, width = 80, height = 80) %>%
  add_mean_dash() %>%
  add_sem_errorbar() %>%
  add_data_points_beeswarm()

# 2.using the adjust_size() function

study %>%
  tidyplot(x = treatment, y = score, color = treatment) %>%
  add_mean_dash() %>%
  add_sem_errorbar() %>%
  add_data_points_beeswarm() %>% 
  adjust_size(80, 80) # 调整size
  

```