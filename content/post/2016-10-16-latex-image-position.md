---
title: 'Latex图片位置控制设置'
author: 波比
date: '2016-10-16'
slug: Latex图片位置控制设置
categories:
  - 科研
tags:
  - latex
  - markdown写作
  - 图片位置
---

LaTeX 控制图片的位置，就是加感叹号来忽略“美学”标准。

 ```latex
\begin{figure}[!htb] \usepackage{float} \begin{figure}[H] # 插到你代码相应的位置
 ```

1. 插入并列的子图 

```latex
\usepackage{subfigure} \begin{figure}[H] \centering \subfigure[SubfigureCaption]{ \label{Fig.sub.1} \includegraphics[width=0.4\textwidth]{figurename.eps}} \subfigure[SubfigureCaption]{ \label{Fig.sub.2} \includegraphics[width=0.4\textwidth]{figurename.eps}} \caption{MainfigureCaption} \label{Fig.lable} \end{figure} 
```

2. 控制图片位置

如果不喜欢让Latex自动安排图片位置，可以使用float包，然后用

```latex
 \begin{figure}[H] \usepackage{float}  #插入jpg图片
```

 在命令行环境下，使用命令： 

```latex
ebb figure.jpg  #生成bounding box文件figure.bb
```

 使用如下命令：

```latex
 \includegraphics[width=0.8\textwidth]{figure.jpg} 
```

可以使用Pdf Texify直接编译成pdf文件。 

插入bmp图片 还没有找到直接插入bmp图片的方法。现在的方法是，使用gimp将bmp转换成jpg，然后按上述方法插入。转换时不要使用windows自带的painter，图片质量损失太多。

用gimp或fastone image viewer，将jpg质量选为最高，转换之后得到的图片质量较好。 

同时插入jpg和eps图片 插入的命令不变。

编译时使用Latex, dvi2pdf，两种格式的图片都可以显示。 

插入eps图片 使用

```latex
\includegraphics[选项]{文件} 
```

命令可以插入eps图片。 



下面是一个最简单的例子： 

```latex
\documentclass{article}
\usepackage{graphicx} # 使用graphicx包 
\begin{document} \includegraphics{file.eps} #插入图片,按图片原尺寸插入 \end{document} 
```

注意： 

- eps文件和tex文件放在同一个文件夹，只用文件名就可以调用，不用写路径。 

- 编译时不能使用pdflatex，会出错。即使不出错，也看不到图。

- 应使用latex编译生成dvi，然后dvi2ps, ps2pdf就可以看到图了。 

- 使用\[选项\]可以指定图片大小：

```latex
\includegraphics[width=3in]{file.eps} #设定图片宽度为3 inches，图片高度会自动缩放。 
\includegraphics[width=\testwidth]{file.eps} #设定图片宽度为文本宽度。 \includegraphics[width=0.8\textwidth]{file.eps} #设定图片宽度为文本宽度的0.8倍 \includegraphics[width=\testwidth-2.0in]{file.eps} #设定图片宽度比文本宽度少2 inches。 
# 使用[选项]指定图片旋转角度： 
\includegraphics[angle=270]{file.eps} #将图片旋转270度。 
#两个选项同时使用，中间用逗号隔开： 
\includegraphics[width=\testwidth, angle=270]{file.eps}
```

