入门篇

## 1、MQ基本概念

### 1.1 概述

MQ全称Message Queue（消息队列），是在消息的传输过程中保存消息的容器，多用于分布式系统之间进行通信。

总结：

- MQ，消息列表，存储消息的中间件
- 分布式系统通信的两种方式：**直接远程调用**和**借助第三方**完成间接通信
- 发送方称为**生产者**，接收方称为**消费者**。



### 1.2 MQ的优势和劣势

#### 优势

- **应用解耦**：使用MQ使得**应用间解耦**，**提升容错性**和**可维护性**。举个例子，如订单系统需要调用库存系统、支付系统、物流系统完成服务，如果通过远程调用，二者就耦合在一起了，如果使用MQ，订单系统将消息发送到MQ，其它系统去MQ中取出消息进行消费即可，大大降低了模块之间的耦合。且如果需要加入新系统，原订单系统也无需修改代码，只需要让新模块同样去MQ中取出订单系统的消息消费即可。

- **异步提速**：使用MQ提升**用户体验**和**系统吞吐量**。举个例子，如用户下订单，根据传统的执行流程，首先需要花费300ms调用库存系统，再花费300ms调用支付系统，再花费300ms调用物流系统，最后花费20ms写入数据库。这样依赖就花费了大约1s时间，较为影响用户体验，因此，如果使用MQ，则用户下完订单，花费20ms写入数据库，花费5ms将消息发送至MQ中，然后返回成功消息，一共花费25ms，而其它系统自行取消息执行，时间不计入。这样依赖就大大降低了用户等待时间，提升了响应速度。

- **削峰填谷**：使用MQ**提高系统稳定性**。举个例子，如某网站推出十二点准时一元抢家电活动，等到十二点，一瞬间有五千个请求涌入，如果直接用服务器去接收这些流量，服务器会瞬间宕机，严重影响用户体验。如果使用MQ，作为中间间缓存请求，即让请求首先访问的是MQ，而不是直接打到服务器，然后让服务器去按处理阈值去MQ中取请求进行处理，这样服务器就能正常工作，这就是MQ削峰，服务器填谷。

  

#### 劣势

- **系统可用性降低**：传统方式使用远程调用只要考虑两个系统的可用性，如果使用MQ，则还需要考虑MQ的工作状态。即系统引入的外部依赖越多，系统稳定性越差。一旦MQ宕机，就会对业务造成严重影响，所以还需要考虑MQ的高可用。
- **系统复杂度提高**：使用了MQ，还需要考虑很多可能发生的问题，如如何保证消息没有被重复消费，如何处理消息丢失情况，如何保证消息传递的顺序性等等问题，对这些问题的处理将大大增加系统的复杂度。
-  **一致性问题**：举个例子，如A系统处理完业务，通过MQ给B，C，D三个系统发送消息数据，如果B系统、C系统处理成功，D系统处理失败，这个就需要探讨一致性问题的处理，如何保证数据消息处理的一致性问题。



#### 总结

使用MQ既有优势也有劣势，使用MQ最好满足以下条件再考虑使用：

1. 生产者**不需要从消费者处获得反馈**。即使用MQ允许异步的条件是生产者发送完消息后不需要考虑消费者的返回值，这才让所谓异步称为可能。
2. **容许短暂的不一致性**。
3. 使用后**效益大于成本**。即解耦、提速、削峰这些方面的收益，超过加入MQ带来的系统成本。



### 1.3 常见的MQ产品

目前业界有很多的MQ产品，例如RabbitMQ、RocketMQ、ActiveMQ、Kafka、ZeroMQ、MetaMQ等，当然，也有直接使用Redis充当消息队列的案例，而这些消息队列产品，**各有侧重**，在实际选型时，需要结合自身需求及MQ产品特征，**综合考虑**。

|                |                           RabbitMQ                           |                ActiveMQ                 |         RocketMQ         |                     Kafka                      |
| :------------: | :----------------------------------------------------------: | :-------------------------------------: | :----------------------: | :--------------------------------------------: |
|   公司/社区    |                            Rabbit                            |                 Apache                  |         阿里巴巴         |                     Apache                     |
|    开发语言    |                            Erlang                            |                  Java                   |           Java           |                   Scale&Java                   |
|    协议支持    |                   AMOP、XMPP、SMTP、STOMP                    |    OpenWire，STOMP，REST，XMPP，AMQP    |          自定义          |       自定义协议，社区封装了HTTP协议支持       |
| 客户端支持语言 | 官方支持Erlang、Java、Ruby等，社区产出多种API，几乎支持所有语言 | Java，C，C++，Python，PHP，perl，.net等 |   Java，C++（不成熟）    |  官方支持Java，社区产出多种API，如PHP，Python  |
|   单机吞吐量   |                         万级（其次）                         |              万级（最差）               |      十万级（最好）      |                 十万级（次之）                 |
|    消息延迟    |                            微秒级                            |                 毫秒级                  |          毫秒级          |                    毫秒以内                    |
|    功能特性    |    并发能力强，性能极其好，延时低，社区活跃，管理界面丰富    |      老牌产品，成熟度高，文档较多       | MQ功能比较完备，扩展性佳 | 只支持主要的MQ功能，毕竟是为大数据领域准备的。 |



### 1.4 RabbitMQ简介

介绍RabbitMQ前，先介绍下**AMQP协议**，即Advanced Message Queuing Protocol（高级消息队列协议）是一个网络协议，是**应用层协议**的一个**开放标准**，为面向消息的中间件设计规范。基于此协议的客户端与消息中间件可传递消息，并不受客户端/中间件不同产品。不同的开发语言等条件的限制，该协议规范于2006年发布。

![image-20220829112801439](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20220829112801439.png)

在2007年，Rabbit公司基于AMQP标准开发的RabbitMQ1.0发布。RabbitMQ采用Erlang语言开发。架构如下：

