---
title: 'MIMIC-III 数据库字典部分解读'
author: 波比
date: '2018-07-03'
slug: MIMIC-III 数据库字典部分解读
categories:
  - 数据分析
tags:
  - MIMIC-iii数据库
  - 数据库字典
  - 数据挖掘
---

## 1. ADMISSIONS —— 入院信息

该表格包括以下信息： ROW\_ID：行号，这个是和Excel里行号是一个性质，我们基本用不到，不用去理他 SUBJECT\_ID：这个表示病人标识码，每一位病人对应唯一的标识码，可以理解为国内医院的住院号 HADM\_ID：这个对应于病人的每一次住院标识码，可以理解为国内医院病人每次住院的病案号 ADMITTIME：病人入院时间，精确到分钟，这里请注意MIMIC数据库里的年份信息为保护患者信息都是经过随意转换的，仅可计算相对时间 DISCHTIME：病人入院时间，精确到分钟 DEATHTIME：病人如在住院期间死亡，则显示具体死亡时间，精确到分钟，如住院期间无死亡事件发生，则为NULL ADMISSION\_TYPE：入院类型，包括以下4种类型

*   ELECTIVE
*   URGENT
*   NEWBORN
*   EMERGENCY

ADMISSION\_LOCATION：入院地点，包括以下9种类型：

*   EMERGENCY ROOM ADMIT
*   TRANSFER FROM HOSP/EXTRAM
*   TRANSFER FROM OTHER HEALT
*   CLINIC REFERRAL/PREMATURE
*   \*\* INFO NOT AVAILABLE \*\*
*   TRANSFER FROM SKILLED NUR
*   TRSF WITHIN THIS FACILITY
*   HMO REFERRAL/SICK
*   PHYS REFERRAL/NORMAL DELI

DISCHARGE\_LOCATION：出院后目的地 INSURANCE：保险类型 LANGUAGE：语种 RELIGION：宗教信仰 MARITAL\_STATUS：婚姻状况 ETHNICITY：种族 EDREGTIME：急诊留观登记时间 EDOUTTIME：急诊留观出观时间 DIAGNOSIS：初步诊断，注意初步诊断不提供ICD9码，而且写得相对随意 HOSPITAL\_EXPIRE\_FLAG：院内死亡标记

*   0表示存活
*   1表示院内死亡

HAS\_CHARTEVENTS\_DATA：是否有CHARTEVETNS的表格记录

*   0表示没有
*   1表示有

## 2. PATIENTS —— 病人信息

该表格包括以下信息（与上面重复的部分，我就介绍地简略一些） ROW\_ID：行号 SUBJECT\_ID：住院号 GENDER：患者性别 DOB：患者出生日期，对于年龄大于89岁的病人，将出生日期由入院日期向前调整300年，这部分患者年龄中位数为91.4。其余病人随机调整 DOD：患者死亡日期 DOD\_HOSP：患者院内登记的死亡日期 DOD\_SSN：患者社保局登记的死亡日期 EXPIRE\_FLAG：患者死亡标记

*   0表示存活
*   1表示死亡

## 3. CALLOUT —— 监护室出科信息

这个表格个人用的很少，因为国内病人出科基本都是联系好床位，办好出科文书，护士进一步办理出科手续，叫工友运送，很少会去考虑在这里能挖出什么宝来。但MIMIC既然给了这么详细的出科经过，我们也来看看老外的出科流程。 该表格包括以下信息（与上面重复的部分，我就介绍地简略一些） 先说明一下，因为Beth Israel医院有经过搬迁，同一个科室代码可能会出现表示科室前后不一致的情况。 ROW\_ID：行号 SUBJECT\_ID：住院号 HADM\_ID：病案号 SUBMIT\_WARDID：提交CALLOUT出科申请的科室代码 SUBMIT\_CAREUNIT：提交CALLOUT申请的是否为ICU收费中心，如果是，则显示ICU类型，否则显示NULL CURR\_WARDID：提交CALLOUT申请时患者所在科室代码 CURR\_CAREUNIT：提交CALLOUT申请时患者所在监护室类型，具体缩写表示下面在TRANSFERS表内会介绍 CALLOUT\_WARDID：CALLOUT申请目标科室代码

