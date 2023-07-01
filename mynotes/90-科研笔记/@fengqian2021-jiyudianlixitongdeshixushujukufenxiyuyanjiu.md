# 基于电力系统的时序数据库分析与研究

## Metadata
- **CiteKey**: {{citekey}}
 - **Type**: Journal Article
 - **Title**: 基于电力系统的时序数据库分析与研究, 
 - **Author**: 冯仟,; 龚鸣,; 邓宏涛,, 
 - **Year**: 2021 ;
- **Journal**: 长江信息通信, 
- **Pages**: 140-143
- **Publisher**: {{publisher}},
- **Location**: {{place}},
- **DOI**: {{DOI}}
------


## Files and Links
- **Url**: [Open online](https://kns.cnki.net/KCMS/detail/detail.aspx?dbcode=CJFD&dbname=CJFDLAST2021&filename=HBYD202109047&v=)
- **zotero entry**: [Zotero](zotero://select/library/items/PI7YKB68)
- **open pdf**: [基于电力系统的时序数据库分析与研究_冯仟.pdf](zotero://open-pdf/library/items/7ACME4C3)

- **Keywords**: InfluxDB; ObsCite; 时序数据; 时序数据库; 电力系统

## Abstract
文章通过分析现有电力系统的现状,发现关系数据库并不适用于电力系统的数据存储。针对按照时间的变化而变化的数据,更适合用时序数据库进行数据的存储。文章提出在现有电力系统中用时序数据库替代关系数据库,并给出实现方案,进一步研究时序数据库的优势及其未来的发展趋势。

----

## Comments



----

## Extracted Annotations

关系数据库转换时序数据库方法(2023/7/1 下午2:35:07)

- *“现将上述关系数据表“数据监控表”转换为时序数据时序数据表： （1）提取表：将监控数据分类，并以该分类作为表。监控数据表的数据可分为四类，分别是电流、电压、功率因数、无功功率。所以该关系数据表对应四张时序数据表。 （2）提取 Tags：将 station_id,device_id,line_id 作为 Tags，以区分数据。 （3）提取 Fields：这里以电压表为例，电压分为 A、B、C 三向，故以 A、B、C 为表的 Field。这里并不需要类似于 MYSQL 那样单独设计一张表用于存放各个采集量的区间段内的最大值和最小值。只需要管理机能够将采集量出现较大变动时将数据上传即可。由时序数据库完成最大最小值的计算。 最后，成功将原有关系数据库中的表转换为时序数据库中的表。表结构如图 6 所示。 这里从原始数据中抽取了四张表，分别为电流、电压、功率因数、无功功率。以 device_id,line_id,station_id 为作为数据的标签，以查询数据，每个表中包含 A、B、C 三相数据。”* [(冯仟 等, 2021, p. 142)](zotero://open-pdf/library/items/7ACME4C3?page=3&annotation=QXQGPYJ3) 

> *“先分类，将设备等基础信息作为tag，便于引索，fields中存放采集的各个数据** [ ](zotero://open-pdf/library/items/PI7YKB68?page=undefined&annotation=)  



****



## Summary

  
## Research Objective(s)


## Background / Problem Statement


## Method(s)


## Evaluation


## Conclusion


## Notes
