---
title: Rselenium使用摘要
author: 波叔
date: '2021-06-16'
slug: rselenium使用摘要
categories:
  - 网络技术
tags:
  - 爬虫
  - 自动化
draft: yes
mathjax: no
type: post
---

大概5年前稍稍研究过Rselenium这个包，写过几行代码，至今有很长时间没有用了。偶尔需要用httr或者revst也能搞定。

最近因为需要批量完成选题，又祭出Rselenium折腾了一下。顺便把部分基础的代码贴着供大家参考。

```r
library(RSelenium)
library(httr)
library(magrittr)
library(rvest)
library(stringr)
library(tidyverse)
library(purrr)


# 路径需要根据各自情况修改
system("java -Dwebdriver.chrome.driver=\"D:/C-Hub/chromedriver.exe\" -jar \"D:/C-Hub/selenium-server-standalone-3.9.1.jar\"",wait = FALSE,invisible = FALSE)

remDr <- remoteDriver(remoteServerAddr = "localhost" 
                      , port = 4444
                      , browserName ="chrome")

url <- "http://xxx***xxx.com"

remDr$open(silent = T)
remDr$navigate(url = url)

```

  需要注意的是，国内可能无法访问selenium API的网址无法自动下载程序文件，需事先配置好“chromedriver”、“selenium-server-standalone”两个文件。我这边是直接丢到项目根文件夹下。
  
  如果没有特殊情况的话，运行上面的代码可以看到程序会打开chrome浏览器开启调试状态，并跳转到指定的URL。后续的工作要怎么做需要根据各自的需求而写代码。简单来说就是：
  
  - 根据CSS 或者 XPath定位网页位置，获取所需的信息（文字、链接、图片等）。

```r
  PageSource = remDr$getPageSource() %>% unlist() %>% read_html() %>% html_nodes(xpath = "//*[@id=\"answer\"]") %>% html_text()

```

  - 根据CSS 或者 XPath定位网页位置，模拟鼠标行为。
  
```r
  webElem <- remDr$findElement(using = "xpath", value = "/html/body/div[1]/div/div[3]/div[1]/div[6]/a[2]")
  webElem$clickElement()
```
  
  - 完成工作需要将页面关闭
  
```r 

  remDr$close()

```

  对于常规的页面来说，还是比较简单的。国内对于爬虫还是比较敏感，鉴于安全考虑，整个源码就不贴出来了，只是从技术从面探讨Rselenium使用的思路。
