# 基础篇

## 认识Redis

Redis的特征有：

- 键值型（key-value）型，value支持多种不同数据结构，功能丰富。
- 单线程，单个命令具备原子性。
- 低延迟，速度快（基于内存、IO多路复用、良好的编码）。
- 支持数据持久化
- 支持主从集群，分片集群
- 支持多语言客户端（Java、Python等）



## Redis通用命令

|  命令  |                        描述                        |       举例       |
| :----: | :------------------------------------------------: | :--------------: |
|  KEYS  | 查看符合模板的所有key，不建议在生产环境设备上使用  | KEYS * ; KEYS a? |
|  DEL   |                 删除一个指定的key                  |     DEL name     |
| EXISTS |                  判断key是否存在                   |   EXISTS name    |
| EXPIRE | 给一个key设置有效期，有效期到期时该key会被自动删除 | EXPIRE token 10  |
|  TTL   |              查看一个KEY的剩余有效期               |    TTL token     |



## String类型常用命令

|    命令     |                            描述                             |         举例          |
| :---------: | :---------------------------------------------------------: | :-------------------: |
|     SET     |        添加或者修改已经存在的一个String类型的键值对         |     SET name lisa     |
|     GET     |                根据key获取String类型的value                 |       GET name        |
|    MSET     |               批量添加多个String类型的键值对                | MSET name lisa age 20 |
|    MGET     |            根据多个key获取多个String类型的value             |     MGET name age     |
|    INCR     |                    让一个整型的key自增1                     |       INCR age        |
|   INCRBY    |                让一个整型的key自增并指定步长                |      INCR age 2       |
| INCRBYFLOAT |             让一个浮点类型的数字自增并指定步长              |  INCRBYFLOAT age 2.0  |
|    SETNX    | 添加一个String类型的键值对，前提是这个key不存在，否则不执行 |     SETNX age 60      |
|    SETEX    |         添加一个String类型的键值对，并且指定有效期          |     SETEX time 10     |



## Hash类型常用命令

|         命令         |                             描述                             |            举例             |
| :------------------: | :----------------------------------------------------------: | :-------------------------: |
| HSET key field value |              添加或者修改hash类型key的field的值              |     HSET user name Lucy     |
|    HGET key field    |                获取一个hash类型key的field的值                |       HGET user name        |
|        HMSET         |              批量添加多个hash类型key的field的值              | HMSET user name Mike age 20 |
|        HMGET         |              批量获取多个hash类型key的field的值              |     HMGET user name age     |
|       HGETALL        |         获取一个hash类型的key中的所有的field和value          |        HGETALL user         |
|        HKEYS         |             获取一个hash类型的key中的所有的field             |         HKEYS user          |
|        HVALS         |             获取一个hash类型的key中的所有的value             |         HVALS user          |
|       HINCRBY        |           让一个hash类型key的字段值自增并指定步长            |     HINCRBY user age 2      |
|        HSETNX        | 添加一个hash类型的key的field值，前提是这个field不存在，否则不执行 |    HSETNX user name Mike    |



## List类型常用命令

|        命令         |                             描述                             |       举例       |
| :-----------------: | :----------------------------------------------------------: | :--------------: |
|  LPUSH key element  |                 向列表左侧插入一个或多个元素                 | LPUSH eles 1 2 3 |
|      LPOP key       |        移除并返回列表左侧的第一个元素，没有则返回nil         |    LPOP eles     |
|  RPUSH key element  |                 像列表右侧插入一个或多个元素                 | RPUSH eles 4 5 6 |
|      RPOP key       |        移除并返回列表右侧的第一个元素，没有则返回nil         |    RPOP eles     |
| LRANGE key star end |                 返回一段角标范围内的所有元素                 | LRANGE eles 0 2  |
|     BLPOP/BRPOP     | 与LPOP和RPOP类似，不过该命令是允许阻塞等待的，如果列表中没有元素，可以设置等待时间，而不是直接返回nil。 |  BLPOP eles 100  |



## Set类型常用命令

|         命令         |            描述             |       举例        |
| :------------------: | :-------------------------: | :---------------: |
|   SADD key member…   |  向set中添加一个或多个元素  | SADD chart a b c  |
|   SREM key member…   |     移除set中的指定元素     |  SREM chart a b   |
|      SCARD key       |     返回set中元素的个数     |    SCARD chart    |
| SISMEMBER key member | 判断一个元素是否存在于set中 | SISMEMBER chart c |
|       SMEMBERS       |     获取set中的所有元素     |  SMEMBERS chart   |
|  SINTER key1 key2..  |     求key1与key2的交集      |                   |
|  SDIFF key1 key2..   |     求key2与key2的差集      |                   |
|  SUNION key1 key2..  |     求key1和key2的并集      |                   |



## SortedSet类型的常见命令

|             命令             |                            描述                             |           举例           |
| :--------------------------: | :---------------------------------------------------------: | :----------------------: |
|    ZADD key score member     | 添加一个或多个元素到sorted set，如果已经存在则更新其score值 | ZADD stus 90 Amy 89 Mike |
|       ZREM key member        |               删除sorted set中的一个指定元素                |      ZREM stus Mike      |
|      ZSCORE key member       |             获取sorted set中的指定元素的score值             |     ZSCORE stus Amy      |
|       ZRANK key member       |              获取sorted set中的指定元素的排名               |      ZRANK stus Amy      |
|          ZCARD key           |                 获取sorted set中的元素个数                  |        ZCARD stus        |
|      ZCOUNT key min max      |           统计score值在给定范围内的所有元素的个数           |    ZCOUNT stus 80 90     |
| ZINCRBY key increment member |    让sorted set中的指定元素自增，步长为指定的increment值    |    ZINCRBY stus 5 Amy    |
|      ZRANGE key min max      |          按照score排序后，获取指定排名范围内的元素          |     ZRANGE stus 0 2      |
|  ZRANGEBYSCORE key min max   |         按照score排序后，获取指定score范围内的元素          | ZRANGEBYSCORE stus 80 90 |
|     ZDIFF\ZINTER\ZUNION      |                   分别求差集、交集、并集                    |                          |

