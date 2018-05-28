#操作日志

操作日志可以记录用户在系统中对每一个注册功能点的操作行为和操作数据，使用阿里云日志服务存储这些数据，在阿里云日志服务中可统计查询。在权限系统中也有页面查看操作日志的功能

**模块中添加操作日志方法**
模块提供操作日志记录支持需要添加一个controller拦截器和两个springBean

1. 配置在`mvc-dispatch-servlet.xml`中：

~~~xml
<bean id="opLogInterceptor" class="cn.com.yunyoutianxia.auth.log.OperatorLogMethodInterceptor"></bean>
<aop:config>
	<aop:pointcut id="controllerAndFunc" expression="(execution(* cn.com.yunyoutianxia.tour.business.controller..*.*(..)))" />
	<aop:advisor advice-ref="opLogInterceptor" pointcut-ref="controllerAndFunc"></aop:advisor>
</aop:config>
~~~

2. 配置在`spring-base.xml`中：

~~~xml
<bean class="cn.com.yunyoutianxia.auth.log.AliyunLogService">
	<property name="accessKeyId" value="${aliyun.log.user.key.id}" />
	<property name="accessKeySecret" value="${aliyun.log.user.key.secret}"/>
</bean>

~~~

如果需要记录操作数据，controller方法中的参数需要继承接口`IamParam`

