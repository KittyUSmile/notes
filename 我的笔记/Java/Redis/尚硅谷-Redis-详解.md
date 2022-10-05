[TOC]



# Redis入门

> Redis是一种NoSQL(Not Only SQL)数据库，即非关系型数据库。正如Java线程、Hadoop、Nginx、MQ、ElasticSearch一样，它的出现也是为了解决程序的关键问题，它可以用来存储关系型数据库难以存储的数据。其可以解决分布式情况下的session登录问题、也可以解决数据库查询时的速度和压力问题等。 

## 概述

1、Redis是一个开源的key-value存储系统。
2、Redis支持的value类型相对较多，包括String(字符串)、list(链表)、set(集合)、zset(sorted set有序集合)和hash(哈希类型)
3、Redis支持value的push/pop、add/remove、取交集、并集、差集等丰富操作，且这些操作都是原子性。
4、Redis支持各种不同方式的排序。
5、Redis的数据都是存储在缓存中，同时也可周期性的把更新的数据写入磁盘。并在此基础上实现了master-slave(主从)同步。
6、Redis命令不区分大小写。

**功能举例**：

1、通过List类型实现按自然时间排序的数据 —– 获取最新的N个数据。
2、利用zset(有序集合) —– 排行榜，Top N。
3、使用Expire过期 —– 时效性数据。如验证码，登录信息。
4、利用原子性，自增方法INCR、DECR —– 计数器，秒杀服务。
5、利用Set集合 —– 去除大量数据中的重复数据。
6、利用list集合 —– 构建队列。
7、pub/sub模式 —– 发布订阅消息系统。



## NoSQL扩展

1、NoSQL数据库不依赖业务之间的关系而进行分表存储，而是通过KV键值对的方式进行存储，因此极大地增加了其扩展能力。 
2、NoSQL有自己一套规范，不遵循SQL标准，同时不支持ACID(但支持事务)，且性能远超关系型数据库。 
3、NoSQL适用于常见高并发和海量数据读写的情况。
4、NoSQL不适用于需要事务支持或需要复杂结构关系支持的情况。
5、常用的NoSQL数据库有Memcache(较为古早，不支持持久化，KV存储，类型单一，执行为多线程+锁)、Redis(支持持久化，存储数据类型丰富，执行为单线程+多路IO复用)、mongoDB(文档型数据库，数据可存硬盘，KV存储，查询功能丰富，支持二进制数据及大型对象存储)
6、NoSQL的使用打破了传统关系型数据库以业务逻辑为依据的存储模式，而针对不同数据结构类型改为以性能为最优先的存储方式。



# Redis安装

```
安装版本(视频版本): redis-6.2.1.tar.gz
    
安装要求: Linux环境下的Redis版本(一般不考虑Windows环境下的版本)。
    
安装步骤: 
1、先下载redis-6.2.1.tar.gz
2、通过xftp传送至Linux的/opt目录下，再通过xshell连接上Linux，进入/opt目录。进行解压前，需要在Linux环境中安装好C语言的编译环境(因为Redis是C语言编写的)。
    2.1、安装命令(默认安装在/usr/local/bin目录下):
		yum install centos-release-scl scl-utils-build
         yum install -y devtoolset-8-toolchain
         scl enable devtoolset-8 bash
    2.2、或者只安装gcc环境(选一个即可):
		yum install gcc
3、解压压缩包。解压命令:
		tar -zxvf redis-6.2.1.tar.gz
4、解压完成后进入文件解压目录: 
		cd/redis-6.2.1
   4.1、执行编译命令: 
		make
   4.2、若在make阶段出现问题，首先检查是否安装好C语言编译环境，使用命令: gcc --version 查看是否有版本，若无，使用命令(下载C语言编译环境): yum install gcc。若有，则执行命令: make distclean，然后再执行编译命令make即可。
   4.3、执行安装命令(默认安装目录为/usr/local/bin):
		make install
   4.4、安装目录下共有六个安装文件，其作用分别是:
		redis-benchmark: 性能测试工具，可以测试自己的服务器性能如何。
         redis-check-aof: 修复有问题的AOF文件。
         redis-check-dump: 修复有问题的dump.rdb文件。
         redis-sentinel: Redis集群使用。
         redis-server: Redis服务器启动命令。
         redis-cli: 客户端，操作入口。
5、启动命令:
	前台启动(不推荐，不能进行其它操作，窗口关闭即停止，ctrl + c停止服务): redis-server
     后台启动(推荐，窗口关闭仍运行，redis-cli shutdown停止服务): 
		5.1、首先进入/opt/redis-6.2.1.tar.gz目录中，将redis.conf文件复制一份(保留源文件，目的地址哪都行): cp redis.conf /etc/redis.conf。
         5.2、修改etc下的配置文件vi /ect/redis.conf，将128行处的daemonize no修改为yes(允许后台启动)
         5.3、进入redis默认安装目录: cd /usr/local/bin，执行后台启动命令(后面跟的是复制修改后的redis.conf文件目录):redis-server /etc/redis.conf
         5.4、启动成功后，查看redis状态: ps -ef | grep redis
		5.5、通过客户端连接上redis命令: redis-cli
```



