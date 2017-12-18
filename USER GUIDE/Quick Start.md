---
title: 快速开始
date: 2017/12/18
categories: 文档翻译
---
## Quick Start
This quick start guide is a detailed instruction of setting up RocketMQ messaging system on your local machine to send and receive messages.

## 快速开始
快速开始指引由详细的命令组成，告诉你如何在本机配置RocketMQ消息投递系统并且收发消息。

## Prerequisite
The following softwares are assumed installed:
1. 64bit OS, Linux/Unix/Mac is recommended;
2. 64bit JDK 1.8+;
3. Maven 3.2.x
4. Git

## 前期必要的准备
以下软件必须安装：
1. 64位操作系统，linux/Unit/Mac（推荐）；
2. 64位JDK，1.8+；
3. Maven 3.2.x
4. Git

## Clone & Build
```
  > git clone -b develop https://github.com/apache/rocketmq.git
  > cd rocketmq
  > mvn -Prelease-all -DskipTests clean install -U
  > cd distribution/target/apache-rocketmq
```

## 克隆代码仓库和构建程序
```
  > git clone -b develop https://github.com/apache/rocketmq.git
  > cd rocketmq
  > mvn -Prelease-all -DskipTests clean install -U
  > cd distribution/target/apache-rocketmq
```
(译者注：命令集合无需翻译。下同。)

## Start Name Server
```
  > nohup sh bin/mqnamesrv &
  > tail -f ~/logs/rocketmqlogs/namesrv.log
  The Name Server boot success...
```

## 启动Name Server
```
  > nohup sh bin/mqnamesrv &
  > tail -f ~/logs/rocketmqlogs/namesrv.log
  The Name Server boot success...
```

## Start Broker
```
  > nohup sh bin/mqbroker -n localhost:9876 &
  > tail -f ~/logs/rocketmqlogs/broker.log
  The broker[%s, 172.30.30.233:10911] boot success...
```

## 启动Broker
```
  > nohup sh bin/mqbroker -n localhost:9876 &
  > tail -f ~/logs/rocketmqlogs/broker.log
  The broker[%s, 172.30.30.233:10911] boot success...
```

## Send & Receive Messages
Before sending/receiving messages, we need to tell clients the location of name servers. RocketMQ provides multiple ways to achieve this. For simplicity, we use environment variable NAMESRV_ADDR
```
 > export NAMESRV_ADDR=localhost:9876
 > sh bin/tools.sh org.apache.rocketmq.example.quickstart.Producer
 SendResult [sendStatus=SEND_OK, msgId= ...

 > sh bin/tools.sh org.apache.rocketmq.example.quickstart.Consumer
 ConsumeMessageThread_%d Receive New Messages: [MessageExt...
```

## 发送和接收消息
在发送或接收消息之前，我们需要通知客户端name servers的位置。RocketMQ提供多种实现方式。为简单起见，我们现在展示环境变量NAMESRV_ADDR的用法
```
 > export NAMESRV_ADDR=localhost:9876
 > sh bin/tools.sh org.apache.rocketmq.example.quickstart.Producer
 SendResult [sendStatus=SEND_OK, msgId= ...

 > sh bin/tools.sh org.apache.rocketmq.example.quickstart.Consumer
 ConsumeMessageThread_%d Receive New Messages: [MessageExt...
```

## Shutdown Servers
```
> sh bin/mqshutdown broker
The mqbroker(36695) is running...
Send shutdown request to mqbroker(36695) OK

> sh bin/mqshutdown namesrv
The mqnamesrv(36664) is running...
Send shutdown request to mqnamesrv(36664) OK
```

## 关闭所有服务器
```
> sh bin/mqshutdown broker
The mqbroker(36695) is running...
Send shutdown request to mqbroker(36695) OK

> sh bin/mqshutdown namesrv
The mqnamesrv(36664) is running...
Send shutdown request to mqnamesrv(36664) OK
```
