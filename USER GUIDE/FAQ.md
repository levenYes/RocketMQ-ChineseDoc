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

5. 如何添加一个新boker？
  1. 启动一个新的boker，然后在相应的name servers所在的list上注册。
  2. 默认情况下，只有内部系统主题和消费者群组会自动被创建。如果你想在新节点上使用你自己的业务主题和消费者群组，请从已有的broker上复制它们。RocketMQ提供了Admin管理工具和命令行来满足这些需求。

## Configuration related
The following answers are all default values and can be modified by configuration.

## 配置相关问题
以下回答都是基于默认值不变的情况，默认值可以通过修改配置文件进行更改。

1. How long are the messages saved on the server?
Stored messages are will be saved for up to 3 days, and messages that are not consumed for more than 3 days will be deleted.

1. 消息会在服务器上保存多久？
存储的消息会最多保存3天，如果3天之后还没有被消费则会被删除。

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
1. 如果你启动一个生产者服务或消费者服务时失败了，错误提示是生产者群组重复或消费者群组重复，要如何处理？
原因：使用相同的生产者、消费者群组在同一个JVM初始化多个生产者/消费者示例可能会导致客户端启动失败。
解决方法：确保一个JVM对应一个生产者/消费者群组只启动一个生产者/消费者实例。

2. If consumer failed to start loading json file in broadcast mode?
Reason: Fastjson version is too low to allow the broadcast consumer to load local offsets.json, causing the consumer boot failure. Damaged fastjson file can also cause the same problem.
Solution: Fastjson version has to be upgraded to rocketmq client dependent version to ensure that the local offsets.json can be loaded. By default offsets.json file is in /home/{user}/.rocketmq_offsets. Or check the integrity of fastjson.

2. 如果在广播模式，消费者在开始加载JSON文件时失败，要如何处理？
原因：Fastjson的版本太低，以至于无法允许广播消费者加载offsets.json文件，导致该消费者启动失败。损坏的fastjson文件也会导致同样的问题。
解决方法：Fastjson的版本要升级到rocketmq客户端依赖版本，以确保本地offsets.json文件可以被正常加载。默认条件下offsets.json文件存放位置是/home/{user}/.rocketmq_offsets。或者检查一下fastjson是否完整。

3. What is the impact of a broker crash?
Master crashes
Messages can no longer be sent to this broker set, but if you have another broker set available, messages can still be sent given the topic is present. Messages can still be consumed from slaves.

3. broker崩溃有什么影响？
Master崩溃
如果你有另外一个broker集合可用，那么消息就可以不再发送到这个已经崩溃的boker集合。目前，消息仍然可以被发送到原来的主题。消息仍然可以通过slaves被消费。

Some slave crash
As long as there is another working slave, there will be no impact on sending messages. There will also be no impact on consuming messages except when the consumer group is set to consume from this slave preferably. By default, comsumer group consumes from master.

部分salve崩溃
只要还有另外一个正在运作的slave，就不会对发送消息有任何影响。消费消息也不会受到影响，除非消费者群组设置了优先消费该slave的消息。默认情况下，消费者群组优先消费来自master的消息。

All slaves crash
There will be no impact on sending messages to master, but, if the master is SYNC_MASTER, producer will get a SLAVE_NOT_AVAILABLE indicating that the message is not sent to any slaves. There will also be no impact on consuming messages except that if the consumer group is set to consume from slave preferably. By default, comsumer group consumes from master.

所有slaves崩溃
发送消息到master不会有任何影响，但是，如果master设置了SYNC_MASTER，那么生产者将会收到一个SLAVE_NOT_AVAILABLE的提醒，提醒说该消息未能发送到任意一个slaves。消费消息也不会受到影响，除非消费者群组设置了优先消费该slave的消息。默认情况下，消费者群组优先消费来自master的消息。

4. Producer complains “No Topic Route Info”, how to diagnose?
This happens when you are trying to send messages to a topic whose routing info is not available to the producer.

4. 生产者报告“没有主题路由信息”，如何判断哪里出了问题？
这种问题会发生在当你尝试发送消息到一个主题而这个主题的路由信息对该生产者不可用的时候。

  1. Make sure that the producer can connect to a name server and is capable of fetching routing meta info from it.
  2. Make sure that name servers do contain routing meta info of the topic. You may query the routing meta info from name server through topicRoute using admin tools or web console.
  3. Make sure that your brokers are sending heartbeats to the same list of name servers your producer is connecting to.
  4. Make sure that the topic’s permssion is 6(rw-), or at least 2(-w-).

  1. 确定生产者可以连接到name server，而且能够从该处获取路由元信息。
  2. 确定name servers的确保存了主题的路由元信息。通过使用admin工具或者web控制台，你可以根据topicRoute查询name server的路由元信息。
  3. 确定你的brokers正在发送heartbeats给同一list上的name servers，而且你的生产者正在跟它们连接着。
  4. 确定该主题的权限是6(rw-)，或者至少是2(-w-).

If you can’t find this topic, create it on a broker via admin tools command updateTopic or web console.

如果你找不到这个主题，那就通过admin工具命令updateTopic或者web控制台在一个boker上创建它。