## Redis介绍

1、Redis默认16个库，类似数组下标从0开始，初始默认使用0号库。
2、使用select <dbid>进行数据库切换。如select 8。
3、dbsize命令查看但概念数据库的key数量，flushdb清空当前库，flushall清空所有库。
4、Redis使用的是单线程 + 多路IO复用。多路IO复用的意思是线程提出操作申请，然后去做自己的事情，等到被通知运行操作时，再回来进行操作。

## 配置文件介绍

```
启动前将redis.conf复制到/etc下，通过vi/etc/redis.conf进行查看。

开头第一部分: ### Units单位 ###
1、配置内存大小单位，定义了一些基本的度量单位，只支持bytes，不支持bit，且对大小写不敏感。

### INCLUDES ###

1、允许包含其它配置文件并生效。

### NETWORK ###

1、已存在配置: bind 127.0.0.1 -::1。表示只允许本机访问，若要允许其它机器访问，将其注释。
2、已存在配置: protected-mode yes。表示已开启保护模式，即远程不能访问，若要允许远程机器访问，将其修改为no即可。
3、已存在配置: port 6379。表示默认端口号为6379。
4、已存在配置: timeout 0。表示多少时间(秒)未操作即需要重新连接，默认永不超时。
5、已存在配置: tcp-keepalive 300。表示每隔300秒检查一次连接是否仍存活。

### GENERAL ###

1、已存在配置: daemonize no。表示不允许Redis后台启动，修改为yes即表示允许后台启动。
2、已存在配置: loglevel notice。表示日志级别为notice。共有四种日志级别，分别为debug(详细信息，用于写代码/测试阶段)、verbose(有用的信息，不像debug那么多)、notice(一般生产环境中使用)、warning(重要信息)
3、已存在配置: logfile ""。表示默认的日志输出路径，默认无。
4、已存在配置: database 16。即表示Redis默认有16个库。

### CLIENTS ###

1、未配置: maxclients 10000。设置redis同时可以与多少个客户端进行连接。默认情况下为10000个。
2、maxmemory。设置redis可以使用的内存量，建议设置，否则内存占满后，会造成服务器宕机。当内存到达使用上限后，redis将会试图移除内部数据，移除规则可以通过maxmemory-policy来指定。

### SNAPSHOTTING ###
1、已存在配置: dbfilename dump.rdb。表示持久化文件为dump.db。
2、已存在配置: dir./。表示存储位置。默认为当前目录。
3、已存在配置: stop-writes-on-bgsave-error yes。表示当无法写入硬盘时是否取消写入。默认为是。
4、已存在配置: rdbcompression yes。表示存储到磁盘中的快照是否压缩存储。默认为是。
5、已存在配置: rdbchecksum yes。表示是否进行完整性检查。默认为是。这么做大概会增加10%的性能消耗。
6、未配置: save 900 1。表示90秒内至少有一个key改变就进行持久化。save是个自动操作，进行save时只管保存，其它都不管，全部阻塞，不建议使用。建议使用bgsave命令，Redis会在后台异步进行快照操作，同时还可以响应客户端请求。
```

## Redis发布和订阅

```
Redis的发布订阅(pub/sub)是一种消息通信模式: 发送者(pub)发送消息，订阅者(sub)接收消息。Redis的客户端可以订阅任意数量的频道(channel)接收消息，也可以向任意数量的频道(channel)发送消息。当客户端向某频道发送消息后，订阅了这个频道的客户端就能接收到消息。

通过Redis发布和订阅频道消息:
1、subscribe <channel>
	订阅<channel>频道消息。
2、publicsh <channel> <message>
	向<channel>频道发送消息<message>
```



# Redis数据类型

> Redis中的共有五大数据类型: 字符串(String)、列表(List)、集合(Set)、哈希(hash)、有序集合(Zset)。



## Redis键(key)