注意：所有的排名查询默认都是升序，如果要降序，则在命令的Z后添加REV即可，如ZREVRANGE key min max。



## Redis的Java常用客户端

1. Jedis。其方法都是以Redis命令作为方法名称的，学习成本低，简单实用，但Jedis实例是线程不安全的，多线程环境下需要基于连接池来使用。
2. lettuce。Lettuce是基于Netty实现的，支持同步、异步和响应式编程方式，并且是线程安全的。支持Redis的哨兵模式、集群模式和管道模式。
3. Redisson。Redisson是一个基于Redis实现的分布式、可伸缩的Java数据结构集合。包含了诸如Map、Queue、Lock、Semaphore、AtomicLong等强大功能。



## 使用SpringBoot集成Redis

首先在SpringBoot项目中引入两个依赖：

```xml
<!--redis依赖-->
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<!--连接池依赖-->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-pool2</artifactId>
</dependency>
```

然后在yaml文件中配置好Redis相关配置：

```yaml
server:
  port: 8080

spring:
  redis:
#   服务器地址
    host: xxx
#   连接端口
    port: 6379
#   数据库索引(默认为0)
    database: 0
#   连接超时时间
    timeout: 1800000
#   连接池设置
    lettuce:
      pool:
#       连接池最大连接数(使用负值表示没有限制)
        max-active: 20
#       最大阻塞等待时间(负数表示没限制)
        max-wait: -1
#       连接池中的最大空闲连接
        max-idle: 5
#       连接池中的最小空闲连接
        min-idle: 0

```

然后就可以通过注入RedisTemplate直接使用了（SpringBoot都为我们配置好了基本组件并放入容器了）：

```java
@SpringBootTest
class Test1ApplicationTests {

    @Autowired
    private StringRedisTemplate redisTemplate;

    @Test
    void contextLoads() {
        redisTemplate.opsForValue().set("name","张三");
        System.out.println(redisTemplate.opsForValue().get("name"));
    }

}
```



## RedisTemplate的两种序列化方案



### 1、RedisSerializer序列化器

Redis中默认使用的是 JDK 的序列化器，我们使用时一般使用将对象转为 Json 类型的序列化器，因此修改RedisTemplate的序列化器：

```java
@Bean
public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
    RedisTemplate<String, Object> template = new RedisTemplate<>();
    RedisSerializer<String> redisSerializer = new StringRedisSerializer();
    Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
    template.setConnectionFactory(factory);
    //设置“key"的序列化方式
    template.setKeySerializer(new StringRedisSerializer());
    //设置“值”的序列化方式
    template.setValueSerializer(jackson2JsonRedisSerializer);
    //设置“hash”类型数据的序列化方式
    template.setHashValueSerializer(jackson2JsonRedisSerializer);
    return template;
}
```

### 2、StringRedisTemplate类

如果使用RedisSerializer + JSON 序列化器，在存储 JSON 对象到Redis时会同时存入对象的全类名，相当占用空间，因此可以使用StringRedisTemplate + 手动转 JSON 的方式来优化。将对象手动转为JSON写入Redis中，在读取时，则手动将读取到JSON反序列化为对象。



# 实战篇



## 缓存更新策略

|          |                           内存淘汰                           |                           超时剔除                           |                   主动更新                   |
| :------: | :----------------------------------------------------------: | :----------------------------------------------------------: | :------------------------------------------: |
|   说明   | 不用自己维护，利用Redis的内存淘汰机制，当内存不足时自动淘汰部分数据，下次查询时再更新缓存。 | 给缓存数据添加TTL时间，到期后自动删除缓存，下次查询时再更新缓存 | 编写业务逻辑，再修改数据库的同时，更新缓存。 |
|  一致性  |                              差                              |                             一般                             |                      好                      |
| 维护成本 |                              无                              |                              低                              |                      高                      |

业务场景：

- 低一致性需求：使用内存淘汰机制。例如首页类型的查询缓存。
- 高一致性需求：主动更新，并以超时剔除作为兜底方案。例如店铺详情查询的缓存。



**主动更新有三种策略**：

1. 由缓存的调用者，在更新数据库的同时更新缓存。（可操作性强）
2. 缓存与数据库整合为一个服务，由服务来维护一致性。调用者调用该服务，无需关心缓存一致性。（无操作性）
3. 调用者只操作缓存，由其他线程异步的将缓存数据持久化到数据库，保证最终一致性。

一般我们使用第一种方案，手动在业务中**更新数据库时更新缓存**。



**手动更新有三个问题要考虑**：

1. 删除缓存还是更新缓存？
   - 更新缓存：每次更新数据库时都更新缓存，无效写操作较多
   - 删除缓存：更新数据库时让缓存失效，查询时再更新缓存
2. 如何保证缓存与数据库的操作同时成功或失败？
   - 单体系统：将缓存与数据库操作放在一个事务。
   - 分布式系统：利用TCC等分布式事务方案。
3. 先操作缓存还是先操作数据库？
   - 先删除缓存，再操作数据库（多线程下数据不同步概率较高）
   - 先操作数据库，再删除缓存（相对数据库不同步概率更低）



**缓存更新策略最佳方案**：

1. 低一致性需求：使用Redis自带的内存淘汰机制。

2. 高一致性需求：主动更新，并以超时剔除作为兜底方案。

   - 读操作
     - 缓存命中则直接返回
     - 缓存未命中则查询数据库，并写入缓存，设定超时时间

   -  写操作
     - 先写数据库，然后再删除缓存
     - 要确保数据库与缓存操作的原子性



## 缓存穿透的解决思路

缓存穿透是指请求的数据在Redis中没有，然后进入数据库查询，也没有，这样缓存不会生效，所有这样的请求都会打到数据库查询，给数据库带来巨大压力。

常见的解决方案有两种：

1. 缓存空对象
   - 优点：实现简单，维护方便
   - 可能造成短期的不一致
