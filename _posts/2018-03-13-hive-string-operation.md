---
layout: default
title:  Hive 字符串操作
---

<h1>{{ page.title }}</h1>

### 字符串切割
在很多情况下，Hive中保存的字段格式并不是我们想要的格式，我们需要的只是字段中的部分内容。

表tbl_location如图所示：
|user|location|
|:---:|:---:|
|a|中国,广东,广州|

但在不同的场合中，我们可能会对国家/省份/城市中的一个或者多个更感兴趣，因此需要对location字段进行拆分  

```sql
CREATE VIEW view_location AS
SELECT `tbl_location`.`user`,
split(`tbl_location`.`location`, ',')[0] as `country`,
split(`tbl_location`.`location`, ',')[1] as `province`,
split(`tbl_location`.`location`, ',')[2] as `city`
FROM `tbl_location`;
```

最后得到view_location如下：
|user|country|province|city|
|:---:|:---:|:---:|:---:|
|a|中国|广东|广州|

至此我们可以直接对view_location进行操作获取单独的国家/省份/城市信息。


#### 参考：
* [Hive常用字符串函数](https://www.iteblog.com/archives/1639.html)
* [hive array、map、struct使用](http://blog.csdn.net/yfkiss/article/details/7842014)

<p>{{ page.date | date_to_string }}</p>