```
关于键值的常用命令:
1、keys *
    查看当前库的所有key。如(查询一号库所有键): keys *1
2、exists <key>
    判断某个key是否存在，存在返回1，不存在返回0。如(判断key a1是否存在): exists a1 
3、type <key>
    查看key的类型。如(查看键值a的类型): type a
4、del <key>
    删除指定的key数据。如(删除键值为a的键值对): del a
5、unlink <key>
    根据value选择非阻塞删除。即执行命令后仅将key从keyspace元数据中删除，而真正的删除会在后续异步操作中删除。
6、expire <key> time
    给key设置过期时间time(秒)。如(键值a的存活时间为10s): expire a 10
7、ttl <key>
    查看key还有多少秒过期，-1表示永不过期，-2表示已过期。如(查询a过期时间): ttl a

关于库的命令:
1、select 库
    切换当前使用的数据库。如(切换为使用2号库): select 2
2、dbsize
    查看当前数据库的key的数量
3、flushdb
    清空当前库
4、flushall
    清空所有库
```

## 字符串(String)

> String是Redis最基本的类型，一个key对应一个value。String类型是二进制安全的，即只要一个数据类型可以使用字符串表示，就都可以存储到Redis中。一个Redis的字符串value最多可以存储512M数据。

```
常用命令:
1、set <key> <value>
	向当前库中添加key对应的value，若key已存在，则直接覆盖。如: set a1 1
2、get <key>
	获取当前库中key对应的value。如: get a1
3、append <key> <value>
	在key对应的value后追加指定的value，并返回总长度。如: append a1 2
4、strlen <key>
	得到key对应的value的长度。
5、setnx <key> <value>
	当key不存在时，向当前库中添加key对应的value，若key存在，则设置不成功。
6、incr <key>
	当key对应的value为数字时，value加一，如果value为空，则新增值为1。返回增1后的值。
7、decr <key>
	当key对应的value为数字时，value减一，如果value为空，则新增值为-1。返回减1后的值。
8、incrby / decrby <key> <步长>
	当key对应的value为数字时，增/减对应的值(步长)并返回。改操作均是原子操作，即不会被线程调度机制打断的操作，基于Redis是单线程，这种操作也不会被其它线程打断。

其它命令:
1、mset <key1> <value1> <key2> <value2>...
	同时设置一个或多个key-value对。
2、mget <key1> <key2>...
	同时获取一个或多个value。
3、msetnx <key1> <value1> <key2> <value2>...
	当其中所有key都不存在时才设置成功，若其中有一个key已存在，则全部设置失败。
4、getrange <key> <起始位置> <结束位置>
	获取指定key对应的value值的下标从<起始位置>到<结束位置>的值。 
5、setrange <key> <起始位置> <value>
	在key对应的value的下标为<起始位置>处插入<value>。
6、setex <key> <过期时间> <value>
	设置key-value的同时设置<过期时间>(秒)。
7、getset <key> <value>
	以新换旧，获取到key对应的旧value的同时设置value为新value。
```

### 底层数据结构

```
String的数据结构为简单动态字符串(Simple Dynamic String，缩写为SDS)，是可以修改的字符串，其内部结构实现上类似于Java的ArrayList，采用了预分配冗余空间的机制减少内存的频繁分配。即实际使用的内存len小于实际分配的内存capacity，当字符串长度小于1M时，扩容是加倍capacity，当字符串超过1M时，扩容只会每次多扩1M空间，注意String类型最大长度为512M。
```

## 列表(List)

> 单键多值。即一个key对应一个字符串列表，列表有序，并允许添加元素到列表头或列表尾。列表底层由双向链表构成，因此操作两端数据时效率较高，查询或操作中间数据时效率较低。

```
常用方法:
1、lpush / rpush <key> <value1> <value2> <value3>
	从左/右边向key对应的列表插入<value1>,<value2>,<value3>。如(向a列表中从左依次插入1,2,3): lpush a 1 2 3。最终list为3,2,1。
2、lpop / rpop <key>
	从左边/右边弹出key对应的列表的一个值，若值弹完，列表亡。
3、rpoplpush <key1> <key2>
	从列表<key1>右边弹出一个值，插入<key2>的左边。
4、lrange <key> <start> <stop>
	按照<start>到<stop>下标获取key中对应值。如(获取k1中所有值):lrange k1 0 -1。(-1表示最后一位，-2表示倒数第二位，以此类推)
5、lindex <key> <index>
	按照索引下标<index>获取<key>列表中的值。
6、llen <key>
	获取key对应的列表的长度。
7、linsert <key> before/after <value> <newvalue>
	找到key对应列表从左其第一个<value>，在其左边插入<newvalue>。如(在k1对应列表的左起第一个v1左边插入v0): linsert k1 before v1 v0
8、lrem <key> <n> <value>
	从左边其删除key对应列表中的n个<value>。
9、lset <key> <index> <value>
	将key对应列表中下标为<index>的值替换为<value>。
```

