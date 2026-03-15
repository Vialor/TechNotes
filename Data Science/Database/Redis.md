> A in-memory store
# Use Cases
Database Caching: cache often-used result from the database
Session Data: store session information from web server
Replication: replicate the cache of the main server in case of shut down
# Basic Operations
`redis-server`
`redis-cli`
## Transaction

> NOT atomic
```JavaScript
MULTI
EXEC
```
# 5 Basic Data Types
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
### ZSet(Sorted Set)
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
## Underlying implementations
| String | List                             | Hash         | Set         | Zset             |
| ------ | -------------------------------- | ------------ | ----------- | ---------------- |
| SDS    | LinkedList-->ZipList-->QuickList | Dict、ZipList | Dict、Intset | ZipList、SkipList |
SDS Simple Dynamic String
# Expiration
Expire：在设置键值对的同时，可以设置一个过期时间，Redis 会自动在该键值对在指定的时间内过期，过期后会自动删除该键值对。 

Evict：通过配置 maxmemory 限制 Redis 的内存使用量，在内存满时 Redis 会将一些键值对从内存中删除，优先删除的是那些离过期时间最近的键值对，以此来保证 Redis 的内存使用量不会超过限制。 

Lazy deletion：在访问一个键值对时，Redis 会先检查该键值对是否过期，如果过期则会立即删除该键值对。惰性删除的优点是不需要额外的删除操作，节省了服务器资源，缺点是可能会有大量过期的键值对占用内存。 一般情况下，使用过期时间是最常见的过期策略，而惰性删除可以作为补充策略来保证 Redis 的内存使用量不会超过限制。当然，在特定场景下也可以使用其他的过期策略。
# Challenges
**缓存雪崩**：同一时间大量缓存失效，导致大量请求直接到达数据库，可能造成数据库过载甚至崩溃
**缓存击穿**：大量并发请求同时查询同一未缓存数据数据，这些请求未命中缓存而直接打到数据库，导致数据库压力过大。
	应对：Redis分布式锁
**缓存穿透**：缓存和数据库中都不存在某些数据，大量请求绕过缓存直接查询数据库，导致数据库压力过大。