2. 布隆过滤
   - 优点：内存占用较少，没有多余的key
   - 缺点：
     - 实现复杂
     - 存在误判可能



## 缓存雪崩的解决思路

缓存雪崩是指某一时刻大量缓存同时失效或者Redis服务宕机导致大量请求冲向数据库，给数据库带来巨大压力。

常见的解决方法有：

1. 给不同的key的TTL添加随机值
2. 利用Redis集群提高服务的可用性
3. 给缓存业务添加降级限流策略
4. 给业务添加多级缓存



## 缓存击穿的解决思路

缓存击穿也被称成为热点key问题，即一个被高并发访问并且缓存重建业务较为复杂的key突然失效，无数的请求访问会在瞬间给数据库带来巨大的压力。

常见的解决方案有：

1. 互斥锁。即当某线程发现缓存过期时，先获取互斥锁，然后再去数据库查询数据并重建缓存。其它线程则在这期间获取互斥锁失败，自旋查询缓存和获取互斥锁，直到线程重建缓存成功。
2. 逻辑过期。逻辑过期则不为热点数据设置过期时间，当某线程更新缓存时，先获取互斥锁，然后开启新线程去查询数据库重建缓存，这期间，所有其它缓存都直接使用旧数据。

| 解决方案 |                     优点                     |                   缺点                   |
| :------: | :------------------------------------------: | :--------------------------------------: |
|  互斥锁  | 没有额外的内存消耗、能够保证一致性、实现简单 | 线程需要等待，性能受影响、可能有死锁风险 |
| 逻辑过期 |            线程无需等待，性能较好            | 不保证一致性、有额外的内存消耗、实现复杂 |



## 全局ID生成器

全局ID生成器目的是解决**分布式情况**下ID的生成策略，全局唯一ID需要具备以下特征：**唯一性、高可用、递增性、安全性、高性能**。

我们可以使用 **long** 类型来作为 ID 类型，因为 long 共有8个字节，64个比特位，将第一个比特位作为符号位，后31位作为时间戳，最后32位作为真正的序列号。31位作为时间戳以秒为单位，可以存储69年，32为作为序列号，支持每秒产生2^32次方个不同ID。因此理论上支持每秒生成42亿个不同 ID。

| 符号位 |                 1bit，永远为0                 |
| :----: | :-------------------------------------------: |
| 时间戳 |        31bit，亿秒为单位，可以使用69年        |
| 序列号 | 32bit，秒内的计数器，支持每秒产生2^32个不同ID |

代码：

```java
//2022年1月1日0时0分0秒作为基准
private static final long BEGIN_TIMESTAMP = 1640995200L;

@Autowired
private StringRedisTemplate stringRedisTemplate;

public long nextId(String keyPrefix){
    //1、生成时间戳
    LocalDateTime now = LocalDateTime.now();
    long timestamp = now.toEpochSecond(ZoneOffset.UTC) - BEGIN_TIMESTAMP;
    //2、生成唯一ID
    long increment = stringRedisTemplate.opsForValue().increment("incr:" + keyPrefix + ":" + now.format(DateTimeFormatter.ofPattern("yyyy:MM:dd")));
    //3、拼接
    return timestamp << 32 | increment;
}
```



## 秒杀下单

秒杀下单时需要判断的两点：

- 秒杀是否开始或结束，如果尚未开始或已经结束则无法下单。
- 库存是否充足，不足则无法下单。



### 超卖问题

超卖问题是高并发场景下的常见问题，也是**不可接受**的问题。场景如用户进行秒杀时分两步，首先查询库存是否足够，若足够再更新库存，因此若线程1和线程二同时查询到库存还剩1，并都去更新库存，那么库存就变为-1了，这就是典型的超卖。

解决超卖问题常用方案是**加锁**，加锁可以选择**悲观锁**或**乐观锁**，悲观锁即在操作数据前先获取锁，确保线程串行执行，乐观锁即不加锁，只是在更新数据时判断有没有其它线程对数据做了修改。悲观锁有性能问题，因此可选的就是**乐观锁**。

乐观锁杜判断之前查询得到的数据是否有被修改过有常用的有两种方式：**版本号法**和**CAS方案**。

**版本号法**即在数据库中新增一个version字段，每次查询时连版本号一起查，修改时加上版本号进行判断：

```sql
update set stock = stock - 1, version = version + 1 where id = xx and version = xx;
```

**CAS方案**即Compare And Set，该方案不需要额外字段，直接加入需要判断的条件。

```sql
update set stock = stock - 1 from xx where id = xx and  stock > 0;
```

版本号法可能存在成功率低问题（CAS则成功率高）。因此最好综合情况采用合适方式。



### 一人一单

秒杀下单通常只允许一人下一单，通常前端就可以拦截请求，但是对于非常规操作，后端来拦截很必要。如果单纯在下单前查询是否满足一人一单，在高并发情况下就容易出现问题，如多个线程同时进来查询判断是否一人一单，而此时该用户还未下单，则多个并发请求能同时绕过判断进行下单。

因此就需要采用锁机制保证线程的串行执行了。但是该锁只能保证在单机条件下的一人一单。



## 分布式锁

在分布式条件下，多个相同服务的 JVM 的锁监视器不同，无法保证一人一单实现，因此需要引入分布式锁（使用统一的锁监视器）。

基于Redis的分布式锁实现思路如下：

- 利用set nx ex获取锁，并设置过期时间，保存线程标识。
- 释放锁时判断线程标识是否与自己一致，一致则删除。（此处小问题很多，可以使用lua脚本实现）



## Redisson解决分布式锁问题



## 消息队列

消息队列（Message Queue），字面意思就是存放消息的队列，最简单的消息队列模型包括三个角色：

- 消息队列：存储和管理消息，也被称为消息代理（Message Broker）
- 生产者：发送消息到消息队列
- 消费者：从消息队列获取消息并处理消息

