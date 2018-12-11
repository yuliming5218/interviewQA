# interviewQA

后端架构师技术图谱

https://www.cnblogs.com/xdecode/p/9212881.html 

**整理的一些面试题目**

1.一次完成的HTTP请求过程

答：Http 的header会给我们的请求包装，比如AF中经常设置的可接受的Accept（text/html） --》域名解析，根据域名找到服务器的IP --> 发起TCP的3次握手 --> 建立TCP连接后发起http请求 --> 服务器响应http请求，浏览器得到html代码 --> 浏览器解析html代码，并请求html代码中的资源（如js、css、图片等） --> 浏览器对页面进行渲染呈现给用户
每次都请求都会经过  客户端的应用层（http协议）-->  客户端的传输层（tcp或udp协议） -->客户端的网络层（ip协议） --> 客户端的链路层（网卡，路由器等） -->  ------------------经过dns解析，穿越多个isp（互联网服务提供商，移动，联通，电信等），各种数据交换，找到了服务器------------------- 服务器的链路层  -->服务器的网络层  -->服务器的传输层  -->服务器的应用层。 这个请求完成了。

精简版本：
客户端应用层-->客户端传输层（TCP）-->客户端协议层（IP）-->链路层（网卡，路由器）-->  DNS解析 通过ISP 数据交换 找到指定服务器--> 服务端链路层 --> 服务端协议层 --> 服务端传输层 --> 服务端应用层 



2.APP桌面登陆的全过程，登陆二维码里面存储的关键信息是什么
二维码关键信息UUID token 等


3.限流和熔断的区别，有几种限流策略
单机限流 分布式限流
策略 滑动窗口  
限流 令牌桶和漏桶算法 适合阻塞限流熔断
基于时间窗口的限流算法 适合否决式的限流熔断


4.设计一个连接池


5.自定义序列化协议，常用序列化方式对比


6.DNS


7.负载均衡网络拓扑

8.从一个很大的集合数据集合里面找到重复的数据，不考虑空间复杂度
关键字：数据量集合很大，不考虑空间复杂度，那么就用每个集合的value作为 另一个集合的index，将集合一的数据存入集合二中，同样位置有存在数据的就是重复的数据  时间复杂度  O（N）

9.领域驱动模型设计
将  名词  动词  形容词  进行划分 来设计类和方法 ？


10.mysql 事物隔离级别
未提交读：脏读
已提交读：一个事务范围内两个相同的查询却返回了不同数据
可重复读：幻读 不可重复读对应的是修改，即UPDATE操作。但是可能还会有幻读问题。因为幻读问题对应的是插入INSERT操作，而不是UPDATE操作。
序列化：串行化顺序执行，效率低下，一般不用


11.RetrainLock ReadWriteLock
java多线程采用的是抢占式算法， RetrainLock  分公平锁 和费公平锁 ，默认是非公平锁，非公平锁即，有新锁获取需求来了可以直接抢占正好释放掉的锁，类比路口车辆启动 启动快的车辆快速启动抢占道路，公平所消耗比非公平锁要答，因为内部需要考虑获取锁的顺序，需要判断顺序。ReadWriteLock  多个获取读锁的可共享，但是读和写，写和写 都互斥


12.线程池ThreadPoolExecutor实现原理

execute方法执行逻辑有这样几种情况：

如果当前运行的线程少于corePoolSize，则会创建新的线程来执行新的任务；
如果运行的线程个数等于或者大于corePoolSize，则会将提交的任务存放到阻塞队列workQueue中；
如果当前workQueue队列已满的话，则会创建新的线程来执行任务；
如果线程个数已经超过了maximumPoolSize，则会使用饱和策略RejectedExecutionHandler来进行处理。

需要注意的是，线程池的设计思想就是使用了核心线程池corePoolSize，阻塞队列workQueue和线程池maximumPoolSize，这样的缓存策略来处理任务，实际上这样的设计思想在需要框架中都会使用。


13.memcached
采用多路复用技术提高并发性。
slab分配算法： memcached给Slab分配内存空间，默认是1MB。分配给Slab之后 把slab的切分成大小相同的chunk，Chunk是用于缓存记录的内存空间，Chunk 的大小默认按照1.25倍的速度递增。好处是不会频繁申请内存，提高IO效率，坏处是会有一定的内存浪费。


某缓存系统采用LRU淘汰算法，假定缓存容量为4,并且初始为空，那么在顺序访问一下数据项的时候：1,5,1,3,5,2,4,1,2出现缓存直接命中的次数是？，最后缓存中即将准备淘汰的数据项是？

