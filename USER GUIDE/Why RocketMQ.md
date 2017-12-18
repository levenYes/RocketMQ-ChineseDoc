---
title: 为什么选择RocketMQ
date: 2017/12/18
categories: 文档翻译
---
## Motivation
At early stages, we constructed our distributed messaging middleware based on ActiveMQ 5.x(prior to 5.3). Our multinational business uses it for asynchronous communication, search, social network activity stream, data pipeline, even in its trade processes. As our trade business throughput rises, pressure originating from our messaging cluster also becomes urgent.

## 动机
在早期阶段，我们在基于ActiveMQ 5.x(早于5.3)构建我们的分布式消息中间件。我们的跨国业务使用它来实现异步通信、检索、社交网络活动流、数据管道，甚至是在交易过程中也在使用。随着我们的交易业务量增加，来自消息集群的压力也变得非常急迫。

## Why RocketMQ ?
Based on our research, with increased queues and virtual topics in use, ActiveMQ IO module reaches a bottleneck. We tried our best to solve this problem through throttling, circuit breaker or degradation, but it did not work well. So we begin to focus on the popular messaging solution Kafka at that time. Unfortunately, Kafka can not meet our requirements especially in terms of low latency and high reliability, see here for details.

## 为什么选择RocketMQ
根据我们的研究，随着使用中的队列越来越长、虚拟主题越来越多，ActiveMQ的IO模型会到达一个瓶颈。我们曾经努力尝试，试图通过节流、断路器或者降级这些手段来解决这个问题，但是都没有很好的效果。因此，我们开始注意到当时非常流行的消息解决方案，Kafka。不幸的是，Kafka并不能满足我们的需求，尤其是在低延迟和高可用这两点上，点击链接进一步了解细节。

In this context, we decided to invent a new messaging engine to handle a broader set of use cases, ranging from traditional pub/sub scenarios to high volume real-time zero-loss tolerance transaction system. We believe this solution can be beneficial, so we would like to open source it to the community. Today, more than 100 companies are using the open source version of RocketMQ in their business. We also published a commercial distribution based on RocketMQ, a PaaS product called the Alibaba Cloud Platform.

在这样的情况下，我们决定写一个全新的消息引擎来处理这一类用途更广泛的案例，无论是传统的发布/订阅情景，还是高流量实时零差错交易系统。我们坚信这个解决方案能带来好处，所以我们非常乐意把它向社区开源。今时今日，已经有超过100家企业在他们的业务里使用开源版本的RocketMQ。我们也在阿里云平台发布了一个基于RocketMQ的商业版PaaS产品。

The following table demonstrates the comparison between RocketMQ, ActiveMQ and Kafka (Apache’s most popular messaging solutions according to awesome-java)

下面这张表格对比了RocketMQ、ActiveMQ和Kafaka(Apache里最受欢迎的基于java的消息解决方案)

## RocketMQ vs. ActiveMQ vs. Kafka
(译者注：表格比较复杂，暂且搁置，延后处理)