|             生产者             |               |      消费者      |
| :----------------------------: | :-----------: | :--------------: |
|       判断秒杀时间和库存       | Message Queue | 接受消息完成下单 |
|          校验一人一单          |               |                  |
| 发送优惠卷id和用户id到消息队列 |               |                  |

Redis提供了三种不同的方式来实现消息队列：

- list结构：基于List结构模拟消息队列
- PubSub：基本的点对点消息模型
- Stream：比较完善的消息队列模型



### List结构模拟消息队列

消息队列（Message Queue），字面意思就是存放消息的队列。而Redis的**List**数据结构是一个**双向链表**，很容易模拟出队列效果。

队列是入口和出口不在一边，因此我们可以利用：**LPUSH**结合**RPOP**、或者**RPUSH**结合**LPOP**来实现，不过需要注意的是，当队列中没有消息时RPOP或LPOP操作会返回NULL，并不像JVM的阻塞队列那样会阻塞并等待消息。因此这里应该使用BRPOP或者BLPOP来实现阻塞效果。

基于List的消息队列的**优缺点**：

1. 优点
   - 利用Redis存储，不受限于JVM内存上限
   - 基于Redis的持久化机制，数据安全性有保证
   - 可以满足消息有序性
2. 缺点
   - 无法避免消息丢失
   - 只支持单消费者



### PubSub的消息队列

PubSub（发布订阅）是Redis2.0版本引入的消息传递模型。顾名思义，消费者可以订阅一个或多个channel，生产者向对应channel发送消息后，所有订阅者都能受到相关消息。

常用命令：

- SUBSCRIBE channel [channel]：订阅一个或多个频道
- PUBLISH channel msg：向一个频道发送消息
- PSUBSCRIBE pattern [pattern]：订阅与pattern格式匹配的所有频道（使用*，?通配符）

基于PubSub的消息队列的优缺点：

1. 优点
   - 采用发布订阅模型，支持多生产、多消费
2. 缺点
   - 不支持数据持久化（实时发送，没收到就丢失了）
   - 无法避免消息丢失
   - 消息堆积有上限，超出时数据丢失（消息堆积在消费端，超出消费端存储长度则丢弃）



### Stream的消息队列

Stream是Redis5.0引入的一种新数据类型，可以实现一个功能非常完善的消息队列。

发送消息的命令：XADD

```
XADD key [NOMKSTREAM] [MAXLEN|MINID [=|~] threshold [LIMIT count]] *|ID field value..

[NOMKSTREAM]: 如果队列不存在，是否自动创建队列。默认为自动创建
[MAXLEN|MINID [=|~] threshold [LIMIT count]]: 设置消息队列的最大消息数量
*|ID: 消息的唯一id，*代表由Redis自动生成，格式是"时间戳-递增数字"，例如"1644803221283-0"
field value: 发送到队列中的消息，成为Entry。格式就是多个key-value键值对。

例:
#创建名为 users 的队列，向其中发送一个消息，内存是:{name=jack,age=21}，并使用Redis自动生成ID
XADD users * name jack age 21
```

读取消息的方式之一：XREAD

```
XREAD [COUNT count] [BLOCK milliseconds] STREAMS key [key...] ID [ID...]

COUNT count]: 每次读取消息的最大数量
[BLOCK milliseconds]: 当没有消息时，是否阻塞、阻塞时长
STREAMS key [key...]: 要从哪个队列读取消息，key就是队列名
ID [ID...]: 起始id，只返回大于该ID的消息。0: 代表从第一个消息开始 $: 代表从最新的消息开始

例:
#XREAD阻塞方式，读取最新的消息
XREAD COUNT 1 BLOCK 1000 STREAMS users $
```



### Stream的消费者组模式

消费者组模式不同于单个消费者，消费者组将多个消费者划分到一个**组**中，并监听同一个队列。

消费者组具有以下特点：

1. **消息分流**。队列中的消息会分流给组内的不同消费者，而不是重复消费，从而加快消息处理的速度。
2. **消息标示**。消费者组会维护一个标示，记录最后一个被处理的消息，哪怕消费者宕机重启，还会从标示之后读取消息，确保每一个消息都会被消费。
3. **消息确认**。消费者获取消息后，消息处于pending（adj. 即将发生的）状态，并存入一个pending-list，当处理完成后需要通过XACK来确认消息，标记消息为已处理，才会从pending-list中移除。

```
#创建消费者组
XGROUP CREATE key groupName ID [MKSTREAM]

key: 队列名称
groupName: 消费者组名称
ID: 起始ID标示，$代表队列中最后一个消息，0则代表队列中第一个消息
MKSTREAM: 队列不存在时自动创建队列

#删除指定的消费者组
XGROUP DESTORY key groupName

#给指定的消费者组添加消费者
XGROUP CREATECONSUMER key groupname consumername

#删除消费者组中的指定消费者
XGROUP DELCONSUMER key groupname consumername

#从消费者组读取消息
XREADGROUP GROUP group consumer [COUNT count] [BLOCK milliseconds] [NOACK] STREAMS key [key ...] ID [ID ...]

group: 消费者组名称
consumer: 消费者名称，如果消费者不存在，会自动创建一个消费者
count: 本次查询的最大数量
BLOCK milliseconds: 当没有消息时最长等待时间
NOACK: 无需手动ACK，获取到消息后自动确认
STREAMS key: 指定队列名称
ID: 获取消息的起始ID
	">": 从下一个未消费的消息开始
	其它: 根据指定id从pending-list中获取已消费但未确认的消息。例如0，是从pending-list中的第一个消息开始。
	
#确认消息
XACK key group ID [ID ...]
```

消费者组模式的特点：

1. 消息可回溯。
2. 可以多消费者争抢消息，加快消费速度。
3. 可以阻塞读取。
4. 没有消息漏读的风险。
5. 有消息确认机制，保证消息至少被消费一次。



### Redis消息队列对比