*   0表示CALLOUT目的地为“HOME”，回家去了
*   1表示CALLOUT目的地为“First available ward”，若干目标科室谁先有床就先转谁

其他数字则表示具体的接受科室的代码 CALLOUT\_SERVICE：出科病人需接受或继续接受的治疗服务，具体英文缩写表示什么，我后续会在SERVICES表中解释 REQUEST\_TELE：需要心电遥测服务 REQUEST\_RESP：需对呼吸道采取预防或保护措施，不特指病原体 REQUEST\_CDIFF：艰难梭菌定植病人，请采取预防措施 REQUEST\_MRSA：耐甲氧西林金葡菌定植病人，请采取预防措施 REQUEST\_VRE：耐万古霉素肠球菌定植病人，请采取预防措施 CALLOUT\_STATUS：CALLOUT申请状态，有以下两种状态

*   Active表示有效
*   Inactive表示已失效

CALLOUT\_OUTCOME：CALLOUT申请结果，有以下两种状态

*   Discharged表示出科成功
*   Cancelled表示出科失败

DISCHARGE\_WARDID：患者出科时的最终科室代码，需注意当显示为0时表示在家中 ACKNOWLEDGE\_STATUS：CALLOUT申请审批回复 Acknowledged表示审批通过 Revised表示表示CALLOUT需修改后再行审批 Unacknowledged表示审批拒绝 Reactivated表示CALLOUT重置为有效状态 CREATETIME：CALLOUT申请建立时间 UPDATETIME：CALLOUT申请更新时间 ACKNOWLEDGETIME：CALLOUT申请首次审批时间 OUTCOMETIME：CALLOUT申请结果发生时间 FIRSTRESERVATIONTIME：首次病房预约时间 CURRENTRESERVATIONTIME：当前病房预约时间

## 4.ICUSTAYS —— 监护室入住信息

该表格包括以下信息（与上面重复的部分，我就介绍地简略一些） ROW\_ID：行号 SUBJECT\_ID：住院号 HADM\_ID：病案号 ICUSTAY\_ID：ICU病案号，对应于每一次监护室入住 DBSOURCE：数据源，是来自CareVue数据系统，还是Metavision数据系统 FIRST\_CAREUNIT：患者入住监护室24小时内的首个监护室类别 LAST\_CAREUNIT：患者入住监护室24小时内的最终监护室类别，出现24小时内监护室类别主要是因为有的病人可能一开始收住的A监护室，但随后根据病情需要又转至了B监护室 FIRST\_WARDID：入住首个监护室代码 LAST\_WARDID：入住末次监护室代码 INTIME：此次监护室入科时间 OUTTIME：此次监护室出科时间 LOS：此次监护室住院时长

## 5. TRANSFERS —— 病人周转信息

看到这张表是不是感觉很熟悉，上面的ICUSTAYS就是从这张表里提取的，并删除了没有ICUSTAY\_ID的部分。 该表格包括以下信息（与上面重复的部分，我就介绍地简略一些） ROW\_ID：行号 SUBJECT\_ID：住院号 HADM\_ID：病案号 ICUSTAY\_ID：ICU病案号 DBSOURCE：数据源 EVENTTYPE：病人周转项目，包括以下内容

*   admit表示入院
*   transfer表示院内转运
*   discharge表示出院

PREV\_CAREUNIT：患者前次所在监护室类型，如不是监护室则显示NULL CURR\_CAREUNIT：患者当前所在监护室类型，如不是监护室则显示NULL PREV\_WARDID：患者前次所在科室代码，如不是科室则显示NULL CURR\_WARDID：患者当前所在科室代码，如不是科室则显示NULL INTIME：此次监护室入科时间 OUTTIME：此次监护室出科时间 。注意：INTIME和OUTTIME都是针对CURR\_CAREUNIT而言的LOS：此次监护室住院时长我们前面一直在说的CAREUNIT监护室的类别主要有以下：

