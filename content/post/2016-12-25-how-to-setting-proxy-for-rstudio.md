---
title: 'how to setting proxy for Rstudio'
author: 波比
date: '2016-12-25'
slug: how to setting proxy for Rstudio
categories:
  - 网络技术
tags:
  - httr
  - rstudio
  - proxy
  - R语言
---

很多时候，因为各种因素，可能你需要的网络数据无法访问，但是需要proxy的时候，*R*这个*httr* 包就能帮你搞定。

```R
install.packages("httr")

library(httr)
with_config(use_proxy())
```

这真是一个神器的包，可以修改use\_proxy, user\_agent()，关键的是httr可以用于写爬虫，慢慢挖掘它的好，比RCURL好用。