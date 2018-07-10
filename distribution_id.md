# 统一ID生成服务

------

为了实现对ID生成规则的统一管理，系统提供ID生成服务，这是一个62进制的步长可配置的顺序分布式ID生成器
目前系统内使用统一ID生成服务的功能有：Hibernate实体主键、游客全局标识ID。
ID生成服务按类型的方式隔离，类型由注册时传入的`key`区分。
Hibernate实体的每一个主键都作为一个独立的类型存在。

##使用方法
**服务配置**
spring-xx-dubbo-consumer.xml配置dubbo服务消费者

![](/assets/did_image1.png)

** 服务接口**

>ID生成服务申请\注册

`void register(RegisterUpdateParam updateParam);`

在使用ID生成服务之前应先进行注册，传入ID生成所需配置参数

RegisterUpdateParam结构：

字段|类型|含义|默认值
---|---|---|---
key|String|ID生成服务类型标识；必须设置|无
keyCombination|KeyCombination|格式设置：1.大小写混合；2.只有大写|1
length|Integer|ID长度|20
minSkip|Integer|最小步长|1|
maxSkip|Integer|最大步长；必须设置|无|

>ID生成配置变更

`void update(DidUpdateParam updateParam);`

变更ID生成器的最小步长或最大步长

DidUpdateParam结构：

字段|类型|含义
---|---|---
key|String|标识修改哪一个类型的ID生成器
minSkip|Integer|最小步长修改值
maxSkip|Integer|最大步长修改值

>获取单个ID

`String getDistributedId(String key);`

获取`key`类型的最新ID

>批量获取ID

`List<String> getDistributedIdBatch(String key, int count);`

获取`count`个`key`类型的ID

>批量获取多种类型的ID

`HashMap<String,List<String>> getDistributedIdHash(HashMap<String,Integer> hashMap);`

调用时传入一个map，map的key为类型，value为该类型所需ID数量；
以map的形式返回不同类型的ID集合

##实现原理
以0-9、A-Z、a-z这62个字符的顺序递增，递增到z进一位，能够表示的数值取决于设置的长度，假如长度为n，则能够表示的数值就是62的n次方。
设置长度为6位就已经可以表示56800235584（五百多亿）个数值，默认值为20位，这个数量级足以满足长期的业务。