答案：3， 5
解答：
1调入内存 1

5调入内存 1 5

1调入内存 5 1（命中 1，更新次序）

3调入内存 5 1 3

5调入内存 1 3 5 （命中5）

2调入内存 1 3 5 2

4调入内存（1最久未使用，淘汰1） 3 5 2 4

1调入内存（3最久未使用，淘汰3） 5 2 4 1

2调入内存 5 4 1 2（命中2）

因此，直接命中次数是3,最后缓存即将准备淘汰的数据项是5



14. redis 
分布式reids复习精华
https://blog.csdn.net/xzqxiaoqing/article/details/82662145

redis集群 数据迁移方式 Hash槽 和 一致性hash对比，优缺点比较

集群：
是一个提供多个Redis（分布式）节点间共享数据的程序集。
集群部署
Redis 集群的键空间被分割为 16384 hash个槽（slot）， 集群的最大节点数量也是 16384 个
关系:cluster>node>slot>key

Redis Cluster在设计中没有使用一致性哈希（Consistency Hashing），而是使用数据分片引入哈希槽（hash slot）来实现；

一个 Redis Cluster包含16384（0~16383）个哈希槽，存储在Redis Cluster中的所有键都会被映射到这些slot中，

集群中的每个键都属于这16384个哈希槽中的一个，集群使用公式slot=CRC16（key）/16384来计算key属于哪个槽，其中CRC16(key)语句用于计算key的CRC16 校验和。

为什么Redis集群要设置16384个槽？

这是一个很有趣的问题。我们都知道redis集群有16384个槽（slot）。那么为什么就要是16384呢？redis在用key进行定位的时候，使用CRC16进行校验，CRC16是16位的，理论上说有2^16 -1=65535个不同的值，为什么不用65535呢？

1，在一个节点的心跳包文中，包含着这个节点所有的配置信息。使用16K个槽，需要的内存空间大约是2K。如果使用65K个槽，那么需要的内存空间是8K。


2，reids集群中主节点数量基本不可能超过1000。(however the suggested max size of nodes is in the order of ~ 1000 nodes).所以可以保证每个主节点上槽的数量不会太少。


3，redis节点的配置信息在通过bitmap存储的。bitmap在传输过程中会进行压缩。而压缩比和（槽的数量/节点数）有关，这个值越大。压缩比越小。所以如果采用65535个槽，那么压缩率就会比较小。

综合考虑，redis的作者选择了16384

redis 的 rehash 机制，渐进式序列化 ？？

redis 五种数据类型

类型常量	对象的名称
REDIS_STRING	字符串对象
REDIS_LIST	列表对象
REDIS_HASH	哈希对象
REDIS_SET	集合对象
REDIS_ZSET	有序集合对象


底层数据结构共有八种，如下表所示：

编码常量	编码所对应的底层数据结构
REDIS_ENCODING_INT	long 类型的整数
REDIS_ENCODING_EMBSTR	embstr 编码的简单动态字符串
REDIS_ENCODING_RAW	简单动态字符串
REDIS_ENCODING_HT	字典
REDIS_ENCODING_LINKEDLIST	双端链表
REDIS_ENCODING_ZIPLIST	压缩列表
REDIS_ENCODING_INTSET	整数集合
REDIS_ENCODING_SKIPLIST	跳跃表和字典


Redis 底层中的 SDS   没有使用C语言传统的字符串表示，而是自己构建了一种名为简单动态字符串（simple dynamic string，SDS） 的抽象类型，并将SDS用作Redis的默认字符串表示。
SDS相比较于c字符串的好处
取字符长度，SDS直接读取len属性。
杜绝缓冲区溢出。C字符串拼接时时假定已经为拼接的字符串预留了足够多的内存，如果这个假定不成立，那么就会产生缓冲区溢出。而SDS是这样做的：SDS的API会会先检查SDS的空间是否满足所需的要求，如果不满足，API自动将空间扩展至所需大小。
减少修改字符串长度时所需的内存重分配次数。
二进制安全。
Redis基于以上数据结构创建了一个对象系统，每种对象都用到了至少一种之前介绍的数据结构。
每次当我们在Redis中新创建一个键值对时，我们至少会创建两个对象，一个对象用作键值对的键（键对象），另一个对象用作键值对的值（值对象）。
使用对象系统的好处：

