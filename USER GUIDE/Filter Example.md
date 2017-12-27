---
title: 消息过滤器示例
date: 2017/12/21
categories: 文档翻译
---

## Filter Example
In most cases, tag is a simple and useful design to select message you want. For example:
```
DefaultMQPushConsumer consumer = new DefaultMQPushConsumer("CID_EXAMPLE");
consumer.subscribe("TOPIC", "TAGA || TAGB || TAGC");
```

## 消息过滤器示例
在大多数情况下，标签是一种帮助你选择所需消息的简单有用的设计。例如：
```
DefaultMQPushConsumer consumer = new DefaultMQPushConsumer("CID_EXAMPLE");
consumer.subscribe("TOPIC", "TAGA || TAGB || TAGC");
```

The consumer will recieve messages that contains TAGA or TAGB or TAGC. But the limitation is that one message only can have one tag, and this may not work for sophisticated scenarios. In this case, you can use SQL expression to filter out messages.

该消费者会接收含有TAGA、TAGB或TAGC标签的消息。但是，因为有一条消息只能打一个标签的限制，在复杂的场景下可能会失效。在这种情况下，你可以用SQL表达式来过滤消息。

### Principle
SQL feature could do some calculation through the properties you put in when sending messages. Under the grammars defined by RocketMQ, you can implement some interesting logic. Here is an example:
![将文字转为图片](http://p0zshkwk1.bkt.clouddn.com/17-12-21/54128752.jpg)

### 规则
根据你在发送消息时设定的属性，SQL特性可以做相应的运算。遵循RocketMQ预设的语法，你可以实现一些有趣的逻辑。这里有一个例子：
![将文字转为图片](http://p0zshkwk1.bkt.clouddn.com/17-12-21/54128752.jpg)

### Grammars
RocketMQ only defines some basic grammars to support this feature. You could also extend it easily.

1. Numeric comparison, like >, >=, <, <=, BETWEEN, =;
2. Character comparison, like =, <>, IN;
3. IS NULL or IS NOT NULL;
4. Logical AND, OR, NOT;

Constant types are:
1. Numeric, like 123, 3.1415;
2. Character, like ‘abc’, must be made with single quotes;
3. NULL, special constant;
4. Boolean, TRUE or FALSE;

### 语法
RocketMQ只预设了一些基本的语法来支持这个特性。你也可以很容易地扩展它。
1. Numeric comparison, like >, >=, <, <=, BETWEEN, =;
2. Character comparison, like =, <>, IN;
3. IS NULL or IS NOT NULL;
4. Logical AND, OR, NOT;

以下是基本类型：
1. Numeric, like 123, 3.1415;
2. Character, like ‘abc’, must be made with single quotes;
3. NULL, special constant;
4. Boolean, TRUE or FALSE;

### Usage constraints
Only push consumer could select messages by SQL92. The interface is:
```
public void subscribe(final String topic, final MessageSelector messageSelector)
```

### 使用限制
只有push类型的消费者可以通过SQL92标准的语句来筛选消息。接口如下：
```
public void subscribe(final String topic, final MessageSelector messageSelector)
```

### Producer example
You can put properties in message through method putUserProperty when sending.
```
DefaultMQProducer producer = new DefaultMQProducer("please_rename_unique_group_name");
producer.start();

Message msg = new Message("TopicTest",
    tag,
    ("Hello RocketMQ " + i).getBytes(RemotingHelper.DEFAULT_CHARSET)
);
// Set some properties.
msg.putUserProperty("a", String.valueOf(i));

SendResult sendResult = producer.send(msg);

producer.shutdown();
```

### 生产者示例
在发送消息时，你可以使用putUserProperty()方法为消息设置属性。
```
DefaultMQProducer producer = new DefaultMQProducer("please_rename_unique_group_name");
producer.start();

Message msg = new Message("TopicTest",
    tag,
    ("Hello RocketMQ " + i).getBytes(RemotingHelper.DEFAULT_CHARSET)
);
// Set some properties.
msg.putUserProperty("a", String.valueOf(i));

SendResult sendResult = producer.send(msg);

producer.shutdown();
```

### Consumer example
Use MessageSelector.bySql to select messages through SQL92 when consuming.
```
DefaultMQPushConsumer consumer = new DefaultMQPushConsumer("please_rename_unique_group_name_4");

// only subsribe messages have property a, also a >=0 and a <= 3
consumer.subscribe("TopicTest", MessageSelector.bySql("a between 0 and 3");

consumer.registerMessageListener(new MessageListenerConcurrently() {
    @Override
    public ConsumeConcurrentlyStatus consumeMessage(List<MessageExt> msgs, ConsumeConcurrentlyContext context) {
        return ConsumeConcurrentlyStatus.CONSUME_SUCCESS;
    }
});
consumer.start();
```

### 消费者示例
在消费消息时，使用MessageSelector.bySql()方法和遵循SQL92标准的语句来筛选消息
```
DefaultMQPushConsumer consumer = new DefaultMQPushConsumer("please_rename_unique_group_name_4");

// only subsribe messages have property a, also a >=0 and a <= 3
consumer.subscribe("TopicTest", MessageSelector.bySql("a between 0 and 3");

consumer.registerMessageListener(new MessageListenerConcurrently() {
    @Override
    public ConsumeConcurrentlyStatus consumeMessage(List<MessageExt> msgs, ConsumeConcurrentlyContext context) {
        return ConsumeConcurrentlyStatus.CONSUME_SUCCESS;
    }
});
consumer.start();
```