*   CCU：心血管重症监护病房
*   CSRU：心血管手术康复病房
*   MICU：内科重症监护病房
*   NICU：新生儿重症监护病房
*   NWARD：新生儿病房
*   SICU：外科重症监护病房
*   TSICU：创伤/外科重症监护病房

## 6.  SERVICES —— 病人需接受的医疗服务

该表格包括以下信息（与上面重复的部分，我就介绍地简略一些） ROW\_ID：行号 SUBJECT\_ID：住院号 HADM\_ID：病案号 TRANSFERTIME：主要医疗服务种类更改时间，即下面PREV\_SERVICE改至CURR\_SERVICE的时间 PREV\_SERVICE：前次医疗服务类别 CURR\_SERVICE：当前医疗服务类别 这里的医疗服务SERVICES主要指以下内容（前面的CALLOUT里的CALLOUT\_SERVICE可以参考这里） CMED：心血管内科治疗——心血管疾病的保守治疗 CSURG：心血管外科治疗——心血管疾病的手术为治疗 DENT：牙科治疗——牙齿/颌骨相关 ENT：耳鼻喉科治疗——主要治疗耳鼻喉相关区域 GU：泌尿生殖器治疗——泌尿系统和生殖系统 GYN：妇科治疗——女性生殖系统和乳房等 MED：全科治疗——未特指，泛指内科相关治疗 NB：新生儿服务——主要指院内出生的 NBB：新生婴儿服务——主要指院内出生的 NMED：神经内科治疗——与脑相关的非手术治疗 NSURG：神经外科治疗——与脑相关的手术治疗 OBS：产科——产妇分娩及护理 ORTHO：骨科外科治疗——主要为涉及骨骼肌肉系统的手术 OMED：骨科治疗——主要为涉及骨骼肌肉系统内科保守治疗 PSURG：整形治疗——主要为人体的修复或重建（包括以美容或美学为目的的） PSYCH：精神卫生治疗——主要指与情绪、行为、认知或认知相关的精神障碍 SURG：普外科治疗——主要指无法进行专科分类的手术种类 TRAUM：创伤外科治疗——由外来物理因素造成的身体伤害或损坏 TSURG：胸外科治疗——主要指腹部及颈部之间的胸部手术 VSURG：血管外科治疗——主要指与循环系统相关手术

## 7.  CAREGIVERS —— 护理人员信息

该表格包括以下信息（与前一专题重复的部分，我就介绍地简略一些） ROW\_ID：行号 CGID：护理人员标志符，类似于单位里的员工码一样的存在 LABEL：主要为头衔缩写，如MD表示医学博士，RN表示注册护士等 DEION：对LABEL的进一步解释，说明该护理人员的类别

## 8. CHARTEVENTS —— 病人观察记录数据

该表格包括以下信息（与前面重复的部分，我就介绍地简略一些） ROW\_ID：行号 SUBJECT\_ID：住院号 HADM\_ID：病案号 ICUSTAY\_ID：ICU病案号 ITEMID：项目标志符 CHARTTIME：记录时间 STORETIME：录入数据上传至信息系统及确认时间 CGID：护理人员标志符 VALUE：ITEMID对应项目值 VALUENUM： 如果VALUE值是数值型，则显示为VALUE相同的值； 如果VALUE值不是数值型，则显示NULL； 如果VALUE的ITEMID对应于评分项目，则VALUE包含项目名称及分值，而VALUENUM只包含分值 VALUEUOM：VALUE值如有单位，则显示其单位，否则为NULL WARNING：Metavision数据系统专有。提示是否数值触发了警报 ERROR：Metavision数据系统专有。提示测量记录过程是否发生错误 RESULTSTATUS：CareVue数据系统专有。表示测量记录类型，有以下2种