（1）在执行命令时，根据对象类型判断一个对象是否可以执行给定的命令

（2）针对不同的使用场景，为对象设置多种不同的数据结构，从而优化对象在不同场景下的使用效率；

（3）基于对象引用计数技术实现内存的回收；

（4）通过引用计数技术实现对象的共享；

在Redis中每个对象都由一个redisObject结构表示，包含了type，encoding，ptr属性：

Redis的五种对象类型和它们的底层实现。事实上，Redis的高效性和灵活性正是得益于对于同一个对象类型采取不同的底层结构，并在必要的时候对二者进行转换；以及各种底层结构对内存的合理利用。

Redis 持久化类型

RDB
AOF

RDB 持久化可以在指定的时间间隔内生成数据集的时间点快照（point-in-time snapshot）。

AOF 持久化记录服务器执行的所有写操作命令，并在服务器启动时，通过重新执行这些命令来还原数据集。 AOF 文件中的命令全部以 Redis 协议的格式来保存，新命令会被追加到文件的末尾。 Redis 还可以在后台对 AOF 文件进行重写（rewrite），使得 AOF 文件的体积不会超出保存数据集状态所需的实际大小。
Redis 还可以同时使用 AOF 持久化和 RDB 持久化。 在这种情况下， 当 Redis 重启时， 它会优先使用 AOF 文件来还原数据集， 因为 AOF 文件保存的数据集通常比 RDB 文件所保存的数据集更完整。
你甚至可以关闭持久化功能，让数据只在服务器运行时存在。


Redis单线程架构
1 单线程模型

Redis客户端对服务端的每次调用都经历了发送命令，执行命令，返回结果三个过程。其中执行命令阶段，由于Redis是单线程来处理命令的，所有每一条到达服务端的命令不会立刻执行，所有的命令都会进入一个队列中，然后逐个被执行。并且多个客户端发送的命令的执行顺序是不确定的。但是可以确定的是不会有两条命令被同时执行，不会产生并发问题，这就是Redis的单线程基本模型。

2 单线程模型每秒万级别处理能力的原因

（1）纯内存访问。数据存放在内存中，内存的响应时间大约是100纳秒，这是Redis每秒万亿级别访问的重要基础。

（2）非阻塞I/O，Redis采用epoll做为I/O多路复用技术的实现，再加上Redis自身的事件处理模型将epoll中的连接，读写，关闭都转换为了时间，不在I/O上浪费过多的时间。

（3）单线程避免了线程切换和竞态产生的消耗。

（4）Redis采用单线程模型，每条命令执行如果占用大量时间，会造成其他线程阻塞，对于Redis这种高性能服务是致命的，所以Redis是面向高速执行的数据库。

redis的回收策略

Redis的回收策略

 volatile-lru：从已设置过期时间的数据集（server.db[i].expires）中挑选最近最少使用的数据淘汰

volatile-ttl：从已设置过期时间的数据集（server.db[i].expires）中挑选将要过期的数据淘汰

volatile-random：从已设置过期时间的数据集（server.db[i].expires）中任意选择数据淘汰

allkeys-lru：从数据集（server.db[i].dict）中挑选最近最少使用的数据淘汰

allkeys-random：从数据集（server.db[i].dict）中任意选择数据淘汰

no-enviction（驱逐）：禁止驱逐数据

使用Redis有哪些好处？

(1) 速度快，因为数据存在内存中，类似于HashMap，HashMap的优势就是查找和操作的时间复杂度都是O(1)

(2) 支持丰富数据类型，支持string，list，set，sorted set，hash

(3) 支持事务，操作都是原子性，所谓的原子性就是对数据的更改要么全部执行，要么全部不执行

(4) 丰富的特性：可用于缓存，消息，按key设置过期时间，过期后将会自动删除



2. redis相比memcached有哪些优势？

(1) memcached所有的值均是简单的字符串，redis作为其替代者，支持更为丰富的数据类型

(2) redis的速度比memcached快很多

(3) redis可以持久化其数据



3. redis常见性能问题和解决方案：

(1) Master最好不要做任何持久化工作，如RDB内存快照和AOF日志文件

(2) 如果数据比较重要，某个Slave开启AOF备份数据，策略设置为每秒同步一次

(3) 为了主从复制的速度和连接的稳定性，Master和Slave最好在同一个局域网内

(4) 尽量避免在压力很大的主库上增加从库

(5) 主从复制不要用图状结构，用单向链表结构更为稳定，即：Master <- Slave1 <- Slave2 <- Slave3...

