Performance Monitor: Sentry
  
TPS: Transaction Per Second
Number of concurrent threads 并发量
QPS: Queries Per Second: 平均每秒处理多少请求
QPS = 并发量 / 平均响应时间

> Qps基本类似于Tps，但是不同的是，对于一个页面的一次访问，形成一个Tps；但一次页面请求，可能产生多次对服务器的请求，服务器对这些请求，就可计入“Qps”之中
  
Long tail latencies: the higher percentiles (such as 98th, 99th) of latency in comparison to the average latency time. (small number of users experienced much worse latencies than the average)

# 设计秒杀系统
场景解释：瞬时流量大，有时限，库存少，用户多 --> 高并发快响应，读多写少
额外注意：防刷子，防超卖，服务降级的紧急措施，订单失败补偿

高并发快响应，读多写少：
1. 上线前做压测，然后优化代码和服务
2. redis+热数据，提前录入商品信息
3. mq，不要同步返回秒杀结果而是交给mq异步返回
4. 前端静态化，cdn
5. 带宽扩展，集群负载均衡
防刷子：
	用户id限流，ip限流
	隐藏秒杀真链接，时间到了才能看到