|              |                   List                   |       PubSub       |                         Stream                         |
| :----------: | :--------------------------------------: | :----------------: | :----------------------------------------------------: |
|  消息持久化  |                   支持                   |       不支持       |                          支持                          |
|   阻塞读取   |                   支持                   |        支持        |                          支持                          |
| 消息堆积处理 | 受限于内存空间，可以利用多消费者加快处理 | 受限于消费者缓冲区 | 受限于队列长度，可以利用消费者组提高消费速度，减少堆积 |
| 消息确认机制 |                  不支持                  |       不支持       |                          支持                          |
|   消息回溯   |                  不支持                  |       不支持       |                          支持                          |



# 高级篇

高级篇旨在解决日常使用或分布式情况下的Redis可能会出现的问题。如**数据丢失问题**、**并发能力问题**、**存储能力问题**、**故障恢复问题**等。

这些问题都已有**解决方案**，如：

1. 数据丢失问题：通过实现Redis数据的持久化解决
2. 并发能力问题：可以通过搭建主从集群，实现读写分离
3. 存储能力问题：搭建分片集群，利用插槽机制实现动态扩容
4. 故障恢复问题：利用Redis哨兵，实现健康检测和自动恢复

接下来我们将学习以上这些解决方案的具体实现。

 

## Redis持久化

能够解决Redis出现的数据丢失问题，共有两种持久化方案，分别为RDB和AOF。



### RDB

RDB全称Redis Database Backup file（Redis数据备份文件），也叫做Redis数据快照。简单说就是将Redis内存中所有数据全部记录到磁盘中，当Redis服务故障重启后，会从磁盘中读取对应快照文件，恢复数据。

RDB方式持久化有两种运行方式，一种是手动执行保存命令，由Redis主进程来执行RDB命令，由于Redis是单线程，因此会阻塞其它所有正在运行的命令，执行命令为 **save** 。

另一种也是手动执行保存命令，但Redis会开启一个子进程来执行RDB，因此不会影响其它命令的运行，执行命令为 **bgsave**。

Redis服务在正常停止服务前会自动执行一次 **RDB** 。并在保存在运行目录下，默认命名为dump.rdb文件。

假设Redis在持续运行一段时间后，没有对Redis执行手动保存命令，且Redis出现异常宕机，那么这些长时间以来在Redis中存储的数据就会丢失。因此为解决这个问题，就需要让Redis自动进行持久化，而Redis中就存在这种机制，即触发RDB的机制，相关配置可以在redis.conf文件中找到，格式如下：

```
#900秒内，如果至少1个key被修改，则触发bgsave，如果是save "" 则表示禁用RDB
save 900 1
save 300 10
save 660 10000
```

相关的其它RDB配置也可以在redis.conf文件中设置：

```
#是否压缩存储，建议不开启，压缩过程会消耗cpu，而磁盘相对更不值钱
rdbcompression yes

#RDB文件名称
dbfilename dump.rdb

#文件保存的路径目录
dir ./
```

然后再谈谈 **bgsave** 异步保存数据的过程，bgsave 不像 save 命令，直接阻塞Redis进程进行RDB持久化操作，bgsave命令只有 fork 主进程得到子进程这一段时间内才会阻塞Redis。bgsave 命令开始时会先 fork 主进程得到子进程，子进程**共享主进程的内存数据**。这里需要先了解操作系统的相关知识：进程是无法直接访问操作系统的物理内存，只能通过操作与物理内存存在映射的虚拟内存，而子进程得到的就是虚拟内存数据，然后子进程根据映射**获取对应的内存数据并写入新的RBD文件**，然后用**新RDB文件替换旧RDB文件**。子进程在进行RDB操作时，主进程只能通过 copy-on-write 技术对内存数据进行操作。

RDB是Redis默认的持久化方案，但RDB也有自己的局限性，如自动持久化可能导致数据丢失，并且 fork 子进程，压缩，写出RDB文件都比较耗时。



### AOF

AOF全称为Append Only File（追加文件），AOF可以解决RDB中出现的一些问题。AOF持久化方案不通过每次保存所有内存中的数据而实现持久化，而是通过记录每次的写操作来记录操作，因此AOF也可以看作是命令的日志文件。当Redis出现故障重启后，会通过从头执行AOF中记录的写命令来还原内存数据。

AOF默认是关闭的，需要在配置文件中手动开启：

```
# 是否开启AOF功能，默认是no
appendonly yes
# AOF文件的名称
appendfilename "appendonly.aof"
```

AOF执行保存命令的频率也可以在配置文件中配置：

```
# 表示每执行一次写命令，就将操作记录到AOF文件中（可靠性好，但开销太大）
appendfsync always
# 写命令执行完后先将操作放入AOF缓冲区，然后每隔一秒将缓冲区数据写入AOF文件，这是默认方案
appendsync everysec
# 写命令执行完后先将操作放入AOF缓冲区，然后由操作系统自己决定什么时候将缓冲区内容写到磁盘文件。（不推荐）
appendsync no
```

使用AOF需要禁用RDB，在redis.conf中将RDB的配置save改为：

```
#save 900 1
save ""
```

虽然使用AOF能够解决RDB的部分问题，但AOF也有自己的问题，如果set记录过多，那么AOF文件会很庞大，因此可以通过执行bgrewriteaof命令，让AOF文件执行重写功能，缩减AOF文件大小，该命令的执行与主进程是异步的。

bgrewriteaof：记录set key value的最后一次操作、整合多个set为mset。

Redis既提供了手动执行命令缩减AOF文件大小，也可以通过触发阈值自动重写AOF文件，相关配置在redis.conf中配置：

```
# AOF文件比上次文件 增长超过多少百分比则触发重写
auto-aof-rewrite-percentage 100
# AOF文件体积最小多大以上才触发重写
auto-aof-rewrite-min-size 64mb
```



### 比较

RDB和AOF各有各的优点，如果对数据安全性要求较高，在实际开发中往往会结合两者来使用。