这样的结构方便解决单点故障问题，实现Slave对Master的替换。如果Master挂了，可以立刻启用Slave1做Master，其他不变。


15.消息中间件 MQ

消息中间件的主要功能是消息的路由(Routing)和缓存(Buffering)。

AMQP中提供类似功能的两种域模型：Exchange 和 Message queue。

一种分类是推和拉 。

还有一种分类是 Queue 和 Pub/Sub 。

JMS中定义了两种消息模型：点对点（point to point， queue）和发布/订阅（publish/subscribe，topic）。主要区别就是是否能重复消费。

点对点：Queue，不可重复消费
消息生产者生产消息发送到queue中，然后消息消费者从queue中取出并且消费消息。
消息被消费以后，queue中不再有存储，所以消息消费者不可能消费到已经被消费的消息。
Queue支持存在多个消费者，但是对一个消息而言，只会有一个消费者可以消费。
注：Kafka不遵守JMS协议，所以Kafka实际应用中，很可能会需要ack，然后多个消费者能够会同时消费。。需要具体看。

发布/订阅：Topic，可以重复消费
消息生产者（发布）将消息发布到topic中，同时有多个消息消费者（订阅）消费该消息。
和点对点方式不同，发布到topic的消息会被所有订阅者消费。

支持订阅组的发布订阅模式：

发布订阅模式下，当发布者消息量很大时，显然单个订阅者的处理能力是不足的。
实际上现实场景中是多个订阅者节点组成一个订阅组负载均衡消费topic消息即分组订阅，这样订阅者很容易实现消费能力线性扩展。

流行模型比较

RabbitMQ

生产端通过路由规则发送消息到不同queue，消费端根据queue名称消费消息。

RabbitMQ既支持内存队列也支持持久化队列，消费端为推模型，消费状态和订阅关系由服务端负责维护，消息消费完后立即删除，不保留历史消息。

（1）点对点

生产端发送一条消息通过路由投递到Queue，只有一个消费者能消费到。 

（2）多订阅

当RabbitMQ需要支持多订阅时，发布者发送的消息通过路由同时写到多个Queue，不同订阅组消费不同的Queue。
所以支持多订阅时，消息会多个拷贝。



Kafka

Kafka只支持消息持久化，消费端为拉模型，消费状态和订阅关系由客户端端负责维护，消息消费完后不会立即删除，会保留历史消息。

因此支持多订阅时，消息只会存储一份就可以了。但是可能产生重复消费的情况。

（1）点对点&多订阅（因为不删消息，所以这两种就不区分了） 

发布者生产一条消息到topic中，不同订阅组消费此消息。



消费者而言有两种方式从消息中间件获取消息：

①Push方式：由消息中间件主动地将消息推送给消费者；
②Pull方式：由消费者主动向消息中间件拉取消息。

比较：

采用Push方式，可以尽可能快地将消息发送给消费者(stream messages to consumers as fast as possible)

而采用Pull方式，会增加消息的延迟，即消息到达消费者的时间有点长(adds significant latency per message)。
但是，Push方式会有一个坏处：

如果消费者的处理消息的能力很弱(一条消息需要很长的时间处理)，而消息中间件不断地向消费者Push消息，消费者的缓冲区可能会溢出。
ActiveMQ是怎么解决这个问题的呢？那就是  prefetch limit
当推送消息的数量到达了perfetch limit规定的数值时，消费者还没有向消息中间件返回ACK，消息中间件将不再继续向消费者推送消息。

如果消息的数量很少(生产者生产消息的速率不快)，但是每条消息 消费者需要很长的时间处理，那么prefetch limit设置为1比较合适。
这样，消费者每次只会收到一条消息，当它处理完这条消息之后，向消息中间件发送ACK，此时消息中间件再向消费者推送下一条消息。


在javax.jms.Session接口中:
 
AUTO_ACKNOWLEDGE = 1    自动确认
CLIENT_ACKNOWLEDGE = 2    客户端手动确认   
DUPS_OK_ACKNOWLEDGE = 3    自动批量确认
SESSION_TRANSACTED = 0    事务提交并确认

此外AcitveMQ补充了一个自定义的ACK模式:
INDIVIDUAL_ACKNOWLEDGE = 4    单条消息确认

RabbitMQ：基于AMQP协议（Advanced Message Queue Protocol）
ActiveMQ：基于STOMP协议（注：我只知道是基于JMS）

