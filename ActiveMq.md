# ActiveMQ 
```
基于Java开发的异步消息队列中间件(JMS)，具有解耦、削峰和异步等特性。具有高可用、可持久化、延时投递、签收机制等特点。
其通过ConnectionFactory创建Connection，进而创建Session，通过Session创建消费者、生产者和消息，而后消费者
和生产者通过Desination投递消息的方式完成工作。
同时支持同步阻塞recieve(timeout) 和异步监听的方式 MessageListener 消费消息。
JavaEE: 是一套使用Java进行企业级开发应用的大家一致遵守的13个核心规范的标准，这些组件可快速设计、开发、装配及部署企业级应用
JDBC | JNDI | EJB | RMI | Java IDL | JSP | Servlet | XML | JMS | JTA | JTS | JMAIL | JAF 

注：Active存在消息丢失、重复消费等问题，其性能尚可，但入手快，支持主从且文档丰富。
```

---
## Destination
### Topic
```
发布/定阅模式，无状态且非持久消息会在无订阅者时丢弃，订阅者较多时会显著影响Broker性能
```
### QUEUE
```
负载均衡方式消费，文件存储且消息不会被丢弃，多个监听站不会影响broker性能。
```
---
## JMS消息
### 消息头
- Destination: 消息目的地，一般是Queue或者Topic
- Delivery: 投递方式，持久或非持久，非持久消息会丢失，持久消息有且仅有一次消费，不会丢失
- Expiration: 过期时间，默认为0，即永不过期
- Priority: 优先级，默认为4[0-4 普通，5-9 紧急]，仅保证紧急消息优先于普通消息，不保证顺序
- MessageID: 消息唯一标识，默认由MQ产生，可保证幂等性
### 消息体：封装具体的消息数据
- TextMessage: 文本消息，即普通字符串消息
- MapMessage: Map类型消息，key为String，value为基本类型或者String
- BytesMessage: 二进制消息方式，包含一个byte[]
- StreamMessage: Java数据流信息，用标准流来顺序的填充和读取
- ObjectMessage: 对象消息，可包含一个可序列化的Java对象
### 消息属性
```
设置一些其他的消息，包含识别、去重、重点标注等内容，使用key：value方式设置。
```
---
## 支持的协议
### 支持的协议一览
```
支持的协议非常广泛：
wire protocl: amq称为 OpenWrite协议，主要使用TCP长连接方式，也是amq默认的连接方式。高效且快速，稳定、高效广泛支持。
NIO：类似TCP，但是比tcp更基于底层的访问操作，允许对有更多连接的应用增加服务器负载，支持此项需要手动增加对应的连接
AMQP：统一消息服务的应用层标准高级消息队列协议，开放标准的面向消息的中间件协议设计。
STOMP：流文本定向消息协议，面向消息的MOM简单文本协议
MQTT：消息队列遥测传输协议，IBM开发的通信协议，对物联网更有吸引力。
```

### 自动侦测
```
1. OpenWire, STOMP, AMQP, and MQTT 可以被自动侦测并使用，一般配置NIO使用
2. NIO的配置 <transportConnector name="nio" uri="nio://localhost:61618"/>
3. 自动侦测配置<transportConnector name="nio+auto" uri="auto://localhost:61618"/>
  此时，如果使用tcp://localhost:61618 就可以使用nio+tcp模式，也可以使用nio+mqtt等
```

