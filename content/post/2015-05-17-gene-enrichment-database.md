---
title: '组学富集分析，如何选择工具？'
author: 波比
date: '2015-05-17'
slug: 组学富集分析，如何选择工具
categories:
  - 生物信息学
tags:
  - DAVID
  - goterm
  - KEGG
  - KOBAS
  - OmicsBean
  - 富集分析
---

研究背景：
-----

功能富集分析是挖掘基因或蛋白数据背后生物学意义的主要方法，很多的生物信息学软件都实现了这样的功能，比如DAVID，KOBAS，OmicsBean等。不同的软件有不同的特点和优势，但基本的思路都是将基因关联到生物学注释比如：GO Term，KEGG Pathway等，之后从统计的层面，在数千个关联的注释中，标识出比较显著的生物通路或信息。 这样做目的都是通过分子的功能富集从更高的层次比如细胞水平来解释生物实验中的现象，比如不同的药物处理引起肝细胞中某些基因表达发生变化，不同的环境引起植物叶片中某些蛋白表达发生变化等。从系统生物学角度讲，大部分上下调的基因会涉及到特定的生物过程，具有更丰富的生物学意义。下面是对大鼠的一些基因用三个不同软件进行富集分析，通过对比富集结果对三个软件进行比较分析。

**研究方法：**
---------

DAVID(The Database for Annotation，Visualization and Integrated Discovery)系统([http://david.abcc.ncifcrf.gov/](http://david.abcc.ncifcrf.gov/))主要针对上游的生物数据分析产生的差异基因或重要基因列表提供生物功能注释信息。DAVID系统的核心工具是ID转换，功能注释，功能分类，最大的特点是注释种类较多，而且将文件上传之后可以对文件中基因列表用各个工具进行分析，这是比较方便的。 ![](http://www.ebiotrade.com/imagewatermark/UploadFile/2015071417291472.GIF) 

图为 DAVID 进行基因ID转换后的列表，将用户的基因ID转换为基因名字，给研究者更加直观的印象。 ![](http://www.ebiotrade.com/imagewatermark/UploadFile/2015071417292675.JPG) 

图为DAVID富集列表，第一列为注释种类，包括GO注释，KEGG注释，蛋白相互作用，生物代谢通路等，研究者可以对基因列表进行更全面的生物信息注释。 ![](http://www.ebiotrade.com/imagewatermark/UploadFile/2015071417294644.GIF) 

图为DAVID对每个检索基因富集的结果总表，将检索的每个基因在选定数据库中的注释以表格形式呈现。 ![](http://www.ebiotrade.com/imagewatermark/UploadFile/2015071417300840.JPG) 

图为DAVID富集功能信息的簇分析，Terms被分成多组，并给出聚类的分值,分值越高，代表该组内的基因在基因列表中越重要。 KOBAS(KEGG Orthology Based Annotation System)系统([http://kobas.cbi](http://kobas.cbi/). pku.edu.cn/home.d) 是一个基于KEGG Ortholog的注释系统，主要运用常用的通路和疾病知识库(包括Gene Ontology，KEGG Pathway)对相关的基因或蛋白质进行功能注释。从系统生物学角度讲，同源基因在不同的物种里会参与类似的通路，KOBAS在系统中整合了Blast的功能，实现了跨物种的基因注释，但序列比对会比较耗时。 ![](http://www.ebiotrade.com/imagewatermark/UploadFile/2015071417303181.GIF) 

KOBAS软件的功能模块中首先是ID Mapping，图为将检索的基因ID通过Blast或直接映射到相应的基因名字，接下来会将这些基因通过数据库富集到相应的Pathway， Go term，Disease。 ![](http://www.ebiotrade.com/imagewatermark/UploadFile/2015071417304468.GIF) 

图为KOBAS软件将每个检索基因富集到的KEGG Pathway 并给出相应的P-Value值。 ![](http://www.ebiotrade.com/imagewatermark/UploadFile/2015071417305570.GIF) 

图为KOBAS软件将每个检索基因富集到GO ontology，每个Go term 会有相应的P-Value值。 OmicsBean ([http://www.omicsbean.com:88/](http://www.omicsbean.com:88/) )是一款针对不同实验设计的组学数据分析系统，整合了GO ontology，KEGG，String等数据库，更侧重输出结果的图表设计，相比富集分析系统增加了数据分析和分子互作模型构建等功能模块。 ![](http://www.ebiotrade.com/imagewatermark/UploadFile/2015071417312689.GIF) 

图为OmicsBean 对检索基因的富集分析结果，包括Go富集，KEGG富集。GO富集包括Biological Process, Cell Component, MolecularFunction. ![](http://www.ebiotrade.com/imagewatermark/UploadFile/2015071417313944.GIF) 

图为OmicsBean对检索基因的GO富集结果，不同的检索基因富集到同一个GO Term,并有相应的热图表示。 ![](http://www.ebiotrade.com/imagewatermark/UploadFile/2015071417314932.GIF) 

图为OmicsBean 对检索基因的KEGG富集结果，每条KEGG通路都有相应的P-Value值。 ![](http://www.ebiotrade.com/imagewatermark/UploadFile/2015071417320067.GIF) 

图为OmicsBean对检索基因构建的分子互作网络调控模型，分子只有相互作用才能实现分子功能。

**小结：**
-------

DAVID，KOBAS，OmicsBean系统都提供富集结果文件的下载，满足科研用户对结果数据进行分析作图发表文章。 从数据库层面上讲， DAVID注释范围从最初的GO注释，扩展到现在超过40个注释种类，包括GO注释，KEGG注释，蛋白相互作用，蛋白功能区域，疾病相关，生物代谢通路，序列特点，异构体，基因功能总结，基因在组织里的表达和论文等。KOBAS 整合了 Gene Ontology，OMIM, KEGG DISEASE, FunDO, GAD，NHGRI GWAS Catalog 疾病数据库， PATHWAY, PID, BioCarta (from PID), Reactome, BioCyc等通路数据库。OmicsBean整合了KEGG, Gene Ontology,String数据库。 从功能模块层面讲，DAVID除了富集功能外还有功能信息的簇分析，将功能信息进行分组，并通过打分标识其在功能列表中的显著性。KOBAS除了富集功能外，将Blast整合进系统中，可以针对序列Fasta文件进行富集分析，实现了跨物种的富集分析而且还整合了疾病的多个数据库。OmicsBean可以对多组学的多种实验设计数据进行分析，包括PCA,Clustering,HeatMap,Venn等数据分析功能，除了富集功能外，还可以动态构建分子互作网络模型。 从结果展示层面讲，DAVID，KOBAS主要是表格输出，OmicsBean的结果展示是图表、模型，可以下载用于文章的发表。 

[摘自]：http://www.ebiotrade.com/