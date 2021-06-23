##NoSql

NotOnlySql

常用的SQL数据库是关系型数据库

Nosql种类：

![](../pics/c1.png)

单一数据库最大的好处在于事务性实现简单，由数据库自己保证。

举个简单的例子，下订单需要扣除一个库存，然后插入一条订单条目，如果库存和订单都是数据库表项的话这个事务是无懈可击的，如果库存在redis里，订单条目是MySQL，通常就需要先写redis，成功之后再写数据库，如果写数据库失败了还需要回滚redis，如果最后这个回滚因为网络之类的原因失败了，就会多扣一个库存。不要以为这些事情很好解决，事务性处理的复杂性远远超过你的想象，比如说还有写MySQL在提交的一瞬间连接断了这种情况，你都没法判断提交到底成功了还是失败了，那你的redis是回滚还是不回滚？


##CAP

分布式系统的最大难点，就是各个节点的状态如何保持一致。

CAP理论是在设计分布式系统的过程中，处理数据一致性问题时必须考虑的理论。

CAP即：Consistence  	Availability  partition tolerance

一致性  可用性  分区容错性
cap
![](../pics/c2.png)

一个分布式系统，不可能同时做到这三点

一致性：更新操作成功并返回客户端后，所有节点在同一时间的数据完全一致，这就是分布式的一致性。一致性的问题在并发系统中不可避免，对于客户端来说，一致性指的是并发访问时更新过的数据如何获取的问题。从服务端来看，则是更新如何复制分布到整个系统，以保证数据最终一致。

可用性：服务一直可用，而且是正常响应时间。好的可用性主要是指系统能够很好的为用户服务，不出现用户操作失败或者访问超时等用户体验不好的情况。

分区容忍性：分布式系统在遇到某节点或网络分区故障的时候，仍然能够对外提供满足一致性或可用性的服务。

分区容错性要求能够使应用虽然是一个分布式系统，而看上去却好像是在一个可以运转正常的整体。比如现在的分布式系统中有某一个或者几个机器宕掉了，其他剩下的机器还能够正常运转满足系统需求，对于用户而言并没有什么体验上的影响。


对于一个分布式系统而言，P是前提，必须保证，因为只要有网络交互就一定会有延迟和数据丢失，这种状况我们必须接受，必须保证系统不能挂掉。


对于网络情况良好的分布式系统，CAP在大多时间能够同时满足C和A

![](../pics/c3.png)

假设一种极端情况，N1和N2之间的网络断开了，但我们仍要支持这种网络异常，也就是满足分区容错性，那么这样能不能同时满足一致性和可用性呢？

![](../pics/c4.png)

有二种选择

第一，牺牲数据一致性，响应旧的数据DB0给用户；

第二，牺牲可用性，阻塞等待，直到网络连接恢复，数据更新操作完成之后，再给用户响应最新的数据DB1。


##BASE

BASE是对CAP中 AP理论的延申，一致性和可用性权衡的结果，其来源于对大规模互联网分布式系统实践的总结，是基于CAP定律逐步演化而来。

核心思想是即使无法做到强一致性，但每个应用都可以根据自身业务特点，才用适当的方式来使系统打到最终一致性。

	也就是牺牲数据的一致性来满足系统的高可用性，系统中一部分数据不可用或者不一致时，仍需要保持系统整体“主要可用”。但保留最终一致性。


CAP在大多时间能够同时满足C和A。

AP方案只是在系统发生分区的时候放弃一致性，而不是永远放弃一致性。

在分区故障恢复后，系统应该达到最终一致性。这一点其实就是 BASE 理论延伸的地方。

BASE理论三要素：

1. basically available 基本可用


2. soft-state 软状态


3. Eventually consistent 最终一致性


1：基本可用

基本可用是指分布式系统在出现不可预知故障的时候，允许损失部分可用性。但是，这绝不等价于系统不可用。

允许损失部分可用性：

1. 响应时间上的损失: 正常情况下，处理用户请求需要 0.5s 返回结果，但是由于系统出现故障，处理用户请求的时间变为 3 s。
2. 系统功能上的损失：正常情况下，用户可以使用系统的全部功能，但是由于系统访问量突然剧增，系统的部分非核心功能无法使用。


