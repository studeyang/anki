# Redis

## 一、 高性能

### 特点

- 支持更丰富的数据类型
- 支持持久化
- 支持发布订阅模型 Lua脚本 事务

### 6.0之后支持多线程

- IO读写支持多线程
- 部分命令多线程
- 命令操作还是主线程

### 为什么单线程快

- 内存操作快
- 单线程不需要处理锁竞争，没有多线程的上下文切换
- 多路IO复用：epoll
- 数据结构快

### 异步子线程

- AOF日志
- 键值对删除
- 文件关闭

### 影响性能的因素

- 内部阻塞操作
  - 客户端网络IO
  - 读写磁盘 AOF RDB
  - 主从节点切换 传输RDB文件
  - 切片集群实例 数据迁移
  - BigKey删除
- CPU核和NUMA架构影响
- 多核缓存行失效
- 多核上下文切换
- Redis关键系统配置
- Redis内存碎片
- Redis缓冲区

## 二、数据结构

### 数据类型

- String
- List
- Set
- Hash
- SortedSet
- GeoHash
- HyperLogLog
- Bitmap

### 底层数据结构

- RedisObject
  - int编码：整形
  - embstr 编码：<=44字节 连续内存区域
  - raw编码：Object+指向sds指针
- 简单动态字符串 sds
  - 组成：len 已用长度、alloc 已分配长度、buf 字节数组
- 双向链表
- 压缩列表
  - 组成：zlbytes, zltail, zllen, entry, zlend

- 哈希表
  - 全局哈希表
    - 哈希冲突
    - rehash：渐进式rehash

- 跳表
  - 原理
    - 链表
    - 向前指针、向后指针
    - 每个节点有多层，层数随机
    - 每层执行下个节点的对应层数
  - 为什么用跳表不用红黑树
    - 红黑树需要通过旋转树来达到平衡
    - 区间查询跳表时间复杂度低
    - 跳表实现难度低

- 整数数组

## 三、源码分析

- 底层数据结构
- RESP协议
- 数据类型
- io多路复用
- 事件机制
- AOF RDB
- 集群
- 过期Key清理

## 四、其他

### 分布式锁

- setnx ex：缺点 主备切换时有问题
- redlock：尝试在多个 redis 节点之间获取锁
- redission

### RESP协议

## 五、Redis能否做消息队列

### List

不支持 ACK 只支持单个消费者

### PubSub

- 支持多个消费者
- 只支持在线消费者
- 不支持堆积 持久化

### Stream

可支持 看业务场景

## 六、缓存四大问题

### 缓存穿透

原因：请求了DB中也不存在的 Key

应对方案

- 缓存空值
- 布隆过滤器

### 缓存击穿

原因：热点数据过期 大量请求访问同一条数据

应对方案：

- 热点数据永不过期
- 分布式锁更新缓存

### 缓存雪崩

原因：缓存大面积失效、Redis 宕机

应对方案：

- 过期时间浮动 避免同时过期
- Redis 高可用部署

### 缓存与DB数据一致

- Cache Aside
- 延迟双删
- Read/Write through
- MySQL binlog 同步

## 七、过期数据删除策略

### 惰性删除

取出时才对 Key 过期检查 cpu 友好

### 定期删除

每隔时间删除一部分数据

### 内存淘汰

- volatile-lru
- volatile-ttl
- volatile-random
- allkeys-lru
- allkeys-random
- no-eviction
- volatile-lfu
- allkeys-lfu

## 八、高可用

### 集群方案

- 哨兵模式 Sentinel
- RedisCluster
- hash槽：为什么是 16384 个

### 持久化机制

- AOF
  - 日志内容是什么样的？
  - 三种写回策略：Always, Everyse, No
  - 重写机制
    - 重写会不会阻塞主线程？
    - 重写过程：一个拷贝，两处日志
  
- RDB
  - 执行效率
  - 快照时是否会阻塞主线程：fork子进程、copyonwrite
  - 命令：save、bgsave

### 主从同步

- 第一次同步：FULLRESYNC
- 增量同步：replication buffer
- 主-从-从模式

### 哨兵机制

- 监控：周期心跳，主从库
- 选主（三轮打分）
- 通知
  - 通知所有从库有了新主库
  - 通知客户端

