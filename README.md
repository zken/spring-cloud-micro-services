微服务架构改造说明手册

1引言	2
1.1编写目的	2
1.2背景	2
1.3定义	2
2. 模块介绍	2
2.1服务生产者	2
2.2服务消费者	4
2.3服务注册中心	5
2.4架构图	6
3. 应用访问	6
3.1服务注册中心	6
3.2服务消费	7
4. 待优化的内容	7
4.1 springcloud组件	7
4.2 规范化	7
4.3 学习资料	7


1引言
1.1编写目的
本文档是基于springboot+springcloud实现的微服务架构改造，提供对研发人员的技术架构支持。版本0.0.1主要实现了四个模块的功能：服务生产者，服务消费者，服务注册中心，客户端负载均衡。后续还会根据项目进展进行持续优化和扩展。
1.2背景
 	对现有项目进行服务化改造实践，提高团队技术储备。
1.3定义
服务生产者：独立的应用，负责提供微服务
服务消费者：独立的应用，负责消费微服务
服务注册中心：独立的应用，接受服务生产者注册进来的微服务，同时对服务消费者提供服务发现机制，服务消费者可以通过服务注册中心获取到可用服务并消费
客户端负载均衡：服务消费者通过ribbon+restTemplate调用多个服务名称相同的微服务，从而实现负载均衡
2. 模块介绍
2.1服务生产者
	示例中提供了两个服务提供者应用，分别是cloud-provider-001和cloud-provider-002，目的是通过ribbon组件实现负载均衡。该应用实现了一个完整的用户服务（数据来自真实的数据库，数据库配置信息位于application.yml中），阅读代码即可理解。
	负责模块的同学可以依此为模板自建应用实现更多微服务。
代码结构截图如下：
 
![](https://github.com/iamzken/spring-cloud-micro-services/blob/master/image/1.png)

入口main类截图如下：
![](https://github.com/iamzken/spring-cloud-micro-services/blob/master/image/2.png) 

运行该main类即可启动应用

应用所使用的配置文件截图如下：
 ![](https://github.com/iamzken/spring-cloud-micro-services/blob/master/image/3.png)
应用启动时会去classpath根目录下寻找名为application.yml（端口等重要信息）的配置文件，配置文件中的配置项active: dev代表实际选择的配置文件为application-dev.yml，从截图中可以看出配置文件一共分为开发、测试和生产三个，开发这可以根据实际情况选择对应的配置文件（通过修改applicatiton.yml中的active即可）
2.2服务消费者
	cloud-ribbon-consumer为服务消费者应用，通过ribbon+restTemplate实现客户端负载均衡，具体看代码可以更清楚。代码结构截图如下：
 ![](https://github.com/iamzken/spring-cloud-micro-services/blob/master/image/4.png)

入口main类截图如下：
 ![](https://github.com/iamzken/spring-cloud-micro-services/blob/master/image/5.png)
运行该main类即可启动应用
配置文件的介绍可参考服务生产者模块的介绍，都是相似的
2.3服务注册中心
	cloud-eureka-server为服务注册中心，通过springcloud的组件eureka实现
代码结构截图如下：
	 ![](https://github.com/iamzken/spring-cloud-micro-services/blob/master/image/6.png)
入口main类截图如下：
 ![](https://github.com/iamzken/spring-cloud-micro-services/blob/master/image/7.png)
配置文件介绍可参考服务生产者模块
2.4架构图
 ![](https://github.com/iamzken/spring-cloud-micro-services/blob/master/image/8.png)
3. 应用访问
3.1服务注册中心
	依次启动上述应用后，访问
http://localhost:8761/
这是服务注册中心，通过服务注册中心可以看到目前注册的微服务的名称、地址和状态等基本信息，截图如下：
 ![](https://github.com/iamzken/spring-cloud-micro-services/blob/master/image/9.png)
3.2服务消费
访问http://localhost:9093/queryUserById/1，即可调用微服务，效果截图：
 ![](https://github.com/iamzken/spring-cloud-micro-services/blob/master/image/10.png)
4. 待优化的内容
4.1 springcloud组件
根据业务复杂度及技术人员掌握情况，后续要整合更多的springcloud组件

4.2 规范化
现在的应用都是用maven管理的，后续要慢慢改造，作为微服务的全新实践，项目还是要先run起来最要紧

4.3 学习资料
springcloud中文社区: http://bbs.springcloud.com.cn/