|                |                     RDB                      |                           AOF                            |
| :------------: | :------------------------------------------: | :------------------------------------------------------: |
|   持久化方式   |             定时对整个内存做快照             |                   记录每一次执行的命令                   |
|   数据完整性   |          不完整，两次备份之间会丢失          |                 相对完整，取决于刷盘策略                 |
|    文件大小    |             会有压缩，文件体积小             |                  记录命令，文件体积很大                  |
|  宕机恢复速度  |                     很快                     |                            慢                            |
| 数据恢复优先级 |          低，因为数据完整性不如AOF           |                  高，因为数据完整性更高                  |
|  系统资源占用  |            高，大量CPU和内存消耗             | 低，主要是磁盘IO资源，但AOF重写时会占用大量CPU和内存资源 |
|    使用场景    | 可以容忍数分钟的数据丢失，追求更快的启动速度 |                   对数据安全性要求较高                   |



## Redis主从集群

单节点Redis的并发能力是有上限的，要进一步提高Redis的并发能力，就要搭建主从集群，实现读写分离。即读在从，写在主。



### 搭建主从集群步骤

1. 在Linux下创建出三个目录，7001、7002，7003，用来模拟三台Redis主从，在需要使用到的redis.conf中关闭AOF配置（appendonly=false），注释save “”，即开启RDB。

2. 将redis.conf复制到7001、7002，7003目录中，修改各自redis.conf中的端口号和RDB保存位置（/tmp/7001/）。

3. 指定Redis所绑定的ip，让Redis不去自己读取ip，而是直接使用绑定ip（添加在redis.conf首行即可）：

   ```xml
   relica-announce-ip xxx.xxx.xxx.xxx
   ```

4. 分别通过各自的redis.conf启动三台Redis

   ```
   redis-server xxx/redis.conf
   ```

5. 开启主从关系。开启主从关系有**临时**和**永久**两种模式。

   - 临时：使用redis-cli客户端连接到redis服务，执行slaveof命令（重启后失效）：

     ```
     slaveof <masterip> <masterport>
     ```

   - 永久：修改配置文件，在redis.conf中添加一行配置：

     ```
     slaveof <masterip> <masterport>
     ```

     

### 主从同步原理

#### 全量同步

主从第一次同步是**全量同步**（较为消耗内存），全量同步分为三步：

1. **第一阶段**：slave执行了slaveof命令，成为主节点的从节点，建立起连接，并向主机请求数据同步，主机接到请求后，先判断是否是第一次同步，如果是第一次，则先返回master的数据版本消息，slave接收到数据版本信息进行保存。
2. **第二阶段**：主节点执行bgsave，生成RDB文件（在生成RDB文件这段时间内的命令会被记录到repl_baklog文件中），然后向slave发送RDB文件，slave接收到RDB文件后，清空本地数据，并加载RDB文件。
3. **第三阶段**：主节点向从节点发送repl_baklog文件，从节点接收到后执行接收到文件中的命令。

master如何判断slave**是不是第一次**来同步数据？这里用到了两个很重要的概念：

- **Replication Id**：简称replid，是数据集的标记，id一致则说明是同一数据集。每一个master都有唯一的replid，slave则会集成master节点的replid。
- **offset**：偏移量，随着记录在repl_baklog中的数据增多而逐渐增大。slave完成同步时也会记录当前同步的offset。如果slave的offset小于master的offset，说明slave数据落后于master，需要更新。

因此slave做数据同步，必须向master**声明自己的replication id和offset**，master才可以判断到底需要同步哪些数据。



#### 增量同步

主从第一次同步是全量同步，但如果slave重启后同步，则执行**增量同步**。增量同步分为两步：

1. **第一阶段**：slave向master发送replid和offset数据，master接收到后判断slave是否是第一次同步（通过slave发送的replid是否和自己相同），若不是第一次同步，返回continue指令。
2. **第二阶段**：master去repl_baklog中获取offset后的数据，即slave未同步的数据，向slave发送。

注意：repl_baklog本质上是一个**环形数组**，大小有上限，当写满后会覆盖最早的数据，若slave断开时间过久，导致尚未备份的数据被覆盖，则无法基于log再做增量同步，则只能做**全量同步**了。



#### 同步优化

主从之间的频繁触发全量同步很**消耗性能**，因此可以从以下几个方面来优化Redis主从集群：

- 在master中配置repl-diskless-sync yes**启用无磁盘复制**，避免全量同步时的磁盘IO（正常情况下主机全量同步是先将RDB文件写入磁盘，再通过网络IO传输给从机）。
- Redis单节点上的**内存占用不要太大**，减少RDB导致的过多的磁盘IO。
- 适当**提高repl_baklog的大小**，发现slave宕机时尽快实现故障恢复，尽可能避免全量同步。
- 限制一个master上的slave节点数量，如果实在太多slave，则可以**采用主-从-从链式结构**（即从-从节点以从节点为主节点），减少master压力。



### 总结

#### 简述全量同步和增量同步的区别？

- 全量同步：master将完整内存数据生成RDB，发送RDB到slave，后续命令则记录再repl_baklog，逐个发送给slave。
- 增量同步：slave提交自己的offset到master，master获取repl_baklog中从offset之后的命令给slave。



#### 什么时候执行全量同步？

- slave节点第一次连接master节点时。
- slave节点断开时间过久，repl_baklog中的offset已经被覆盖时。



#### 什么时候执行增量同步？

- slave节点断开又恢复，并且在repl_baklog中能找到offset时。



## Redis哨兵集群

### Redis哨兵

#### 哨兵的作用

Redis提供了哨兵（Sentinel）机制来实现主从集群的**自动故障恢复**。哨兵的结构和作用如下：

- **监控**：Sentinel会不断检查您的master和slave是否按预期工作。
- **自动故障恢复**：如果master故障，Sentinel会将一个slave提升为master，当故障实例恢复后也以新的master为主。
- **通知**：Sentinel充当Redis客户端的服务发现来源，当集群发生故障转移时，会将最新消息推送给Redis的客户端。



#### 服务状态监控

Sentinel基于**心跳机制**检测服务状态，每隔一秒向集群的每个实例发送ping命令。状态检测分为主观下线和客观下线两种。