![img](https://img2020.cnblogs.com/blog/1552936/202010/1552936-20201024103921637-693350551.png)

其中的相关概念为：

- **Broker**：**接收和分发消息**的应用，RabbitMQ Server就是Message Broker
- **Virtual host**：基于**多租户**和**安全因素**设计的，把AMQP的基本组件划分到一个虚拟的分组中，类似于网络中的namespace概念。当多个不同的用户使用同一个RabbitMQ Server提供的服务时，可以划分出多个vhost，每个用户在自己的vhost创建exchange / queue等。
- **Connection**：publisher / consumer 和 broker之间的**TCP连接**。
- **Channel**：如果每一次访问RabbitMQ都建立一个Connection，在消息量大的时候建立 TCP Connection 的开销将是巨大的，效率也较低。Channel是在connection内部建立的**逻辑连接**，如果应用程序支持多线程，通常每个thread创建**单独**的channel进行通信，AMQP method包含了channel id帮助客户端和message broker 识别channel，所以channel之间是完全**隔离**的。Channel作为轻量级的Connection极大减少了操作系统建立TCP connection的开销。
- **Exchange**：message到达broker的第一站，根据分发规则，匹配查询表中的routing key，分发消息到queue中去。常用的类型有：**direct**（point-to-point），**topic**（public-subscribe），**fanout**（multicast）。
- **Queue**：消息最终被送到这里等待**consumer**取走。
- **Binding**：exchange和queue之间的**虚拟连接**，binding中可以**包含routing key**。Binding信息被保存到exchange中的**查询表**中，用于message的分发依据。



RabbitMQ提供了六种工作模式：**简单模式**、**work queues**、**Publish/Subscribe发布与订阅模式**、**Routing路由模式**、**Topic主题模式**、**RPC远程调用模式**（远程调用，不太算MQ，不作介绍）。



## 2、RabbitMQ的安装和配置



## 3、RabbitMQ简单模式快速入门

使用 Java 代码测试一下RabbitMQ的**简单模式**。首先在项目中引入RabbitMQ的客户端依赖（操作RabbitMQ），而RabbitMQ的服务要启动起来。

```xml
<dependency>
	<groupId>com.rabbitmq</groupId>
    <artifactId>amqp-client</artifactId>
    <version>5.6.0</version>
</dependency>
```

然后根据MQ架构来编写**生产者**相关代码：

### 1、生产者代码

```java
//1、创建连接工厂
ConnectionFactory factory = new ConnectionFactory();
//2、设置参数
factory.setHost("192.168.xx.xx");//ip地址 默认值为Localhost(不设置情况下)
factory.setPort(5672);//端口 默认值为5672
factory.setVirtualHost("/");//设置虚拟机 默认为/
factory.setUsername("guest");//用户名 默认为guest
factory.setPassword("guest");//密码 默认为guest
//3、创建连接 Connection
Connection connection = factory.newConnection();
//4、创建Channel
Channel channel = connection.createChannel();
//5、创建队列Queue
/*
*  队列创建方法: queueDeclare(String queue, boolean durable, boolean exclusive, boolean autoDelete, Map<String, Object> arguements):没有该队列时自动创建该队列
*  参数:
*	1、queue: 队列名称
*	2、durable: 是否持久化，即当mq重启之后，其中的消息还在
*	3、exclusive: 是否独占，即是否只能有一个消费者监听这队列，当Connection关闭时，是否删除队列。
*	4、autoDelete: 是否自动删除。即当没有Consumer时，自动删除队列。
*	5、arguments: 删除的一些参数
*/
channel.queueDeclare("hello_world", true, false, false, null);
//6、发送消息
/*
*  消息发送方法:  basicPublish(String exchange, String routingKey, BasicProperties props, byte[] body)
*  参数:
*	1、exchange: 交换机名称。简单模式下交换机会使用默认的""
*	2、routingKey: 路由名称。
*	3、props: 配置信息
*	4、body: 发送消息数据
*
*/
channel.basicPublic("", "hello_world", null, "hello rabbitmq".getBytes());
//7、释放资源
channel.close();
connection.close();
```

### 2、消费者代码

```java
//1、创建连接工厂
ConnectionFactory factory = new ConnectionFactory();
//2、设置参数
factory.setHost("192.168.xx.xx");//ip地址 默认值为Localhost(不设置情况下)
factory.setPort(5672);//端口 默认值为5672
factory.setVirtualHost("/");//设置虚拟机 默认为/
factory.setUsername("guest");//用户名 默认为guest
factory.setPassword("guest");//密码 默认为guest
//3、创建连接 Connection
Connection connection = factory.newConnection();
//4、创建Channel
Channel channel = connection.createChannel();
//5、接收消息
//创建回调对象在接收到消息后进行方法回调
Consumer consumer = new DefaultConsumer(channel){
    /*
    *  覆写回调方法，当受到消息后，会自动执行该方法
    *	参数:
    *	 1、consumerTag: 唯一标识
    *	 2、envelope: 获取一些信息，交换机，路由Key...
    *	 3、properties: 配置信息
    *	 4、body: 消息数据
    */
    @Override
    public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body){
        System.out.println("consumerTag:" + consumerTag);
        System.out.println("Exchange:" + envelope.getExchange());
        System.out.println("RoutingKey:" + envelope.getRoutingKey());
        System.out.println("properties:" + properties);
        System.out.println("body:" + new String(body));
    }
}
/*
*  监听方法: basicConsume(String queue, boolean autoAck, Consumer callback)
*	参数:
*	  1、queue: 队列名称
*	  2、autoAck: 是否自动确认
*	  3、callback: 回调对象
*/
channel.basicConsume("hello_world", true, consumer);
//释放资源?不用
```



## 4、RabbitMQ的工作模式

上面已经对简单模式进行了测试。不难看出，RabbitMQ的各种工作模式其实就是消息的**路由策略**和**分发方式**不一样。

### 4.1、Work queues 工作队列模式

![RabbitMQ的6种消息类型实现_GoslingJ的博客-CSDN博客_rabbitmq 消息类型](https://img-blog.csdnimg.cn/20200310001253725.png)

Work queues工作队列模式相较于简单模式，只是在**多了一个或多个消费者**，这些消费者**竞争**同一个队列中的消息。对于**任务较多**情况或**任务过重**的情况下使用工作队列可以**提高任务处理的速度**。



### 4.2、Pub / Sub 订阅模式

![Spring Boot消息队列应用实践- JeffWong - 博客园](https://images2018.cnblogs.com/blog/32361/201804/32361-20180430112647832-1604774090.png)

Pub / Sub订阅模型中，多了一个**Exchange**角色（其实其它模式也有，只不过使用的是默认交换机），在使用时与简单模式和工作队列模式略有区别。

如上图中，简单介绍下：

- P：**生产者**，也就是要发送消息的程序，但是不再直接发送消息到队列中，而是发给X（交换机）

- C：**消费者**，消息的接收者，会一直监听队列等待消息。
- Queue：**消息队列**，接收消息，缓存消息。
- Exchange：**交换机**（X），一方面，接收生产者发送的消息。另一方面，直到如何处理消息，例如递交给某个特定队列、递交给所有队列、或是将消息丢弃。到底如何操作，取决于Exchange的类型。Exchange常见类型有以下三种类型：
  - Fanout：**广播**，将消息交给所有绑定到交换机的队列
  - Direct：**定向**，把消息交给符合指定routing key的队列
  - Topic：**通配符**，把消息交给符合routing pattern（路由模式）的队列

Exchange（交换机）只负责**转发消息**，并不具备存储消息的能力，因此如果没有任何队列与Exchange绑定，或者没有符合路由规则的队列，那么消息会**丢失**。

#### 4.2.1、生产者代码（Fanout模式）

```java
//1、创建连接工厂
ConnectionFactory factory = new ConnectionFactory();
//2、设置参数
factory.setHost("192.168.xx.xx");//ip地址 默认值为Localhost(不设置情况下)
factory.setPort(5672);//端口 默认值为5672
factory.setVirtualHost("/");//设置虚拟机 默认为/
factory.setUsername("guest");//用户名 默认为guest
factory.setPassword("guest");//密码 默认为guest
//3、创建连接 Connection
Connection connection = factory.newConnection();
//4、创建Channel
Channel channel = connection.createChannel();
//5、创建交换机
/*
*  创建交换机方法: exchangeDeclare(String exchange, BuiltExchangeType type, boolean durable, boolean autoDelete, boolean internal, Map<String, Object> arguements)
*  参数:
*	1、exchange: 交换机名称
*	2、type: 交换机类型(枚举)
*		DIRECT("direct"): 定向
*		FANOUT("fanout"): 扇形(广播),发送消息到每一个与之绑定队列
*		TOPIC("topic"): 通配符的方式
*		HEADERS("headers"): 参数匹配
*	3、durable: 是否持久化
*	4、autoDelete: 自动删除
*	5、internal: 内部使用。一般为false。
*	6、arguments: 参数
*/
channel.exchangeDeclare("test_fanout", BuiltExchangeType.FANOUT, true, false, false, null);
//6、创建队列Queue
/*
*  队列创建方法: queueDeclare(String queue, boolean durable, boolean exclusive, boolean autoDelete, Map<String, Object> arguements):没有该队列时自动创建该队列
*  参数:
*	1、queue: 队列名称
*	2、durable: 是否持久化，即当mq重启之后，其中的消息还在
*	3、exclusive: 是否独占，即是否只能有一个消费者监听这队列，当Connection关闭时，是否删除队列。
*	4、autoDelete: 是否自动删除。即当没有Consumer时，自动删除队列。
*	5、arguments: 删除的一些参数
*/
channel.queueDeclare("test_fanout_queue1", true, false, false, null);
channel.queueDeclare("test_fanout_queue2", true, false, false, null);
//7、绑定队列和交换机
/*
*  绑定方法: queueBind(String queue, String exchange, String routingKey)
*  参数:
*	1、queue: 队列名称
*	2、exchange: 交换机名称
*	3、routingKey: 路由键，即绑定规则。如果交换机的类型为fanout，routingKey无论怎么设置都会给绑定的队列发送消息。
*/
channel.queueBind("test_fanout_queue1", "test_fanout", "");
channel.queueBind("test_fanout_queue2", "test_fanout", "");
//6、发送消息
/*
*  消息发送方法:  basicPublish(String exchange, String routingKey, BasicProperties props, byte[] body)
*  参数:
*	1、exchange: 交换机名称。简单模式下交换机会使用默认的""
*	2、routingKey: 路由名称。
*	3、props: 配置信息
*	4、body: 发送消息数据
*
*/
channel.basicPublic("test_fanout", "", null, "hello rabbitmq".getBytes());
//7、释放资源
channel.close();
connection.close();
```

#### 4.2.2、消费者代码

多个消费者的话监听不同队列即可。

##### 消费者1

```java
//1、创建连接工厂
ConnectionFactory factory = new ConnectionFactory();
//2、设置参数
factory.setHost("192.168.xx.xx");//ip地址 默认值为Localhost(不设置情况下)
factory.setPort(5672);//端口 默认值为5672
factory.setVirtualHost("/");//设置虚拟机 默认为/
factory.setUsername("guest");//用户名 默认为guest
factory.setPassword("guest");//密码 默认为guest
//3、创建连接 Connection
Connection connection = factory.newConnection();
//4、创建Channel
Channel channel = connection.createChannel();
//5、接收消息
Consumer consumer = new DefaultConsumer(channel){
    @Override
    public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body){
        System.out.println("body:" + new String(body));
        System.out.println("将数据保存到数据库...");
    }
}
channel.basicConsume("test_fanout_queue1", true, consumer);
//释放资源?不用
```

##### 消费者2

```java
//1、创建连接工厂
ConnectionFactory factory = new ConnectionFactory();
//2、设置参数
factory.setHost("192.168.xx.xx");//ip地址 默认值为Localhost(不设置情况下)
factory.setPort(5672);//端口 默认值为5672
factory.setVirtualHost("/");//设置虚拟机 默认为/
factory.setUsername("guest");//用户名 默认为guest
factory.setPassword("guest");//密码 默认为guest
//3、创建连接 Connection
Connection connection = factory.newConnection();
//4、创建Channel
Channel channel = connection.createChannel();
//5、接收消息
Consumer consumer = new DefaultConsumer(channel){
    @Override
    public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body){
        System.out.println("body:" + new String(body));
        System.out.println("将消息日志打印到控制台...");
    }
}
channel.basicConsume("test_fanout_queue2", true, consumer);
//释放资源?不用
```

### 4.3、Routing工作模式

Routing工作模式是基于**routingKey**来工作的，比如在发送消息到交换机前要指定routingKey，然后交换机通过routingKey发送到不同的队列中（队列在创建时可以指定自己的**routingKey**），让不同消费者消费。

#### 4.3.1、direct交换机模式

根据发送的 routingKey发送到指定routingKey的队列中。

生产者：

```java
//1、创建连接工厂
ConnectionFactory factory = new ConnectionFactory();
//2、设置参数
factory.setHost("192.168.xx.xx");//ip地址 默认值为Localhost(不设置情况下)
factory.setPort(5672);//端口 默认值为5672
factory.setVirtualHost("/");//设置虚拟机 默认为/
factory.setUsername("guest");//用户名 默认为guest
factory.setPassword("guest");//密码 默认为guest
//3、创建连接 Connection
Connection connection = factory.newConnection();
//4、创建Channel
Channel channel = connection.createChannel();
//5、创建交换机
/*
*  创建交换机方法: exchangeDeclare(String exchange, BuiltExchangeType type, boolean durable, boolean autoDelete, boolean internal, Map<String, Object> arguements)
*  参数:
*	1、exchange: 交换机名称
*	2、type: 交换机类型(枚举)
*		DIRECT("direct"): 定向
*		FANOUT("fanout"): 扇形(广播),发送消息到每一个与之绑定队列
*		TOPIC("topic"): 通配符的方式
*		HEADERS("headers"): 参数匹配
*	3、durable: 是否持久化
*	4、autoDelete: 自动删除
*	5、internal: 内部使用。一般为false。
*	6、arguments: 参数
*/
channel.exchangeDeclare("test_direct", BuiltExchangeType.DIRECT, true, false, false, null);
//6、创建队列Queue
/*
*  队列创建方法: queueDeclare(String queue, boolean durable, boolean exclusive, boolean autoDelete, Map<String, Object> arguements):没有该队列时自动创建该队列
*  参数:
*	1、queue: 队列名称
*	2、durable: 是否持久化，即当mq重启之后，其中的消息还在
*	3、exclusive: 是否独占，即是否只能有一个消费者监听这队列，当Connection关闭时，是否删除队列。
*	4、autoDelete: 是否自动删除。即当没有Consumer时，自动删除队列。
*	5、arguments: 删除的一些参数
*/
channel.queueDeclare("test_direct_queue1", true, false, false, null);
channel.queueDeclare("test_direct_queue2", true, false, false, null);
//7、绑定队列和交换机
/*
*  绑定方法: queueBind(String queue, String exchange, String routingKey)
*  参数:
*	1、queue: 队列名称
*	2、exchange: 交换机名称
*	3、routingKey: 路由键，即绑定规则。如果交换机的类型为fanout，routingKey无论怎么设置都会给绑定的队列发送消息。
*/
//queue1和direct交换机绑定routingKey为error
channel.queueBind("test_direct_queue1", "test_direct", "error");
//queue2和direct交换机绑定routingKey为info,error,warning
channel.queueBind("test_direct_queue2", "test_direct", "info");
channel.queueBind("test_direct_queue2", "test_direct", "error");
channel.queueBind("test_direct_queue2", "test_direct", "warning");
//6、发送消息
/*
*  消息发送方法:  basicPublish(String exchange, String routingKey, BasicProperties props, byte[] body)
*  参数:
*	1、exchange: 交换机名称。简单模式下交换机会使用默认的""
*	2、routingKey: 路由名称。
*	3、props: 配置信息
*	4、body: 发送消息数据
*
*/
channel.basicPublic("test_direct", "error", null, "error rabbitmq".getBytes());
channel.basicPublic("test_direct", "info", null, "info rabbitmq".getBytes());
//7、释放资源
channel.close();
connection.close();
```



#### 4.3.2、Topic交换机模式

通过Topic通配符方式绑定队列，规则是队列1定义自己的routingKey为a.b.c。Topic交换机通过通配符绑定队列，即通过***，#**的方式进行匹配，*****表示不多不少只匹配**一个单词**，而**#**表示能够匹配**一个或多个单词**。

生产者：

```java
//1、创建连接工厂
ConnectionFactory factory = new ConnectionFactory();
//2、设置参数
factory.setHost("192.168.xx.xx");//ip地址 默认值为Localhost(不设置情况下)
factory.setPort(5672);//端口 默认值为5672
factory.setVirtualHost("/");//设置虚拟机 默认为/
factory.setUsername("guest");//用户名 默认为guest
factory.setPassword("guest");//密码 默认为guest
//3、创建连接 Connection
Connection connection = factory.newConnection();
//4、创建Channel
Channel channel = connection.createChannel();
//5、创建交换机
/*
*  创建交换机方法: exchangeDeclare(String exchange, BuiltExchangeType type, boolean durable, boolean autoDelete, boolean internal, Map<String, Object> arguements)
*  参数:
*	1、exchange: 交换机名称
*	2、type: 交换机类型(枚举)
*		DIRECT("direct"): 定向
*		FANOUT("fanout"): 扇形(广播),发送消息到每一个与之绑定队列
*		TOPIC("topic"): 通配符的方式
*		HEADERS("headers"): 参数匹配
*	3、durable: 是否持久化
*	4、autoDelete: 自动删除
*	5、internal: 内部使用。一般为false。
*	6、arguments: 参数
*/
channel.exchangeDeclare("test_topic", BuiltExchangeType.TOPIC, true, false, false, null);
//6、创建队列Queue
/*
*  队列创建方法: queueDeclare(String queue, boolean durable, boolean exclusive, boolean autoDelete, Map<String, Object> arguements):没有该队列时自动创建该队列
*  参数:
*	1、queue: 队列名称
*	2、durable: 是否持久化，即当mq重启之后，其中的消息还在
*	3、exclusive: 是否独占，即是否只能有一个消费者监听这队列，当Connection关闭时，是否删除队列。
*	4、autoDelete: 是否自动删除。即当没有Consumer时，自动删除队列。
*	5、arguments: 删除的一些参数
*/
channel.queueDeclare("test_topic_queue1", true, false, false, null);
channel.queueDeclare("test_topic_queue2", true, false, false, null);
//7、绑定队列和交换机
/*
*  绑定方法: queueBind(String queue, String exchange, String routingKey)
*  参数:
*	1、queue: 队列名称
*	2、exchange: 交换机名称
*	3、routingKey: 路由键，即绑定规则。如果交换机的类型为fanout，routingKey无论怎么设置都会给绑定的队列发送消息。
*/
//routingKey规定格式为: 系统名称.日志级别
//需求1: 所有error级别，order系统的日志入队列1
channel.queueBind("test_topic_queue1", "test_topic", "#.error");
channel.queueBind("test_topic_queue1", "test_topic", "order.*");
//需求2: 所有日志信息发送到队列2
channel.queueBind("test_topic_queue2", "test_topic", "*.*");
//6、发送消息
/*
*  消息发送方法:  basicPublish(String exchange, String routingKey, BasicProperties props, byte[] body)
*  参数:
*	1、exchange: 交换机名称。简单模式下交换机会使用默认的""
*	2、routingKey: 路由名称。
*	3、props: 配置信息
*	4、body: 发送消息数据
*
*/
channel.basicPublic("test_topic", "error", null, "error rabbitmq".getBytes());
channel.basicPublic("test_direct", "info", null, "info rabbitmq".getBytes());
//7、释放资源
channel.close();
connection.close();
```



## 5、Spring整合RabbitMQ

步骤：

**生产者**：

1. 创建生产者工程
2. 添加依赖
3. 配置整合
4. 编写代码发送消息



**消费者**：

1. 创建生产者工程
2. 添加依赖
3. 配置整合
4. 编写消息监听器



Spring方式配置整合（SpringBoot则按照其指定的名字进行配置）：

```properties
rabbitmq.host=192.168.xx.xx
rabbitmq.port=5672
rabbitmq.username=guest
rabbitmq.username=guest
rabbit.virtual-host=/
```

发送消息则通过RabbitTemplate进行发送，前提是对应交换机、队列(已设置队列路由)创建完成。且容器中存在ConnectionFactory，连接工厂需要配置好host、port、username、password、virtual-host。

接收消息则创建一个类实现MessageListener接口并重写其中的onMessage()方法，并将该类绑定为监听某个队列并加入容器。



## 6、SpringBoot整合RabbitMQ

步骤：

**生产者**：

1. 创建生产者SpringBoot工程

2. 引入依赖坐标

   ```xml
   <dependency>
   	<groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-amqp</artifactId>
   </dependency>
   ```

3. 编写yml配置，基本信息配置（host、port、username、password、virtual-host）

   ```yaml
   spring:
     rabbitmq:
       host: xxx.xxx.xxx.xxx
       port: 5672
       username: guest
       password: guest
       virtual-host: /
   ```

4. 定义交换机，队列以及绑定关系的配置类

   ```java
   @Configuration
   public class RabbitMQConfig{
       
       public static final String EXCHANGE_NAME = "boot_topic_exchange";
       public static final String QUEUE_NAME = "boot_queue";
   
       //1、交换机
       @Bean("bootExchange")
       public Exchange bootExchange(){
           return ExchangeBuilder.topicExchange(EXCHANGE_NAME).durable(true).build();
       }
   
       //2、队列
       @Bean("bootQueue")
       public Queue bootQueue(){
           return QueueBuilder.durable(QUEUE_NAME).build();
       }
   
       //3、交换机和队列绑定关系 Binding
       /**
        * 1、队列
        * 2、交换机
        * 3、routingKey
        */
       @Bean
       public Binding bindQueueExchange(@Qualifier("bootExchange") Exchange exchange,
                                        @Qualifier("bootQueue") Queue queue){
           return BindingBuilder.bind(queue).to(exchange).with("boot.#").noargs();
       }
       
   }
   ```

5. 注入RabbitTemplate，调用方法，完场消息发送

   ```java
   @Autowired
   public RabbitTemplate rabbitTemplate;
   
   @Test
   public void testRabbitMQ(){
       rabbitTemplate.convertAndSend(RabbitMQConfig.EXCHANGE_NAME,
                                     "boot.log", "log发送...".getBytes(StandardCharsets.UTF_8));
   }
   ```

6. 消费者代码：

   ```java
   @Component
   public class MQConsumer1{
       @RabbitListener(queues = RabbitMQConfig.QUEUE_NAME)
       public void ListenerQueue(Message message){
           System.out.println("监听到消息: " + new String(message.getBody()));
       }
   }
   ```



小结：

- SpringBoot提供了快速整合RabbitMQ的方式
- 基本信息在yml中配置，队列交换机以及绑定关系在配置类使用Bean的方式配置
- 生产段直接注入RabbitTemplate完成消息发送
- 消费端直接使用@RabbitListener完成消息接收



## 7、RabbitMQ高级特性

### 7.1、消息可靠性投递

消息可靠性投递需要区分消息的经过路段，生产者到交换机，交换机是否宕机，消费者是否宕机，这些都影响消息投递的可靠性。

而通过以下**四点**，**基本**可以保证消息投递的可靠性：

- 生产者消息确认
- 消息持久性
- 消费者消息确认
- 消费失败重试机制

#### 7.1.1、生产者确认机制

RabbitMQ提供了**publisher confirm**机制来避免消息发送到MQ的过程中丢失。消息发送到MQ以后，会返回一个结果给发送者，表示消息是否成功到达交换机，或是否到达队列。

- publisher-confirm，**消息投递确认**
  - 消息成功投递到交换机，返回**ack**
  - 消息未投递到交换机，返回**nack**
- publisher-return，**消息投递回执**
  - 消息投递到交换机了，但是没有路由到队列。返回**ACK及路由失败原因**。

当然，为了区分不同消息，在发送消息前需要给每个消息设置一个**全局唯一id**，以区分不同消息，避免ack冲突。

##### 代码实现

**1、**使用SpringBoot来实现MQ确认机制，首先先引入依赖，配置好基本信息配置，再添加**生产者确认**的相关配置：

```yaml
spring:
  rabbitmq:
    host: 47.97.26.154
    port: 5672
    username: guest
    password: guest
    virtual-host: /
    #开启消息投递确认的异步回调
    publisher-confirm-type: correlated
    #开启消息投递回执的回调
    publisher-returns: true
    #路由失败策略
    template:
      mandatory: true
```

常用配置说明：

- **publisher-confirm-type**：开启publisher-confirm**投递确认**。该参数支持两种类型：
  - simple：同步等待confirm结果，直到超时。
  - correlated：异步回调，定义ConfirmCallback，MQ返回结果时会回调这个ConfirmCallback。
- **publisher-returns**：开启publish-return**投递回执**功能，同样是基于callback机制，不过是定义ReturnCallback。一般为到达交换机，但处理失败的回调。
- **template.mandatory**：定义消息**路由失败时的策略**，为true时调用ReturnCallback，为false时则直接丢弃消息。



**2、**根据以上的配置来配置回调方法，先配置ReturnCallback，触发ReturnCallback说明消息到达了交换机，但未路由到队列。

由于RabbitTemplate交由**Spring**管理，因此该RabbitTemplate为**单例**，而一个RabbitTemplate只能配置一个ReturnCallback，因此需要考虑RabbitTemplate设置ReturnCallback的**时机**，最好创建一个Bean实现**容器通知方法**，然后在容器**启动时**拿到RabbitTemplate来设置。

```java
@Configuration
@Slf4j
public class CommonConfig implements ApplicationContextAware {
    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        RabbitTemplate rabbitTemplate = applicationContext.getBean(RabbitTemplate.class);
        rabbitTemplate.setReturnCallback((message, replyCode, replyText, exchange, routingKey) ->{
            log.info("消息发送失败，应答码{}，原因{}，交换机{}，路由键{}，消息{}", 
                    replyCode, replyText, exchange, routingKey, message.toString());
        });
    }
}
```



**3、**然后就是ConfirmCallback回调，这种回调**每次**发送消息都可以设置，并不是每个RabbitTemplate只有一个，ConfirmCallback回调的情况分为三种：**ACK，NACK，无**。ACK表示投递成功，NACK表示投递失败，无表示代码可能抛出了异常。代码为：

```java
@Test
    public void test3(){
        //消息体
        String message = "hello, spring amqp";
        //消息ID，需要封装到CorrelationData中，因为我们设置的投递确认模式为correlated
        CorrelationData correlationData = new CorrelationData(UUID.randomUUID().toString());
        //添加callback
        correlationData.getFuture().addCallback(
                result -> {
                    if (result.isAck()){
                        //ack，成功投递
                        log.debug("消息发送成功，ID：{}",correlationData.getId());
                    }else{
                        //nack，投递失败
                        log.error("消息发送失败，ID：{}，原因：{}",correlationData.getId(), result.getReason());
                    }
                },
                ex -> {
                    //出现异常
                    log.error("消息发送异常，ID：{}，原因：{}", correlationData.getId(), ex.getMessage());
                }
        );
        //发送消息
        rabbitTemplate.convertAndSend("amq.direct","simple", message, correlationData);
    }
```

总结在SpringAMQP中处理消息确认的几种情况：

- publisher-comfirm：
  - 消息成功发送到exchange，返回ack。
  - 消息发送失败，没有达到交换机，返回nack。
  - 消息发送过程中出现异常，没有受到回执。
- 消息成功发送到exchange，但没有路由到queue，调用ReturnCallback。

当然，如果出现失败或者异常或者ReturnCallback的情况，可以进行重试，并且至少要有一个**兜底方案**。



#### 7.1.2、消息持久化

在生产者处保证了消息的可靠投递，那么在Broker处如果出现如**机器宕机**的情况，那么仍可能出现消息丢失问题，这时就需要考虑消息在Broker处的**持久化问题**。

通过**代码配置**交换机、队列和消息的持久化：

交换机：

```java
//1、交换机
@Bean("bootExchange")
public Exchange bootExchange(){
    //return ExchangeBuilder.topicExchange(EXCHANGE_NAME).durable(true).build();
    //三个参数：交换机名称，是否持久化，是否自动删除(交换机绑定的队列全部被删除后是否自动删除交换机)
    return new TopicExchange(RabbitMQConfig.EXCHANGE_NAME,true, false);
}
```

队列：

```java
//2、队列
@Bean("bootQueue")
public Queue bootQueue(){
    return QueueBuilder.durable(QUEUE_NAME).build();
}
```

消息是默认持久化的，如果仍需要配置，则使用Message类型进行配置，然后将Message作为消息发送即可：

```java
Message message = MessageBuilder.withBody("hello, spring amqp".getBytes(StandardCharsets.UTF_8))//消息体
    .setDeliveryMode(MessageDeliveryMode.PERSISTENT)//持久化
    .build();
```

其实在使用SpringAMQP且不手动设置情况下，默认底层方法对交换机、队列、消息都是**默认持久化**的。



#### 7.1.3、消费者消息确认

在保证了消息投递可靠和消息持久化可靠后，压力就来到了消费者这边，如果消费者在消费消息时挂了，那么就无法保证消息的可靠性。

因此RabbitMQ支持**消费者确认机制**，即：消费者处理消息后可以向MQ**发送ack回执**，MQ受到ack回执后才会删除该消息。而SpringAMQP则允许配置三种确认模式：

- **manual**：手动ack，需要在业务代码结束后，调用api发送ack。即try catch代码，在finally中发送ack（增加代码侵入性）
- **auto**：自动ack，由Spring监测listener代码是否出现异常，没有异常则返回ack，抛出异常则返回nack。（由Spring中AOP的**环绕通知**实现）
- **none**：关闭ack，MQ假定消费者获取消息后会成功处理，因此消息投递后立即被删除。

消费者消息确认配置方式是修改application.yml文件，添加下面配置：

```yaml
spring:
  rabbitmq:
    listener:
      simple:
        prefetch: 1
        acknowledge-mode: none #none:关闭ack; manual:手动ack; auto:自动ack
```

MQ支持的这三种确认模式中，当确认模式为**none**时，消费者拿到了消息就向MQ发送ack，MQ就立刻删除队列中存储的消息。

当确认模式为**auto**时，消费者拿到消息且执行过程中没有产生异常，就返回ack，MQ就删除队列中的消息，但若消费者拿到消息执行过程中产生了异常，就返回nack，MQ就不删除消息，让消费者再次获取消息。



#### 7.1.4、消费失败重试机制

以上的auto消费者确认模式存在一些问题，当代码有问题时，每次拿到消息后进行执行都会抛出异常，然后每次都返回nack，然后又去队列中再次获取该消息，如此一直循环，将造成服务器较大压力。

因此需要消费失败重试机制，但该重试机制不能无限重试，因此我们可以利用Spring的**retry**机制，在消费者出现异常时利用Spring的限制重试，而不是无限制的requeue到mq队列。

```yaml
spring:
  rabbitmq:
    host: 47.97.26.154
    port: 5672
    username: guest
    password: guest
    virtual-host: /
    #开启消息投递确认的异步回调
    publisher-confirm-type: correlated
    #开启消息投递回执的回调
    publisher-returns: true
    #路由失败策略
    template:
      mandatory: true
    listener:
      simple:
        acknowledge-mode: manual
        prefetch: 1
        #retry机制[注意:所有重试结束，消息无论是否被成功消费，都会被MQ丢弃(最后消费端返回reject)]
        retry:
          enabled: true # 开启消费者失败重试
          initial-interval: 1000 # 第一次失败等待时长
          multiplier: 2 # 后续失败的等待时长倍率。即若倍率为2，第一次等1s再试，第二次则2s，第三次则4s
          max-attempts: 3 #最大重试次数
          stateless: true # 是否设置为无状态，true即无状态，false为有状态，作用即是否保持原状态，开启事务情况下要改为false，保持事务状态。
```

使用Spring的重试机制也存在消息丢失问题，即如果重试次数过多，耗尽重试次数，消息就会被丢弃而丢失（默认处理方案），在Spring中，消息重试次数耗尽后，处理策略其实是由MessageRecoverer接口来处理的，该接口包含三种不同的实现：

- **RejectAndDontRequeueRecoverer**：重试耗尽后，直接reject，丢弃消息。为默认的处理策略。
- **ImmediateRequeueMessageRecoverer**：重试耗尽后，返回nack，消息重新入队。
- **RepublishMessageRecoverer**：重试耗尽后，将失败消息投递到指定的交换机。

而使用重试次数耗尽后投递到指定交换机的方法能够**彻底解决**消息丢失问题。

然后测试下RepublishMessageRecoverer处理模式：

1. 首先需要定义**接收失败消息**的交换机、队列及其绑定关系。

   ```java
   //直接交换机 
   @Bean
   public DirectExchange errorMessageExchange(){
       return ExchangeBuilder.directExchange("error.direct").build();
   }
   
   //持久化队列
   @Bean
   public Queue errorQueue(){
       return QueueBuilder.durable("error.queue").build();
   }
   
   //绑定
   @Bean 
   public Binding errorBinding(){
       return BindingBuilder.bind(errorQueue()).to(errorMessageExchange()).with("error");
   }
   ```

2. 定义自己的失败重试策略，SpringBoot中默认配置的是RejectAndDontRequeueRecoverer，但是@ConditionOnMissingBean注解表明只要我们配置了MessageRecoverer该接口的实现类，就不自动配置。

   ```java
   //RepublishMessageRecoverer策略
   @Bean
   public RepublishMessageRecoverer republishMessageRecoverer(){
       return new RepublishMessageRecoverer(rabbitTemplate, "error.direct", "error");
   }
   ```



#### 7.1.5、总结

如何确保RabbitMQ消息投递的**可靠性**？

1. **开启生产者确认机制**，确保生产者的消息能到达队列。
2. **开启交换机、队列、消息持久化功能**，确保消息未消费前在队列中不会丢失。
3. **开启消费者确认机制为auto**，由Spring确认消息处理成功后返回ack。
4. **开启消费者失败重试机制**，并配置失败策略为**MessageRecoverer**，多次重试失败后将消息投递到异常交换机，交由人工处理。



### 7.3、消费端限流





### 7.4、TTL

即Time-To-Live，如果一个队列中的消息TTL结束仍未被消费，则会变成死信，TTL超时分为**两种超时**：

1. 消息所在**队列**TTL超时
2. **消息本身**存活时间超时

样例：设置消息超时时间ttl=5000；设置队列超时时间x-message-ttl=10000

**消费者消费死信队列代码**：

```java
/**
* 在声明消费者时声明死信交换机和死信队列即绑定
*/
@RabbitListener(bindings = @QueueBinding(
    value = @Queue(name = "dl.queue", durable = "true"),
    exchange = @Exchange(name = "dl.direct"),
    key = "dl"
))
public void listenDlQueue(String msg){
    log.info("接收到 dl.queue的延迟消息：{}", msg);
}
```

**创建正常交换机和队列**：

```java
@Bean
public DirectExchange ttlExchange(){
    return new DirectExchange("ttl.direct");
}

@Bean
public Queue ttlQueue(){
    return QueueBuilder.durable("ttl-queue")//指定队列名称，并持久化
        .ttl(10000) //设置队列的超时时间为10s
        .deadLetterExchange("dl.direct")//指定死信交换机
        .deadLetterRoutingKey("dl")//指定队列发送到交换机的routingKey
        .build();
}

@Bean
public Binding simpleBinding(){
    return BindingBuilder.bind(ttlQueue()).to(ttlExchange()).with("ttl");
}
```

如果要对消息本身设置存活时间，按如下方式即可：

```java
@Test
public void test4(){
    Message message = MessageBuilder.withBody("hello, ttl message".getBytes(StandardCharsets.UTF_8))//消息体
        .setDeliveryMode(MessageDeliveryMode.PERSISTENT)//持久化
        .setExpiration("5000")//设置消息超时时间为5s
        .build();
    //为每条消息设置唯一的ID
    CorrelationData correlationData = new CorrelationData(UUID.randomUUID().toString());
    //发送消息
    rabbitTemplate.convertAndSend("ttl.direct", "ttl", message, correlationData);
    log.info("消息发送成功！");
}
```



### 7.5、死信交换机

先谈谈死信，当一个队列中的消息满足下列情况之一时，就可以被称为死信（dead letter，也可以理解为即将被丢弃的消息）：

- 被消费者**reject**或**nack**重试超出限制的消息，并且消息的**requeue**参数设置为false。
- 消息变成**过期消息**。
- 投递的**消息队列堆积满了**，最早入队的消息就成了死信。

队列如果配置了**dead-letter-exchange**属性，指定好一个交换机，那么队列中的死信就会由队列投递到这个交换机中，而这个交换机就被称为**死信交换机**（Dead Letter Exchange，简称DLX）。当然队列还可以配置与死信交换机绑定的死信队列的RoutingKey，可以通过**dead-letter-routing-key**属性来指定。

死信交换机的方式**类似于**RepublishMessageRecoverer策略，不过Republish是通过**消费端**将死信发送至交换机，而死信交换机是由**队列**来投递消息的，可以指定队列绑定的死信交换机。

<img src="https://img-blog.csdnimg.cn/20210104181018452.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0JiMTUwNzAwNDc3NDg=,size_16,color_FFFFFF,t_70#pic_center" alt="查看源图像" style="zoom: 50%;" />

#### 总结

1. **让消息超时的两种设置方式是**？

   1. 设置队列超时时间
   2. 设置消息超时时间

2. **如何实现发送一个消息20秒后消费者才收到消息**？

   让消费者去监听**死信队列**，生产者设置消息过期时间为20s，然后发送到正常交换机中，并路由到正常队列（已绑定死信交换机和死信队列），等20s超时后由队列转发到死信交换机，再转到死信队列被消费者监听消费。



### 7.6、延迟队列

延迟队列的效果类似TTL + 死信队列，但没那么复杂。RabbitMQ官方使用了一个**插件**实现了延迟队列，首先需要在Linux中或Docker容器中进行安装：https://blog.csdn.net/DZP_dream/article/details/118391439

虽然被称作延迟队列，但起作用的其实是**交换机**，即x-delayed-message。在安装好后，可以在新增交换机位置处选择x-delayed-message类型的交换机。

使用该延迟交换机的步骤为：

1. 声明交换机类型为**x-delayed-message**类型，在Arguments参数中指定路由类型，即x-delayed-type=direct。
2. 消息发送时带上Header信息x-delay=5000。

延迟交换机受到带有Header消息的消息时，会将消息缓存指定时间（与其它交换机不同，交换机一般不存储消息），然后再根据路由方式发送到指定队列。

延迟交换机的声明有**两种**方式，一种是**基于注解**的方式，一种是**基于Bean**的方式：

基于注解：

```java
@RabbitListener(bindings = @QueueBinding(
        value = @Queue(name = "delay.queue", durable = "true"),
        exchange = @Exchange(name = "delay.direct", delayed = "true"),
        key = "delay"
))
public void listenDelayedQueue(String msg){
    log.info("接收到 delay.queue的延迟消息：{}", msg);
}
```

基于Bean：

```java
@Bean
public DirectExchange delayedExchange(){
    return ExchangeBuilder.directExchange("delay.direct")
            .delayed()//设置delayed属性为true
            .durable(true)//持久化设置
            .build();
}

@Bean
public Queue delayedQueue(){
    return new Queue("delay.queue");
}

@Bean
public Binding delayedBinding(){
    return BindingBuilder.bind(delayedQueue()).to(delayedExchange()).with("delay");
}
```



#### 总结

**延迟队列插件的使用步骤包括哪些**？

- 声明一个交换机，**添加delayed属性**为true
- 发送消息时，**添加x-delay头**，值为超时时间



### 惰性队列

#### 消息堆积问题

谈惰性队列前先谈**消息堆积问题**，当生产者发送消息的速度**超过了**消费者处理消息的速度，就会导致队列中的消息堆积，一直到队列存满，而最早入队的消息，就成了**死信**，就可能会被丢弃，这就是消息堆积带来的问题，因此消息堆积是不健康的现象。

解决消息堆积有三种思路：

1. **增加更多的消费者**，提高消费速度。
2. 在消费者内**开启线程池**加快消息处理速度。
3. **扩大队列容积**，提高堆积上限。



因此就需要引出**惰性队列**了，RabbitMQ队列是基于**内存**的，速度快，但是有存储上限，一般到达内存限额的40%左右，RabbitMQ就会停止接收消息，先将一部分消息存入磁盘，降低内存使用，再接收消息，而消息堆积问题最容易引起这种情况，而导致RabbitMQ性能忽高忽低。

而惰性队列中基本解决了这个问题，因为惰性队列中的消息是存入**磁盘**而非内存中的，惰性队列有以下特征：

- 接收到的消息**直接存入磁盘**而非内存
- 消费者消费到消息时惰性队列才从磁盘中读取并加载到内存
- 因为基于磁盘，因此支持**百万级别**的消息存储



如果要设置一个队列为惰性队列，只需要在声明队列时，**指定x-queue-mode属性为lazy即可**。或者直接通过Linux命令将一个运行中的队列修改为惰性队列。

```
rabbitmqctl set_policy Lazy "^lazy-queue$" '{"queue-mode":"lazy"}' --apply-to queues
```

而使用SpringAMQP声明惰性队列分为两种方式：

**基于Bean**：

```java
@Bean
public Queue lazyQueue(){
    return QueueBuilder
            .durable("lazy.queue")
            .lazy()//开启 x-queue-mode为Lazy
            .build();
}
```

基于注解：

```java
@RabbitListener(queuesToDeclare = @Queue(
        name = "lazy.queue",
        durable = "true",
        arguments = @Argument(name = "x-queue-mode", value = "lazy")
))
public void listenLazyQueue(String msg){
    log.info("接收到 lazy.queue 的消息：{}", msg);
}
```

#### 总结

**消息堆积问题的解决方案**？

1. 增加消费者数量
2. 消费者使用多线程
3. 使用惰性队列，可以在mq中保存更多消息

**惰性队列的优点？**

1. 基于磁盘存储，消息上限高
2. 没有间歇性的page-out，性能较稳定

**惰性队列的缺点？**

1. 速度取决于磁盘IO的速度
2. 基于磁盘存储，消息时效性会降低



### 7.7、日志与监控



### 7.8、消息可靠性分析与追踪



### 7.9、管理



## 8、RabbitMQ应用问题

### 8.1、消息可靠性保障



### 8.2、消息幂等性处理



## 9、RabbitMQ集群搭建

### 集群分类

RabbitMQ基于**Erlang**语言编写，Erlang是一个面向并发的语言，天然支持集群模式。

RabbitMQ的集群有两种模式：

- **普通集群**：一种**分布式集群**，将队列分散到集群的各个节点，即每个机器上都存储各自队列。可以提高集群的并发能力，缺点是不支持高可用，宕机后某队列直接下线。
- **镜像集群**：一种**主从集群**，在普通集群的基础上，添加了主从备份功能，提高了集群的可用性。

镜像集群虽然支持主从，但其主从并不是**强一致**的，在某些情况下（如备份时宕机）可能有数据丢失的风险。因此RabbitMQ在3.8版本后，推出新的功能：**仲裁队列**来代替镜像集群，底层采用Raft协议来确保主从数据一致性。

### 普通集群

普通集群，或者叫标准集群（classic cluster），其具备以下特征：

- 在集群的各个节点之间可以**共享部分数据**（虽然各节点中的队列或交换机都是独立的），如：交换机、队列元信息，但**不包含队列中的消息**。
- 当访问集群某节点时，如果队列不在该节点中，会通过存储的队列元信息找到目标队列，在目标队列中拿到消息返回。
- 队列所在节点如果宕机，队列中的消息就会丢失。



### 镜像集群

镜像集群：本质是主从模式，但是具备以下特征：

- 交换机、队列、队列中的消息会在镜像节点中存有**同步备份**。
- 拥有队列的节点被称为主节点，具有该队列备份的节点被称为镜像节点。
- 主节点也可以是其它主节点的镜像节点。
- 所有操作都应该由**主节点**完成，然后同步给镜像节点。
- 主节点宕机后，镜像节点会替代称为新的主节点。



镜像选择有三种模式：

|  ha-mode(模式)  |     ha-params     |                             效果                             |
| :-------------: | :---------------: | :----------------------------------------------------------: |
| 准确模式exactly | 队列的副本量count | 集群中队列的副本数(包括主节点)，count如果为1表示只有单个副本:即队列主节点。count为2表示2个副本：1主队列和1个镜像队列。换句话说：count = 镜像数量 + 1。如果集群中的节点数量小于count，则该队列将镜像到所有节点。如果有集群总数大于count + 1，且如果包含镜像的节点出现故障，则将在另一个节点上创建一个新的镜像。 |
|       all       |      (none)       | 队列在集群中的所有节点之间镜像。队列将镜像到任何新加入的节点。镜像到所有节点会对所有集群节点施加额外的压力，如网络IO，磁盘IO，推荐使用exactly模式，设置副本数量为（n/2 + 1） |
|      nodes      |    node names     | 指定队列创建到哪些具体节点，如果指定的系欸但不存在，则出现异常。 |



### 仲裁队列

仲裁队列的出现主要是为了**解决镜像队列数据丢失**的问题，RabbitMQ3.8以后才有的新功能，用来代替镜像队列，具备以下特征：

- 与镜像队列一样，都是主从模式，支持主从数据同步。
- 使用非常简单，没有复杂的配置。
- 主从同步基于Raft协议，强一致，不用担心数据丢失问题。

仲裁队列的添加十分简单：

![image-20220902150821530](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20220902150821530.png)



测试使用SpringAMQP代码声明仲裁队列：

1. 首先如果是集群状态，要先通过SpringAMQP连接集群，需要在yaml中配置：

   ```yaml
   spring:
     rabbitmq:
       addresses: 192.168.150.xxx:8071, 192.168.150.xxx:8072, 192.168.150.xxx:8073
       username: guest
       password: guest
       virtual-host: /
   ```

2. 使用@Bean创建仲裁队列：

   ```java
   @Bean
   public Queue quorumQueue(){
       return QueueBuilder.durable("quorum.queue2")
               .quorum()//设置为仲裁队列
               .build();
   }
   ```

   