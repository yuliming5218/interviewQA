# interviewQA

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

