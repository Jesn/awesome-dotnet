<!-- 
* redis是单线程还是多线程，如何使redis和数据库数据同步
  4.0之前单线程，之后多线程；先删除缓存，后更新数据库（数据库同步redis） -->

&emsp;&emsp; Redis，英文全称是Remote Dictionary Server（远程字典服务），是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。

&emsp;&emsp; 与MySQL数据库不同的是，Redis的数据是存在内存中的。它的读写速度非常快，每秒可以处理超过10万次读写操作。因此redis被广泛应用于缓存，另外，Redis也经常用来做分布式锁。除此之外，Redis支持事务、持久化、LUA 脚本、LRU 驱动事件、多种集群方案。


## Redis 五种基本数据结构类型

* String（字符串）
* Hash（哈希）
* List（列表）
* Set（集合）
* zset（有序集合）

## 三种特殊数据结构类型
* Geospatial
* Hyperloglog
* Bitmap


## 数据类型使用场景

### String（字符串）
* 简介:String是Redis最基础的数据结构类型，它是二进制安全的，可以存储图片或者序列化的对   象，值最大存储为512M
* 简单使用举例: set key value、get key等
* 应用场景：共享session、分布式锁，计数器、限流。
* 内部编码有3种，int（8字节长整型）/embstr（小于等于39字节字符串）/raw（大于39个字节字符串）


### Hash（哈希）
* 简介：在Redis中，哈希类型是指v（值）本身又是一个键值对（k-v）结构
* 简单使用举例：hset key field value 、hget key field
* 内部编码：ziplist（压缩列表） 、hashtable（哈希表）
* 应用场景：缓存用户信息等。
* 注意点：如果开发使用hgetall，哈希元素比较多的话，可能导致Redis阻塞，可以使用hscan。而如果只是获取部分field，建议使用hmget。


### List（列表）
* 简介：列表（list）类型是用来存储多个有序的字符串，一个列表最多可以存储2^32-1个元素。
* 简单实用举例：lpush key value [value ...] 、lrange key start end
* 内部编码：ziplist（压缩列表）、linkedlist（链表）
* 应用场景：消息队列，文章列表


### Set（集合）
* 简介：集合（set）类型也是用来保存多个字符串元素，但是不允许重复元素
* 简单使用举例：sadd key element [element ...]、smembers key
* 内部编码：intset（整数集合）、hashtable（哈希表）
* 注意点：smembers和lrange、hgetall都属于比较重的命令，如果元素过多存在阻塞Redis的可能性，可以使用sscan来完成。
* 应用场景：用户标签,生成随机数抽奖、社交需求。

### 有序集合（zset）
* 简介：已排序的字符串集合，同时元素不能重复
* 简单格式举例：zadd key score member [score member ...]，zrank key member
* 底层内部编码：ziplist（压缩列表）、skiplist（跳跃表）
* 应用场景：排行榜，社交需求（如用户点赞）。

### Redis 的三种特殊数据类型
* Geo：Redis3.2推出的，地理位置定位，用于存储地理位置信息，并对存储的信息进行操作。
* HyperLogLog：用来做基数统计算法的数据结构，如统计网站的UV。
* Bitmaps ：用一个比特位来映射某个元素的状态，在Redis中，它的底层是基于字符串类型实现的，可以把bitmaps成作一个以比特位为单位的数组

## 单线程模型
* Redis是单线程模型的，而单线程避免了CPU不必要的上下文切换和竞争锁的消耗。也正因为是单线程，如果某个命令执行过长（如hgetall命令），会造成阻塞。Redis是面向快速执行场景的数据库。，所以要慎用如smembers和lrange、hgetall等命令。

* Redis 6.0 引入了多线程提速，它的执行命令操作内存的仍然是个单线程。


## 虚拟内存机制
&emsp;&emsp; Redis直接自己构建了VM机制 ，不会像一般的系统会调用系统函数处理，会浪费一定的时间去移动和请求。

* Redis的虚拟内存机制是啥呢？
> 虚拟内存机制就是暂时把不经常访问的数据(冷数据)从内存交换到磁盘中，从而腾出宝贵的内存空间用于其它需要访问的数据(热数据)。通过VM功能可以实现冷热数据分离，使热数据仍在内存中、冷数据保存到磁盘。这样就可以避免因为内存不足而造成访问速度下降的问题。


##  什么是缓存击穿、缓存穿透、缓存雪崩？

### 缓存穿透问题

## Redis 为什么这么快
* 基于内存实现
* 高效的数据结构
* 合理的数据编码，不同场景使用不同的数据类型
* 合理的线程模型，单线程模型避免上下文切换，I/O多路复用
* 虚拟内存机制