*   Manual表示手动
*   Automatic表示自动

STOPPED：CareVue数据系统专有。表示测量记录是否被终止

## 9. DATETIMEEVENTS —— 病人操作相关日期时间信息

该表格包括以下信息（与上面重复的部分，我就介绍地简略一些） ROW\_ID：行号 SUBJECT\_ID：住院号 HADM\_ID：病案号 ICUSTAY\_ID：ICU病案号 ITEMID：项目标志符 CHARTTIME：记录时间 STORETIME：录入数据上传至信息系统及确认时间 CGID：护理人员标志符 VALUE：ITEMID对应项目日期时间数据 VALUEUOM：VALUE值类型，主要有2种类型 Date表示日期 Date and Time表示日期+时间 WARNING：Metavision数据系统专有。提示是否数值为警报值 ERROR：Metavision数据系统专有。提示测量记录过程是否发生错误 RESULTSTATUS：CareVue数据系统专有。表示测量记录类型 STOPPED：CareVue数据系统专有。表示测量记录是否被终止

## 10. INPUTEVENTS_CV —— Philips CareVue系统入量数据

该表格包括以下信息（与上面重复的部分，我就介绍地简略一些） ROW\_ID：行号 SUBJECT\_ID：住院号 HADM\_ID：病案号 ICUSTAY\_ID：ICU病案号 CHARTTIME：记录时间 ITEMID：项目标志符，CareVue常用药物的ITEMID在30000-39999之间，而出入量项目的ITEMID在40000-49999之间 AMOUNT：前次记录时间至现在的总入量，大多是按每小时计 AMOUNTUOM：入量单位 RATE：入量速率，如无则显示NULL RATEUOM：速率单位，RATE为NULL时，该项也为NULL STORETIME：录入数据上传至信息系统及确认时间 CGID：护理人员标志符 LINKORDERID：药品组合标志符 ORDERID：官网的说法是药品组合标志符，但与LINKORDERID又有所不同，它是LINKORDERID下的药品组合给药速率标志符，每个ORDERID对应一个给药速率，每次给药速率改变时即产生一个新的ORDERID，可用于追踪给药速率，所以将它理解为药品组合给药速率标志符会更好理解 STOPPED：输液是否中断或续药等 NEWBOTTLE：新液体准备状态。1表示已准备新液体已化好就绪，NULL表示无 ORIGINALAMOUNT：药品组合最初总量 ORIGINALAMOUNTUOM：药品组合总量的单位 ORIGINALROUTE：最初给药途径，如经口、静脉等 ORIGINALRATE：最初给药速率 ORIGINALRATEUOM：最初给药速率单位 ORIGINALSITE：最初人体给药部位，如左手、左脚等

## 11. INPUTEVENTS_MV — IMDSoft Metavision系统入量数据

该表格包括以下信息（与上面重复的部分，我就介绍地简略一些） ROW\_ID：行号 SUBJECT\_ID：住院号 HADM\_ID：病案号 ICUSTAY\_ID：ICU病案号 STARTTIME：出入量活动开始时间 ENDTIME：出入量活动结束时间 ITEMID：项目标志符，Metavision的ITEMID均大于220000 AMOUNT：从STARTTIME至ENDTIME的总入量 AMOUNTUOM：入量单位 RATE：入量速率，如无则显示NULL RATEUOM：速率单位，RATE为NULL时，该项也为NULL STORETIME：录入数据上传至信息系统及确认时间 CGID：护理人员标志符 LINKORDERID：药品组合标志符 ORDERID：LINKORDERID下的药品组合给药速率标志符 ORDERCATEGORYNAME：所给药品一级分类目录名 SECONDARYORDERCATEGORYNAME：所给药品次级分类目录名 ORDERCOMPONENTTYPEDEION：药品在药品组合中的作用 ORDERCATEGORYDEION：所给药品类型，针剂、非针剂等等 PATIENTWEIGHT：患者体重，以千克计 TOTALAMOUNT：输注液体最初体积总量 TOTALAMOUNTUOM：输注液体体积单位 ISOPENBAG：表示装有溶液的输液袋是否已打开