AMQP 是一个开放标准，目前还在 0.9 版本。尚未成熟，但市场上已经出现了很多这个标准的实现产品。

afka和RabbitMQ的比较

1、  RabbitMq比kafka成熟，在可用性上，稳定性上，可靠性上，RabbitMq超过kafka

2、  Kafka设计的初衷就是处理日志的，可以看做是一个日志系统，针对性很强，所以它并没有具备一个成熟MQ应该具备的特性

3、  Kafka的性能（吞吐量、tps）比RabbitMq要强，这篇文章的作者认为，两者在这方面没有可比性。

Topic方式发送的消息与queue方式发送的消息，发送和接收的效率，在一个订阅者和100个订阅者的前提下没有明显差异，但在500个订阅者并发的前提下，topic方式的效率明显低于queue。

Queue方式发送的消息，在一个订阅者、100个订阅者和500个订阅者的前提下，发送和接收的效率没有明显变化。

jms传递消息有两种方式，一种是点对点的Queue，还有一个是发布订阅的Topic方式。区别在于：

对于Queue模式，一个发布者发布消息，下面的接收者按队列顺序接收，比如发布了10个消息，两个接收者A,B那就是A,B总共会收到10条消息，不重复。

对于Topic模式，一个发布者发布消息，有两个接收者A,B来订阅，那么发布了10条消息，A,B各收到10条消息。


RabbitMQ之消息确认机制（事务+Confirm）

RabbitMQ为我们提供了两种方式：

通过AMQP事务机制实现，这也是AMQP协议层面提供的解决方案；

通过将channel设置成confirm模式来实现；

事务机制

RabbitMQ中与事务机制有关的方法有三个：txSelect(), txCommit()以及txRollback(), txSelect用于将当前channel设置成transaction模式，txCommit用于提交事务，txRollback用于回滚事务，在通过txSelect开启事务之后，我们便可以发布消息给broker代理服务器了，如果txCommit提交成功了，则消息一定到达了broker了，如果在txCommit执行之前broker异常崩溃或者由于其他原因抛出异常，这个时候我们便可以捕获异常通过txRollback回滚事务了。


Confirm模式

生产者将信道设置成confirm模式，一旦信道进入confirm模式，所有在该信道上面发布的消息都会被指派一个唯一的ID(从1开始)，一旦消息被投递到所有匹配的队列之后，broker就会发送一个确认给生产者（包含消息的唯一ID）,这就使得生产者知道消息已经正确到达目的队列了，如果消息和队列是可持久化的，那么确认消息会将消息写入磁盘之后发出，broker回传给生产者的确认消息中deliver-tag域包含了确认消息的序列号，此外broker也可以设置basic.ack的multiple域，表示到这个序列号之前的所有消息都已经得到了处理。

对于固定消息体大小和线程数，如果消息持久化，生产者confirm(或者采用事务机制)，消费者ack那么对性能有很大的影响.

消息持久化的优化没有太好方法，用更好的物理存储（SAS, SSD, RAID卡）总会带来改善。生产者confirm这一环节的优化则主要在于客户端程序的优化之上。归纳起来，客户端实现生产者confirm有三种编程方式：

普通confirm模式：每发送一条消息后，调用waitForConfirms()方法，等待服务器端confirm。实际上是一种串行confirm了。
批量confirm模式：每发送一批消息后，调用waitForConfirms()方法，等待服务器端confirm。
异步confirm模式：提供一个回调方法，服务端confirm了一条或者多条消息后Client端会回调这个方法。

从编程实现的复杂度上来看： 

第1种 

普通confirm模式最简单，publish一条消息后，等待服务器端confirm,如果服务端返回false或者超时时间内未返回，客户端进行消息重传。 

第2种 

批量confirm模式稍微复杂一点，客户端程序需要定期（每隔多少秒）或者定量（达到多少条）或者两则结合起来publish消息，然后等待服务器端confirm, 相比普通confirm模式，批量极大提升confirm效率，但是问题在于一旦出现confirm返回false或者超时的情况时，客户端需要将这一批次的消息全部重发，这会带来明显的重复消息数量，并且，当消息经常丢失时，批量confirm性能应该是不升反降的。 

第3种

异步confirm模式的编程实现最复杂，Channel对象提供的ConfirmListener()回调方法只包含deliveryTag（当前Chanel发出的消息序号），我们需要自己为每一个Channel维护一个unconfirm的消息序号集合，每publish一条数据，集合中元素加1，每回调一次handleAck方法，unconfirm集合删掉相应的一条（multiple=false）或多条（multiple=true）记录。从程序运行效率上看，这个unconfirm集合最好采用有序集合SortedSet存储结构。实际上，SDK中的waitForConfirms()方法也是通过SortedSet维护消息序号的。 

