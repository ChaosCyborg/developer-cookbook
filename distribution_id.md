# 统一ID生成服务

------

为了实现对ID生成规则的统一管理，系统提供ID生成服务，这是一个62进制的步长可配置的分布式ID生成器
目前系统内使用统一ID生成服务的功能有：Hibernate实体主键、游客全局标识ID。
ID生成服务按类型的方式隔离，类型由注册时传入的`key`区分。
Hibernate实体的每一个主键都作为一个独立的类型存在。

**使用方法**

spring-xx-dubbo-consumer.xml配置dubbo服务消费者

![](/assets/did_image1.png)

 服务接口

>ID生成服务申请\注册

`void register(RegisterUpdateParam updateParam);`

在使用ID生成服务之前应先进行注册，传入ID生成所需配置参数
RegisterUpdateParam结构：

字段|类型|含义|默认值
---|---|---|---
key|String|ID生成服务类型标识；必须设置|无
keyCombination|KeyCombination|格式设置：1.大小写混合；2.只有大写|1




>ID生成配置变更