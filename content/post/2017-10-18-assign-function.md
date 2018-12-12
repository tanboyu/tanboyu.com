---
title: 'R语言-assign函数的应用'
author: 波比
date: '2017-10-18'
slug: R语言-assign函数的应用
categories:
  - R语言
tags:
  - assign函数
  - R语言
---

在学习R语言的时候，可能在书本或者教程中你可能看过assign这个函数，或许你当初会觉得这个可能不知道怎么用，没怎么关注吧。这个函数的功能最好的用处就是循环中给变量赋值，比如你想自动生成一批变量，这个时候assign就有价值了。例如：

for (i in 1:5){  
  assign(paste("var", i, sep = ""), i:10)  
}

ls() # 查看变量名

# 系统自动就生成了5个变量
\[1\] "i"     "var1" "var2" "var3" "var4" "var5"

>var1
\[1\] 1 2 3 4 5 6 7 8 9 10

是不非常有意义了。