---
## 消息的可靠性
### 持久化
- 持久消息：保证消息一次仅仅被消费一次（Queue默认为持久消息）
- 非持久消息：故障时会被丢失（Topic默认是非持久消息）
- Topic持久化是需要设置ClientId，同时，要使用TopicSubscribers发布消息
### 事务
```
事务偏生产者，签收偏消费者。如果开启了事务，则必须手动commit。未开启事务时，默认自动提交ack应答。
```
### 签收机制
```
1. 事务大于签收，即如果开启了事务，那么事务提交即认为ack签收了，ack签收但事务未提交，则认为未签收
2. 非事务默认为自动签收，也可以手动提交，此时需要显式调用ack进行签收
```
### 可持久化
```
1. AMQ : 基于文件的存储方式，5.3后被删除

2. kahaDB：基于日志文件方式的持久化，为默认的持久化方式。可用于任何场景，性能优秀且支持故障恢复。使用了一个
   事务日志和一个索引文件来存储所有消息的地址，消息使用追加的方式，放入日志中，不需要时会被删除或归档（丢弃）
   - db-<number>.log: 存储消息的日志，可制定大小，满后递增创建新文件存放
   - db.data: 包含了持久化的BTree索引，索引指向消息，即执行db-n.log
   - db.free: 存放的时那些文件是空闲的，内容是空闲文件的的ID
   - db.redo: 用来进行消息回复，在强制退出后启动，用来恢复BTree索引
   - lock： 文件锁，表示当前可以获取kahaDB读写权限的broker
   * 4文件一把锁
   -> 配置（默认是这样的  journalMaxFileLength 指定单个文件的最大值）
    <persistenceAdapter>
      <kahaDB directory="activemq-data" journalMaxFileLength="32mb"/>
    </persistenceAdapter>
   -> 集群配置
   conf/ha.xml 下配置，需要zk支持，配置在该文件，启动activemq是，加上该bean配置：
   >>  $ACTIVEMQ_HOME/bin/activemq xbean:ha.xml
   
3. jdbc: 使用独立的数据库进行数据的持久化，但性能较差，如果需要执行以下步骤：
   a. 将jdbc驱动包放到activemq/lib目录下
   b. 配置持久化方式为<jdbcPersistenceAdapter datasource="#datasource">
   c. 在 </broker> 和 <import/> 之间，配置datasource数据源
   * 可使用dbcp2的对应的数据库连接池，需要三方的，则需要主动增加对应的jar包
   * createTableOnStartup="true" 可在启动时自动创建所需要的表结构，可以设置为false
   * 生成三张表，activemq_msgs[存放queue]、activemq_acks[存放topic]和activemq_lock，用于存放集群的master
   * 如果出现BeanFactory not initialized or already closed，可能主机名含"_"，重命名重启即可
   ->  配置jdbc   不使用journal
     <persistenceAdapter>
        <journaledJDBC datasource='#mysql-ds"/>
      </persistenceAdapter>
    -> 配置数据源
    <bean id="mysql-ds" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
      <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
      <property name="url" value="jdbc:mysql://localhost/activemq?relaxAutoCommit=true"/>
      <property name="username" value="activemq"/>
      <property name="password" value="activemq"/>
      <property name="poolPreparedStatements" value="true"/>
    </bean>
    ->  暂时不关心此时需要使用数据库的集群
   
4. levelDB：类似kahaDB，但是使用levelDB索引而非BTree索引，较kahaDB性能更好。
   -> 配置方式:
    <persistenceAdapter>
      <levelDB directory="activemq-data"/>
    </persistenceAdapter>
   -> 集群配置方式：
     <persistenceAdapter>
        <replicatedLevelDB
          directory="activemq-data"
          replicas="3"
          bind="tcp://0.0.0.0:0"
          zkAddress="zoo1.example.org:2181,zoo2.example.org:2181,zoo3.example.org:2181"
          zkPassword="password"
          zkPath="/activemq/leveldb-stores"
          hostname="broker1.example.org"
          />
      </persistenceAdapter>

5. jdbc journal： 配合jdbc使用，使用高速缓存写入技术，提升了性能。消息产生后，先放入journal文件，如果此时被消息，
   则无需重复读库写库。如果消费过慢，则journal中未消费内容会批量入库。
   ->  配置（去除jdbc的 persistenceAdapter 对应标签的配置）
   <persistenceFactory>
　　　　<journalPersistenceAdapterFactory journalLogFiles="4" journalLogFileSize="32768"
　　　　　　useJournal="true" useQuickJournal="true" dataSource="#mysql_ds" dataDirectory="activemq-data">
　　</persistenceFactory>
  
注： 对于集群部署的activemq，需要使用failover Transport方式进行访问，如:
failover:(tcp://localhost:61616,tcp://remotehost:61616)?initialReconnectDelay=100
```
---
## 集群
- zk + levelDB: 可复制的levelDB集群方式
- shared File System: 共享文件系统，如SAN
- jdbc master slave : 数据库集群方式
---
## 异步投递
```
- 使用useAsyncSend=true来配置，tcp://locahost:61616?jms.useAsyncSend=true | 或AMQFactory或AMQConnection设置该属性
- 同步投递：强制同步或者在没有使用 事务的情况下，生产者以PERSISTENT 传送模式发送消息都是同步
- 异步投递：默认异步投递，但不不能保证消息一定发送成功。为了保证发送成功，一般使用ActvieMQMessageProducer对象设置MessageId,
  而后使用带有回调函数的方法发送消息，回调函数有onSuccess和onException方法以便于确认是否发送成功。
```
---
## 延迟投递和定时投递
### 功能开启
- 在conf/activemq.xml 中，在 <broker> 标签中增加开启标志，<broker ... scheduledSupport="true">
### 延时投递
```
  AMQ_SCHEDULED_DELAY       long  定义延迟时间
  AMQ_SCHEDULED_PERIOD      long  定义延时周期
  AMQ_SCHEDULED_REPEAT      int   定义重复次数
  AMQ_SCHEDULED_CRON        string 定义corn表达式
  -- 延时投递
  MessageProducer producer = session.createProducer(destination);
  TextMessage message = session.createTextMessage("test msg");
  long delay = 30 * 1000;
  long period = 10 * 1000;
  int repeat = 9;
  message.setLongProperty(ScheduledMessage.AMQ_SCHEDULED_DELAY, delay);
  message.setLongProperty(ScheduledMessage.AMQ_SCHEDULED_PERIOD, period);
  message.setIntProperty(ScheduledMessage.AMQ_SCHEDULED_REPEAT, repeat);
  producer.send(message);

  -- cron表达式方式
  MessageProducer producer = session.createProducer(destination);
  TextMessage message = session.createTextMessage("test msg");
  message.setStringProperty(ScheduledMessage.AMQ_SCHEDULED_CRON, "0 * * * *");
  producer.send(message);

  -- 混合模式
  MessageProducer producer = session.createProducer(destination);
  TextMessage message = session.createTextMessage("test msg");
  message.setStringProperty(ScheduledMessage.AMQ_SCHEDULED_CRON, "0 * * * *");
  message.setLongProperty(ScheduledMessage.AMQ_SCHEDULED_DELAY, 1000);
  message.setLongProperty(ScheduledMessage.AMQ_SCHEDULED_PERIOD, 1000);
  message.setIntProperty(ScheduledMessage.AMQ_SCHEDULED_REPEAT, 9);
  producer.send(message);
```

