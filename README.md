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