2：软状态
软状态指允许系统中的数据存在中间状态（CAP 理论中的数据不一致），并认为该中间状态的存在不会影响系统的整体可用性，即允许系统在不同节点的数据副本之间进行数据同步的过程存在延时。

3：最终一致性

最终一致性强调的是系统中所有的数据副本，在经过一段时间的同步后，最终能够达到一个一致的状态。因此，最终一致性的本质是需要系统保证最终数据能够达到一致，而不需要实时保证系统数据的强一致性。


一致性三种级别：


1. 强一致性 ：系统写入了什么，读出来的就是什么。

2. 弱一致性 ：不一定可以读取到最新写入的值，也不保证多少时间之后读取到的数据是最新的，只是会尽量保证某个时刻达到数据一致的状态。

3. 最终一致性 ：弱一致性的升级版。系统会保证在一定时间内达到数据一致的状态，


##Redis简介

**背景**

Redis是C语言开发的一个开源的（遵从BSD协议）高性能键值对（key-value）的内存数据库，可以用作**数据库**、**缓存**、**消息中间件等**。它是一种NoSQL（not-only sql，泛指非关系型数据库）的数据库。

Redis作为一个内存数据库。

1、性能优秀，数据在内存中，读写速度非常快，支持并发10W QPS；

2、单进程单线程，是线程安全的，采用IO多路复用机制；

3、丰富的数据类型，支持字符串（strings）、散列（hashes）、列表（lists）、集合（sets）、有序集合（sorted sets）等；

4、支持数据持久化。可以将内存中数据保存在磁盘中，重启时加载；

5、主从复制，哨兵，高可用；

6、可以用作分布式锁；

7、可以作为消息中间件使用，支持发布订阅


redis默认有16个数据库，可以使用select进行切换。

基本操作 

select 3 / flush all / keys * / set name wubo / get name / flushdb / FLUSHALL / EXISTS key / move name 1

EXPIRE name seconds / ttl name / type name / 




**Redis为什么快**

多线程的目的，就是通过并发的方式来提升I/O的利用率和CPU的利用率。

而Redis的操作基本都是基于内存的，CPU资源根本就不是Redis的性能瓶颈。

Redis的瓶颈是机器的内存和网络带宽。

redis是单线程的，省去了很多上下文切换线程的时间；

在进行一些复杂结构的细粒度的操作，由于是单线程，所以不需要考虑锁的问题。不存在加锁释放锁死锁等操作导致的性能消耗。

redis使用多路复用技术，可以处理并发的连接。非阻塞IO 内部实现采用epoll，采用了epoll+自己实现的简单的事件框架。epoll中的读、写、关闭、连接都转化成了事件，然后利用epoll的多路复用特性，绝不在io上浪费一点时间。

Redis使用的是非阻塞IO，IO多路复用，使用了单线程来轮询描述符，将数据库的开、关、读、写都转换成了事件，减少了线程切换时上下文的切换和竞争。

数据结构也帮了不少忙，Redis全程使用hash结构，读取速度快，还有一些特殊的数据结构，对数据存储进行了优化，如压缩表，对短数据进行压缩存储，再如，跳表，使用有序的数据结构加快读取的速度。

单线程 ： Redis网络IO和键值对读写是由一个线程完成的，而其他的如持久化存储模块、集群支撑模块等是多线程的。



### IO多路复用 ###

多进程并发模型：每进来一个新的IO流会分配一个新的进程管理。

IO多路复用：单个线程，通过记录每个IO的状态，同时管理多个IO流。

![](../pics/IO1.png)

I/O multiplexing 这里面的 multiplexing 指的其实是在单个线程通过记录跟踪每一个Sock(I/O流)的状态(对应空管塔里面的Fight progress strip槽)来同时管理多个I/O流. 发明它的原因，是尽量多的提高服务器的吞吐能力。

单线程或单进程同时监测若干个文件描述符是否可以执行IO操作的能力。

应用程序通常需要处理来自多条事件流中的事件，比如我现在用的电脑，需要同时处理键盘鼠标的输入、中断信号等等事件，再比如web服务器如nginx，需要同时处理来来自N个客户端的事件。

CPU单核在同一时刻只能做一件事情，一种解决办法是对CPU进行时分复用(多个事件流将CPU切割成多个时间片，不同事件流的时间片交替进行)。。。

