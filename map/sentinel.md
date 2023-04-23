# sentinel

## 一、运行流程

### SphU.entry()

### lookProcessChain

责任链模式 每个Slot对应一个处理

### ProcessorSlot

- NodeSelectorSlot：负责收集资源的路径，并将这些资源的调用路径，以树状结构存储起来，用于根据调用路径来限流降级；
- ClusterBuilderSlot：则用于存储资源的统计信息以及调用者信息，例如该资源的 RT, QPS, thread count 等等，这些信息将用作为多维度限流，降级的依据
- StatistcSlot：则用于记录，统计不同纬度的 runtime 信息；
- FlowSlot：则用于根据预设的限流规则，以及前面 slot 统计的状态，来进行限流；
- AuthorizationSlot：则根据黑白名单，来做黑白名单控制；
- DegradeSlot：则通过统计信息，以及预设的规则，来做熔断降级；
- SystemSlot：则通过系统的状态，例如 load1 等，来控制总的入口流量；

## 二、限流规则

- 快速失败
- warm up
- 排队

## 三、限流算法

- 滑动窗口
- 令牌桶：肯定速率产生令牌放入桶中 可以容忍一定的突发流量
- 漏斗：固定速率限流 起到整流左右

## 四、指标采集

- 系统指标：定时线程JMX
- qps
  		滑动窗口
  		LongAdder

## 五、与Hystrix对比

### sentinel

- 信号量隔离
- 基于响应时间和失败比率
- 滑动窗口
- 基于QPS限流
- 支持慢启动 匀速器模式
- 支持系统负载保护
- 控制台完善

### hystrix

- 信号量+线程池隔离
- 基于失败比率
- 滑动窗口
- 不支持QPS限流
- 不支持慢启动
- 不支持系统负载保护
- 控制台不完善
