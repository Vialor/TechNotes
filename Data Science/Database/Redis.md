**Redis REmote DIctionary Service**: 开源的键值对数据库服务器
	需要docker/WSL/Linux
	内存优先的存储系统，包含内存控制和淘汰策略
	多个后端共享访问：网络通信使用TCP 连接 + Redis RESP协议，支持大量原子操作
UI工具：RedisInsight， Another Redis Desktop Manager
# Basic Operations
## Start
```bash
redis-server --daemonize yes  
redis-cli ping
redis-cli shutdown

help @list
```
## Basic Data Types
### String
```JavaScript
SET <key> <value>
SETEX <key> <seconds> <value>
SETNX <key> <value> // set when key does not exist
MSET <key1> <value1> <key2> <value2> ...
INCR <key>
DECR <key>

GET <key>
MGET <key1> <key2> ...
EXISTS <key>
STRLEN <key>
KEYS *
DEL <key>

FLUSHALL <key>
EXPIRE <key> <seconds>
TTL <key> // time to live
```
### List
```JavaScript
lpush/rpush <arrayName> <value1> <value2> ...
lrange <arrayName> <rangeStart> <rangeEnd>
lpop/rpop <arrayName>
```
### Set
```JavaScript
sadd <setName> <value1> <value2> ...
smembers <setName>
srem <setName> <value1> <value2> ...
```
### ZSet(Sorted Set) = Hash table + Skip List
增删为log(N)复杂度
```JavaScript
zadd <setName> <sortScore1> <member1> <sortScore2> <member2> ...
zscore <setName> <member>
zrange/zrevrange <setName> <rangeStart> <rangeEnd> (withscore) // rev for reverse
zrank/zrevrank <setName> <member>
```
### Hash
```JavaScript
hset <hashName> <fieldName> <fieldValue>
hget <hashName> <fieldName>
hgetall <hashName>
hdel <hashName> <fieldName>
hexists <hashName> <fieldName>
```
### Underlying implementations
| String                    | List                             | Hash         | Set         | Zset             |
| ------------------------- | -------------------------------- | ------------ | ----------- | ---------------- |
| SDS Simple Dynamic String | LinkedList-->ZipList-->QuickList | Dict、ZipList | Dict、Intset | ZipList、SkipList |
# 消息队列
## List模拟阻塞队列
无法避免消息丢失。
`LPUSH & BRPOP`
## Pub/Sub
不带保存的实时通讯。无法避免消息丢失，消息会在消费者那里堆积且有上限。
```
SUBSCRIBE channel_name
PSUBSCRIBE channel_pattern

PUBLISH channel_name "hello"
```
## Redis Stream
Redis>=5.0，带保存的可靠任务处理
新的数据类型：stream
```
往key为email的消息队列中添加内容，redis自动生成消息的唯一id。添加了两个entry，to: alice@example.com和subject: hello
XADD email * to alice@example.com subject hello

email的消息队列内信息数量
XLEN email

读取email的消息队列；从第一个消息开始
XREAD STREAMS email 0

阻塞永久等待读取email的消息队列的一条消息；从当前 Stream 的最新位置开始，只读以后新来的消息
XREAD COUNT 1 BLOCK STREAMS email $
```
### Consumer Group
一个队列的消息分流给一个消费者组内的同消费者。
消费者组维护一个**消息标示**，以记录最后一个被处理的消息。
消费者获取消息后，消息处于pending状态，并被计入**pending-list**。处理完成后需要通过XACK来确认，从而消息被标记为已处理并从pending-list移除。
```
XGROUP CREATE 队列key 消费者组名 消费起始ID
XGROUP DESTROY
XGROUP CREATECONSUMER
XGROUP DELCONSUMER

XREADGROUP GROUP group consumer [COUNT count] [BLOCK milliseconds] [NOACK] STREAMS key [key ...] id [id ...]
如果消费者不存在会自动创建
NOACK可以让消息不进入pending-list，直接视为完成
消费起始ID：
    >：消费者组里读取“还没有投递给任何消费者的新消息”
    其它：根据指定id从pending-list获取已消费但未确认的消息
    
XACK key group ID
确认消息读取完成，并移出pending-list
```
## 分布式锁
用 Redis 的原子操作实现跨服务互斥。
# Memory Control
## 默认行为
Lazy deletion：在访问一个键值对时，Redis 会先检查该键值对是否过期，如果过期则会立即删除该键值对。惰性删除的优点是不需要额外的删除操作，节省了服务器资源，缺点是可能会有大量过期的键值对占用内存。 一般情况下，使用过期时间是最常见的过期策略，而惰性删除可以作为补充策略来保证 Redis 的内存使用量不会超过限制。
Active expiration：Redis 会周期性抽样检查设置了过期时间的 key，删除已经过期的 key。
## 内存策略
Expire：在设置键值对的同时，可以设置一个过期时间，Redis 会自动在该键值对在指定的时间内过期，过期后会自动删除该键值对。 
```
5min后过期
SET code:phone:123456 8888 EX 300
```
Evict：通过配置 maxmemory 限制 Redis 的内存使用量，在内存满时 Redis 会将一些键值对从内存中删除。
```
CONFIG SET maxmemory 1gb
优先删除离过期时间最近的键值对：
CONFIG SET maxmemory-policy volatile-ttl
```
## Challenges
**缓存雪崩**：同一时间大量缓存失效，导致大量请求直接到达数据库，可能造成数据库过载甚至崩溃
**缓存击穿**：大量并发请求同时查询同一未缓存数据数据，这些请求未命中缓存而直接打到数据库，导致数据库压力过大。
	应对：Redis分布式锁
**缓存穿透**：缓存和数据库中都不存在某些数据，大量请求绕过缓存直接查询数据库，导致数据库压力过大。
# 持久化
**RDB（Redis 数据库）**：按指定的时间间隔保存数据集的快照
缺点：快照之间的数据会丢失
**AOF（Append Only File）**：持久化每个写操作，再次操作即可重建原始数据集。
缺点：比 RDB 文件使用更多的磁盘。
```
redis-cli CONFIG GET save 查看RDB快照规则（多少秒内至少多少次写入）
redis-cli CONFIG GET appendonly 查看AOF是否开启
redis-cli CONFIG GET dir 持久化文件保存目录
redis-cli CONFIG GET dbfilename RDB持久化文件名
redis-cli CONFIG GET appendfilename AOF持久化文件名

appendfsync always 每次写命令都刷盘，最安全，但最慢
appendfsync everysec 每秒刷盘一次，性能和安全折中，常用
appendfsync no 交给操作系统决定，最快，但丢失窗口更大
```
## Transaction

> 打包命令一次执行
> NOT atomic
```
MULTI
<commands>
EXEC
```