在计算机系统中，我们用线程或者进程来表示一条执行流，通过不同的线程或进程在操作系统内部的调度，来做到对CPU处理的时分复用。这样多个事件流就可以并发进行，不需要一个等待另一个太久，在用户看起来他们似乎就是并行在做一样。

但凡事都是有成本的。线程/进程也一样，有这么几个方面：

1：线程/进程创建成本

2：CPU切换不同线程/进程的成本

3：多线程的资源竞争

在单线程中处理多个事件流的方法 ---- IO多路复用。

Linux下的IO模型 ： 阻塞IO / 非阻塞IO / IO多路复用 / 信号驱动IO / 异步IO

阻塞IO：阻塞IO意味着当我们发起一次IO操作后一直等待成功或失败之后才返回，在这期间程序不能做其它的事情。阻塞IO操作只能对单个文件描述符进行操作，详见read或write。

非阻塞IO：我们在发起IO时，通过对文件描述符设置O_NONBLOCK flag来指定该文件描述符的IO操作为非阻塞。非阻塞IO通常发生在一个for循环当中，因为每次进行IO操作时要么IO操作成功，要么当IO操作会阻塞时返回错误EWOULDBLOCK/EAGAIN，然后再根据需要进行下一次的for循环操作，这种类似轮询的方式会浪费很多不必要的CPU资源，是一种糟糕的设计。和阻塞IO一样，非阻塞IO也是通过调用read或write来进行操作的，也只能对单个描述符进行操作。

IO多路复用：单线程或单进程同时监测若干个文件描述符是否可以执行IO操作的能力。

信号驱动IO：信号驱动IO是利用信号机制，让内核告知应用程序文件描述符的相关事件。

异步IO：相比信号驱动IO需要在程序中完成数据从用户态到内核态(或反方向)的拷贝，异步IO可以把拷贝这一步也帮我们完成之后才通知应用程序。我们使用 aio_read 来读，aio_write 写。

同步IO指的是程序会一直阻塞到IO操作如read、write完成 2. 异步IO指的是IO操作不会阻塞当前程序的继续执行

![](../pics/epoll.png)





##五大基本数据类型

![](../pics/c5.png)


###String
String 是Redis的最基本的数据类型，可以理解为与 Memcached 一模一样的类型，一个key 对应一个 value。string 类型是二进制安全的，意思是 Redis 的 string 可以包含任何数据，比如图片或者序列化的对象，一个 redis 中字符串 value 最多可以是 512M。

APPEND key1 hello (key1不存在相当于set)/ STRLEN key1 / 

set view 0 / incr view (+1) / decr view (-1) / INCRBY view 10 (+10) / DECRBY view 10 (-10)

GETRANGE key 1 3 / GETRANGE key 0 -1 (查看全部) / SETRANGE key 1 xx / 

setex key second value (set with expire 设置过期时间) / setnx key "value"    (set if not exit 不存在再设置)

mset k1 v1 k2 v2 k3 v3 / mget k1 k2 k3 / msetnx k1 v1 k2 v2 (要么一起成功，要么一起失败)

set user:1 {name:wubo,age:25} （设置一个对象） / mset user:1:name wu user:1:age 18 / mget user:1:name user:1:age

user:id {filed}

getset name wu  (如果不存在返回null . 如果存在 先返回旧值 再设置新值) 

redis:计数器  对象缓存 

###List

List:列表  相当于一个双向链表 左右插入 前后插入

所有list操作都是l开头

LPUSH list one  放在左头部 / LRANGE list 0 1 查看范围  / RPUSH list one 放在右尾部 / LPOP list 

lindex list 0 查看索引出值 / Llen list长度 / lrem list 1 one 移除1个指定值 / ltrim list 1 2 截断

rpoplpush list otherlist 移动 / EXITS list / lset list 1 other 替换当前下标值 /linsert list after one two 插入值

###Set

Set: 值不重复

sadd set "hello" / smembers set / sismember set wubo / scard set 获取set集合个数 / srem set wubo 移除 / 

srandmember set 1 随机抽取一个元素 / spop set / smove set1 set2 "wubo" 移动指定元素到另一个集合 / sdiff set1 set2 

sinter set1 set2 交集 / sunion set1 set2 并集  

###Hash

Hash: key-Map集合