消息确认（Consumer端） 消费成功与否 是通过 ACK 机制确认的

为了保证消息从队列可靠地到达消费者，RabbitMQ提供消息确认机制(message acknowledgment)。消费者在声明队列时，可以指定noAck参数，当noAck=false时，RabbitMQ会等待消费者显式发回ack信号后才从内存(和磁盘，如果是持久化消息的话)中移去消息。否则，RabbitMQ会在队列中消息被消费后立即删除它。

采用消息确认机制后，只要令noAck=false，消费者就有足够的时间处理消息(任务)，不用担心处理消息过程中消费者进程挂掉后消息丢失的问题，因为RabbitMQ会一直持有消息直到消费者显式调用basicAck为止。

当noAck=false时，对于RabbitMQ服务器端而言，队列中的消息分成了两部分：一部分是等待投递给消费者的消息；一部分是已经投递给消费者，但是还没有收到消费者ack信号的消息。如果服务器端一直没有收到消费者的ack信号，并且消费此消息的消费者已经断开连接，则服务器端会安排该消息重新进入队列，等待投递给下一个消费者（也可能还是原来的那个消费者）。

RabbitMQ不会为未ack的消息设置超时时间，它判断此消息是否需要重新投递给消费者的唯一依据是消费该消息的消费者连接是否已经断开。这么设计的原因是RabbitMQ允许消费者消费一条消息的时间可以很久很久。

16.TCP三次握手 和 四次握手

三次握手

客户端发送一个带SYN标志的TCP报文（报文1）到服务器端，表示希望建立一个TCP连接。

服务器发送一个带ACK标志和SYN标志的TCP报文（报文2）给客户端，ACK用于对报文1的回应，SYN用于询问客户端是否准备好进行数据传输。

客户端发送一个带ACK标志的TCP报文（报文3），作为报文2的回应。


四次握手

【终止TCP连接】（四次握手）

   由于TCP连接是全双工的，因此每个方向都必须单独进行关闭。原则是主动关闭的一方（如已传输完所有数据等原因）发送一个FIN报文来表示终止这个方向的连接，收到一个FIN意味着这个方向不再有数据流动，但另一个方向仍能继续发送数据，直到另一个方向也发送FIN报文。四次挥手的具体过程如下：
客户端发送一个FIN报文（报文4）给服务器，表示我将关闭客户端到服务器端这个方向的连接。
服务器收到报文4后，发送一个ACK报文（报文5）给客户端，序号为报文4的序号加1。
服务器发送一个FIN报文（报文6）给客户端，表示自己也将关闭服务器端到客户端这个方向的连接。
客户端收到报文6后，发回一个ACK报文（报文7）给服务器，序号为报文6的序号加1。
   至此，一个TCP连接就关闭了。（4次挥手不是关闭TCP连接的唯一办法）
   
   
CLOSE_WAIT：如果我们的服务器一直处于CLOSE_WAIT状态的话，说明套接字是被动关闭的！，并且没有发送Fin信令，原因往往是没有调用TCP的CloseSocket
1、一般原因都是TCP连接没有调用关闭方法。需要应用来处理网络链接关闭。
2、对于Web请求出现这个原因，经常是因为Response的BodyStream没有调用Close.
解决方法：TCP的KeepLive功能，可以让操作系统替我们自动清理掉CLOSE_WAIT的连接。

TIME_WAIT
根据TCP协议定义的3次握手断开连接规定,发起socket主动关闭的一方 socket将进入TIME_WAIT状态。TIME_WAIT状态将持续2个MSL(Max Segment Lifetime),在Windows下默认为4分钟，即240秒。TIME_WAIT状态下的socket不能被回收使用. 具体现象是对于一个处理大量短连接的服务器,如果是由服务器主动关闭客户端的连接，将导致服务器端存在大量的处于TIME_WAIT状态的socket， 甚至比处于Established状态下的socket多的多,严重影响服务器的处理能力，甚至耗尽可用的socket，停止服务。

 为什么需要TIME_WAIT？TIME_WAIT是TCP协议用以保证被重新分配的socket不会受到之前残留的延迟重发报文影响的机制,是必要的逻辑保证。

