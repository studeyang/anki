# RocketMQ

## 一、组成

### namesrv

- 存储topic元信息
- 存储路由信息
- 存储broker信息
- 存储producer
- 存储consumer
- 为什么不用 zk?相比于 zk 有什么优势?

### client

### broker

- 接收消息
- commitLog刷盘
- 构建ConsumerQueue
- 构建IndexFile
- 主从同步

### remote

## 二、消息接收流程

### ReBalance怎么实现？

- 获取所有MessageQueue
- 获取所有Consumer
- 根据分配策略分配
  - 同机房优先
  - 平均
  - 环形平均
  - 配置
  - 一致性hash

### Push

循环发送拉数据请求 自动提交位点

### Pull

循环拉取 手动控制提交位点

### 消息过滤

- 按 tag 过滤
- 按 sql 过滤

### 异常重试

- 重试次数 16
- 渐进式重试时间：delayLevel

### 死信队列

超过重试次数进死信队列

## 三、消息发送流程

### 可靠性保证

- 同步
- 异步
- 一次

### 完整流程

- 写 commitLog
- commitLog 同步 or 异步刷盘
- ReputMessageService 构建 ConsumerQueue 和 IndexFile
- FlushConsumeQueueService ConsumerQueue 数据刷盘

### 消息类型

- 普通消息
- 顺序消息：rebalance 时对 MessageQueue 加锁 保证同一时刻只有一个消费者消费一个队列提交消息到消费者线程时加锁 保证只有一个线程处理单个队列
- 广播消息：rebalance 时每个消费者订阅所有的 MessageQueue 各个 consumer 本地自己记录消费位点
- 事务消息
  - 发送端
      producer 发送待事务标记的消息
      根据发送结果 结束事务
      提供事务状态回查接口
  - Broker
      判断是否有事务标识 如果有 存储到半消息 topic 中
      根据事务提交 回滚结果 发送到真实消息中
      超时主动询问 producer 消息状态
- 延迟消息
  - commitLog 时修改 topic queueId
  - reput 时写入不同队列
  - 定时任务调度 重新写入原始 commitLog
  - 原始 commitLog 写入到 MessageQueue

## 四、其他

### 与Kafka区别

- 数据可靠性
  			rmq同步刷盘+异步刷盘
  			rmq同步复制+异步复制
  			kafka异步
- 写消息性能
  			kafka高 通过合并小消息批量提交实现
- 单机队列数
  			rmq>kafka
- 消息投递实时性
  			kafka短轮询 实时性取决于轮询时间
  			rmq长轮询 跟push一样实时
- 失败重试
  			kafka消费失败不支持重试
  			rmq支持
- 定时消息
  			kafka不支持
  			rmq支持

### 如何保证消息有序



