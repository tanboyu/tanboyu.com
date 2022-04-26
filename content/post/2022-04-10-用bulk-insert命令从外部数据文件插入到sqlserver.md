---
title: 用BULK INSERT命令从插入外部TXT文件到SQL Server
author: 波比
date: '2022-04-10'
slug: Insert_TXT_file_to_SQL_Server_with_BULK_INSERT_commandr
categories:
  - 数据分析
tags:
  - BULK INSERT
  - insert
  - SQL server
draft: no
mathjax: no
type: post
---

最近有一批数据前期经过R语言清洗后需要插入到SQL Server。尝试了多种办法：

 1. 用`DBI`、`RODBC`包
 
从目前经验来看，用R来写入数据库只适应于少量的数据，如果数据量大，写入效率极低。这批数据写入跑了1天1晚。
 
 2. 用Kettle Spoon
 
用方法1写入了一个表，想着效率太低了，考虑用ETL工具插入数据。Kettle Spoon是一个非常经典的ETL工具，用Kettle Spoon效率比R单线程写入要快几十倍。可能我第一次用没处理好的缘故，也出现了一些问题。例如：列分割的时候指定分隔符，遇到空值老是有分割错的情况，导致插入的数据错列。如果是数据比较规整，用ETL效率比R写入快很多。几万十条记录，几分钟能搞定。

3. 用SQL Server命令直接插入外部文件

对比了方法1和方法2总有各种不能搞定的意外情况。想着SQL Server这么强大的数据库工具，应该效率不会低，开始用自带的数据导入导出工具，在数据列分割的时候也存在各种问题。最后转到用命令行导入。

这里列出一个例子，供参考。

首先创建数据表。

```SQL
drop table if exists new_ther;
create table new_ther (
primaryid bigint not null,
caseid int not null,
dsg_drug_seq varchar(255),
start_dt varchar(255),
end_dt varchar(255),
dur varchar(255),
dur_cod varchar(255)
);

```
 
 接着，用BULK INSERT 从外部TXT文件中插入数据
 
 ```SQL
 BULK INSERT new_ther
FROM 'E:/Research/new_ther.txt'
WITH
(
FIELDTERMINATOR = '$' ,
ROWTERMINATOR = '0x0a' ,
KeepNulls ,
FirstRow = 2
);

 ```
 
 这里稍稍解释下几个参数：
 - `BULK INSERT`后紧接的是表名`new_ther`
 
 - `FROM`后指定外部TXT文件的位置
 
 - `FIELDTERMINATOR` 指定列分割符号
 
 - `ROWTERMINATOR` 指定换行符，换行符怎么看呢？我使用NotePad++打开文件查看的，具体操作为菜单栏**视图** >> **显示符号** >> **显示行尾符**。激活后可以看到行尾有`CRLF`或者字样，意思是回车换行，对应案例中用`0x0a`。
 
 - `KeepNulls` 指定空列在大容量导入操作期间应保留空值，而不是插入列的任何默认值。
 
 - `FirstRow` 数据从第2行开始导入。
 
 
 用这种方法效率上与ETL工具差不多，似乎略觉还要快。更重要的是简单直接，把脚本写好，测试没有问题就可以批量导入数据了。

