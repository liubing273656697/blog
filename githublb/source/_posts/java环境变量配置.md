---
title: java环境变量配置
date: 2018-10-09 10:17:19
tags:
categories:
- config
---
配置环境变量
## 1 此电脑→属性→高级系统设置→高级→环境变量
![java图片](/images/config/java/1.png)
## 2 系统变量→新建 JAVA_HOME 变量 。
变量值填写jdk的安装目录（本人是 C:\software\util\java\jdk\java\jdk1.7.0_67)
![java图片](/images/config/java/2.png)
## 3 系统变量→寻找 Path 变量→编辑
在变量值最后输入 %JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;
（注意原来Path的变量值末尾有没有;号，如果没有，先输入；号再输入上面的代码）
![java图片](/images/config/java/3.png)
## 4 系统变量→新建 CLASSPATH 变量
变量值填写   .;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar（注意最前面有一点）系统变量配置完毕
![java图片](/images/config/java/4.png)
## 5 检验是否配置成功
 运行cmd 输入 java -version （java 和 -version 之间有空格）
![java图片](/images/config/java/5.png)
