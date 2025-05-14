---
title: 'xaringan包rmarkdown制作幻灯片的神器'
author: 波比
date: '2017-04-04'
slug: xaringan-for-slides
categories:
  - R语言
tags: 
  - markdown写作
  - xaringan包
  - tech
---

Rstudio最近出来一些包简直要逆天了，这完全已经不再局限于IDE这样的工具了。Rstudio已经成为了我日常工作中的工具，写代码、画图、写文档、制作幻灯片，写博客。

# 安装

```R
devtools::install_github('yihui/xaringan')
```

开始slide之旅
=========

利用xaringan制作slide步骤：File -> New File -> R Markdown -> From Template -> Ninja Presentation xaringan的核心主要是两个：1.Knit；2. remark.js 最牛逼的就是所有的样式修改是通过CSS来配置的，在rmarkdown头部还可以使用YAML定义。

细节
==

头部的title、author使用yaml就定义完了，单张幻灯通过"---"分割开，如果要实现单页幻灯内的延迟加载可以使用“--”进行分割。例如  

更酷的是，通过快捷键P，直接可以切换到演讲者模式，可以将备注内容通过三个问号注释"???" 看看源码啥样：

当然，还有更多黑科技.

详细见益辉的博客[https://github.com/yihui/xaringan/wiki](https://github.com/yihui/xaringan/wiki)，

完整的xaringan演示 [Demo](https://slides.yihui.name/xaringan/#1)