### 底层数据结构

```
1、List底层的数据结构为快速链表qucikList，快速链表其实由压缩列表(zipList)构成，压缩列表即一块连续的内存空间，当列表元素较少时就采用压缩列表存储，其中的元素挨个存储，因此也具有双向链表特征。
2、当列表元素较多时，就再开一片压缩列表空间用来存储列表元素，并与之前的压缩列表互相指向。
3、Redis采用这种存储方法的原因为: 若每个列表元素都附加两个指针，那么内存开销会很大，因此使用zipList进行互相指向减少内存消耗，zipList中的元素由于连续，也拥有双向链表的特性。
```

## 集合(Set)

> Set提供的功能与List类似，也是[key-列表]结构，不同点在于Set可以自动去重且无序的。Set是String类型的无序集合，其底层实际上是一个value为null的hash表，因此添加，删除，修改的时间复杂度都为O(1)。

```
常用方法:
1、sadd <key> <value1> <value2>...
	将一个或多个member元素加入到key对应的Set中，已经存在的member元素会被忽略。
2、smembers <key>
	取出<key>对应的Set中的所有值。
3、sismember <key> <value>
	判断<key>对应的Set中是否存在值<value>，存在返回1，不存在返回0。
4、scard <key>
	返回key对应的Set中元素个数。
5、srem <key> <value1> <value2>...
	删除key对应的Set中的元素<value1>,<value2>..。
6、spop <key>
	随机从key对应的Set中弹出一个值。
7、srandmember <key> <n>
	随机从key对应的Set中获取<n>个value，不会进行删除操作。
8、smove <source> <destination> <value>
	把<value>从<source>集合中移到<destination>集合中。
9、sinter <key1> <key2>
	返回key1和key2分别对应的Set的交集元素。
10、sunion <key1> <key2>
	返回key1和key2分别对应的Set的并集元素。
11、sdiff <key1> <key2>
	返回两个集合的差集元素(key1中包含的，不包含key2中的)。
```

### 底层数据结构

```
Set底层数据结构其实是hash结构，所有的value都指向同一个内部值。
```

## 哈希(Hash)

```
介绍:
1、哈希也是一个键值对集合，每个键值对都是一个String类型key对应一个或多个field和value的键值对，hash适合存储对象，其类似于Java中的Map<String, Object>。
	即:
		key 		value
		
			 	field  value
				  id     1
        user  ->  name   张三
        		  age     20

常用命令:
1、hset <key> <field1> <value1>
	给<key>对应哈希的<field1>赋值为<value1>。
2、hget <key1> <field>
	从<key1>对应哈希中取<field>对应的value。
3、hmset <key1> <field1> <value1> <field2> <value2>...
	批量设置key1对应hash的值。
4、hexists <key1> <field>
	查看哈希表key1中是否存在给定域<field>。
5、hkeys <key>
	列出该hash集合中所有的field。
6、hvals <key>
	列出该hash集合中所有的value。
7、hincrby <key> <field> <increment>
	为哈希表key中的域field对应的value加上增量<increment>
8、hsetnx <key> <field> <value>
	当且仅当域field不存在时，将哈希表key中的域field的值设置为value。
```

### 底层数据结构

```
Hash类型对应的数据结构为两种: zipList(压缩列表)，hashtable(哈希表)。当field-value长度较短且个数较少时，使用zipList，否则使用hashtable。
```

## 有序集合(ZSet)

> ZSet和Set非常相似，都是一个没有重复元素的字符串集合。不同之处为ZSet中每个成员都关联了一个评分(score)，这个评分(score)被用来按照从最低分到最高分的方式排序ZSet中的成员，集合中的成员是唯一的，评分可以重复。因为ZSet中的元素是有序的，因此可以很快通过评分(score)或者次序(position)来获取一个范围的元素。访问ZSet的中间元素也是非常快的，因此能够使用ZSet作为一个没有重复成员的智能列表。

