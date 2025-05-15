---
title: '彻底解决RODBC中文乱码'
author: 波比
date: '2018-03-02'
slug: 彻底解决RODBC中文乱码
categories:
  - R语言
tags:
  - chinese encoding
  - coding erro
  - RODBC
  - R语言
  - tech
---

数据分析很多时候用到了数据库来存储数据，但从R读取或者写入数据很多时候蛋痛的是中文乱码的问题。今天算是彻底搞定了RODBC这玩儿。 

rstudio的设置 tools >> global options >>code>>saving 面板下将“default text encoding” 改为“utf8”，切记这里是“UTF8”，而不是“UTF-8”，这两是有区别的编码，至于细节暂不讨论，我只关心我这样设置就解决了中文问题。   

MSSQL链接，在Rstudio中 建立connectionstring

```R
library(RODBC)
con <- odbcDriverConnect('driver={SQL Server};server=localhost;database=数据库名;trusted_connection=true;DBMSencoding=utf8')
```

  关键在于con设置中的 “DBMSencoding=utf8”，至于这是为什么，我也不清楚，反正这么弄了下，就中文的可以写入，可以读取了，再也不会出现乱码的问题了。 当然这些设置的前提是，你在建立数据库的时候，必须要设定数据库的字符集类型为中文的。