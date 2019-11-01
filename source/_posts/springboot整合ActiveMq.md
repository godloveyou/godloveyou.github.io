---
title: springboot整合ActiveMq入门
categories: ActiveMq
tags: ActiveMq
---

![mark](http://blog.sjjtcloud.com/blog/20191021/FKqfRpofuTfa.png?imageslim)

### 1.下载activeMq并启动(版本apache-activemq-5.15.4解压运行)
启动成功后管理路径是http://127.0.0.1:8161 
用户名密码admin/admin


### 2.springboot项目中加入activeMq依赖
```xml
<!--引入ActiveMq依赖--> 
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-activemq</artifactId>
</dependency>

<!--  启用JMS 的池化, 就一定要加上这个 jar--> <dependency>
    <groupId>org.apache.activemq</groupId>
    <artifactId>activemq-pool</artifactId>
</dependency>
```

### 3. application.yml配置activeMq服务器的配置
```yml
# MQ所在的服务器的地址
spring.activemq.broker-url: tcp://127.0.0.1:61616
# 是否使用内置的MQ， true  使用； fale  不使用
spring.activemq.in-memory: false 
# 是否在回滚回滚消息之前停止消息传递。这意味着当启用此命令时，消息顺序不会被保留。
spring.activemq.non-blocking-redelivery: false
# 用户名
spring.activemq.password: admin
# 密码
spring.activemq.user: admin
```

具体的配置信息解析
```
spring.activemq.broker-url=tcp://127.0.0.1:61616
# 在考虑结束之前等待的时间
#spring.activemq.close-timeout=15s 
# 默认代理URL是否应该在内存中。如果指定了显式代理，则忽略此值。
spring.activemq.in-memory=true 
# 是否在回滚回滚消息之前停止消息传递。这意味着当启用此命令时，消息顺序不会被保留。
spring.activemq.non-blocking-redelivery=false
# 密码
spring.activemq.password=admin
# 等待消息发送响应的时间。设置为0等待永远。
spring.activemq.user=admin
# 是否信任所有包
#spring.activemq.packages.trust-all=
# 要信任的特定包的逗号分隔列表（当不信任所有包时）
#spring.activemq.packages.trusted=
# 当连接请求和池满时是否阻塞。设置false会抛“JMSException异常”。
#spring.activemq.pool.block-if-full=true
# 如果池仍然满，则在抛出异常前阻塞时间。
#spring.activemq.pool.block-if-full-timeout=-1ms
# 是否在启动时创建连接。可以在启动时用于加热池。
#spring.activemq.pool.create-connection-on-startup=true
# 是否用Pooledconnectionfactory代替普通的ConnectionFactory。
#spring.activemq.pool.enabled=false 
# 连接过期超时。
#spring.activemq.pool.expiry-timeout=0ms
# 连接空闲超时
#spring.activemq.pool.idle-timeout=30s
# 连接池最大连接数
#spring.activemq.pool.max-connections=1
# 每个连接的有效会话的最大数目。
#spring.activemq.pool.maximum-active-session-per-connection=500
# 当有"JMSException"时尝试重新连接
#spring.activemq.pool.reconnect-on-exception=true
# 在空闲连接清除线程之间运行的时间。当为负数时，没有空闲连接驱逐线程运行。
#spring.activemq.pool.time-between-expiration-check=-1ms
# 是否只使用一个MessageProducer
#spring.activemq.pool.use-anonymous-producers=true
```

### 4.创建ActiveMqConfig
```java
@Configuration 
public class ActivemqConfig {
   @Bean(name = "topic-myqueue")
   public Queue queue() {
      return new ActiveMQQueue("yw-advice-worker");
   }

   @Bean(name = "topic-company")
   public Topic topicCompany() {
      return new ActiveMQTopic("topic-company");
   }

   @Bean(name = "topic-worker")
   public Topic topicWorker() {
      return new ActiveMQTopic("topic-company");
   }

  // topic模式的ListenerContainer 
  @Bean 
  public JmsListenerContainerFactory<?> jmsListenerContainerTopic(ConnectionFactory  activeMQConnectionFactory) {
     DefaultJmsListenerContainerFactory bean = new DefaultJmsListenerContainerFactory();
     bean.setPubSubDomain(true);
     bean.setConnectionFactory(activeMQConnectionFactory);
     return bean;
  }

// queue模式的ListenerContainer 
  @Bean 
  public JmsListenerContainerFactory<?> jmsListenerContainerQueue(
    ConnectionFactory activeMQConnectionFactory) {
     DefaultJmsListenerContainerFactory bean = new DefaultJmsListenerContainerFactory();
     bean.setConnectionFactory(activeMQConnectionFactory);
     return bean;
  }
}
```

### 5.编写生产者测试
```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = ApiApplication.class)
public class ActiveMqProducterTest {

  @Autowired
  private JmsMessagingTemplate jmsTemplate;

  @Resource(name = "topic-company")
  private Topic adviceCompany;

  @Resource(name = "queue-yw")
  private Queue queueAdvice;

  @Test
  public void testProducter(){
       jmsTemplate.convertAndSend(adviceCompany, "您有新的外卖订单,已为您自动接单");
  }
  @Test 
  public void testSendQueue(){
  jmsTemplate.convertAndSend(queueAdvice,"你好,我来自队列");
 }

}
```

### 6.编写消费者测试
```java
@Component 
public class ActiveMqConsumer {
    private final static Logger logger = LoggerFactory.getLogger(ActiveMqConsumer.class);

    @JmsListener(destination = "topic-company",containerFactory = "jmsListenerContainerTopic")
    public void receiveTopic(String msg) {
        logger.info("接收到topic消息：{}",msg);
    }

    @JmsListener(destination = "queue-yw",containerFactory = "jmsListenerContainerQueue")
    public void receiveQueue(String msg) {
        logger.info("接收到queue-yw消息：{}",msg);
    }
}
```


参考资料
* (https://blog.csdn.net/qq_43652509/article/details/83926758)
* (https://blog.csdn.net/cs_hnu_scw/article/details/81040834)



