---
title: 'R语言自动获取miRNA数据库靶基因预测数据案例'
author: 波比
date: '2018-02-12'
slug: R语言自动获取miRNA数据库靶基因预测数据案例
categories:
  - 生物信息学
tags: 
  - httr
  - miRNA预测
  - rvest
  - R爬虫
  - 生物信息学
---

最近需要用到miRNA预测相关的功能，在线的检索已经很方便了，但是多个数据库的数据获取，保存成本地文档后再读入R处理，也还是比较繁琐的，索性自己的花了几分钟时间写了个post提交的代码。下面以http://www.mirdb.org/为例说下，怎么用好R来在线获取数据。

*   网页分析 我一般用chrome浏览器，利用F12快捷键查看网页的数据类型。具体操作，chrome打开后，按F12打开工具，在浏览器打开http://www.mirdb.org/网页，然后根据自己的需要进行选择物种、输入基因或者miRNA名字，然后检索，在chrome自带的工具栏可以看到数据提交的类型POST or GET. 具体这一步骤的细节可以自己bing or 百度。重点关注下图红色框出来的部分。

![](https://ws1.sinaimg.cn/large/8f5e6680gy1foe3v0ha93j211g0mndix.jpg)

* 构造数据

    urls <- 'http://www.mirdb.org/cgi-bin/search.cgi' #request url 对应的网址

    heads <- c(
      'Accept'='text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,\*/\*;q=0.8',
      'Connection'='keep-alive',
      'Origin'='http://www.mirdb.org',
      'User-Agent'='Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36'
    )#构造浏览器访问头部数据

*   构建post数据
  
    dat <- list(
      'species'='Rat',
      'geneChoice'='symbol',
      'searchBox'='\*\*\*\*\*', #你需要检索的基因
      'submitButton'='Go',
      'searchType'='gene'
    ) # 源自上图 form data 部分，值得注意的是这里必须要用list格式
    
*   post提交，获取数据
  
    h <- handle(urls) # 这里主要是用于获取cookers，session等信息，其实这个网页不需要也没关系。
    htmlsource <- POST(url =urls,add\_headers(.headers = heads),handle = h,body=dat) #post提交获取网页
    mirdb <- htmlsource$content%>%read\_html()%>%html\_nodes("#table1") %>%html\_table(header = T)
    %>%as.data.frame()%>%filter(Target.Score>='90')# 利用rvest 从源码中挑选tabel1抓取，然后转成数据框，在进一步过滤筛选。
    
*   用到的数据包
  
    library(httr)
    library(rvest)
    library(dplyr)
    
    如果上面的代码都看懂了，那么你获取其他网页的数据也就没有多少困难了。当然，咱这个案例中，并没有完全去实现如何全自动的获取数据，但是核心的东西都在这里了，如果你觉得有需要可以自己用函数封装，在用的时候直接传参数给函数，返回对应想要的数据即可。