hset hash key value / hget hash key / hmset hash k1 v1 k2 v2 / hmget hash k1 k2 / hgetall hash / hdel set k1

hlen set / hexits hash key1 / hkeys hash / hvals hash / hincrby hash key1 1 给value加1 / hsetnx set key1 value1 /

hash更适合对象的存储、String更适合字符串

###Zset

Zset:在set的基础上，增加了一个值。 score  有序集合。

zset set score v1  / zrange set 0 -1 / zrangebyscore set 0 1 按score排序 / zcard set / zrem set 1

zrevrange key 0 -1 降序


##三种特殊数据类型
###geo
###hyperloglog
###bitmap



##事务

Redis单条命令是保证原子性的，但事务是不保证原子性的。


![](../pics/r1.png)

Redis事务是通过MULTI、EXEC、DISCARD、WATCH这四个命令完成的。

Redis事务：一组命令的集合，将多个命令打包，这些命令会被顺序的添加到队列中，并按顺序执行这些命令。

不能回滚。

1：MULTI 用于标记事物块的开始，Redis会将后续的命令逐个的放入队列中，然后使用EXEC命令原子化执行这个命令序列。

2：EXEC 在一个事物中执行所有先前放入队列的命令，然后恢复正常的连接状态。

3：DISCARD 清除先前在一个事务中放入队列的命令，然后恢复正常的连接状态。

4：WATCH：当某个事务需要按条件执行时，就要使用这个命令给指定的键设置为受监控的状态。

当被监控的数据发生改变后，开启的事务执行是无法成功的，只有被监控的数据不发生变化，事务才能正常执行。

UNWATCH:清除先前为一个事务监控的键

事务失败处理：

1：Redis语法错误（编译期错误）：在一个事务中，当命令出现错误时，后续命令正确依旧是可以添加到命令队列中去得，但是使用 EXEC 命令执行命令队列的时候，就会报错。----事务中所有的命令不会被执行

2：Redis类型错误（运行期异常） ： 事务队列中存在语法性错误，则执行命令时，其他命令是可以被执行的，错误命令就抛出异常。

---事务不保证原子性。  不支持事务回滚---- 在开发阶段错误可被预见  性能要求。

**乐观锁、悲观锁**

悲观锁：认为出现并发问题的可能性较大，所以在操作前加锁。

乐观锁：认为出现并发问题的可能性比较小，比较乐观。这时不需要加锁，只需要在执行修改操作的时候，比较一下原来的数据是否发生变化，如果没有变化就修改，有变化就不修改。

redis提供了watch命令，可以监控修改数据时，数据是否被其他线程修改过，如果修改过，则本次修改失败，如果没有修改过，则修改成功。其实watch命令就可以看做是redis的乐观锁的实现。

![](../pics/r2.png)

此时另一终端修改acc1的值，则在exec后会导致此事务失败。 并没有阻塞其他线程修改，redis事务无隔离性。


##Springboot集成redis


1:pom.xml导入依赖

	<!--redis依赖包-->
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-data-redis</artifactId>
      </dependency>

2：在application.yml文件中配置redis连接信息

	#redis配置
	#Redis服务器地址
	spring.redis.host=127.0.0.1
	#Redis服务器连接端口
	spring.redis.port=6379
	#Redis数据库索引（默认为0）
	spring.redis.database=0 

3：注入RedisTemplate操作

	@SpringBootTest
	class SpringbootdemoApplicationTests {

    @Autowired
    StringRedisTemplate stringRedisTemplate; //操作key-value都是字符串，最常用

    @Test
    public void test01(){
        //字符串操作
        stringRedisTemplate.opsForValue().append("msg","coder");

        //列表操作
        stringRedisTemplate.opsForList().leftPush("mylist","1");
        stringRedisTemplate.opsForList().leftPush("mylist","2");
    }
	}

4：可以自定义序列化类，将存在Redis的对象序列化为JSON格式，防止乱码

	@Configuration
	@EnableAutoConfiguration
	public class MyRedisConfig {
    @Bean
    public RedisTemplate<Object, User> redisTemplate(RedisConnectionFactory redisConnectionFactory){
        RedisTemplate<Object, User> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory);

        Jackson2JsonRedisSerializer<User> ser = new Jackson2JsonRedisSerializer<>(User.class);
        template.setDefaultSerializer(ser);
        return template;
    }
	}

