---
title: oracle数据库连接
date: 2018-05-22 17:24:53
tags:
categories:
- oracle
---
有很多软件是直接连数据库的，oracle的好多直接连数据的方式都是采用net manager的。好处就是只要安装了oracle哪怕只是客户端，都很容易实现连接数据库。用起来方便，安全。挺不错的。
# 采用net manager配置
1,首先确认安装了oracle，8、9、10什么的哪个版本都行，最低也要安装个oracle的客户端。
![oracle图片](/images/oracle/create/connet/1.png)
2,选择netmanager进行如图所示界面
![oracle图片](/images/oracle/create/connet/2.png)
3,打开本地，进入服务命名
![oracle图片](/images/oracle/create/connet/3.png)
4,单击+号，选择创建，输入服务名，这里输入shujuku
![oracle图片](/images/oracle/create/connet/4.png)
5,单击下一步，出现如图所示界面，用默认为tcp/ip协议就行
![oracle图片](/images/oracle/create/connet/5.png)
6,填写上服务器的ip地址。注意劲量不用使用localhost这样的地址。
![oracle图片](/images/oracle/create/connet/6.png)
7,输入服务器的服务名
![oracle图片](/images/oracle/create/connet/7.png)


