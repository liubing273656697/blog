---
title: oracle数据库创建(配置)
date: 2018-05-22 12:47:11
tags:
categories:
- oracle
---

# 通过配置来创建数据库 Database Configuration Assistant
    oracle使用的步骤
1,在开始-》所有程序中找到Oracle下级菜单-》配置和移植工具--》Database Configuration Assistant
![oracle图片](/images/oracle/create/config/1.png)
2,首先会进入首页，只需要点击下一步即可，进入到真正配置的第一步。在这步我们选择：创建数据库，点击下一步
![oracle图片](/images/oracle/create/config/2.png)
3,第二步是选择要创建的数据库的模板，Oracle提供的带数据文件的模板，里面包含了预先创建的数据库，这样可以很快速的完成数据库的创建。一般情况下，我们选择：一般用途或事物处理
![oracle图片](/images/oracle/create/config/3.png)
4,这一步需要我们设置数据库名和SID，输入数据库名后自动生成相应的SID,但是我们也可以自己修改
![oracle图片](/images/oracle/create/config/4.png)
5,接下来的一步，直接使用默认配置即可
![oracle图片](/images/oracle/create/config/5.png)
6,进入配置数据库口令的步骤，可以选择为不同账户设置不同的口令，也可以统一所有的口令。如果不设置则会使用默认的口令。此处为了安全考虑，建议大家设置一下口令(Admin123456)。接着点击下一步，接下来的步骤使用默认设置即可
![oracle图片](/images/oracle/create/config/6.png)
![oracle图片](/images/oracle/create/config/7.png)
7,第7-10步包含了数据库恢复区、内存等的设置，直接使用Oracle的默认值即可。在第11步，勾选创建数据库的同时，也能选择性的将自己建立的数据库存为数据库模板，或者生成数据库创建的脚本。点击完成，等待数据库初始化创建完成，关闭窗口即可。
![oracle图片](/images/oracle/create/config/8.png)
8,最后可以使用我们创建的数据库进行登录了，登录后可以进行建表等操作啦。
Oracle创建数据库很多步骤直接使用默认的设置即可满足日常工作学习的需求了，自己另外配置需消耗较多时间
# 创建表空间

需要使用SYS登陆,创建数据库时的口令Admin123456

  删除表空间
   ```javascript
  drop tablespace WORKFLOW01 including contents;
   ```
    创建固定表空间
     ```javascript
    CREATE TABLESPACE "WORKFLOW01"
    LOGGING DATAFILE 'D:\tablespace\oracle\work\WORKFLOW01.ora' SIZE 500m autoextend on
    next 32m maxsize 1024m
    extent management local;
    commit;
     ```
# 创建实例授权

需要使用SYS登陆,创建数据库时的口令Admin123456

删除用户
     ```javascript
drop user myroot cascade;
     ```
创建用户 Create the myroot
```javascript
create user myroot
identified by "123456"
default tablespace WORKFLOW01
temporary tablespace TEMP
profile DEFAULT;
     ```
授权 Grant/Revoke role privileges
```javascript
grant connect to myroot with admin option;
grant dba to myroot with admin option;
     ```
授权 Grant/Revoke system privileges
```javascript
grant alter any table to myroot with admin option;
grant create any table to myroot with admin option;
grant delete any table to myroot with admin option;
grant update any table to myroot with admin option;
grant drop any table to myroot with admin option;
grant insert any table to myroot with admin option;
grant select any table to myroot with admin option;
grant unlimited tablespace to myroot with admin option;
     ```