5：可以写RedisUtil工具类相关方法



##Redis持久化

Redis是内存数据库，一旦Redis服务器进程退出或者运行Redis服务器的计算机停机，Redis服务器中的数据就会丢失。

所以为了防止数据丢失，需要持久化。

两种持久化方式： RDB持久化 和 AOF持久化

RDB持久化： 将某个时间点上Redis中的数据保存到一个RDB文件中，所以RDB持久化也叫做快照持久化。

Redis提供2个命令来创建RDB文件，一个是SAVE，一个是BGSAVE

SAVE命令会阻塞Redis服务器进程，直到RDB文件创建完毕为止，在服务器进程阻塞期间，服务器不能处理任何命令请求

BGSAVE命令会派生出一个子进程，然后由子进程负责创建RDB文件，服务器进程（父进程）继续处理命令请求


bgsave会异步的执行备份，其实是fork出了一个子进程，用子进程去执行快照持久化操作，将数据保存在一个.rdb文件中。

子进程刚刚产生的时候，是和父进程共享内存中的数据的，但是子进程做持久化时，是不会修改数据的，而父进程是要持续提供服务的，所以父进程就会持续的修改内存中的数据，这个时候父进程就会将内存中的数据，Copy出一份来进行修改。

父进程copy的数据是以数据页为单位的（4k一页），对那一页数据进行修改就copy哪一页的数据。

我们可以手动执行该命令，但还是推荐设置下Redis服务器配置文件的save选项，让服务器每隔一段时间自动执行一次BGSAVE命令。

默认的配置条件表示，只要满足以下3个条件中的任意1个，BGSAVE命令就会被执行：

save 900 1 : 服务器在900s（即15分钟）之内，对数据库进行了至少1次修改

save 300 10 : 服务器在300s（即5分钟）之内，对数据库进行了至少10次修改

save 60 10000 : 服务器在60s（即1分钟）之内，对数据库进行了至少10000次修改

当满足条件执行BGSAVE命令时，生成的RDB文件会根据Redis配置文件中的名称和路径来保存

载入RDB文件：载入RDB文件的目的是为了在Redis服务器进程重新启动之后还原之前存储在Redis中的数据。

Redis载入RDB文件并没有专门的命令，而是在Redis服务器启动时自动执行的

Redis服务器启动时是否会载入RDB文件还取决于服务器是否启用了AOF持久化功能

![](../pics/r3.png)

默认情况下，Redis服务器的AOF持久化功能是关闭的，所以Redis服务器在启动时会载入RDB文件。

RDF快照是一个全量备份，而AOF是增量备份。

快照是内存数据的二进制序列化形式，存储上很紧凑。AOF是增量备份，记录的是内存数据修改的指令记录文本。

执行命令后Redis服务器生成了1个名为appendonly.aof的文件，打开该文件，执行的写命令都存储在该文件中：

当AOF持久化功能处于打开状态时，Redis服务器在执行完一个写命令之后，会以协议格式（如上面截图中AOF文件里保存写命令的格式）将被执行的写命令追加到服务器状态的AOF缓冲区的末尾，然后Redis服务器会根据配置文件中appendfsync选项的值来决定何时将AOF缓冲区中的内容写入和同步到AOF文件里面。

appendfsync选项有以下3个值：

always:安全但是效率慢。每次执行完一个命令后，会写入AOF缓冲区，并同步到AOF文件中

everysec: 只会丢失1s的写入命令数据。 每1s将AOF缓冲区命令同步到AOF文件中

no模式下，如果出现故障停机，数据库会丢失上次同步AOF文件之后的所有写命令数据，具有不确定性，因为服务器在每个事件循环都要将AOF缓冲区中的所有内容写入到AOF文件，至于何时对AOF文件进行同步，则由操作系统控制。从效率来说，no模式和everysec模式的效率差不多。

appendfsync默认为everysec

载入AOF文件：

AOF文件包含了重建数据库所需的所有写命令，所以Redis服务器只要读入并重新执行一遍AOF文件里面保存的写命令，就可以还原Redis服务器关闭之前的数据。

![](../pics/r4.png)

如果Redis服务器开启了AOF持久化功能，那么Redis服务器在启动时会载入AOF文件.

