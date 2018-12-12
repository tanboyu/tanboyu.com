---
title: 'Bioconductor中的几个注释信息数据库'
author: 波比
date: '2015-04-24'
slug: Bioconductor中的几个注释信息数据库
categories:
  - 生物信息学
tags:
  - bioconductor
  - R语言
  - 数据库
  - 生物信息学
---

最近在学习生信，有一些数据库文件需要认真研究，收集了一些资料如下。GO.db KEGG.db. org.Hs.ed.db 这三个软件包或者数据库。先看看这几个包大致是干什么的，有哪些内容。

```R
#安装下面模块
#当然还有其它很多其它的有用的数据注释包，像芯片探针什么的
source("http://bioconductor.org/biocLite.R");
biocLite(c("GO.db", "KEGG.db", "org.Hs.eg.db"));

#载入
library("GO.db",verbose=F);
library("KEGG.db");
library("org.Hs.eg.db",verbose=F);

###############################################################################
#
# 这里介绍下GO.db 这个包， 主要是各种信息的查询检索
#
###############################################################################

#列举各个包中的函数及数据集
ls("package:GO.db");

# [1] "GO" "GO.db" "GO_dbconn" "GO_dbfile" "GO_dbInfo"
# [6] "GO_dbschema" "GOBPANCESTOR" "GOBPCHILDREN" "GOBPOFFSPRING" "GOBPPARENTS"
# [11] "GOCCANCESTOR" "GOCCCHILDREN" "GOCCOFFSPRING" "GOCCPARENTS" "GOMAPCOUNTS"
# [16] "GOMFANCESTOR" "GOMFCHILDREN" "GOMFOFFSPRING" "GOMFPARENTS" "GOOBSOLETE"
# [21] "GOSYNONYM" "GOTERM"

ls("package:KEGG.db");

# [1] "KEGG" "KEGG_dbconn" "KEGG_dbfile"
# [4] "KEGG_dbInfo" "KEGG_dbschema" "KEGGENZYMEID2GO"
# [7] "KEGGEXTID2PATHID" "KEGGGO2ENZYMEID" "KEGGMAPCOUNTS"
# [10] "KEGGPATHID2EXTID" "KEGGPATHID2NAME" "KEGGPATHNAME2ID"

ls("package:org.Hs.eg.db");

# [1] "org.Hs.eg" "org.Hs.eg.db" "org.Hs.eg_dbconn"
# [4] "org.Hs.eg_dbfile" "org.Hs.eg_dbInfo" "org.Hs.eg_dbschema"
# [7] "org.Hs.egACCNUM" "org.Hs.egACCNUM2EG" "org.Hs.egALIAS2EG"
# [10] "org.Hs.egCHR" "org.Hs.egCHRLENGTHS" "org.Hs.egCHRLOC"
# [13] "org.Hs.egCHRLOCEND" "org.Hs.egENSEMBL" "org.Hs.egENSEMBL2EG"
# [16] "org.Hs.egENSEMBLPROT" "org.Hs.egENSEMBLPROT2EG" "org.Hs.egENSEMBLTRANS"
# [19] "org.Hs.egENSEMBLTRANS2EG" "org.Hs.egENZYME" "org.Hs.egENZYME2EG"
# [22] "org.Hs.egGENENAME" "org.Hs.egGO" "org.Hs.egGO2ALLEGS"
# [25] "org.Hs.egGO2EG" "org.Hs.egMAP" "org.Hs.egMAP2EG"
# [28] "org.Hs.egMAPCOUNTS" "org.Hs.egOMIM" "org.Hs.egOMIM2EG"
# [31] "org.Hs.egORGANISM" "org.Hs.egPATH" "org.Hs.egPATH2EG"
# [34] "org.Hs.egPFAM" "org.Hs.egPMID" "org.Hs.egPMID2EG"
# [37] "org.Hs.egPROSITE" "org.Hs.egREFSEQ" "org.Hs.egREFSEQ2EG"
# [40] "org.Hs.egSYMBOL" "org.Hs.egSYMBOL2EG" "org.Hs.egUCSCKG"
# [43] "org.Hs.egUNIGENE" "org.Hs.egUNIGENE2EG" "org.Hs.egUNIPROT"

# 可以help 下看看包里都有些什么
help(package="GO.db");

# 应用举例
#1. 在RNA-seq分析中得到差异表达基因集后一般就要做GO或者KEGG富集性分析，
#例如使用GOseq包或者GOstats包，有时就需要查询富集的GO ID对应到底是
#什么条目以及其它信息， 该富集条目下到底有哪些基因等

#GO ID 例子
goIDs.example

# 1.1 首先我们要看下这些ID是否是已经废除的ID
# 通过ID 在list 中检索，这种方法在后面都可以这么用
# 因为后面好多其它的数据集其实就是以ID作为检索的key
# 具体的信息为list的内容来组成list的
allGOOBSOLETE allGOOBSOLETE[goIDs.example];

#都为空 表示这些ID是还在用的

# 1.2 我们可能想知道这几个GO ID到底是属于什么类别
# MF BP 还是CC， ID的简要描述， 详细描述， 同义描述等
# 这时就要用到GO.db 中得GOTERM数据集及其函数

allGOTERMs allGOTERMs[1]; # 通过下标

#输出列表中第一个元素的信息
# $`GO:0000001`
# GOID: GO:0000001
# Term: mitochondrion inheritance
# Ontology: BP
# Definition: The distribution of mitochondria, including the mitochondrial genome,
# into daughter cells after mitosis or meiosis, mediated by interactions between
# mitochondria and the cytoskeleton.
# Synonym: mitochondrial inheritance

allGOTERMs[goIDs.example];

#当然也可以利用相关的函数， 根据ID来检索，而不必将所有的GO Term都调出来
GOID(goIDs.example); # GO ID 标识符 这里其实就是goIDs.example
Term(goIDs.example); # 每个GO ID 对应的意义简称
Synonym(goIDs.example); # 每个GO ID对应的同义的简称及其ID 就是和这个ID的多个描述，
# 也可以用 GOSYNONYM 数据集 GOSYNONYM[goIDs.example]
Secondary(goIDs.example); # 这个ID包含的其它属于它的ID
Definition(goIDs.example); # 详细的描述信息
Ontology(goIDs.example); # 所属的类别 MF BP CC

# 1.3 假如知道了某些ID是属于什么类别的，
# 还想知道这个ID在有向无环图（DAG）中得上下节点信息
# 这时需要用到些其它的数据集

# GOBPANCESTOR, GOBPCHILDREN, GOBPOFFSPRING, GOBPPARENTS
# 分别存储着每个ID在DAG中的祖先节点，子节点，后代节点，父节点
# MF 和 CC 也是这样
# 由上面的 Ontology(goIDs.example); 得到例子中得几个ID都为BP类型的
# 所以这里就以BP为例
allBPANCESTOR <-as.list(GOBPANCESTOR);
allBPCHILDREN <-as.list(GOBPCHILDREN);
allBPOFFSPRING <-as.list(GOBPOFFSPRING);
allBPPARENTS <-as.list(GOBPPARENTS);

allBPANCESTOR[goIDs.example];
allBPCHILDREN[goIDs.example];
allBPOFFSPRING[goIDs.example];
allBPPARENTS[goIDs.example];

#后面还有些数据库连接查询的东西 这里不讲了

#GO的介绍就到此

###############################################################################
#
# 这里介绍下KEGG.db 这个包，通过上面GO.db的介绍，我们可以看到其实就是对数据集
# 建立对应的连接关系，倒来倒去的，其实也没什么
#
###############################################################################
help(package="KEGG.db");

keggIDs.example #这里挨个介绍吧
# 1. KEGGENZYMEID2GO 和 KEGGGO2ENZYMEID kegg 中酶与GO的对应关系
# 2. KEGGEXTID2PATHID 和 KEGGPATHID2EXTID kegg 中pathway id 和Entrez Gene之间的对应关系
# 3. 当然这也只是一些模式生物才会做的比较详细， 想human mouse rat yeast
# 4. KEGGPATHID2NAME 和 KEGGPATHNAME2ID pathway ID 与 描述名称之间的转换
allENZYMEID2GO allGO2ENZYMEID allEXTID2PATHID allPATHID2EXTID allPATHID2NAME allPATHNAME2ID

#输出第一个元素
allENZYMEID2GO[1];
allGO2ENZYMEID[1];
allEXTID2PATHID[1];
allPATHID2EXTID[1];
allPATHID2NAME[1];
allPATHNAME2ID[1];

allPATHID2NAME[keggIDs.example];

# 输出
# $`05200`
# [1] "Pathways in cancer"
#
# $`05412`
# [1] "Arrhythmogenic right ventricular cardiomyopathy (ARVC)"

###############################################################################
#
# 这里介绍下org.Hs.eg.db, 这里看Hs就猜到是与人类相关的东西了
# 文档上说该库一年两次更新 “ This package is updated biannually.”
#
###############################################################################
help(package="org.Hs.eg.db");
ls("package:org.Hs.eg.db");

# 依次介绍下各个数据集
# org.Hs.egACCNUM 和 org.Hs.egACCNUM2EG ：Entrez 基因与GenBank 登录号之间的联系
# org.Hs.egALIAS2EG 基因名称与Entrez Gene 的对应关系
#Entrez Gene gene symbol 联系
#org.Hs.egSYMBOL org.Hs.egSYMBOL2EG
#chromosome 相关
#org.Hs.egCHR org.Hs.egCHRLENGTHS org.Hs.egCHRLOC org.Hs.egCHRLOCEND
# Entrez Gene 在哪条染色体上， 在染色体长度， 在染色体上的位置（起始 与结束）
#ENSEMBL 相关
#org.Hs.egENSEMBL org.Hs.egENSEMBL2EG ：ENSEMBL Gene ID 与Entrez Gene 之间的映射关系
#org.Hs.egENSEMBLPROT org.Hs.egENSEMBLPROT2EG ：ENSEMBL protein ID ENSEMBL ID
#org.Hs.egENSEMBLTRANS org.Hs.egENSEMBLTRANS2EG： 转录本
#org.Hs.egENZYME org.Hs.egENZYME2EG 酶编号
#Entrez Gene 与 GO
#org.Hs.egGENENAME Entrez ID 及其名称的对应关系
#org.Hs.egGO org.Hs.egGO2ALLEGS org.Hs.egGO2EG
#Entrez Gene 与 蛋白质的联系 PFAM PROSITE UNIPROT
#org.Hs.egPFAM org.Hs.egPROSITE org.Hs.egUNIPROT
#Entrez Gene 与 Refseq Unigene的联系
#org.Hs.egREFSEQ org.Hs.egREFSEQ2EG org.Hs.egUNIGENE org.Hs.egUNIGENE2EG
#Entrez Gene 与KEGG 的联系
#org.Hs.egPATH org.Hs.egPATH2EG
#Entrez Gene 与PubMed 编号的联系
#org.Hs.egPMID org.Hs.egPMID2EG
#Entrez Gene 与一些其它数据的联系
#org.Hs.egUCSCKG 与UCSC 的 known gene的联系
```

via:[rabbite gao's](http://:http://www.iwhgao.com/?p=297)