```
常用方法:
1、zadd <key> <score1> <value1> <score2> <value2>...
	将一个或多个member元素及其score值加入到有序集合key中。
2、zrange <key> <start> <stop> [WITHSCORES]
	返回有序集key中，下标在<start>与<stop>之间的元素，若携带[WITHSCORES]，则可以让分数一起和值返回到结果集。如(查询topn有序集合中带分数的所有元素): zrange topn 0 -1 withscores 
3、zrangebyscore <key> <min> <max> [withscores] [limit offset count]
	返回有序集合key中的score处于<min>到<max>之间的元素，包括min和max。且返回元素按score从小到大排序。
4、zrevrangebyscore <key> <max> <min> [withscores] [limit offset count]
	返回有序集合key中的score处于<min>到<max>之间的元素，包括min和max。且返回元素按score从大到小排序。
5、zincrby <key> <increment> <value>
	为有序集合<key>的元素<value>的score增加增量<increment>。如(为topn有序集合中的Java的scores增加100): zincrby topn 100 Java
6、zrem <key> <value>
	删除该集合下指定的<value>值。
7、zcount <key> <min> <max>
	统计该集合下指定分数区间内的元素个数。包含左右。
8、zrank <key> <value>
	返回该值在集合中的排序，下标从0开始
```

### 底层数据结构

```
ZSet是Redis提供的一个非常特别的数据结构，一方面它等价于Java中的数据结构Map<String, Double>，可以为每一个元素value赋予一个权重score，另一方面它又类似于TreeSet，其内部的元素会按照权重score进行排序，可以得到每个元素的名次，还可以通过score的范围来获取元素的列表。

详解:
1、ZSet底层使用了两个数据结构，一个是hash，hash的作用是关联元素value和权重score，用来保证元素value的唯一性和能够通过value找到score值。另一个则是跳表，跳表的目的在于给各个元素的value进行排序，并根据score的返回获取元素列表。
```

### 跳表

```
传统的数据结构中，如数组、链表或平衡二叉树，其各有优缺点，数组易查改，链表易增删。但它们都不能兼容二者，直到跳表横空出世，跳表兼顾了数据易查改和链表易增删的优点，其性能堪比红黑树，但实现远比红黑树简单。
```

## Bitmaps

> Bitmaps并不是一种数据类型，其本质上还是字符串。Redis提供这种类型，允许其对字符串的位直接进行操作。Bitmaps单独提供了一套命令，因此在Redis中使用Bitmaps和使用字符串的方法不太一样。可以把Bitmaps想象成一个以位为单位的数组，数组中的每个单元只能存储0和1，数组的下标在Bitmaps中被称为偏移量。

```
常用方法:
1、setbit <key> <offset> <value>
	设置Bitmaps中某个偏移量(下标)的值。如(将user:20220727中偏移量为1的值设置为1): setbit user:20220727 1 1。
2、getbit <key> <offset>
	获取Bitmaps中某个偏移量的值。
3、bitcount <key> [start end]
	统计字符串中被设置为1的bit数量。一般情况下整个字符串都会被进行计数，通过指定额外的start或end参数，可以让计数只在特定的位上进行。start和end都可以使用负数值，如-1表示最后一位，-2表示倒数第二位，start、end指的是bit数组(一个bit数组8位byte)的下标数，二者皆包含。
4、bitop and <name> <key1> <key2>
	统计<key1>和<key2>中偏移量均为1的数量，并将结果存到<name>中。如(查询这两个key中偏移量均为1的数量，将其存入union中): bitop and union user:0726 user:0727


知识点:
1、Bitmaps的应用场景多为统计登录信息，即某用户在某日是否登录过。但也存在细节问题，如用户id以指定数字(如10000)开头，直接将用户id和Bitmaps的偏移量进行对应存储肯定会造成内存浪费，因此进行setbit时通常会用用户id减去这个指定数字。
2、首次初始化Bitmaps时，加入偏移量较大，那么初始化过程会较慢，有可能会造成Redis的阻塞。
3、使用Bitmaps最大的好处是占用资源极少，每个用户只占用一个byte，1/8个字节。仅为Set集合类型存储空间的1/64。当存在1亿用户时，每日统计内存量也仅占用12.5MB。
4、但当用户量过小或者活跃用户过少，则使用Bitmaps就不合适了，因为大部分位都为0。
```

## HyperLogLog

> HyperLogLog可以看作是Redis提供的用来解决Set存储空间大的问题，HyperLogLog可以看成是一种Set，但是其耗空间极少，它存储数据通过基数统计，基数统计即获取去重之后的数据。其底层采用HyperLogLog算法，在Redis中每个键都占用12K内存，但value理论上能够存储2^64个值，其存在约0.81%的标准错误。

