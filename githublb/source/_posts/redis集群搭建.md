---
layout: _post
title: redis集群搭建
date: 2018-05-10 11:20:08
categories:
- redis
tags:
---
介于有人需要，总结一下，不保证对
## 1、下载redis-3.0.0.tar.gz (必须3.0.0及之后版本，之前版本不支持集群模式)
 下载：wget http://download.redis.io/releases/redis-3.0.0.tar.gz
 ```javascript
 tar -zxvf redis-3.0.0.tar.gz -C /usr/local/
 ```
 重命名(如果嫌名字太长，可重命名，我这里就改一下)
 ```javascript
 mv /usr/local/redis-3.0.0 /usr/local/redis3.0
 cd /usr/local/redis3.0
  ```
 编译
  ```javascript
 make
   ```
 未make install时，/usr/local/redis3.0/src/目录下没有redis-cli、redis-server 等启动脚本
   ```javascript
cd /usr/local/redis3.0/src/
   ```
## 2、安装
   ```javascript
make install
   ```
 是否安装成功，可观察/usr/local/redis3.0/src/目录下redis-cli、redis-server 等脚本
## 3、为了方便操作，同时不影响源文件，这里进行以下操作
   ```javascript
mkdir -p /usr/local/redis/bin
mv mkreleasehdr.sh redis-benchmark redis-check-aof redis-check-dump redis-cli redis-server /usr/local/redis/bin
   ```
## 4、创建redis-cluster文件夹，并在下面创建6个文件夹（一般集群最少6个，3主3从）
   ```javascript
mkdir -p /usr/local/redis-cluster
cd /usr/local/redis-cluster
mkdir 7001 7002 7003 7004 7005 7006
   ```
## 5、将/usr/local/redis3.0/redis.conf配置文件分别copy到700*文件下，分别进行修改
   ```javascript
cp /usr/local/redis3.0/redis.conf /usr/local/redis-cluster/7001
vi /usr/local/redis-cluster/7001/redis.conf
   ```
 修改如下：
    ```javascript
daemonize yes # 后台启动
port 7001 # 端口
bind 192.168.0.228 # 服务器IP
dir /usr/local/redis-cluster/7001/ # 数据存放位置，每个节点路径不一样
cluster-enabled yes # 启动集群模式
cluster-config-file nodes-7001.conf # 名称最好与端口一致
cluster-node-timeout 5000 # 5000毫秒
appendonly yes #
   ```
 简单操作
     ```javascript
sed 's/7001/7002/g' /usr/local/redis-cluster/7001/redis.conf > /usr/local/redis-cluster/7002/redis.conf
sed 's/7001/7003/g' /usr/local/redis-cluster/7001/redis.conf > /usr/local/redis-cluster/7003/redis.conf
sed 's/7001/7004/g' /usr/local/redis-cluster/7001/redis.conf > /usr/local/redis-cluster/7004/redis.conf
sed 's/7001/7005/g' /usr/local/redis-cluster/7001/redis.conf > /usr/local/redis-cluster/7005/redis.conf
sed 's/7001/7006/g' /usr/local/redis-cluster/7001/redis.conf > /usr/local/redis-cluster/7006/redis.conf
   ```
## 6、由于redis使用到ruby命令，先需安装ruby
     ```javascript
yum install ruby # 存在依赖，按y
yum install rubygems # 存在依赖，按y
gem install redis # 安装redis和ruby接口
   ```
安装失败 就进行下面操作:
     ```javascript
	 curl -L get.rvm.io | bash -s stable
	 gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
	 curl -sSL https://get.rvm.io | bash -s stable
	 source /usr/local/rvm/scripts/rvm
	 rvm list known
	 rvm install 2.3.0
	 rvm use 2.3.0
	 rvm remove 1.8.7
	 ruby --version
	 gem install redis
	 yum install -y rubygems
	    ```

## 7、启动6个redis
     ```javascript
/usr/local/redis/bin/redis-server /usr/local/redis-cluster/7001/redis.conf
/usr/local/redis/bin/redis-server /usr/local/redis-cluster/7002/redis.conf
/usr/local/redis/bin/redis-server /usr/local/redis-cluster/7003/redis.conf
/usr/local/redis/bin/redis-server /usr/local/redis-cluster/7004/redis.conf
/usr/local/redis/bin/redis-server /usr/local/redis-cluster/7005/redis.conf
/usr/local/redis/bin/redis-server /usr/local/redis-cluster/7006/redis.conf
	    ```
 查看是否启动完成
      ```javascript
ps -ef | grep redis
	    ```
 或
       ```javascript
netstat -tunpl | grep redis
	    ```
##  8、创建集群
       ```javascript
/usr/local/redis3.0/src/redis-trib.rb create --replicas 1 192.168.0.228:7001 192.168.0.228:7002 192.168.0.228:7003 192.168.0.228:7004 192.168.0.228:7005 192.168.0.228:7006
     ```
  create表示创建
  1=主/从(比值)
  按顺序
  主节点：192.168.0.228:7001 192.168.0.228:7002 192.168.0.228:7003
  从节点：192.168.0.228:7004 192.168.0.228:7005 192.168.0.228:7006
  192.168.0.228:7004 是 192.168.0.228:7001 的从节点
  创建过程会询问，yes即可
## 9、进入集群环境
  进入redis客户端，-c 表示集群模式，-h 表示服务器地址， -p 表示端口
       ```javascript
/usr/local/redis/bin/redis-cli -c -h 192.168.0.228 -p 7001
     ```
  如果拒绝连接
第一步启动
 ```javascript
redis-server /usr/local/redis-cluster/7001/redis.conf
redis-server /usr/local/redis-cluster/7002/redis.conf
redis-server /usr/local/redis-cluster/7003/redis.conf
redis-server /usr/local/redis-cluster/7004/redis.conf
redis-server /usr/local/redis-cluster/7005/redis.conf
redis-server /usr/local/redis-cluster/7006/redis.conf
redis-cli
     ```
然后进入集群环境
 ```javascript
/usr/local/redis/bin/redis-cli -c -h 192.168.0.228 -p 7001
     ```
  退出redis客户端
   ```javascript
192.168.0.228:7001> quit
     ```
  关闭redis服务
     ```javascript
/usr/local/redis/bin/redis-cli shutdown
     ```
  注：电脑性能不好，实际环境中自行改成不同机器IP即可收起
