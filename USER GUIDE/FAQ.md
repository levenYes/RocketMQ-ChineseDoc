---
title: 常见问题
date: 2017/12/21
categories: 文档翻译
---

## Frequently Asked Questions
The following questions are frequently asked with regard to the RocketMQ project in general.

## 常见问题
以下是一般情况下关于RocketMQ项目的常见问题。

## General
1. Why did we create rocketmq project instead of selecting other products?
Please refer to Why RocketMQ

## 一般问题
1. 为什么我们要开发RocketMQ项目，而不是选择其他的产品？
请参考《为什么是RocketMQ》。

2. Do I have to install other softewares, such as zookeeper, to use RocketMQ?
No. RocketMQ can run independently.

2. 如果要使用RocketMQ，我必须安装其他的软件吗，例如Zookeeper？
不需要。RocketMQ可以独立运行。

## Usage
1. Where does the newly created Consumer ID start consuming messages?
  1. If the topic sends a message within three days, then the consumer start consuming messages from the first message saved in the server.
  2. If the topic sends a message three days ago, the consumer starts to consume messages from the latest message in the server, in other words, starting from the tail of message queue.
  3. If such consumer is rebooted, then it starts to consume messages from the last consumption location.

## 使用问题
1. 新建消费者ID从哪里开始消费消息？
  1. 如果消息是该主题在3天之内发送的，那么消费者会从保存在服务端的第一条消息开始消费。
  2. 如果消息是主题在3天之前发送的，那么消费者会从保存在服务端的最后一条消息开始消费，换句话来说，从消息队列的队尾开始。
  3. 如果消费者重启，那么它会从重启前最后一个消费位置开始。

2. How to reconsume message when consumption fails?
  1. Cluster consumption pattern. The consumer business logic code returns Action.ReconsumerLater, NULL, or throws an exception, if a message failed to be consumed, it will retry for up to 16 times, after that, the message would be descarded【译者注：原文有错，应为discarded】.
  2. Broadcast consumption pattern. The broadcaset consumption still ensures that a message is consumered at least once, but no resend option is provided.

2. 如果消费失败，要如何重新消费消息？
  1. 集群消费模式。如果一条消息消费失败，消费业务逻辑代码会返回Action.ReconsumerLater、NULL或者抛出一个异常，将会重新尝试消费，尝试次数最多可达16次。在此之后，如果还是消费失败，这条消息将会被丢弃。
  2. 广播消费模式。广播消费仍然会确保一条消息至少被消费一次，但是不提供重新发送选项。

3. How to query the failed message if there is a consumption failure?
  1. Using topic query by time, you can query messages within a period of time.
  2. Using Topic and Message Id to accurately query the message.
  3. Using Topic and Message Key accurately query a class of messages with the same Message Key.

3. 如果发生消费失败，要如何查询消费失败的消息？
  1. 根据时间使用主题查询，你可以查询到在一段时间内消费失败的消息。
  2. 使用主题和消息ID可以准确查询到该消息。
  3. 使用主题和消息键可以准确地查询同一类有相同消息键的消息。

4. Are messages delivered exactly once?
RocketMQ ensures that all messages are delivered at least once. In most cases, the messages are not repeated.

4. 消息送达是否有且仅有一次？
RocketMQ确保所有的消息至少送达一次。在绝大多数情况下，消息不会重复。

5. How to add a new broker?
  1. Start up a new broker and register it to the same list of name servers.
  2. By default, only internal system topics and consumer groups are created automatically. If you would like to have your business topic and consumer groups on the new node, please replicate them from the existing broker. Admin tool and command lines are provided to handle this.

5. 如何添加y一个新boker？
  1. 启动一个新的boker，然后在相应的name servers所在的list上注册。
  2. 默认情况下，只有内部系统主题和消费者群组会自动被创建。如果你想在新节点上使用你自己的业务主题和消费者群组，请从已有的broker上复制它们。RocketMQ提供了Admin管理工具和命令行消息来满足这些需求。

## Configuration related
The following answers are all default values and can be modified by configuration.

## 配置相关问题
以下答案都是默认值，可以通过修改配置文件进行更改。

1. How long are the messages saved on the server?
Stored messages are will be saved for up to 3 days, and messages that are not consumed for more than 3 days will be deleted.

1. 消息会在服务器上保存多久？
存储的消息会最多保存3天，如果3天内都没有被消费则会被删除。

2. What is the size limit for message Body?
Generally 256KB.

2. 消息体的大小限制是？
通常是256KB。

3. How to set the number of consumer threads?
When you start Consumer, set a ConsumeThreadNums property, example is as follows:
```
consumer.setConsumeThreadMin(20);
consumer.setConsumeThreadMax(20);
```

3. 如何设置消费者线程的数量？
启动消费者服务时，设置ConsumeThreadNums属性，举例如下：
```
consumer.setConsumeThreadMin(20);
consumer.setConsumeThreadMax(20);
```

## Errors
1. If you start a producer or consumer failed and the error message is producer group or consumer repeat?
Reason：Using the same Producer /Consumer Group to launch multiple instances of Producer/Consumer in the same JVM may cause the client fail to start.
Solution: Make sure that a JVM corresponding to one Producer /Consumer Group starts only with one Producer/Consumer instance.

## 错误
1. 如果你启动一个生产者服务或消费者服务时失败了，错误消息是生产者群组重复或消费者重复？
原因：使用相同的生产者、消费者群组在同一个JVM初始化多个生产者/消费者示例可能会导致客户端启动失败。
解决方法：确保一个JVM对应一个生产者/消费者群组只启动一个生产者/消费者实例。

2. If consumer failed to start loading json file in broadcast mode?
Reason: Fastjson version is too low to allow the broadcast consumer to load local offsets.json, causing the consumer boot failure. Damaged fastjson file can also cause the same problem.
Solution: Fastjson version has to be upgraded to rocketmq client dependent version to ensure that the local offsets.json can be loaded. By default offsets.json file is in /home/{user}/.rocketmq_offsets. Or check the integrity of fastjson.

2. 如果在广播模式，消费者在开始加载JSON文件时失败，要怎么办？
原因：Fastjson的版本太低，以至于无法允许广播消费者加载offsets.json文件，导致该消费者启动失败。损坏的fastjson文件也会导致同样的问题。
解决方法：Fastjson的版本要升级到rocketmq客户端依赖版本，以确保本地offsets.json文件可以被正常加载。默认条件下offsets.json文件存放位置是/home/{user}/.rocketmq_offsets。或者检查一下fastjson是否完整。