AOF重写：首先从数据库中读取键现在的值，然后用一条命令去记录键值对，代替之前记录这个键值对的多条命令。

AOF持久化是通过保存被执行的写命令来记录数据库数据的，所以随着Redis服务器运行时间的增加，AOF文件中的内容会越来越多，文件的体积会越来越大，如果不做控制，会有以下2点坏处：

过多的占用服务器磁盘空间，可能会对Redis服务器甚至整个宿主计算机造成影响。
AOF文件的体积越大，使用AOF文件来进行数据库还原所需的时间就越多。

为了解决AO文件体积越来越大的问题，Redis提供了AOF文件重写功能，即Redis服务器会创建一个新的AOF文件来替代现有的AOF文件，新旧两个AOF文件所保存的数据库数据相同，但新AOF文件不会包含任何浪费空间的冗余命令，所以新AOF文件的体积通常会比旧AOF文件的体积要小很多。

Redis提供BGREWRITEAOF指令，来进行AOF后台重写。

**RDB AOF的区别**

1：实现方式

RDB持久化是通过将某个时间点Redis服务器存储的数据保存到RDB文件中来实现持久化的。

AOF持久化是通过将Redis服务器执行的所有写命令保存到AOF文件中来实现持久化的。

2：文件体积

RDB持久化记录的是结果，AOF持久化记录的是过程，所以AOF持久化生成的AOF文件会有体积越来越大的问题，Redis提供了AOF重写功能来减小AOF文件体积。

3：安全性

AOF持久化的安全性要比RDB持久化的安全性高，即如果发生机器故障，AOF持久化要比RDB持久化丢失的数据要少。

因为RDB持久化会丢失上次RDB持久化后写入的数据，而AOF持久化最多丢失1s之内写入的数据（使用默认everysec配置的话）。

4：优先级

如果Redis服务器开启了AOF持久化功能，Redis服务器在启动时会使用AOF文件来还原数据，如果Redis服务器没有开启AOF持久化功能，Redis服务器在启动时会使用RDB文件来还原数据，所以AOF文件的优先级比RDB文件的优先级高。

##订阅发布

发布者/订阅者模式：

![](../pics/r5.png)

如支付成功后，需要调用库存-1，积分+1，用户购买记录+1...等接口。

后续扩充业务--需要修改支付接口代码--重度耦合，导致支付慢、接口错误等情况。

调用 B 接口的发生异常。A，B，C 三个下游接口，A 获取成功获取支付结果，但是 B，C 没有拿到，导致三者系统数据不一致的情况。

![](../pics/r6.png)

在发布者、订阅者模式下，消息发布者和订阅者不用直接进行通信。

消息发布者只需要想指定的频道发布消息，订阅该频道的每个客户端都可以接受到到这个消息。 - 解耦

Redis 中提供了一组命令，可以用于发布消息，订阅频道，取消订阅以及按照模式订阅。


发布信息：publish channel message

订阅频道：subscribe channel [channel...]

客户端在执行subscribe命令后会进入订阅状态，只能接受：subscribe\psubscribe\unsubscribe\punsubscribe这四个命令

psubscribe pay.* 订阅以pay开头的频道   


实际应用：

**Redis sentinel节点发现：**

Redis Sentinel」 是 Redis 一套高可用方案，可以在主节点故障的时候，自动将从节点提升为主节点，从而转移故障。

Redis Sentinel 节点主要使用发布订阅机制，实现新节点的发现，以及交换主节点的之间的状态。

![](../pics/r7.png)

每一个sentinel节点会定时向_sentinel_:hello频道发送消息，并且每个sentinel节点都会订阅此频道。

这样，一旦右新的节点加入，它往这个频道发送消息，其他节点收到之后，判断本地列表并没有这个节点，于是就可以当做新的节点加入本地节点列表。

除此之外，每次往这个频道发送消息内容可以包含节点的状态信息，这样可以作为后面 「Sentinel」 领导者选举的依据。

对于我们客户端来讲，比较关心切换之后的主节点，这样我们及时切换主节点的连接

客户端可以订阅 +switch-master频道，一旦 「Redis Sentinel」 结束了对主节点的故障转移就会发布主节点的的消息。


##Redis锁




##主从复制


##哨兵模式
##缓存穿透
##缓存击穿
##缓存雪崩