17.如何在用户程序中新建一个线程了？ 线程池有哪些参数？线程池饱和策略？ 线程的5中状态以及各状态之间的关系？

如何在用户程序中新建一个线程了？

通过继承Thread类，重写run方法；

通过实现runable接口；

通过实现callable接口这三种方式


线程池有哪些参数？
 corePoolSize：表示核心线程池的大小。当提交一个任务时，如果当前核心线程池的线程个数没有达到corePoolSize，则会创建新的线程来执行所提交的任务，即使当前核心线程池有空闲的线程。如果当前核心线程池的线程个数已经达到了corePoolSize，则不再重新创建线程。如果调用了prestartCoreThread()或者 prestartAllCoreThreads()，线程池创建的时候所有的核心线程都会被创建并且启动。

maximumPoolSize：表示线程池能创建线程的最大个数。如果当阻塞队列已满时，并且当前线程池线程个数没有超过maximumPoolSize的话，就会创建新的线程来执行任务。

keepAliveTime：空闲线程存活时间。如果当前线程池的线程个数已经超过了corePoolSize，并且线程空闲时间超过了keepAliveTime的话，就会将这些空闲线程销毁，这样可以尽可能降低系统资源消耗。

unit：时间单位。为keepAliveTime指定时间单位。

workQueue：阻塞队列。用于保存任务的阻塞队列，关于阻塞队列可以看这篇文章。可以使用ArrayBlockingQueue, LinkedBlockingQueue, SynchronousQueue, PriorityBlockingQueue。

threadFactory：创建线程的工程类。可以通过指定线程工厂为每个创建出来的线程设置更有意义的名字，如果出现并发问题，也方便查找问题原因。
handler：饱和策略。当线程池的阻塞队列已满和指定的线程都已经开启，说明当前线程池已经处于饱和状态了，那么就需要采用一种策略来处理这种情况。采用的策略有这几种：

AbortPolicy： 直接拒绝所提交的任务，并抛出RejectedExecutionException异常；

CallerRunsPolicy：只用调用者所在的线程来执行任务；

DiscardPolicy：不处理直接丢弃掉任务；

DiscardOldestPolicy：丢弃掉阻塞队列中存放时间最久的任务，执行当前任务


线程的5中状态以及各状态之间的关系？
https://juejin.im/post/5ae6cf7a518825670960fcc2

 线程创建之后调用start()方法开始运行，当调用wait(),join(),LockSupport.lock()方法线程会进入到WAITING状态，而同样的wait(long timeout)，sleep(long),join(long),LockSupport.parkNanos(),LockSupport.parkUtil()增加了超时等待的功能，也就是调用这些方法后线程会进入TIMED_WAITING状态，当超时等待时间到达后，线程会切换到Runable的状态，另外当WAITING和TIMED _WAITING状态时可以通过Object.notify(),Object.notifyAll()方法使线程转换到Runable状态。当线程出现资源竞争时，即等待获取锁的时候，线程会进入到BLOCKED阻塞状态，当线程获取锁时，线程进入到Runable状态。线程运行结束后，线程进入到TERMINATED状态，状态转换可以说是线程的生命周期。


sleep() VS wait()

两者主要的区别：

sleep()方法是Thread的静态方法，而wait是Object实例方法
wait()方法必须要在同步方法或者同步块中调用，也就是必须已经获得对象锁。而sleep()方法没有这个限制可以在任何地方种使用。另外，wait()方法会释放占有的对象锁，使得该线程进入等待池中，等待下一次获取资源。而sleep()方法只是会让出CPU并不会释放掉对象锁；
sleep()方法在休眠时间达到后如果再次获得CPU时间片就会继续执行，而wait()方法必须等待Object.notift/Object.notifyAll通知后，才会离开等待池，并且再次获得CPU时间片才会继续执行。

sleep()和yield()方法，同样都是当前线程会交出处理器资源，而它们不同的是，sleep()交出来的时间片其他线程都可以去竞争，也就是说都有机会获得当前线程让出的时间片。而yield()方法只允许与当前线程具有相同优先级的线程能够获得释放出来的CPU时间片。

守护线程在退出的时候并不会执行finnaly块中的代码，所以将释放资源等操作不要放在finnaly块中执行，这种操作是不安全的
线程可以通过setDaemon(true)的方法将线程设置为守护线程。并且需要注意的是设置守护线程要先于start()方法，否则会报