```
常用方法:
1、pfadd <key> <element> [element..]
	添加指定元素到key对应的HyperLogLog中。如: pfadd 20220727 "192.132.1.45"
2、pfcount <key> [key...]
	计算指定单个key或多个key对应的HLL中的元素个数。
3、pfmerge <destkey> <sourcekey> [sourcekey...]
	将指定<sourcekey>或多个<sourcekey>对应的HLL中的元素合并到新的HLL<destkey>中。若<destkey>不存在，则自动创建。
	
知识点:
1、HLL一般用来统计网站日活，月活，统计IP等，占用空间远小于同等作用的Set。
```

## Geospatial

> Redis在3.2.0起提供的一种用以存储地理位置的数据类型。一般应用于微信定位、附近的人或者打车距离计算等。其底层采用的是geohash，Redis的数据类型Geospatial能通过上传的经纬度计算出geohash并存储，即长度为12个字符的字符串，不同城市的经纬度算出的geohash前缀约接近，则相距越近。

```
常用方法:
1、geoadd <key> <longitude> <latitude> <member> [<longitude> <latitude> <member>...]
	向key中加入经度为<longitude>，纬度为<latitide>的<member>成员。如: geoadd China:city 121.47 21.23 shanghai。
2、geopos <key> <member> [member...]
	获取key中<member>对应的经纬度。
3、geodist <key> <member1> <member2> [m|km|ft|mi]
	获取key中<member1>和<member2>之间的直线距离，其中m表示米[默认值]，km表示单位为千米，mi表示单位为英里，ft表示单位为英尺。如(key中shanghai和beijing之间的距离，单位为km): geodist China:city shanghai beijing km
4、georadius <key> <longitude> <latitude> radius m|km|ft|mi
	以给定的经<longtitude>纬<latitude>度为中心，找出某半径内的key中的元素。如(查找key中经纬度为115.25,38.28半径为1000km内的元素): georadius China:city 115.25 39.28 1000 km

知识点:
1、有效的经度从-180度到180度，有效的纬度从-85.05112878到85.055112878。其中两极是无法添加的。
2、当坐标位置超出指定范围时，该命令会返回一个错误。
3、已经添加的数据，无法再次往其中添加。
```

# SpringBoot-Redis

SpringBoot项目中使用Jedis步骤：

1、引入Redis依赖(底层引入了lettuce客户端而不是Jedis)。

```xml
<!--        redis依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
```

2、配置application文件：

```yaml
spring:
  redis:
#   服务器地址
    host: xxxxxx
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

3、向容器中加入自定义的RedisTemplate(操作Redis的类)和CacheManager(缓存管理器)：

```java
@EnableCaching
@Configuration
public class RedisConfig extends CachingConfigurationSelector {

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        RedisSerializer<String> redisSerializer = new StringRedisSerializer();
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        template.setConnectionFactory(factory);


        //设置“key"的序列化方式
        template.setKeySerializer(new StringRedisSerializer());

        //设置“值”的序列化方式
        template.setValueSerializer(jackson2JsonRedisSerializer);

        //设置“hash”类型数据的序列化方式
        template.setHashValueSerializer(jackson2JsonRedisSerializer);

        return template;
    }

    @Bean
    public CacheManager cacheManager(RedisConnectionFactory factory) {
        StringRedisSerializer redisSerializer = new StringRedisSerializer();
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);

        //解决查询缓存转换异常的问题
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        //配置序列化(解决乱码问题)过期时间600秒
        RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ofSeconds(600))
                .serializeKeysWith(RedisSerializationContext.SerializationPair.fromSerializer(redisSerializer))
                .serializeKeysWith(RedisSerializationContext.SerializationPair.fromSerializer(jackson2JsonRedisSerializer))
                .disableCachingNullValues();
        RedisCacheManager cacheManager = RedisCacheManager.builder(factory)
                .cacheDefaults(config)
                .build();
        return cacheManager;
    }
}
```

# 事务和锁机制

> Redis中的事务是一个单独的隔离操作。事务中的所有命令都会被序列化，然后串行化执行。且事务在执行的过程中，不会被其它客户端发送来的命令请求所打断。Redis事务的主要作用就是串联多个命令防止别的命令插队。

```
Redis中通过三个命令: 开启事务(Multi)，执行事务(Exec)，放弃事务(discard)来管理事务。当开启事务时，Redis指令会顺序进入命令队列中，等到执行事务时，再依次进行执行。若中途放弃事务，则所有指令不执行。
	
	使用如:
	  >multi (开启事务)
	  >set k1 v1  (加入队列)
	  >set k2 v2  (加入队列)
	  >exec  (提交事务)

