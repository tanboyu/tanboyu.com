---
title: '富集分析代谢数据库'
author: 波比
date: '2016-05-17'
slug: 富集分析代谢数据库
categories:
  - 生物信息学
tags:
  - database
  - metabolic
  - pathway
  - 数据库
---

给定一些基因，从中找出联系，并能很好的显示给人们看，是一个生物信息学中非常常见的问题。其中pathway分析与gene ontology(GO)分析是两个很重要方面。基于种种原因，人们对GO分析往往比较注重，而随之而来的分析工具也多。而对PATH的分析，却因数据库的原因，手段寥寥。为此，我收集了一些关于pathway数据库方面的资料，总结如下：

*   代谢数据库有哪些？

这个问题很难全面回答，只能说出主要的数据库有哪些。在维基百科上就有相关的答案。

 KEGG: Kyoto Encyclopedia of Genes and Genomes. 这是最早出现的代谢数据库之一了，始于1995年。在图形化显示代谢途径方面它做出了重大贡献。因为它的历史地位，它依然是代谢数据库的老大。然而，它有两点深为人们诟病：

其一. 就是注释源的问题。对于注释源，KEGG只提到是相关方面的专家收集整理而成，但你无法知道这些神秘专家的名字与参考文献。虽然无数事实已经证明，它的数据是非常可靠的，但人们还是对此心存疑问。

其二. 就是授权的问题。它的授权过于严格，导致R已经无法继续支持它，转而开始使用更加开源的reactome。

*Reactome*, a database of reactions, pathways and biological processes. 

在Reactome的自我介绍中，它非常强调的一点就是开源，开放，人工整理，两人确认防错的数据库。它虽然出生的晚（始于2002年12月），但它的成长速度却是非常惊人的。 

*BioCyc*,Metabolic network models for hundreds of organisms.其实我对BioCyc应该是比较熟悉的，之前以E.coli为研究模型的时候，总是会用到其下的EcoCyc。它包括了很多物种的代谢信息，其中大多数包含有计算机推测的pathway，只有它所谓的tier 1的数据库是人工从文献中整理的，较为精确的数据库，它们是： 

EcoCyc(Escherichia coli K-12 MG1655), 

HumanCyc(Homo sapiens), 

AraCyc(Arabidopsis thaliana), 

YeastCyc(Saccharomyces cerevisiae), 

LeishCyc(Leishmania major Friedlin)

以及多物种的综合库MetaCyc。

BioCyc给人的印象是文献齐全。如果你想了解某一代谢途径的话，把它整理列举的文献读上一遍，就会对整个途径十分清晰了。

*   如何理解数据库中的一些名词，比如Pathway, reaction, chemical, compounds …

一图以示之（来源： Karp, P.D., and Riley, M., EcoCyc: The Resource and the Lessons Learned. Kluwer Academic Publishers, pp47-62 1999.） 

![](http://blog.qiubio.com:8080/wp-content/uploads/2012/03/metabolicPathwayDBStructure-560x338.gif) 

*   这些数据库的内容是否相近？还是差异很大，应该相信哪个？

有两篇文献讲解这个问题。

1.  Soh D, Dong D, Guo Y, Wong L. Consistency, comprehensiveness, and compatibility of pathway databases. BMC Bioinformatics. 2010 Sep 7;11:449. PubMed PMID: 20819233; PubMed Central PMCID: PMC2944280.
2.  Stobbe MD, Houten SM, Jansen GA, van Kampen AH, Moerland PD. Critical assessment of human metabolic pathway databases: a stepping stone for future integration. BMC Syst Biol. 2011 Oct 14;5:165. PubMed PMID: 21999653; PubMed Central PMCID: PMC3271347.

在第二篇文献当中，作者提到不同数据库之间差异很大。相同的部分远比我们想象中的要低。从两两比较的结果（附件图4），KEGG和HumanCyc的结果还是比较接近，但reactome和它们的差别就较大。

无论如何，从总体上来讲，数据库之间的差别还是非常大的。 

于是问题来了，我们应该相信谁，使用谁？

可笑却又是事实的答案就是看你想干什么，选择对自己分析有利的数据库。往往，这种可笑的答案是生物口文章的事实。

有人提出综合所有的数据库信息来进行分析是否可行？

答案是，在不断努力中，但没有一个很好的解决方案。

 一些有用的数据库： 

BRENDA: http://www.brenda-enzymes.org/index.php4 整合了许多数据库的信息。

 NetPath: http://www.netpath.org/index.html 主要是关于人的信号转导途径的数据库。 

BioGrid: http://thebiogrid.org/ 生物大分子相互作用的数据库。 

Pathway Commons: http://www.pathwaycommons.org/pc/ 整合数据库，信息来源包括biogrid,humanCyc,reactome等等。 

WikiPathways: http://www.wikipathways.org/index.php/WikiPathways 基于维基概念的数据库。

ConsensusPathDB: http://cpdb.molgen.mpg.de/ 整合数据库，信息来源多样化。

主要是三个物种：human, yeast, mouse。 

The Edinburgh Metabolic Pathways database: http://www.ehmn.bioinformatics.ed.ac.uk/account/login/?next=/ 需要注册。 

signalink: http://signalink.org/ 信号转导途径的数据库，主要有三个物种：worm, fly, human。 

Nature pathways database: http://pid.nci.nih.gov/ 主要是关于human的。 

BioCarta: http://www.biocarta.com/genes/index.asp 示例图不错。 

UniProt pathway: http://www.uniprot.org/docs/pathway 

摘自：http://blog.qiubio.com:8080/archives/2616