Java并发知识图谱
https://github.com/CL0610/Java-concurrency/blob/master/Java%E5%B9%B6%E5%8F%91%E7%9F%A5%E8%AF%86%E5%9B%BE%E8%B0%B1.png

![JAVA并发知识图谱.png](https://github.com/CL0610/Java-concurrency/blob/master/Java并发知识图谱.png)

18.设计模式

设计模式的六大原则

开闭原则：对扩展开放,对修改关闭，多使用抽象类和接口。

里氏替换原则：基类可以被子类替换，使用抽象类继承,不使用具体类继承。

依赖倒转原则：要依赖于抽象,不要依赖于具体，针对接口编程,不针对实现编程。

接口隔离原则：使用多个隔离的接口,比使用单个接口好，建立最小的接口。

迪米特法则：一个软件实体应当尽可能少地与其他实体发生相互作用，通过中间类建立联系。

合成复用原则：尽量使用合成/聚合,而不是使用继承。


单例模式有三种：懒汉式单例，饿汉式单例，登记式单例。

![登记式单例](https://img-blog.csdn.net/2018030811254564?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvWUVDcmF6eQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


17. 分布式式任务系统设计理念：clover tbschedule等设计原理
海量分布式定时任务系统 ，触发方式。  类似游标限流方式，将每秒要执行的任务记录到  对应的秒的时间窗口里面，按照时间的推进 分别取出对应的时间窗口里面的任务进行激发任务的执行

18. zookeeper   
zookeeper学习 https://blog.csdn.net/xzqxiaoqing/article/details/80792860

Zookeeper 的核心是原子广播，这个机制保证了各个Server之间的同步。实现这个机制的协议叫做Zab协议。Zab协议有两种模式，它们分别是恢复模式（选主）和广播模式（同步）。当服务启动或者在领导者崩溃后，Zab就进入了恢复模式，当领导者被选举出来，且大多数Server完成了和 leader的状态同步以后，恢复模式就结束了。状态同步保证了leader和Server具有相同的系统状态。

协议zab协议，原子广播协议， 选举机制是统统paxos协议 Zk的选举算法有两种：一种是基于basic paxos实现的，另外一种是基于fast paxos算法实现的。系统默认的选举算法为fast paxos。

Watch是轻量级的，其实就是本地JVM的Callback，服务器端只是存了是否有设置了Watcher的布尔类型

每个Server在工作过程中有三种状态： 
LOOKING：当前Server不知道leader是谁，正在搜寻
LEADING：当前Server即为选举出来的leader
FOLLOWING：leader已经选举出来，当前Server与之同步

zookeeper是如何保证事务的顺序一致性的？
zookeeper采用了递增的事务Id来标识，所有的proposal（提议）都在被提出的时候加上了zxid，zxid实际上是一个64位的数字，高32位是epoch（时期; 纪元; 世; 新时代）用来标识leader是否发生改变，如果有新的leader产生出来，epoch会自增，低32位用来递增计数。当新产生proposal的时候，会依据数据库的两阶段过程，首先会向其他的server发出事务执行请求，如果超过半数的机器都能执行并且能够成功，那么就会开始执行。

Zookeeper数据复制
1、写主(WriteMaster) ：对数据的修改提交给指定的节点。读无此限制，可以读取任何一个节点。这种情况下客户端需要对读与写进行区别，俗称读写分离； 
2、写任意(Write Any)：对数据的修改可提交给任意的节点，跟读一样。这种情况下，客户端对集群节点的角色与变化透明。

对zookeeper来说，它采用的方式是写任意。通过增加机器，它的读吞吐能力和响应能力扩展性非常好，而写，随着机器的增多吞吐能力肯定下降（这也是它建立observer的原因），而响应能力则取决于具体实现方式，是延迟复制保持最终一致性，还是立即复制快速响应。

四种类型的znode
1、PERSISTENT-持久化目录节点 
客户端与zookeeper断开连接后，该节点依旧存在 
2、PERSISTENT_SEQUENTIAL-持久化顺序编号目录节点
客户端与zookeeper断开连接后，该节点依旧存在，只是Zookeeper给该节点名称进行顺序编号 
3、EPHEMERAL-临时目录节点
客户端与zookeeper断开连接后，该节点被删除 
4、EPHEMERAL_SEQUENTIAL-临时顺序编号目录节点
客户端与zookeeper断开连接后，该节点被删除，只是Zookeeper给该节点名称进行顺序编号

Zookeeper做了什么？
1、命名服务
2、配置管理
3、集群管理
4、分布式锁
5、队列管理