*   0表示未打开
*   1表示已打开

CONTINUEINNEXTDEPT：出科后是否续药。这项仅出科时有意义。

*   0表示出科至新科室后不续药
*   1则表示续药

CANCELREASON：医嘱取消原因 STATUSDEION：输液状态，主要有以下几种：

*   Stopped表示停止液体输注或液体输注结束
*   FinishRunning表示液体已输注结束
*   Rewritten表示对液体输注记录进行修正
*   Changed表示对输液参数进行改动
*   Flushed表示一条液体通路已被冲管

COMMENTS\_EDITEDBY：输液医嘱修改标记，显示为操作者头衔 COMMENTS\_CANCELEDBY：输液医嘱取消标记，显示为操作者头衔 COMMENTS\_DATE：COMMENTS\_EDITEDBY和COMMENTS\_CANCELEDBY发生的时间 ORIGINALAMOUNT：记录时输液袋内剩余液体量 ORIGINALRATE：最初输液速率

## 12. NOTEEVENTS —— 病人医疗文书资料

该表格包括以下信息（与上面重复的部分，我就介绍地简略一些） ROW\_ID：行号 SUBJECT\_ID：住院号 HADM\_ID：病案号 CHARTTIME：记录时间 CATEGORY：文书类别，包括出院录，ECG报告，心超报告balabala… DEION：对CATEGORY的进一步解释，比如radiology类别下的MRATHORACICSPINE检查报告 CGID：文书录入医护人员 ISERROR：错误提醒。当显示为1时，表示此处被标记为此处医师认为存在错误，否则显示为NULL TEXT：文书具体内容

## 13. OUTPUTEVENTS —— 病人出量信息

该表格包括以下信息（与上面重复的部分，我就介绍地简略一些） ROW\_ID：行号 SUBJECT\_ID：住院号 HADM\_ID：病案号 ICUSTAY\_ID：ICU病案号 CHARTTIME：记录时间 ITEMID：项目标志符 VALUE：自上次记录时间到现在液体出量 VALUEUOM：出量单位 STORETIME：录入数据上传至信息系统及确认时间 CGID：护理人员标志符 STOPPED：输液是否中断 NEWBOTTLE：新液体准备状态 ISERROR：错误提醒。

## 14. PROCEDUREEVENTS_MV — Metavision系统的操作信息

该表格包括以下信息（与上面重复的部分，我就介绍地简略一些） ROW\_ID：行号 SUBJECT\_ID：住院号 HADM\_ID：病案号 ICUSTAY\_ID：ICU病案号 STARTTIME：操作或手术开始时间 ENDTIME：操作或手术结束时间 ITEMID：项目标志符 VALUE：ITEMID对应项目值 VALUEUOM：VALUE值如有单位，则显示其单位，否则为NULL LOCATION：通路部位，如无则显示为NULL LOCATIONCATEGORY：LOCATION所属目录 STORETIME：录入数据上传至信息系统及确认时间 CGID：护理人员标志符 LINKORDERID：药品组合标志符 ORDERID：LINKORDERID下的药品组合给药速率标志符 ORDERCATEGORYNAME：所给药品一级分类目录名 SECONDARYORDERCATEGORYNAME：所给药品次级分类目录名 ORDERCATEGORYDEION：所给药品类型 ISOPENBAG：表示输液袋是否已打开 CONTINUEINNEXTDEPT：出科后是否续药。 CANCELREASON：医嘱取消原因 STATUSDEION：医嘱状态 COMMENTS\_EDITEDBY：操作或手术修改标记，显示为操作者头衔 COMMENTS\_CANCELEDBY：操作或手术取消标记，显示为操作者头衔 COMMENTS\_DATE：COMMENTS\_EDITEDBY和COMMENTS\_CANCELEDBY发生的时间