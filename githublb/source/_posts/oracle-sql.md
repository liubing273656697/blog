---
title: oracle sql
date: 2018-10-16 09:45:12
tags:
categories:
- oracle
---
# oracle常用sql
## 复制
1、复制表结构以及数据
create table d_table_name as select * from s_table_name;  注意并不会创建索引
2、只复制表结构
create table d_table_name as select * from s_table_name where 1=2;
3、只复制数据
（1）、两个表结构一样
insert into d_table_name select * from s_table_name;
2）、两个表的结构不一样，只复制部分列
insert into d_table_name (column1,column2,column3) select column1x,column2x,column3x from s_table_name;