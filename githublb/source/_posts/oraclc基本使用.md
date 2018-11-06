---
title: oraclc基本使用
date: 2018-05-23 10:39:16
tags:
categories:
- oracle
---
这里所有操作需要用户登陆,用户myroot,口令123456
# 删除表
删除单个表
```javascript
首先你有drop的权限
drop table 用户表名
举例 ：drop table scott.tableA
```
删除当前用户下所有表
场景 ：手动或者动态脚本清除 Oracle清空或者删除当前用户所有的表
方法一：使用pl/sql客户端，使用该用户登录，选中所有表 右键drop即可
方法二：前提：该用户 有此权限
```javascript
    select 'drop table '||table_name||';' from user_tables;
```
然后 拷贝其 SQL ，进行删除
方法三：使用存储过程删除该用户下所有的表
```javascript
    set ECHO ON
    set define off
    SPOOL logs/create_procedure.log

    --删除所有表的存储过程;
    create or replace procedure P_DROP_ALL_TABLE
    as
      --引用user_tables表中的tableName的类型;
      tableName user_tables.table_name%type;
      type ty is record(table_name varchar2(30));
      --定义ref类型游标;-强类型
      type ref_type is ref cursor return ty;
      ref_t ref_type;
      --定义变量存储数量;
      mycount number(10);
      begin
        --打开游标;
        open ref_t for select table_name from user_tables;
             loop
                 --从游标中获取一条记录,放入变量中;
                 fetch ref_t into tableName;
                        SELECT COUNT(*) INTO mycount FROM user_tables WHERE TABLE_NAME = tableName;
                        if mycount>0 then
                           execute immediate 'DROP TABLE '||tableName || ' CASCADE CONSTRAINT ';
                        end if;
                 exit when ref_t%notfound;  --退出;
             end loop;
         close ref_t;
      end;
    /
```
清除的话，将 drop 替换为 truncate 或者 delete ,过程 同上
# 创建函数
```javascript
www
```
# 复制表字段
