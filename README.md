# COVID-19-2019-nCoV-Infection-Data-cleaning-
针对新冠病毒疫情数据的清洗脚本和清洗后的数据。
在原有基础上增加对全世界各国的数据处理。

# 源数据说明
源数据使用 https://github.com/BlankerL 的 https://github.com/BlankerL/DXY-COVID-19-Data/blob/master/csv/DXYArea.csv
其定时从丁香园网站抓取的原始各地区上报数据

##### 感谢 BlankerL 的工作

原始数据格式如下<br/>
continentName | continentEnglishName | countryName | ountryEnglishName | provinceName | provinceEnglishName | province_zipCode | province_confirmedCount | province_suspectedCount | province_curedCount | province_deadCount | updateTime | cityName | cityEnglishName | city_zipCode | city_confirmedCount | city_suspectedCount | city_curedCount | city_deadCount
:-: | :-: | :-: | :-: | :-:| :-: | :-: | :-: | :-:| :-: | :-: | :-: | :-:| :-:| :-: | :-: | :-: | :-: | :-:
亚洲 | Asia | 中国 | China | 重庆市 | Chongqing | 500000 | 573 | 0 | 328 | 6 | 2020-02-23 07:18:21| 渝中区 | Yuzhong District|500103|20|0|15|0
欧洲 | Europe | 卢森堡 | Luxembourg | 卢森堡 | Luxembourg | 961004 | 335 | 0 | 0 | 4 | 2020-03-20 12:33:45 | NA | NA | NA | NA | NA | NA | NA


针对数据的改进：<br/>
1.原始数据每天都会多次抓取数据，同一个地区每天存在多条记录，因为原始统计数据并不是连续时效性的，各地区并不是按小时的时间段发布，因此每天只需要一条数据<br/>
2.源数据不能直观的看出中国的累计数量，只能看出各个省份的信息。
因此需要对中国与其他国家数据分开处理，通过累加各个省份的人数来得到中国的累计信息。其中境外输入数据也包含在中国的累计信息当中。<br/>


# 脚本说明
- world_data.py  对于中国，统计每个城市的信息和；对于其他国家，保留各个国家每天最新的一条数据
- data_step1.py  第一步处理 本脚本将各省市每天的数据进行去重处理，每个省市只保留最新的一条数据 （也可选择保留当天最大数值）
- data_step2.py  第二步处理 基于data_step1.py的输出文件， 计算每天的新增数据，通过当天数据减去前一天数据的方式，计算出每天新增数据

说明：各地区数据质量不同，同时存在后面修正前期数据，进行核销的处理，因此有时候当天数据会比前一天还少，新增数据为负

# Data说明
data 目录存放了清洗出的数据。<br/>
nCov_china_0312 是3月12日中国的数据。<br/>
nCov_world_0516 是5月16日全世界的数据。<br/>
输出数据格式如下
国家 | 确诊 | 治愈 | 死亡 | 日期
:-: | :-: | :-: | :-: | :-:
阿曼 | 19 | 2 | 0 | 2020-03-13



2020.2.16 cz

#  ------ 2020.02.18 22：00 更新脚本和数据 ---------

由于原始数据有一些缺陷，导致之前计算新增数据时存在不准确，新增数据和累计数据对不齐得问题

这两天修改脚本，增加了对原始数据不完整的问题进行动态修正，基本解决了数据的问题

同时这两天原始数据质量也在提升

今天更新了脚本，同时更新了我清洗后的数据，以及excel表格，excel表格现在调整为修改原始数据表单后，所有图表和数据可动态更新，数据表单更新后，只要对数据透视表的分析菜单手动操作一次全部刷新即可

#  ------ 2020.02.24 22：00 更新脚本和数据 ---------

excel文件增加了全国及湖北疑似病例的数据，这个数据是手动收集，原始数据没有

脚本增加了直接数据写入excel文件的代码，我设置了开关，但现在将其屏蔽了，因为发现用py库操作excel文件，数据是正确的，但有些图表样式会丢失

借这个项目也熟悉了PY的数据分析方法，后续可能考虑尝试透视图及图表也用python脚本来做

#  ------ 2020.02.29 22：00 更新数据 ---------

最近excel文件中增加了一些手动维护的数据，湖北省，武汉市，全国的疑似数据，武汉市内各区的数据。并做了预测模型

#  ------ 2020.03.19 12：00 更新脚本 ---------

原始数据格式变化，增加了国外数据，导致城市字段出现空值。调整脚本后可以处理，但是本脚本仅处理中国数据。
海外数据的渠道来源很多，源数据中的海外数据其实信息很少，老实说没必要加入海外数据，或者另外做一个文件才好。

格式变化后，源数据膨胀很多，建议运行脚本处理前删除不必要的列，仅保留 provinceName,cityName,province_confirmedCount,province_curedCount,province_deadCount,city_confirmedCount,city_curedCount,city_deadCount,updateTime  
这些字段，处理速度可以快不少


# 数据下载说明
由于raw.githubusercontent.com 被DNS污染，部分地区不能下载。大家可以试试我的百度云链接，数据更新到5月16号。
链接：https://pan.baidu.com/s/1QRaTV1OCTDDpLtuZ3JDtBA 
提取码：u2d1

# -----------2020.06.01 --------------
合并了 tiffanyXiaoqing 提交的修改，支持世界数据筛选脚本
