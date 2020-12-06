---
title: 'redis指令及部分基础'
date: 2019-03-15 13:52:09
tags:
	- 数据库
categories: 
	- 数据库
	- Redis
---

# **一、Redis**

## **1.什么是Redis？**

Redis 是Remote Dictionary Server（远程数据服务）的缩写由意大利人antirez（Salvatore Sanfilippo）开发的一款**内存高速缓存数据库**该软件使用C语言编写，它的**数据模型为key-value，它支持丰富的数据结构（类型）**，比如String list hash set sorted set。**可持久化，保证了数据安全。**



**使用缓存减轻数据库的负载。**

在开发网站的时候如果有一些数据在短时间之内不会发生变化，而它们还要被频繁访问，为了提高用户的请求速度和降低网站的负载，就把这些数据放到一个读取速度更快的介质上（或者是通过较少的计算量就可以获得该数据），该行为就称作对该数据的缓存。

该介质可以是文件、数据库、内存，内存经常用于数据缓存。



**缓存有两种类型：数据缓存，页面缓存。**

举例：

新闻页面(内容单一，集中)，适合用页面缓存；

商品页面的组成部分根据业务特点，各个部分数据比较独立，适合分别做“数据缓存”。



## **2.redis 和memcache比较**

　　(1)、存储方式

　　　　Memecache把数据全部存在内存之中，断电后会挂掉，数据不能超过内存大小。

　　　　Redis有部份存在硬盘上，这样能保证数据的持久性。

　　(2)、数据支持类型

　　　　Memcache对数据类型支持相对简单。

　　　　Redis有复杂的数据类型。

　　(3)、使用底层模型不同

　　　　它们之间底层实现方式 以及与客户端之间通信的应用协议不一样。

　　　　Redis直接自己构建了VM 机制 ，因为一般的系统调用系统函数的话，会浪费一定的时间去移动和请求。

{%asset_img 01.jpg%}



**3.安装Redis**

