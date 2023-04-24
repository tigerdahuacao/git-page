---
title: sql一例
date: 2022-06-02 15:13:50
categories:
- 题目
tags: SQL
---
(sql复习向)
有个需求:
现在有2张表:
- 债券要素表 - 含到期日信息；
- 行情表 - 含每日成交额

要找出比如从某个时间点开始, 一个债券如果在1年以上到期的话, 这个债券的成交总额.

比如一个债券在2021-02-01号的时候, 它的到期日是2022-10-01 那么2021-02-01到2022-02-01的每日交易量就都统计进来, 2022-02-02到目前的2022-06-01的交易量就不要计入.
sql中时间计算函数如下:
> https://blog.csdn.net/super_qing_/article/details/104018370
然后搞一个汇总.

这个汇总, 一开始让我有点迷惑, 其实就是group by. 
SQL如下:
```sql
--建表阶段
CREATE DATABASE IF NOT EXISTS test_db DEFAULT CHARACTER SET utf8;

CREATE TABLE IF NOT EXISTS `overall_table`(
   `item_id` INT UNSIGNED AUTO_INCREMENT,
   `item_title` VARCHAR(100) NOT NULL,
   `expire_date` DATE,
   PRIMARY KEY ( `item_id` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- 给 债券要素表 插入记录
INSERT INTO overall_table(item_title,expire_date) VALUES('测试1','2021-10-1');
INSERT INTO overall_table(item_title,expire_date) VALUES('测试2','2022-10-1');
INSERT INTO overall_table(item_title,expire_date) VALUES('测试3','2022-1-1');

SELECT * FROM overall_table;

CREATE TABLE IF NOT EXISTS `detail_table`(
   `id` INT UNSIGNED AUTO_INCREMENT,
   `item_id` INT,
   `value` BIGINT NOT NULL DEFAULT 100,
   `date` DATE,
   PRIMARY KEY ( `id` ),
   UNIQUE KEY (`item_id`,`date`)
)ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- 复习一下Alter语句, 一开始写出varchar了
-- ALTER TABLE detail_table MODIFY value BIGINT NOT NULL DEFAULT 100;

-- 给 行情表 插入记录

INSERT INTO detail_table(item_id,value,date) VALUES(1,100,'2021-1-1');
INSERT INTO detail_table(item_id,value,date) VALUES(2,100,'2021-1-1');
INSERT INTO detail_table(item_id,value,date) VALUES(3,100,'2021-1-1');

INSERT INTO detail_table(item_id,value,date) VALUES(1,60,'2021-6-1');
INSERT INTO detail_table(item_id,value,date) VALUES(2,70,'2021-6-1');
INSERT INTO detail_table(item_id,value,date) VALUES(3,80,'2021-6-1');

INSERT INTO detail_table(item_id,value,date) VALUES(1,11,'2021-9-1');
INSERT INTO detail_table(item_id,value,date) VALUES(2,12,'2021-9-1');
INSERT INTO detail_table(item_id,value,date) VALUES(3,13,'2021-9-1');

-- 看看对不对
SELECT * FROM detail_table;

-- 先取个数据看看

SELECT 
d.value  '当天成交额',
d.date '当天成交日',
d.item_id '债券代码号',
o.expire_date '债券到期日',
o.item_title '债券中文名' 
FROM detail_table d LEFT JOIN  overall_table o ON o.item_id =d.item_id;

-- 再看看日期计算
SELECT 
d.value  '当天成交额',
d.date '当天成交日',
d.item_id '债券代码号',
o.expire_date '债券到期日',
o.item_title '债券中文名',
DATEDIFF(o.expire_date,d.date) '日期差值'
FROM detail_table d LEFT JOIN  overall_table o ON o.item_id =d.item_id ; 

-- 我就以300天当一年了, 看看能不能过滤出数据
SELECT 
d.value  '当天成交额',
d.date '当天成交日',
d.item_id '债券代码号',
o.expire_date '债券到期日',
o.item_title '债券中文名',
DATEDIFF(o.expire_date,d.date) date_diff
FROM detail_table d LEFT JOIN  overall_table o ON o.item_id =d.item_id
WHERE date_diff > 300;

-- 汇总一下看看, 这个一开始犯了个错, Having group by 是不能放在 Having 后面的
SELECT 
d.value  '当天成交额',
d.date '当天成交日',
d.item_id '债券代码号',
o.expire_date '债券到期日',
o.item_title '债券中文名',
DATEDIFF(o.expire_date,d.date) date_diff
FROM detail_table d LEFT JOIN  overall_table o ON o.item_id =d.item_id
GROUP BY d.item_id HAVING date_diff > 300 

-- 发现直接sum, 会把300天以上的数据也给加上去,
-- 没办法, 使用类似视图的方式吧. 这里生成的表得给一个名字
SELECT SUM(value) AS '总成交量',
code AS '债券代码' ,
item_title AS '债券中文名' 
FROM (SELECT 
d.value AS value,
d.date AS  history_date,
d.item_id AS code,
o.expire_date AS expire_date,
o.item_title AS item_title,
DATEDIFF(o.expire_date,d.date) date_diff
FROM detail_table d LEFT JOIN  overall_table o ON o.item_id =d.item_id
HAVING date_diff > 300) 
AS temp_table GROUP BY code;
```