- **主观下线**：如果某sentinel节点发现某实例**未在规定时间响应**，则认为该实例主观下线。
- **客观下线**：若超过指定数量（quorum，可在redis.conf中配置）的sentinel都认为该实例主观下线，则该实例客观下线。quorum值最好超过Sentinel实例数量的**一半**。



#### 选举新的master

一旦发现master故障，sentinel需要在slave中选择一个作为新的master，**选取依据**是这样的：

- 首先会判断slave节点与master节点**断开时间长短**，如果超过指定值（down-after-milliseconds * 10，可在redis.conf中配置），则会排除该slave节点。
- 然后判断slave节点的**slave-priority值**，越小优先级越高，如果是0则永不参与选举。
- 如果slave-priority一样，则判断slave节点的**offset值**（**主要选取依据**），越大说明数据越新，优先级越高。
- 最后则判断slave节点的运行id大小（启动时生成），越小优先级越高。



#### 如何实现故障转移

当选中了其中一个slave作为新的master后（例如192.168.1.1上7002端口的slave1），故障的**转移步骤**如下：

- sentinel给备选的slave1节点发送**slaveof no one**命令，让该节点成为master。
- sentinel给所有其它slave发送slaveof 192.168.1.1 7002命令，让这些命令成为新master的从节点，开始从新的master上同步数据。
- 最后，sentinel将故障节点标记为slave（向其发送slave of 192.168.1.1 7002命令），当故障节点恢复后会自动成为新的master的slave节点。



#### 总结

#### Sentinel的三个作用是什么？

- 监控
- 故障转移
- 通知



#### Sentinel如何判断一个redis实例是否健康？

- 每隔1秒发送一次ping命令，如果超过一定时间没有pong则认为是主观下线。
- 如果大多数sentinel都认为实例主观下线，则判定为服务下线。



#### 故障转移步骤有哪些？

- 首先选定一个slave作为新的master，执行slaveof no one
- 然后向其它所有节点执行slaveof new_master命令
- 修改故障节点配置，添加slaveof new_master命令

#### 哨兵的作用

Redis提供了哨兵（Sentinel）机制来实现主从集群的**自动故障恢复**。哨兵的结构和作用如下：

- **监控**：Sentinel会不断检查您的master和slave是否按预期工作。
- **自动故障恢复**：如果master故障，Sentinel会将一个slave提升为master，当故障实例恢复后也以新的master为主。
- **通知**：Sentinel充当Redis客户端的服务发现来源，当集群发生故障转移时，会将最新消息推送给Redis的客户端。



#### 服务状态监控

Sentinel基于**心跳机制**检测服务状态，每隔一秒向集群的每个实例发送ping命令。状态检测分为主观下线和客观下线两种。

- **主观下线**：如果某sentinel节点发现某实例**未在规定时间响应**，则认为该实例主观下线。
- **客观下线**：若超过指定数量（quorum，可在redis.conf中配置）的sentinel都认为该实例主观下线，则该实例客观下线。quorum值最好超过Sentinel实例数量的**一半**。



#### 选举新的master

一旦发现master故障，sentinel需要在slave中选择一个作为新的master，**选取依据**是这样的：

- 首先会判断slave节点与master节点**断开时间长短**，如果超过指定值（down-after-milliseconds * 10，可在redis.conf中配置），则会排除该slave节点。
- 然后判断slave节点的**slave-priority值**，越小优先级越高，如果是0则永不参与选举。
- 如果slave-priority一样，则判断slave节点的**offset值**（**主要选取依据**），越大说明数据越新，优先级越高。
- 最后则判断slave节点的运行id大小（启动时生成），越小优先级越高。



#### 如何实现故障转移

当选中了其中一个slave作为新的master后（例如192.168.1.1上7002端口的slave1），故障的**转移步骤**如下：

- sentinel给备选的slave1节点发送**slaveof no one**命令，让该节点成为master。
- sentinel给所有其它slave发送slaveof 192.168.1.1 7002命令，让这些命令成为新master的从节点，开始从新的master上同步数据。
- 最后，sentinel将故障节点标记为slave（向其发送slave of 192.168.1.1 7002命令），当故障节点恢复后会自动成为新的master的slave节点。



#### 总结

##### Sentinel的三个作用是什么？

- 监控
- 故障转移
- 通知



##### Sentinel如何判断一个redis实例是否健康？

- 每隔1秒发送一次ping命令，如果超过一定时间没有pong则认为是主观下线。
- 如果大多数sentinel都认为实例主观下线，则判定为服务下线。



##### 故障转移步骤有哪些？

- 首先选定一个slave作为新的master，执行slaveof no one
- 然后向其它所有节点执行slaveof new_master命令
- 修改故障节点配置，添加slaveof new_master命令

Redis哨兵集群



### 搭建哨兵集群步骤

1. 在/tmp目录下**创建三个文件夹** mkdir s1 s2 s3

2. 在s1文件夹中**创建sentinel.conf文件**：vi sentinel.conf

   ```
   #端口号
   port 27001
   #声明本sentinel的ip地址，避免Redis自己查询到不同ip导致集群错误
   sentinel announce-ip 192.168.150.101
   #监控名为mymaster，ip和端口分别为192.168.150.101和7001的主节点，且设置选举master的quorum值为2
   #只监控主节点的原因是主节点中有从节点的信息，因此不监控从节点
   sentinel monitor mymaster 192.168.150.101 7001 2
   #slave和master断开的最大超时时间（不配默认也是该超时时间）
   sentinel down-after-milliseconds mymaster 5000
   #slave故障恢复的超时时间（不配默认也是该超时时间）
   sentinel failover-timeout mymaster 60000
   #工作目录
   dir "/tmp/s1"
   ```

3. 将该配置拷贝至s2和s3目录，**修改端口号和工作目录**。

4. 启动哨兵集群：

   ```
   #第一个
   redis-sentinel s1/sentinel.conf
   #第二个
   redis-sentinel s2/sentinel.conf
   #第三个
   redis-sentinel s3/sentinel.conf
   ```