知识点:
1、若组队阶段队列中某个命令出现错误，则执行时整个队列都会取消执行。
2、若执行阶段某个命令出现错误，则只有报错的命令不会被执行，其它正常命令会成功执行。
```

## 悲观锁

> 每次拿到数据后都将数据上锁，其它人想拿这个数据只能等到解锁。好处是安全，缺点是效率很低。

```
在传统的关系型数据库中用到了很多这种锁机制，如行锁，表锁等，读锁，写锁等，都是在操作之前先上锁。
```



## 乐观锁

> 不进行上锁，每次操作数据时加上一个版本号，如获取数据1时同时设置版本号为v1.0，操作结束进行修改时，需要先获取当前数据1版本号，若仍未v1.0，则进行修改。若不为v1.0，说明数据已经被修改过，需要再次获取数据和版本号。

```
乐观锁适用于多读的应用类型，可以提高吞吐量。Redis采用的就是这种机制实现的事务。
```

## WATCH

> WATCH命令可以用来在事务开启之前对key进行监控，如果在事务执行前这写key被其他命令所改动，那么事务将被打断。

```
命令:
1、WATCH key [key1 key2...]
	开启一次事务之前，watch对应的key，在执行事务期间若key被其他命令改写，则事务中命令全部失败。
```

## UNWATCH

> 正与WATCH相反，UNWATCH命令取消WATCH命令对所有的key的监视。

## Redis事务特性

> Redis的事务有三大特性：1、单独的隔离操作。2、没有隔离级别的概念。3、不保证原子性。

1、**单独的隔离操作**。

​	事务中的所有命令都会序列化，按顺序地执行。事务在执行的过程中，不会被其它客户端发送来的命令请求打断。
2、**没有隔离级别的概念**。

​	队列中的命令没有提交之前都不会实际被执行，因为事务提交前任何指令都不会被实际执行。
3、**不保证原子性**。

​	事务中如果有一条命令执行失败，其后的命令仍会被执行，不会进行回滚。

## 模拟秒杀案例

> 通过Redis模拟秒杀案例实现

1、在虚拟机中安装并发测试程序命令：

```
yum install httpd-tools
```

2、查看辅助命令：

```
ab -helo
```

3、使用：

```
ab [options] [http[s]://]hostname[:port]/path

[options]操作举例(同时进行):
1、-n : 请求次数
2、-c : 并发次数
3、-T : 当提交方式为POST/PUT时，设置提交类型为'application/x-www-form-urlencoded'
4、-p : 当提交方式为POST时，将提交参数放入postfile中
```

### 解决超卖问题

> 并发情况下程序可能会出现超卖问题。

```
超卖问题解决:
1、对将执行的操作加上事务。
2、加上事务之前watch key。
3、执行exec，判断结果是否为空。

	如:
		//解决超卖问题
        redisTemplate.watch(List.of(tagProd + prodId, tagPeo + prodId));
        redisTemplate.multi();
        redisTemplate.opsForValue().set(tagProd + prodId, i - 1);
        redisTemplate.opsForSet().add(tagPeo + prodId, uid);
        List exec = redisTemplate.exec();
        if (exec == null){
            return R.fail(400, "抢购失败！");
        }
        return R.success("抢购成功！");
	
```

### 解决库存遗留问题

> 乐观锁造成的库存遗留问题。多个用户同时抢到商品和获取版本号v1.0，其中一个用户付款成功商品数量减一，版本号修改为v1.1，其它用户提交时版本号不同，无法修改库存数量。即可以简单类比为抢到了无法付款，最后造成库存遗留。

**库存遗留问题解决：**
1、通过LUA脚本解决。LUA脚本允许将复杂的多个Redis操作写为一个脚本，让LUA脚本直接提交给Redis执行，这种执行是原子性的，不会被其它命令插队，可以完成一些redis事务性的操作。但是只有Redis2.6以上版本才可使用LUA脚本。LUA脚本可以解决超卖问题和库存遗留问题。

# Redis持久化

> Redis在内存中运行，当然也支持将数据写入硬盘，这种机制被称为Redis持久化。Redis提供了两种持久化方式：RDB(Redis DataBase)、AOF(Append Only File)。

## RDB(Redis DataBase)

> 在指定的时间间隔内将内存中的数据集快照写入硬盘中。数据集快照即某一时间点的内存文件。恢复文件即将快照文件直接读入内存中。RDB是默认开启的。

### RDB持久化流程

1、Redis首先单独创建(fork)出一个子进程来进行持久化操作，这个子进程与当前主进程完全一致(所有数据，包括变量、环境变量、程序计数器等)，但是是一个全新的进程，并作为主进程的子进程。
2、子进程将数据写入临时文件中，等持久化过程结束后，将临时文件中的数据替换上次的持久化文件(dump.rdb)。如果直接将内存文件写入持久化文件中存在一个问题，写到一半时宕机，那么持久化文件中的数据就不完整了，因此先写入临时文件中，再写入持久化文件。该技术被称为写时复制技术。这种技术的缺点是可能丢失最后一次的临时文件。



### RDB配置

1、RDB是Redis中默认开启的，进行查看首先进入redis.conf文件中，可以找到相关配置信息：

dbfilename dump.rdb （持久化文件名字）

dir ./ （表示在启动目录下生成持久化文件）

stop-writes-on-bgsave-error yes（当Redis无法写入磁盘时，关闭Redis的写操作，推荐为yes）



### RDB持久化策略

> RDB持久化提供了两种触发策略，即什么时候进行RDB持久化。

#### 1、手动触发策略

手动触发策略是指通过**save**命令或者**bgsave**命令将内存数据保存到磁盘上的指定持久化文件中。

其中save命令是阻塞式的，在使用save命令保存数据快照时，Redis服务器进程会被阻塞，不能处理其它请求，直到dump.rdb文件创建完毕为止。

而bgsave命令是非阻塞式的，即该命令在执行时，不会影响Redis服务器的正常运行，其仍能够处理客户端的请求。原因是执行该命令时，Redis会fork()一个子进程来进行持久化操作（如创建出新的dump.rdb文件），而父进程则继续处理客户端请求。当子进程处理完成后，会向父进程发送一个信号，通知它已经处理完毕。然后，父进程将用新的dump.rdb覆盖原先的持久化文件。

由于save命令无需创建子进程，因此执行速度略快于bgsave命令，但是save命令是阻塞式的，所以可用性欠佳，在数据量较小的情况下，基本体会不到二者命令的差别，不过仍建议使用bgsave而不是save命令。



#### 2、自动触发策略

自动触发策略即Redis在指定时间内，数据发生了多少次变化，就自动执行BGSAVE命令，自动触发条件需要在redis.conf中进行配置，如：

```
#900秒内数据发生1次变化则自动执行bgsave命令	
save 900 1 
#300秒内数据发生10次变化则自动执行bgsave命令	
save 300 10
#60秒内数据发生10000次变化则自动执行bgsave命令	
save 60 10000
```

只要满足上诉配置的三个条件中的任意一个，服务器就会自动执行bgsave命令。

[^注意 ]: 每次创建RDB文件后，Redis服务器为实现自动持久化而设置的时间计数器和次数计数器就会被清零，并重新开始计数，因此多个配置策略的效果不会叠加。





### RDB的优势

1、适合大规模的数据恢复。
2、更适用于对数据完整性和一致性要求不高的数据。
3、节省磁盘空间。
4、恢复速度快。
5、效率高于AOF。

### RDB的缺点

1、fork时，内存中的数据被克隆了一份，会导致内存扩大为原先的2倍。
2、Redis虽然在fork时使用了写时复制技术，但是当数据庞大时还是较为消耗性能。
3、如果Redis在备份期间down掉了，那么将丢失最后一次快照的所有修改。

### RDB持久化过程举例

1、开启save配置，操作达到持久化的条件，备份dump.rdb文件，让redis停止服务，删除dump.rdb文件，重新启动redis前将备份文件复制回来，并改名为dump.rdb。重启redis，此时redis就会自动扫描dump.rdb文件将保存的数据恢复。

## AOF(Append Only File)

> 以日志的形式来记录每个写操作(增量保存)，将Redis执行过的所有写指令记录下来(不记录读操作)，追加到相关文件内容最后，在Redis启动时会读取该文件重新构建内存数据，即Redis重启通过日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作。AOF默认不开启。

### 开启Redis的AOF

1、在redis.conf配置文件中找到appendonly no配置，该配置表示AOF并未开启，将其修改为yes。

2、AOF配置下一行存在配置appendfilename “appendonly.aof”。该配置表示AOF持久化文件的位置。一般不需要改，默认与RDB文件生成位置相同。



### AOF和RDB同时开启

AOF和RDB如果同时开启，那么持久化复原操作将默认采用AOF方式，即找到appendonly.aof进行复原。而不会通过RDB复原。



### AOF持久化流程

1、每当有修改数据库的命令被执行时，服务器就将命令写入到appendonly.aof文件中，该文件存储了服务器执行过的所有修改命令，因此，只要服务器执行一次appendonly.aof文件，就可以实现还原数据的目的，这个过程被称为“命令重演”。



