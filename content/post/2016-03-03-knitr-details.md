---
title: 'knitr使用说明详细介绍'
author: 波比
date: '2016-03-03'
slug: knitr使用说明详细介绍
categories:
  - R语言
tags:
  - knitr
  - Markdown
  - R语言
---

## 1.相关资料

*   1：自动化报告-谢益辉 [https://github.com/yihui/r-ninja/blob/master/11-auto-report.md](https://github.com/yihui/r-ninja/blob/master/11-auto-report.md)
*   2：knitr与可重复的统计研究（花絮篇） [http://cos.name/2012/06/reproducible-research-with-knitr/](http://cos.name/2012/06/reproducible-research-with-knitr/)
*   3：knitr官网 [http://yihui.name/knitr/](http://yihui.name/knitr/) 在官网中有谢益辉自己录制的一段英文的讲解视频

我的学习都是从以上三个资源中获取。所以特意在文件夹中把1，2转换为PDF， 希望大家仔细阅读。 文件夹中还有一个knitr-refcard.pdf,里面包括了一些常用的参数

2.环境
----

我们需要的是安装过knitr包的RStudio。然后在Tools-option-Sweave-weave Rnw files using -选择knitr **(操作演示)**,新建一个test.Rmd然后后面的内容都在其中示范。

3.1 Markdown 是什么，如何写Markdown
----------------------------

Markdown 是一种轻量级标记语言。类似HTML，但是比html简单的多，我在文件夹中放了一个markdown的语法说明。 大家具体可以用一段时间学会。我们这里知道这么几点就可以了。标题，一个#后面跟一个空格代表一级标题，2个## 后面跟一个空格代表二级标题。

*   _how are you_ 斜体
*   **how are you** 加粗

具体的语法看文件夹中的 _Markdown语法说明（简体中文版）_

3.2 在Markdown中写R代码
------------------

首先我们要新建一个Rmd的文件，或者在File-new file-R markdown，新建一个Rmd的文件。_操作_ Markdown中的R语言的代码是三个后引号（也就是在键盘左上角，Esc下面的那个键）然后后面加上{r}开始,`{r,} 大括号中，r字母后面可以加入不同的参数。然后以三个后引号`结束。当然都是在英文状态下输入的。

```R
a = 1:10  
b = 11:20
```


\##  \[1\]  1  2  3  4  5  6  7  8  9 10

3.3 是否计算代码块参数 **eval**
----------------------

是否计算代码块中内容的选项。 两个选项：TURE或者FALSE

~~~R
```{r,eval=FALSE}
> a
# 1  2  3  4  5  6  7  8  9 10
> b
# 11 12 13 14 15 16 17 18 19 20
~~~

eval是计算代码块中的内容。当eval=TRUE，计算，所以会在html中会显示代码运行的结果，反之，不计算，不显示。 还可以是**数字选项**,我们看一下下面两段代码的区别

~~~R
```{r,eval=C(1)} ```{r,eval=-c(1)} #前面加一个负号，表示排除的意思
> a
# 1  2  3  4  5  6  7  8  9 10
~~~


也就是可以用eval=c()来控制那些行代码运行，那些不运行。

3.4 文本输出相关参数
------------

**echo** 两个选项：TRUE或FALSE,或者是数字，用来控制那些行输出，那些行不输出。

```R
# ```{r,echo=FALSE} ```{r,echo=c(1)} ```{r,echo=-c(1)}
> a
#  1  2  3  4  5  6  7  8  9 10
> b
# 11 12 13 14 15 16 17 18 19 20
```

对比上面两个的结果，我们可以看出,echo是控制代码输出的，但echo=TRUE的时候，在html中是输出代码的，当echo=FALSE的时候是不输出代码的。 **warnings error message** 以上都有两个选项，也就是假如代码中有警告的信息，报错的信息，或者其他的信息，在 最后的报告中是否显示，TURE是显示，FALSE是不显示。

3.5 代码修饰参数
----------

### 3.5.1 tidy

    # ```{r,tidy=FALSE} 首先安装formatR这个包 install.packages('formatR')
    a = 100
    b = 100


这里还有一个**tidy.opts**

    # ```{r,tidy.opts=list(keep.blank.line=FALSE)}
    a = 100
    b = 100


### 3.5.2 prompt

这个就是是否显示在R默认窗口中的>, 一般来说，FALSE是默认选项。没人希望在每一行都输出>

~~~R
```{r,prompt=TRUE}
a
# [1] 100
~~~

### 3.5.3 comment

默认情况下我们生成HTML格式的文件，所有的运行结果前面都有两个##号**(演示)** 在这里我们可以通过comment选项改变,当然这里一般不用改变，没有什么意义。前面是#号，可以在我们复制代码的时候， 这里的结果不会被运行。

### 3.5.4 highlight 代码高亮选项

我没有发现什么变化，大家自己探索一下。

    # ```{r,highlight=FALSE} install.packages('highlight')
    data = c("1", "2")
    for (i in a) {
        c = i
    }


3.6 缓存参数 cache
--------------

cache是代码块计算得到的缓存，可以是True也可以是False。默认为False，也就是每一次生成knit HTML，都会重新计算里面的每一个代码块。但是如果我们的代码非常的复杂。如果我们不希望每一次都重新运行，所有的代码块，那么我们可以设置cache为TRUE。 也就是，当代码块第一次运行的时候会把结果保存下来，然后当我们生成HTML的时候，就不用再重新计算，而是直接把前面保存的结果哪里，当程序复杂度高的时候，这个会节约 一定的时间。

3.7 Plot 作图参数
-------------

### 3.7.1 **fig.width fig.height**

```R
# ```{r,prompt=TRUE}
a
## [1] 100
```

### 3.7.2 **fig.show** = 'asis' 'hold' 'animate' 'hide'

*   asis：表示在哪里生成就在哪里显示
*   hold:的意思是把图片放到代码块以后
*   animate: 如果代码块中有生成几幅图片，可以将几张图片生成动画。

用animate的要点是首先要安装：animation包。

 然后[http://ffmpeg.zeranoe.com/builds/](http://ffmpeg.zeranoe.com/builds/) 从这里下载你需要的ffmpeg，然后直接解压到当前文件夹，比如我解压以后，重命名为ffmpeg,然后放到c:\\Program Files\\ 然后把C:\\Program Files\\ffmpeg\\bin放入环境变量。 然后这个选项就可以用了。 

1: 由于ffmpeg的最新版本出来的动画，不一定在所有的浏览器都能播放，所以我在文件中放了一个ffmpeg.exe 这个是11年的版本， 用起来没有问题。 

2：还有一个问题就是，如果你安装过其他的软件，里面自带的ffmpeg.exe，那么可能你的路径中的ffmpeg就不起作用了，查看ffmpeg位置 的命令行：Sys.which(“ffmpeg”)，这样可以查看当前使用的ffmpeg是在什么地方。 

3：然后提供一个最简单的办法，直接讲我的文件夹中的ffmpeg扔到system32文件夹中就没有任何问题了，也不用配置路径了。 

4:关于并行图片生成的顺序，非循环方式生成图片顺序问题，待验证 当生成动画的时候还有一个参数interval=2 可以设置两个图片之间的间隔。

 所有参数都在网站有详细的解释，我也有可以有理解不对的地方，欢迎大家指正。谢谢。 [http://yihui.name/knitr/options](http://yihui.name/knitr/options)

4.行内代码
------

我们如果想在行文中直接嵌入R的代码也非常的方便，是以后引号加r开头，然后后引号结尾。比如**400**。 非常的方便。

5.生成HTML
--------

6.写Latex
--------

7.致谢
----

最应该感谢的当然是包的制作者-谢益辉，然后RStudio也是很不错的。 同时，本文还参考了魏太云的PPT,同样非常感谢。