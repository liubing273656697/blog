---
title: oracle函数
date: 2018-07-30 15:15:25
tags:
categories:
- oracle
---
# oracle常用函数详解（详细）
   Oracle SQL 提供了用于执行特定操作的专用函数。这些函数大大增强了 SQL 语言的功能。函数可以接受零个或者多个输入参数，并返回一个输出结果。 oracle 数据库中主要使用两种类型的函数：

1.  单行函数：对每一个函数应用在表的记录中时，只能输入一行结果，返回一个结果，

比如：MOD(x,y)返回 x 除以 y 的余数（x 和 y 可以是两个整数，也可以是表中的整

数列）。常用的单行函数有：

Ø  字符函数：对字符串操作。

Ø  数字函数：对数字进行计算，返回一个数字。

Ø  转换函数：可以将一种数据类型转换为另外一种数据类型。

Ø  日期函数：对日期和时间进行处理。

2.  聚合函数：聚合函数同时可以对多行数据进行操作，并返回一个结果。比如 SUM(x)

返回结果集中 x 列的总合
## 一、字符函数
字符函数接受字符参数，这些参数可以是表中的列，也可以是一个字符串表达式。

常用的字符函数：

### CONCAT(X,Y)连接
连接字符串X和Y
示例 : SELECT CONCAT('Hello','world') FROM dual;
示例结果 : Helloworld
### LENGTH(X)长度
返回X的长度
示例 : SELECT LENGTH('Hello') FROM dual;
示例结果 : 5
### LOWER(X)转小写
X转换成小写
示例 : SELECT LOWER('Hello') FROM dual;cc
示例结果 : hello
### UPPER(X)转大写
X转换成大写
示例 : SELECT UPPER('hello') FROM dual;c
示例结果 : HELLO
### REPLACE(X,old,new)替换
在X中查找old，并替换成newcc
示例 : SELECT REPLACE('ABCDE','CD','AAA')FROM dual;
示例结果 : ABAAAE
## 二、数字函数
数字函数接受数字参数，参数可以来自表中的一列，也可以是一个数字表达式。
### ABS(X)绝对值
X的绝对值
示例 : ABS(-3)=3
### CEIL(X)取大
大于或等于X的最小值c
示例 : 大于或等于X的最小值
### FLOOR(X)取小
大于或等于X的最小值
示例 : FLOOR(5.8)=5
### ROUND(X[,Y]四舍五入
X在第Y位四舍五入
示例 : ROUND(3.456，2)=3.46
### TRUNC(X[,Y])截断
X在第Y位截断
示例 : TRUNC(3.456，2)=3.45
## 三、日期函数
日期函数对日期进行运算。常用的日期函数有：
### 1、ADD_MONTHS(d,n)
在某一个日期 d 上，加上指定的月数 n，返回计算后的新日期。
d 表示日期，n 表示要加的月数。
例：SELECT SYSDATE,last_day(SYSDATE) FROM dual;
### 2、LAST_DAY(d)c
2、LAST_DAY(d)
例：SELECT SYSDATE,last_day(SYSDATE) FROM dual;
## 四、转换函数
转换函数将值从一种数据类型转换为另外一种数据类型。常见的转换函数有：
### 1、TO_CHAR(d|n[,fmt])
把日期和数字转换为制定格式的字符串。Fmt是格式化字符串
代码演示：TO_CHAR对日期的处理
SELECT TO_CHAR(SYSDATE,'YYYY"年"MM"月"DD"日" HH24:MI:SS')"date" FROM dual;
### 2、TO_DATE(X,[,fmt])
把一个字符串以fmt格式转换成一个日期类型
### 3、TO_NUMBER(X,[,fmt])
把一个字符串以fmt格式转换为一个数字
代码演示：TO_NUM函数
SELECT TO_NUMBER('-$12,345.67','$99,999.99')"num" FROM dual;
## 五、其它单行函数
### 1、NVL(X,VALUE)
如果X为空，返回value，否则返回X

例：对工资是2000元以下的员工，如果没发奖金，每人奖金100元

代码演示：NVL函数

SQL> SELECT ENAME,JOB,SAL,NVL(COMM,100) FROM EMP WHERE SAL<2000;
### 2、COALESCE(X,VALUE)
如果X为空，返回value，否则返回X
## 六、聚合函数
聚合函数同时对一组数据进行操作，返回一行结果，比如计算一组数据的总和，平均值
等。
### AVG平均值
AVG（表达式）
### SUM求和
代码演示：SUM(表达式)
SQL> SELECT SUM(sal) FROM emp;
### MIN、MAX
MIN(表达式)、MAX(表达式)
### COUNT数据统计
COUNT（表达式）