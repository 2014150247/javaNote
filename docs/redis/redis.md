
# 支持的数据结构：
string（最大512M），list，set,zset,hash

# redis的同步机制：
master-slave 主从数据备份。或者从从同步。master做一次bgsave，并将记录到内存buffer，然后将rdb文件同步到salve节点，salve节点接受完将rdb镜像加载到内存，加载完，通知master将修改记录同步到slave进行重放。

# redis的好：
读写块
支持的数据类型丰富
原子，支持事务
支持publish/subscribe,通知，key过期等


# redis运行：
运行在内存但是可以持久化到磁盘。所以数据量不能大于内存。

# redis和memcached：
不知道是啥，内存缓存？相比redis的数据类型更多，速度快，可以持久化
memcached全存在内存里，断电就挂。

# redis怎么持久化：
RDB+AOF
## redis database：
指定时间间隔内所有键值对的快照，二进制文件dump.rdb。也可以执行save或bgsave（异步）来手动生成rdb。例如save 900 1，就是900s内有1个key被修改，就生成快照保存。
fork子进程来完成写操作，替换原来的rdb文件，好处是copy-on-write。主进程不会进行任何IO,实现IO最大化，保证了性能。
数据量较大时，比AOF启动效率高。
但是隔一段时间才进行持久化，数据安全性低，可能会发生数据丢失。
## append-only file：
数据安全，可以通过配置appendfsync为always，每一次记录都会到aof文件。
就算中间宕机，也可通过redis-check-aof工具解决数据一致性问题。
但是文件更大，恢复更慢。
数据量大，启动效率就低。


>redis 是单进程单线程的。利用队列将并发访问变为串行访问。

# redis的性能：
master不要写内存快照，
如果数据很重要时，用AOF备份。
为了主从复制快点，链接稳定一点，master和slave最好在统一局域网。
避免在压力很大的主库上增加从
主从复制不要用图，用单向链表更加稳定。这样子master挂了，可以快速用slave1代替。

## 主从复制




# redis的删除策略：
## 定时删除
就是设置key的过期时间，过期删除
## 惰性删除
随便key过期，取值得时候再删除
## 定期删除
一段时间就去检查有没有可以删的。


# redis的回收策略：
## volatile-lru：
有设置过期时间的数据集中，最近最少使用
## volatile-ttl：
有设置过期时间的数据集中，即将过期的数据
## volatile-random：
有设置过期时间的数据集中，随便选点数据
## allkeys-lru：
所有数据集，最近最少使用
## allkeys-random：
所有数据集，任意数据
## no-enviction
禁止驱逐数据
>如果数据一部分访问效率高，一部分低，就是幂律分布，使用allkeys-lru
>如果数据所有访问频率都差不多，平等分布，就用allkeys-random

# redis内存优化：
尽量用散列表（hashes），比如说存储一个用户对象，不要为单独属性设置单独的key，而是把所有信息存在一个散列表。



redis适用场景：
会话缓存，比如购物车信息
全页缓存，
队列
排行榜、计数器
发布、订阅  缺点：当消费者下线，生产的消息会丢失，要用更专业的消息队列如rabbitMQ



redis分布式锁：
setnx来争锁，抢到后要加expire过期时间防止忘了释放。