[安装Redis](<https://www.cnblogs.com/renzhicai/p/7773080.html>)

前端启动redis服务

{%asset_img 02.png%}

安装成功后，后台启动redis

{%asset_img 03.png%}

接着启动 redis-cli即可使用

{%asset_img 04.png%}



### 4.Redis与Memcached的选择

**终极策略：** 使用Redis的String类型做的事，都可以用Memcached替换，以此换取更好的性能提升； 除此以外，优先考虑Redis；

### 5.使用redis有哪些好处？

(1) **速度快**，因为数据存在内存中，类似于HashMap，HashMap的优势就是查找和操作的时间复杂度都是O(1)

(2)**支持丰富数据类型**，支持string，list，set，sorted set，hash

(3) **支持事务**，操作都是原子性，所谓的原子性就是对数据的更改要么全部执行，要么全部不执行

(4) **丰富的特性**：可用于缓存，消息，按key设置过期时间，过期后将会自动删除



# **二、redis的具体使用**

  redis中的五种常用类型分别是string,Hash,List,Set,ZSet。

| 类型       | 特点                                                         | 使用场景                                                     |
| :--------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| string     | 简单的key-value类型，value其实不仅是String，也可以是数字     | 定时持久化，操作日志，常规计数， 微博数, 粉丝数等功能        |
| hash       | 是一个string类型的field和value的映射表，hash特别适合用于存储对象 | 存储部分变更数据，如用户信息等                               |
| list       | 有序可重复的列表                                             | twitter的关注列表，粉丝列表，最新消息排行，消息队列          |
| set        | 无序不可重复的列表                                           | 在微博应用中，可以将一个用户所有的关注人存在一个集合中，将其所有粉丝存在一个集合。Redis还为集合提供了求交集、并集、差集等操作，可以非常方便的实现如共同关注、共同喜好、二度好友等功能 |
| Sorted set | 带有score的Set                                               | 排行榜                                                       |

Redis支持的数据类型

**Keys**

给存储在redis内存中的数据起的变量名字。

注意:

(1)key的命名规则不同于一般语言，键盘上除了空格、n换行外其他的大部分字符都可以使用。

像“my key和“mykey\n”这样包含空格和换行的key是不允许的。

(2)我们在使用的时候可以自己定义一个Key的格式。●例如

```
object-type :id:field
```

(3)Key不要太长。占内存，查询慢。

(4)Key不要太短。像u：1000：pwd就不如user：1000；password可读性好

**Redis keys 命令**

下表给出了与 Redis 键相关的基本命令：

```
// keys键操作
exists key                       //测试指定key是否存在
del key1 key2----keyN            //删除给定key
type key                         //返回给定key的value类型
keys pattern                     //返回匹配指定模式的所有key
rename oldkey neukey             //改名字
dbsize                           //返回当前数据的key数量
expire key seconds               //为key 指定过期时间
tt1 key                          //返回key的剩余过期秒数
select db-index                 //选择数据库
move key db-index               //将key 从当前数据库移动到指定数据库
flushdb                         //删除当前数据库中所有key
f1ushall                        //删除所有数据库中的所有key
```

**keys pattern:**

首先创建一些 key，并赋上对应值：

```
redis 127.0.0.1:6379> SET runoob1 redis
OK
redis 127.0.0.1:6379> SET runoob2 mysql
OK
redis 127.0.0.1:6379> SET runoob3 mongodb
OK
```

查找以 runoob 为开头的 key：

```
redis 127.0.0.1:6379> KEYS runoob*
1) "runoob3"
2) "runoob1"
3) "runoob2"
```

获取 redis 中所有的 key 可用使用 *

```
redis 127.0.0.1:6379> KEYS *
```

**不同redis数据库切换  select db-index:**

```
select 10
OK
```

**Values**

Strings（Binary-safe strings）

Lists（Lists of binary-safe strings）

Sets（Sets of binary-safe strings）

Sorted sets（Sorted sets of binary-safe strings）

Hash



### **1.String类型操作**

string是redis最基本的类型

**redis的string 可以包含任何数据。包括ipg.图片或者序列化的对象。**

**单个value值最大上限是1G字节。**

如果只用string类型，redis 就可以被看作加上持久化特性的memcache

```
//string类型操作
set key value                    //设置key对应的值为string类型的value
mset key1 value1.…keyN valueN    //—次设置多个key的值
mget key1 key2.……keyN            //一次获取多个key的值
incr key                         //对key的值做加加操作，并返回新的值
decr key                         //同上，但是做的是减减操作
incrby key integer               //同incr，加指定值
decrby key integer               //同decr，减指定值
append key value                 //给指定key的字符串值追加value
substr key start end            //返回截取过的key的字符串值
```

### **2.数据类型List链表**

**list类型其实就是一个双向链表。通过push，pop操作从链表的头部或者尾部添加删除元素。**

**这使得list既可以用作栈，也可以用作队列。**



该list链表类型应用场合：

获得最新的10个登录用户信息：select*from user order by logintime desc limit 10；以上sql语句可以实现用户需求，但是数据多的时候，全部数据都要受到影响，对数据库的负载比较高。必要情况还需要给关键字段（id或logintime）设置索引，索引也比较耗费系统资源

如果通过list链表实现以上功能，可以在ist链表中只保留最新的10个数据，每进来一个新数据进来就删除一个旧数据。每次就可以从链表中直接获得需要的数据。极大节省各方面资源消耗

{%asset_img 05.png%}

```
127.0.0.1:6379[1]> lpush login marry
(integer) 1
127.0.0.1:6379[1]> lpush login jack
(integer) 2
127.0.0.1:6379[1]> lpush login tom
(integer) 3
127.0.0.1:6379[1]> lpush login jane
(integer) 4
127.0.0.1:6379[1]> lpush login yue
(integer) 5
127.0.0.1:6379[1]> lpush login dian
(integer) 6
127.0.0.1:6379[1]> rpop login
"marry"
127.0.0.1:6379[1]> 
```

**list的一些相关操作:**

```
//1ist类型操作
lpush key string          //在key对应1ist的头部添加字符串元素
rpop key                  //从list 的尾部删删除元素，并返回册删除元素
llen key                  //返回key对应list 的长度，key 不存在返回0，如果key 对应类型不是list 返回错误
lrange key start end      //返回指定区间内的元素，下标从0开始
rpush key string          //同上，在尾部添加
lpop key                  //从ist 的头部删除元素，并返回删除元素
ltrim key start end       //截取1ist，保留指定区间内元素
```

```
127.0.0.1:6379[1]> lrange login 0 100
1) "dian"
2) "yue"
3) "jane"
4) "tom"
5) "jack"
127.0.0.1:6379[1]> llen login
(integer) 5
127.0.0.1:6379[1]> ltrim login 1 3
OK
127.0.0.1:6379[1]> lrange login 0 100
1) "yue"
2) "jane"
3) "tom"
127.0.0.1:6379[1]> 
```

### **3.Redis 哈希(Hash)**

Redis hash 是一个string类型的field和value的映射表，hash特别适合用于存储对象。

Redis 中每个 hash 可以存储 232 - 1 键值对（40多亿）。

hash的常见命令：

```
##赋值语法
hset key field value   //为指定的key,设定field/value
hmset key field value [field1,value1]...同时将多个field-value(阈-值)对设置到哈希表key中。
#例如  hmset users uname guo age 20 address "北京市"

#取值语法：
hget key field    //获取存储在HASH中的值，根据FIELD得到VALUE
hmget key field[field1]  //获取key所有给定字段的值
hgetall key              //返回hash表中所有的字段和值
hkeys key               //获取所有哈希表中的字段
hlen key                //获取哈希表中字段的数量

#删除语法：
hdel key field1[field2]  //删除一个或多个HASH表字段

#其他语法：略
```

**相关操作：**

```
127.0.0.1:6379> hset yue job student
(integer) 1
127.0.0.1:6379> hmset yue home hubei age 25
OK
127.0.0.1:6379> hget yue age
"25"
127.0.0.1:6379> hgetall yue
1) "job"
2) "student"
3) "home"
4) "hubei"
5) "age"
6) "25"
127.0.0.1:6379> hkeys yue
1) "job"
2) "home"
3) "age"
127.0.0.1:6379> hlen yue
(integer) 3
127.0.0.1:6379> hdel yue age
(integer) 1
127.0.0.1:6379> hgetall yue
1) "job"
2) "student"
3) "home"
4) "hubei"
127.0.0.1:6379> 
```

### **4.set集合类型**

redis的set是string类型的无序集合。

set元素最大可以包含（2的32次方-1）个元素。

关于set集合类型除了基本的添加删除操作，其他有用的操作还包含集合的取并集（union），交集（intersection），差集（difference）。通过这些操作可以很容易的实现sns.中的好友推荐功能。



注意：每个集合中的各个元素不能重复。

该类型应用场合：qq好友推荐。

tom朋友圈（与某某是好友）：aary jack xiaoming wang5 wang6

linken 朋友圈（与某某是好友）：yuehan daxiong luce wang5 wang6

{%asset_img 06.png%}

```
//set类型操作
sadd key member        //添加一个string 元素到key对应的set集合中，成功返回1，
                       //如果元素已经在集合中返回0，key对应的set不存在返回错误
srem key member[member]//从key 对应set 中移除给定元素，成功返回1
smove p1 p2            //menber从p1对应set 中移除 nenber 并添加到p2对应set中返回set的元素个数
sismember key nember     //判断 menber是否在set中
sinter key1 key2-.-keyN  //返回所有给定key的交集
sunion key1 key2.--keyN  //返回所有给定key 的并集
sdiff key1 key2-.-keyN   //返回所有给定key的差集
smembers key            //返回key对应set的所有元素，结果是无序的
```

```
127.0.0.1:6379> select 1
OK
127.0.0.1:6379[1]> sadd tom marry
(integer) 1
127.0.0.1:6379[1]> sadd tom jack
(integer) 1
127.0.0.1:6379[1]> sadd tom wang5
(integer) 1
127.0.0.1:6379[1]> sadd tom wang6
(integer) 1
127.0.0.1:6379[1]> sadd tom xiaoming
(integer) 1
127.0.0.1:6379[1]> smembers tom
1) "jack"
2) "marry"
3) "wang5"
4) "wang6"
5) "xiaoming"
127.0.0.1:6379[1]> sadd linken yuehan
(integer) 1
127.0.0.1:6379[1]> sadd linken daxiong
(integer) 1
127.0.0.1:6379[1]> sadd linken luce
(integer) 1
127.0.0.1:6379[1]> sadd linken wang5
(integer) 1
127.0.0.1:6379[1]> sadd linken wang6
(integer) 1
127.0.0.1:6379[1]> smembers linken
1) "wang5"
2) "luce"
3) "daxiong"
4) "yuehan"
5) "wang6"
127.0.0.1:6379[1]> sinter tom linken
1) "wang5"
2) "wang6"
127.0.0.1:6379[1]> sunion tom linken
1) "wang6"
2) "xiaoming"
3) "jack"
4) "marry"
5) "wang5"
6) "luce"
7) "daxiong"
8) "yuehan"
127.0.0.1:6379[1]> sdiff tom linken
1) "jack"
2) "marry"
3) "xiaoming"
127.0.0.1:6379[1]> sdiff linken tom
1) "luce"
2) "daxiong"
3) "yuehan"
127.0.0.1:6379[1]> 
```

### **5.Sort Set 排序集合类型**

和set 一样 sorted set 也是string类型元素的集合，不同的是每个元素都会关联一个权。通过权值可以有序的获取集合中的元素该Sort set 类型适合场合：

获得热门帖子（回复量）信息：select*from message order by backnum desclimit 5；

（以上需求可以通过简单sql语句实现，但是sgl语句比较耗费mysql数据库资源）

【案例】利用sort set 实现获取最热门的前5帖子信息

{%asset_img 07.png%}

排序集合中的每个元素都是值、权的组合

（之前的set集合类型每个元素就只是一个值）

{%asset_img 08.png%}

**相关指令：**

```
//sorted set排序类型
zadd key score member     //添加元素到集合，元素在集合中存在则更新对应 score
zrem key nember           //除指定元素，1表示成功，如果元素不存在返回0
zincrby key incr member   //按照incr 幅度增加对应 nember的score值，返回 score值
zrank key member          //返回指定元素在集合中的排名（下标），集合中元素是按 score从小到大排序的
zrevrank key member       //同上，但是集合中元素是按 score 从大到小排序
zrange key start end      //类似irange操作从集合中去指定区间的元素。返回的是有序结果
zrevrange key start end   //同上，返回结果是按 score逆序的
zcard key                 //返回集合中元素个数
zscore key member       //返回给定元素对应的 score
zrenrangebyrank key min max    //删除集合中排名在给定区间的元素
```

**相关操作：**

```
127.0.0.1:6379[1]> zadd tiezi 102 11
(integer) 1
127.0.0.1:6379[1]> zadd tiezi 141 12
(integer) 1
127.0.0.1:6379[1]> zadd tiezi 159 13
(integer) 1
127.0.0.1:6379[1]> zadd tiezi 72 14
(integer) 1
127.0.0.1:6379[1]> zadd tiezi 203 15
(integer) 1
127.0.0.1:6379[1]> zadd tiezi 189 16
(integer) 1
127.0.0.1:6379[1]> zadd tiezi 191 17
(integer) 1
127.0.0.1:6379[1]> zadd tiezi 305 18
(integer) 1
127.0.0.1:6379[1]> zadd tiezi 184 19
(integer) 1
127.0.0.1:6379[1]> zrang tiezi 0 100
(error) ERR unknown command 'zrang'
127.0.0.1:6379[1]> zrange tiezi 0 100
1) "14"
2) "11"
3) "12"
4) "13"
5) "19"
6) "16"
7) "17"
8) "15"
9) "18"
127.0.0.1:6379[1]> zrevrange tiezi 0 100
1) "18"
2) "15"
3) "17"
4) "16"
5) "19"
6) "13"
7) "12"
8) "11"
9) "14"
127.0.0.1:6379[1]> zrank tiezi 13
(integer) 3
127.0.0.1:6379[1]> zcard tiezi
(integer) 9
127.0.0.1:6379[1]> zscore tiezi 18
"305"
127.0.0.1:6379[1]> 
```

# **三.持久化功能**

redis为了内部数据的安全考虑，会把本身的数据**以文件形式保存到硬盘中一份**，在服务器重启之后会自动把硬盘的数据恢复到内存（redis）的里边。

数据保存到硬盘的过程就称为“持久化”效果。

## **1.snap shotting快照持久化**

该持久化默认开启，一次性把redis中全部的数据保存一份存储在硬盘中(存储在redis-cli所在的文件夹中，文件名**dump.rdb**)，如果数据非常多（10-20G）就**不适合频繁进行该持久化**操作。

**自动快照持久化：**

打开redis.conf可以看到默认快照持久化的频率如下：

```
save 900 1
save 300 10
save 60 10000
```

save 900 1              #900秒内如果超过1个key被修改，则发起快照保存

save 300 10            #300秒超过10个key被修改，发起快照

save 60   10000      #60秒超过10000个key被修改，

发起快照以上三个save的意思：

数据修改的频率非常高，备份的频率也高,数据修改的频率低，备份的频率也低。

key的修改包括添加、删除和修改，不包括查询。

**手动快照持久化：**

redis的持久化相关指令

**bgsave          异步保存数据到磁盘（快照保存）**

lastsave        返回上次成功保存到磁盘的unix时间载

shutdown      同步保存到服务器并关闭redis服务器

bgrewriteaof  当日志文件过长时优化AOF日志文件存储

```
./redis-cli bgrewriteaof
./redis-cli bgsave                        #手动发起快照
./redis-cli-h 127.0.0.1 -p 6379 bgsave    #手动发起快照
```

## **2.append only file（AOF 持久化）**

本质：把用户执行的每个“写”指令（添加、修改、删除）都备份到文件中，还原数据的时候就是执行具体写指令而已。



开启AOF持久化（会清空redis之前的数据，因此若要开启，最好安装后开启）

（同时可以修改备份文件的名字，默认是appendonly.aof和dump.rdb同级目录下）

打开配置文件redis.conf,将appendonly no改为appendonly yes。

配置文件被修改，需要删除旧进程，再根据新的配置文件启动新进程：

{%asset_img 09.png%}

AOF追加持久化的备份频率(在redis.conf文件中)：

```
#appendfsync always     #这种方式每次添加、修改和删除都更新，但性能最低
appendfsync everysec    #每秒更新一次，性能折衷，推荐
#appendfsync no         #更新频率看服务器心情，性能富裕更新得快，由OS决定，持久化无保障，
```

### **AOF持久化文件的优化：**

压缩appendonly.aof文件，例如incr指令执行过程中的中间变量将被删去，只留新的一个，相当于执行set语句。

redis的持久化相关指令

bgsave          异步保存数据到磁盘（快照保存）

lastsave        返回上次成功保存到磁盘的unix时间载

shutdown      同步保存到服务器并关闭redis服务器

**bgrewriteaof  当日志文件过长时优化AOF日志文件存储**

```
./redis-cli bgrewriteaof
./redis-cli bgsave                        #手动发起快照
./redis-cli-h 127.0.0.1 -p 6379 bgsave    #手动发起快照
```

**四、redis的主从模式**

为了降低每个redis服务器的负载，可以多设置几个，并做主从模式一个服务器负载“写”数据，其他服务器负载“读”数据主服务器数据会“自动”同步给从服务器

{%asset_img 10.png%}