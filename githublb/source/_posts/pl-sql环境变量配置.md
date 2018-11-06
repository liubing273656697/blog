---
title: pl/sql环境变量配置
date: 2018-10-09 10:46:48
tags:
categories:
- config
---
1) 变量名：ORACLE_HOME  变量值：E:\PLSQLDeveloper\PLSQL\instantclient_11_2

2) 变量名：TNS_ADMIN  变量值：E:\PLSQLDeveloper\PLSQL\instantclient_11_2

3) 变量名：NLS_LANG   变量值：
                        SIMPLIFIED CHINESE_CHINA.ZHS16GBK
                        AMERICAN_AMERICA.UTF8
4) 修改Path变量，在后面添加 E:\PLSQLDeveloper\PLSQL\instantclient_11_2

配置oracle监听文件

    找到E:\PLSQLDeveloper\PLSQL\instantclient_11_2路径下的tnsnames.ora文件，用编辑器打开（记事本也可以）。

    根据自己实际需要进行编辑，如下图所示。ORCL_27是显示的数据库名，HOST后面填的是所要连接的地址。编辑好以后保存。
