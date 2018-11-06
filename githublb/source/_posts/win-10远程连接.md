---
title: win 10远程连接
date: 2018-10-08 12:49:32
tags:
categories:
- win 10
---
远程连接不上问题解决方案
# problem
    远程访问提示错误如下：访问服务器提示错误发生身份验证错误，要求函数不受支持
![connect图片](/images/connect/desktop/problem/1.png)
## 1,打开编辑器
    打开本地策略编辑器，计算机开始输入：gpedit.msc
![connect图片](/images/connect/desktop/answer/1.png)
## 2,选择计算机配置
    选择计算机配置下管理模板-->系统-->凭据分配下加密Oracle修正。
![connect图片](/images/connect/desktop/answer/2.png)
## 3,修改
    选择已启用，保护级别设置为易受攻击，重新登录即可正常登陆。
![connect图片](/images/connect/desktop/answer/3.png)


