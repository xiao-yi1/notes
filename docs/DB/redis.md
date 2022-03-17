1. ### 为什么要用redis而不用map做缓存?
    Redis 可以用几十 G 内存来做缓存，Map 不行，一般 JVM 也就分几个 G 数据就够大了

    Redis 的缓存可以持久化，Map 是内存对象，程序一重启数据就没了

    Redis 可以实现分布式的缓存，Map 只能存在创建它的程序里

    Redis 可以处理每秒百万级的并发，是专业的缓存服务，Map 只是一个普通的对象

    Redis 缓存有过期机制，Map 本身无此功能

    Redis 有丰富的 API，Map 就简单太多了
2. 为啥 redis 单线程模型也能效率这么高？
    纯内存操作
    核心是基于非阻塞的 IO 多路复用机制
    单线程反而避免了多线程的频繁上下文切换问题
3. 用redis做流控
<https://developer.aliyun.com/article/839401>
4. Redis（1.16）Redis为什么是单线程？为什么快？
<https://www.cnblogs.com/gered/p/12074569.html>
5. Redis不是一直号称单线程效率也很高吗，为什么又采用多线程了？
<https://www.1024sou.com/article/35438.html>
6. 史上最全Redis面试49题(含答案):哨兵+复制+事务+集群+持久化等
<https://studygolang.com/articles/16848>