5. down掉主节点，可以发现哨兵先选出哨兵之间的master，由master去两个从节点间选出主节点，并给主节点发送slaveof no one命令，给其它从节点发送slave of 主节点命令，给宕机主节点修改redis.conf文件为slave of 主节点，让其重启就继续RDB复制。



### RedisTemplate的哨兵模式

在Sentinel集群监管下的Redis主从集群，其节点会因为自动故障转移而发生变化，Redis的客户端必须**感知**这种变化，及时**更新**连接信息。而Spring的RedisTemplate底层利用**lettuce**实现了节点的感知和自动切换。

我们只需要引入springboot-starter-data-redis依赖，然后在yml或者properties中配置号sentinel的信息即可：

```yml
spring:
  redis:
    sentinel:
      master: mymaster #指定master名称
      nodes: #指定redis-sentinel集群信息
        - 192.168.150.101:27001
        - 192.168.150.101:27002
        - 192.168.150.101:27003
```

光配置sentinel哨兵还不够，还需要设置主从Redis的读写分离，这里可以采用代码方式配置：

```java
@Bean
public LettuceClientConfigurationBuilderCustomizer configurationBuilderCustomizer(){
    /**
         * 这里的ReadFrom是配置Redis的读取策略，是一个枚举，包括下面选择：
         *  1、MASTER: 从主节点读取
         *  2、MASTER_PREFERRED: 优先从master节点读取，master不可用才读取replica
         *  3、REPLICA: 从slave(replica)节点读取
         *  4、REPLICA_PREFERRED: 优先从slave(replica)节点读取，所有的slave都不可用才读取master
         */
    return configBuilder -> configBuilder.readFrom(ReadFrom.REPLICA_PREFERRED);
}
```



## Redis分片集群

主从和哨兵可以解决高可用、高并发读的问题，但是仍有两个问题没有解决：

- **海量数据存储问题**（主从同步RDB消耗性能）
- **高并发写的问题**（只有一个主节点用于写）

使用分片集群可以解决上述问题，分片集群特征为：

- 集群中有**多个master**，每个master保存不同数据
- 每个master都可以有**多个slave节点**
- master之间通过**ping**检测彼此健康状态
- 客户端请求可以访问集群**任意**节点，最终都会被转发到**正确**节点



### 搭建分片集群

搭建分片集群步骤为：

1. 创建六个文件夹，对应三对主从，7001，7002，7003，8001，8002，8003。

   ```
   mkdir 7001 7002 7003 8001 8002 8003
   ```

2. 在/tmp下准备一个新的redis.conf文件，文件内容如下（将端口号改为各自端口即可）：

   ```xml
   port 6379
   #开启集群功能
   cluster-enabled yes
   #集群的配置文件名称，该文件只需要指定位置，有Redis自己创建维护
   cluster-config-file /tmp/6379/nodes.conf
   #节点心跳失败的超时时间
   cluster-node-timeout 5000
   #持久化文件存放目录
   dir /tmp/6379
   #绑定地址(0.0.0.0让所有机器都能访问)
   bind 0.0.0.0
   #让redis后台运行
   daemonize yes
   #注册的实例ip
   replica-announce-ip 192.168.150.101
   #保护模式(为no即不做用户名账号密码校验)
   protected-mode no
   #数据库数量
   databases 1
   #日志(开启后台模式后不再在控制台打印日志，需要指定日志存储位置)
   logfile /tmp/6379/run.log
   ```

3. 将该文件分别拷贝到各个夹，修改文件内容对应各自端口。

4. Redis5.0后启动方式：

   ```
   redis-cli --cluster create --cluster-relicas 1 192.168.150.101:7001 192.168.150.101:7002 192.168.150.101:7003 192.168.150.101:8001 192.168.150.101:8002 192.168.150.101:8003
   
   #命令说明
   redis-cli --cluster 或者 ./redis-trib.rb: 代表集群此操作命令
   create: 代表是创建集群
   --replicas 1 或者 --cluster-replicas 1: 指定集群中每个master的副本个数为1，此时 节点总数 / (replicas + 1)得到的就是master的数量，即节点列表中的前n个就是master，其它节点都是slave,随机被分配到不同master。
   ```

5. 启动成功后，通过命令可以查看集群状态：

   ```
   redis-cli -p 7001 cluster nodes
   ```



### 散列插槽(slots)

插槽的出现是为了解决**Redis分片集群中key的查找问题**、**数据转移**和**集群扩容**等问题。首先关于Redis分片集群中的key查找问题，由于不同节点存储不同数据，那么该去哪个节点查找key对应的值？或者说如果节点宕机，又没有哨兵，如何保证消息不丢失？

插槽的出现就是为了解决这些问题，首先，Redis会把每一个master节点映射到0 ~ 16383共**16384**个插槽（hash hot）上（即每个节点**占领一部分插槽**）。而数据key通过**计算插槽值得知应该在哪个节点**，再转到目标节点上。

Redis会根据 key 的**有效部分**计算插槽值，这里可以分为**两种情况**：

- key 中包含“{}”，且“{}”中至少包含一个字符，那么“{}”中的部分就是有效成分
- key 中不包含“{}”，整个key都是有效部分

例如：key是num，那么就根据num计算，如果是{itcast}num，那么就根据itcast计算。计算方式是利用**CRC算法**得到一个hash值，然后对16384取余，得到的结果就是slot值。

**举个例子**：分片集群模式，主节点7001、7002、7003分别占领插槽的一部分，然后在节点7001 set num 10，通过计算key的有效部分得到插槽值，发现该插槽值由7003占领，于是将数据存储到7003节点中。再set score 100，通过计算key的有效部分

**如何将同一类数据固定的保存在同一个Redis实例中？**

给这一类数据使用**相同的{typeId}**来计算插槽值即可。



### 集群伸缩



### 故障转移



### RedisTemplate访问分片集群



# 原理篇

## 网络模型

### 用户空间和内核空间





### 阻塞IO



### 非阻塞IO



### IO多路复用



### 信号驱动IO



### 异步IO



### Redis网络模型