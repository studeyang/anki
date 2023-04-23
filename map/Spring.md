# Spring

## 一、组成结构

### IOC

一种设计思想 将需要手动创建对象的控制权交给框架管理 需要的时候由框架注入对象实例

初始化过程：

- 解析 XML Beans 配置
- 扫描 读取 Resouce
- 解析 BeanDefinition
- 注册到 BeanFactory
- refresh 依赖 注入

### AOP

- 实现方式
  			Java动态代理
  			cglib
- SpringAOP VS AspectJ
  			SpringAOP 运行时增强 基于代理
  			AspectJ  编译时增强 基于字节码操作

### 其他

- Aspect
- JDBC
- JMS
- ORM
- Web
- Test

## 二、Spring 事务控制

### 事务传播行为

- 支持当前事务情况
  - PROPAGATION_REQUIRED 
    			存在则加入，不存在则新建
  - PROPAGATION_SUPPORTS
    			存在则加入，不存在以非事务方式运行
  - PROPAGATION_MANDATORY
    			存在则加入，不存在抛异常
- 不支持当前事务情况
  - PROPAGATION_REQUIRES_NEW
    			存在事务，挂起当前事务并开启一个新事务
  - PROPAGATION_NOT_SUPPORTED
    			挂起当前事务，以非事务方式运行
  - PROPAGATION_NEVER
    			当前存在事务 抛出异常

### Spring中事务不生效场景

- 数据库引擎不支持
- 入口方法不是Public
- 异常回滚
- 方法是否被代理：在同一个类里调用其中方法 不生效
  	

## 三、用到的设计模式

- 工厂模式
- 单例模式
- 代理模式
- 观察者模式
- 适配器模式
- 责任链模式

## 四、Bean作用域

- singleton
- prototype
- request
- session
- global session