---

## 重试机制
### 重试的场景
- client使用了transaction且在session中调用了rollback方法
- client使用了transaction但没有执行commit或关闭前没有执行commit
- client在client_acknowledge模式下，session中调用了recover方法
### 重试间隔和次数(主要是重试次数和重试间隔)
- maximumRedeliveries： 最大重试次数默认为6，当重试次数超过该值，客户端会发送poison ack，服务端将该消息放入死信队列，DLQ
- initialRedeliveryDelay或redeliveryDelay： 重试时间间隔
### 死信队列 DLQ-Dead Letter Queue
```
- 一般有核心业务队列和死信队列组成总体消息队列
- sharedDeadLetterStrategy: 共享死信队列，所有的poison ack消息都放入其中
- individualDeadLetterStrategy: 独立死信队列，默认不区分queue和topic，单独指定
- 非持久信息，过期不会放入死信队列，如需要，使用：<sharedDeadLetterStrategy processNonPersistent="true" />表明
- 自动删除消息自动放入死信队列，不需要，使用：<sharedDeadLetterStrategy processExpired="false" />表明
  ->  配置单独的死信队列 
  <destinationPolicy>
    <policyMap>
      <policyEntries>
        <!-- Set the following policy on all queues using the '>' wildcard -->
        <policyEntry queue=">">
          <deadLetterStrategy>
            <individualDeadLetterStrategy queuePrefix="DLQ." useQueueForQueueMessages="true"/>
            <!-- <sharedDeadLetterStrategy processExpired="false" /> -->
          </deadLetterStrategy>
        </policyEntry>
      </policyEntries>
    </policyMap>
  </destinationPolicy>
```
---
## 幂等性消费
- 使用传统数据库，对消息的messageId做过滤的方式，进行主键冲突检测
- 使用redis，通过消息的唯一标识符确保消息不会重复消费

