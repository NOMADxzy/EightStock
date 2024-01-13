## Java Web

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/3c0ca44d0f1a48e2b71800fcada5cba7.png)

##### 技术栈

数据库（MySQL、JDBC、Maven、MyBatis）、前端（HTML+CSS+JavaScript、Ajax+Vue+Element）、核心技术（Tomcat+HTTP+Servlet、Request+Response、JSP、Cookie+Session、Filter+Listener）

### 一、Java连接数据库——JDBC & Mybatis

#### JDBC

##### 使用步骤

① 创建工程，导入驱动jar包（mysql-connector-java-5.1.48.jar）
② 注册驱动。Class.forName("com.mysql.jdbc.Driver");
③ 获取连接。Connection conn = DriverManager.getConnection(url, username, password);
④ 定义SQL语句。String sql = “update...”;
⑤ 获取执行SQL对象。Statement stmt = conn.createStatement();
⑥ 执行SQL。stmt.executeUpdate(sql);
⑦ 处理返回结果。
⑧ 释放资源。

##### 数据库连接池

数据库连接池是个容器，负责分配、管理数据库连接（Connection）它允许应用程序 **重复使用一个现有的数据库连接，而不是再重新建立一个** ；

```
DataSource、DBCP、C3PO、Drulid
Connection getConnection()
```

##### JDBC的缺点

```markdown
    （1）数据库连接创建、释放频繁会造成系统资源浪费，从而影响系统性能。
    （2）SQL语句在代码中硬编码，造成代码不易维护。在实际应用的开发中，SQL变化的可能性较大。在传统JDBC编程中，SQL变动需要改变Java代码，违反了开闭原则。
    （3）用PreparedStatement向占位符传参数存在硬编码，因为SQL语句的where条件不一定，可能多也可能少，修改SQL需要修改代码，造成系统不易维护。
    （4）JDBC对结果集解析存在硬编码（查询列名），SQL变化导致解析代码变化，造成系统不易维护。
```

#### Mybatis

mybatis 是一个优秀的基于 java 的持久层框架，使开发者只需要关注 sql 语句本身，而不需要花费精力去处理加载驱动、创建连接、创建 statement 等繁杂的过程。

mybatis 通过 xml或注解的方式将要执行的各种 statement 配置起来，并通过 java 对象和 statement 中  sql的动态参数进行映射生成最终执行的 sql 语句，最后由 mybatis 框架执行 sql 并将结果映射为 java 对象并返回。采用  ORM 思想解决了实体和数据库映射的问题，对 jdbc 进行了封装，屏蔽了 jdbc api 底层访问细节，使我们不用与 jdbc api 打交道，就可以完成对数据库的持久化操作。

优点：

- 1、SQL 写在XML 里，解除 sql 与程序代码的耦合，便于统一管理；提供 XML 标签， 支持编写动态 SQL 语句， 并可重用。
- 2、与 JDBC 相比，减少了 50%以上的代码量，消除了 JDBC 大量冗余的代码，不需要手动开关连接。

缺点：

- SQL 语句依赖于数据库， 导致数据库移植性差， 不能随意更换数据库。

[实例](https://blog.csdn.net/qq_52077925/article/details/126917667)

```xml
<mapper namespace="com.southwind.repository.AccountRepository">
    <insert id="save" parameterType="com.southwind.entity.Account">
        insert into t_account(username,password,age) values(#{username},#{password},#{age})
    </insert>
    <update id="update" parameterType="com.southwind.entity.Account">
        update t_account set username = #{username},password=#{password},age=#{age}
    </update>
    <delete id="deleteById" parameterType="long">
        delete from t_account where id = #{id}
    </delete>
    <select id="findAll" resultType="com.southwind.entity.Account">
        select * from t_account
    </select>
    <select id="findById" parameterType="long" resultType="com.southwind.entity.Account">
        select * from t_account where id = #{id}
    </select>
</mapper>

```



### 二、Web服务器——Tomcat

Tomcat 是Web应用服务器,是一个Servlet/JSP容器. Tomcat 作为Servlet容器,负责处理客户请求,把请求传送给Servlet,并将Servlet的响应传送回给客户.而Servlet是一种运行在支持Java语言的服务器上的组件. Servlet最常见的用途是扩展Java Web服务器功能。

![123123](https://gitee.com/xu_zuyun/picgo/raw/master/img/SouthEast.png)

①Tomcat 通过 Socket 读取到这个请求(一个字符串), 并按照 HTTP 请求的格式来解析这个请求, 根据请求中的 Context Path 确定一个 webapp, 再通过 Servlet Path 确定一个具体的 类. 再根据当前请求的方法 (GET/POST/…), 决定调用这个类的 doGet 或者 doPost 等方法。

②表示Servlet容器把响应对象ServletResponse中的处理结果转发给Web服务器，通知Web服务器以HTTP响应的方式把结果发送到客户端，同时把控制返回Web服务器。

```java
class Tomcat{
    // 用来存储所有的 Servlet 对象
    private List<Servlet> instanceList = new ArrayList<>();
    public void start() {
    // 根据约定，读取 WEB-INF/web.xml 配置文件;
    // 并解析被 @WebServlet 注解修饰的类
    // 假定这个数组里就包含了我们解析到的所有被 @WebServlet 注解修饰的类.
        Class<Servlet>[] allServletClasses = ...;
    // 这里要做的的是实例化出所有的 Servlet 对象出来;
        for (Class<Servlet> cls : allServletClasses) {
    // 这里是利用 java 中的反射特性做的
    // 实际上还得涉及一个类的加载问题，因为我们的类字节码文件，是按照约定的
    // 方式（全部在 WEB-INF/classes 文件夹下）存放的，所以 tomcat 内部是
    // 实现了一个自定义的类加载器(ClassLoader)用来负责这部分工作。
            Servlet ins = cls.newInstance();
            instanceList.add(ins);
        }
    // 调用每个 Servlet 对象的 init() 方法，这个方法在对象的生命中只会被调用这一次;
        for (Servlet ins : instanceList) {
            ins.init();
        }
// 利用我们之前学过的知识，启动一个 HTTP 服务器
// 并用线程池的方式分别处理每一个 Request
        ServerSocket serverSocket = new ServerSocket(8080);
// 实际上 tomcat 不是用的固定线程池，这里只是为了说明情况
        ExecuteService pool = Executors.newFixedThreadPool(100);
        while (true) {
            Socket socket = ServerSocket.accept();
// 每个请求都是用一个线程独立支持，这里体现了我们 Servlet 是运行在多线程环境下的
            pool.execute(new Runnable() {
                doHttpRequest(socket);
            });
        }
// 调用每个 Servlet 对象的 destroy() 方法，这个方法在对象的生命中只会被调用这一次;
        for (Servlet ins : instanceList) {
            ins.destroy();
        }
    }
    public static void main(String[] args) {
        new Tomcat().start();
    }
}

```



### 三、动态web 资源开发技术——Servlet

Servlet（小服务程序）是一个与协议无关的、跨平台的Web组件，由Servlet容器所管理。运行在服务器端，可以动态地扩展服务器的功能，并采用“请求一响应”模式提供Web服务。 Servlet的主要功能是交互式地浏览和修改数据，生成动态Web内容。 Servlet是按照Servlet规范编写的Java类。

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/2018120522281643.gif) 

#### Servlet生命周期

    Servlet运行在Servlet容器（web服务器）中，其生命周期由容器来管理，分为4个阶段：
    ① 加载和实例化：默认情况下，当servlet第一次被访问时，由容器创建servlet对象。
    ② 初始化：在Servlet实例化之后，容器将调用servlet的init()方法初始化这个对象，完成一些如加载配置文件、创建连接等初始化的工作。该方法只调用一次。
    ③ 请求处理：每次请求servlet时，Servlet容器都会调用Servlet的service()方法对请求进行处理。
    ④ 服务终止：当需要释放内存或者容器关闭时，容器就会调用Servlet实例的destroy()方法完成资源的释放。在destroy()方法调用之后，容器会释放这个Servlet实例，该实例随后会被Java的垃圾收集器所回收。
#### Servlet常用方法

    初始化方法，在Servlet被创建时执行，只执行一次。void init(ServletConfig config)
    提供服务方法，每次Servlet被访问，都会调用该方法。void service(ServletRequest req, ServletResponse res)
    销毁方法，当Servlet被销毁时，调用该方法。在内存释放或服务器关闭时销毁Servlet。void destroy()
    获取ServletConfig对象ServletConfig getServletConfig()
    获取Servlet信息String getServletInfo()
### 四、动态的网页技术——JSP

![6a0a97e1074252c7fd736993c32c9a1.png](https://gitee.com/xu_zuyun/picgo/raw/master/img/1559286962205314-20240104221848985.png)

**概念：**Java Server Pages，Java服务端页面。
一种动态的网页技术，其中既可以定义HTML、JS、CSS等静态内容，还可以定义Java代码的动态内容。
JSP = HTML + Java，JSP本质上就是一个Servlet 。
**JSP的作用：**简化开发，避免了在Servlet中直接输出HTML标签。

**原理**：JSP本质上就是一个Servlet 。JSP在被访问时，由JSP容器（Tomcat）将其转换为Java文件（Servlet），在由JSP容器（Tomcat）将其编译，最终对外提供服务的其实就是这个 字节码文件 。

**缺点：**JSON 只支持 get，因为 script 标签只能使用 get 请求； JSONP 需要后端配合返回指定格式的数据。

[实例](https://www.cnblogs.com/xiaotang5051729/p/9916895.html)

```html
//test.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" import="java.util.*"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
<%
    Date date=new Date();
    out.write(date.toLocaleString());
%>
</body>
</html>
```

### 五、软件架构模式——MVC

MVC是一种 分层开发的模式 ，其中：M-Model，业务模型，处理业务；V：View，视图，界面展示；C：Controller，控制器，处理请求，调用模型和视图。

MVC好处：职责单一，互不影响；有利于分工协作；有利于组件重用。

![image-20240104214404965](https://gitee.com/xu_zuyun/picgo/raw/master/img/image-20240104214404965.png)

### 六、过滤器——Filter

- 过滤器可以把对资源的请求 拦截 下来，从而实现一些特殊的功能。
- 过滤器一般完成一些 通用的操作 ，比如：权限控制、统一编码处理、敏感字符处理等等。

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/af4106a69e064e06ae323842eb33aa41.png)

```markdown
@WebFilter(" /*")
public class FilterDemo implements Filter

    拦截具体的资源：/index.jsp，只有访问index.jsp时才会被拦截。
    目录拦截：/user/*，访问/user下的所有资源，都会被拦截。
    后缀名拦截：*.jsp，访问后缀名为jsp的资源，都会被拦截。
    拦截所有：/*，访问所有资源，都会被拦截。

## PS 
一个Web应用，可以配置多个过滤器，这多个过滤器称为 过滤器链 
```



### 七、监听器——Listener

- 监听器可以监听就是在application、session、request三个对象创建、销毁或者往其中添加修改删除属性时自动执行代码的功能组件。

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/4ea4e9ddd64244a9936b1d891b97005a.png)

### 八、异步Javascript和XML——AJAX

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/65f33acac29444ff8a984b939bc61fb9.png)

① 与服务器进行数据交换：通过AJAX可以给服务器发送请求，并获取服务器响应的数据。（使用了AJAX和服务器进行通信，就可以使用HTML+AJAX来替换JSP页面了）
② 异步交互：可以在 不重新加载整个页面 的情况下，与服务器交换数据并 更新部分网页 的技术，如：搜索联想、用户名是否可用校验等。

### 九、网络应用程序框架——Netty

Netty是一款卓越的java框架，提供异步的、事件驱动的网络应用程序框架和工具，用以快速开发高性能、高可靠性的网络服务器和客户端程序。它用较简单的抽象，隐藏 Java网络编程底层实现的复杂性。

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/v2-87775bf22ab7b07c63ea7edbf607a67a_1440w.webp)

**Core 核心层**
Core 核心层是 Netty 最精华的内容，它提供了底层网络通信的通用抽象和实现，包括可扩展的事件模型、通用的通信 API、支持零拷贝的 ByteBuf 等。

**Protocol Support 协议支持层**
协议支持层基本上覆盖了主流协议的编解码实现，如 HTTP、SSL、Protobuf、压缩、大文件传输、WebSocket、文本、二进制等主流协议，此外 Netty  还支持自定义应用层协议。Netty 丰富的协议支持降低了用户的开发成本，基于 Netty 我们可以快速开发 HTTP、WebSocket  等服务。

**Transport Service 传输服务层**
传输服务层提供了网络传输能力的定义和实现方法。它支持 Socket、HTTP 隧道、虚拟机管道等传输方式。Netty 对 TCP、UDP 等数据传输做了抽象和封装，用户可以更聚焦在业务逻辑实现上，而不必关系底层数据传输的细节。

#### Netty事件循环

EventLoop （事件循环对象）本质是一个单线程执行器（同时维护了一个 Selector），里面有 run 方法处理 Channel 上源源不断的 io 事件。事件循环组EventLoopGroup 是一组 EventLoop，Channel 一般会调用 EventLoopGroup 的 register 方法来绑定其中一个 EventLoop，后续这个 Channel 上的 io 事件都由此 EventLoop 来处理（保证了 io 事件处理时的线程安全）

```java
public class Test {
    public static void main(String[] args) {
        DefaultEventLoopGroup group = new DefaultEventLoopGroup(2);
        System.out.println(group.next());
        System.out.println(group.next());
        System.out.println(group.next());
    }
}
```

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/3a509ba61d2a4acf96a2f2f0499d48ac.png)

#### Netty处理任务

- 普通任务：通过 NioEventLoop 的 execute() 方法向任务队列 taskQueue 中添加任务。例如 Netty 在写数据时会封装 WriteAndFlushTask 提交给 taskQueue。taskQueue 的实现类是多生产者单消费者队列 MpscChunkedArrayQueue，在多线程并发添加任务时，可以保证线程安全。

- 定时任务：通过调用 NioEventLoop 的 schedule() 方法向定时任务队列 scheduledTaskQueue 添加一个定时任务，用于周期性执行该任务。例如，心跳消息发送等。定时任务队列 scheduledTaskQueue 采用优先队列 PriorityQueue 实现。

- 尾部队列：tailTasks 相比于普通任务队列优先级较低，在每次执行完 taskQueue 中任务后会去获取尾部队列中任务执行。尾部任务并不常用，主要用于做一些收尾工作，例如统计事件循环的执行时间、监控信息上报等。

```java
public class Test { //普通任务
    public static void main(String[] args) {
        NioEventLoopGroup nioEventLoopGroup = new NioEventLoopGroup(2);
        nioEventLoopGroup.execute(()->{
            System.out.println(Thread.currentThread());
            System.out.println("test...");
        });
    }
}
public class Test { //定时任务
    public static void main(String[] args) {
        NioEventLoopGroup nioEventLoopGroup = new NioEventLoopGroup(2);
        nioEventLoopGroup.scheduleAtFixedRate(()->{
            System.out.println("test...");
        },0,1, TimeUnit.SECONDS);
    }
}
```



### 十、消息队列服务器——RabbitMQ

RabbitMQ是使用Erlang语言开发的开源消息队列系统，基于AMQP协议来实现。AMQP的主要特征是面向消息、队列、路由(包括点对点和发布/订阅)、可靠性、 安全。AMQP协议更多用在企业系统内，对数据一致性、稳定性和可靠性要求很高的场景，对性能和吞吐量的要求还在其次。

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/a6efce1b9d16fdfa804ae061a962bc5895ee7b17.jpeg)

1. Channel：网络信道，读写都是在Channel中进行（NIO的概念），包括对MQ进行的一些操作（例如clear queue等）都是在Channel中进行，客户端可建立多个Channel，每个Channel代表一个会话任务
2. Message：由properties（有消息优先级、延迟等特性）和Body（消息内容）组成
3. Exchange：Routing and Filter7、Binding：把Exchange和Queue进行Binding8、Routing key：路由规则
4. Binding：把Exchange和Queue进行Binding8、Routing key：路由规则

- 优点：
  1. 解除业务系统之间的耦合，降低系统之间的依赖关系
  2. 实现消息的异步处理，无需同步等待
  3. 削峰填谷，将流量从高峰期引到低谷期进行处理
- 缺点：
  1. 增加了系统的复杂度，带入了幂等、重复消费、消息丢失的问题
  2. 系统的可用性比原先要低，MQ挂了以后，整个系统也会崩溃.
  3. 消费端数据一致性问题
- 使用场景
  - 消息订阅
  - 日志采集
  - 埋点
  - 订单

```
基于消息的模型，关心的是“通知”，而非“处理”.
在下单时库存系统不能正常使用。也不影响正常下单，因为下单后，订单系统写入消息队列就不再关心其他的后续操作了。实现订单系统与库存系统的应用解耦.
为了保证库存肯定有，可以将队列大小设置成库存数量，或者采用其他方式解决。
```



#### 四种交换机(Exchange)

- i. 直连交换机，Direct exchange：带路由功能的交换机，根据routing_key（消息发送的时候需要指定）直接绑定到队列，⼀个交换机也可以通过过个routing_key绑定多个队列。
- ii. 扇形交换机，Fanout exchange：⼴播消息。
- iii. 主题交换机，Topic exchange：发送到主题交换机上的消息需要携带指定规则的routing_key，主题交换机会根据这个规则将数据发送到对应的(多个)队列上。
- iv. ⾸部交换机，Headers exchange：⾸部交换机是忽略routing_key的⼀种路由⽅式。路由器和交换机路由的规则是通过Headers信息来交换的，这个有点像HTTP的Headers。将⼀个交换机声明成⾸部交换机，绑定⼀个队列的时候，定义⼀个Hash的数据结构，消息发送的时候，会携带⼀组hash数据结构的信息，当Hash的内容匹配上的时候，消息就会被写⼊队列。

##### 如何保证MQ中消息不会丢失？

生产者发生异常没有把消息成功发送给MQ，MQ成功接收到消息之后发生宕机了，消息未被成功，消费消费端要设置签收机制为手动签收，只有当消息最终被处理，才告诉MQ已经消费，此时MQ再去删除这条消息。

##### 如何解决消息堆积？

- 启动多个消费者微服务
- 优化消费者处理消息的能力，提高消费消息的性能
- 调整RabbitMQ的队列容量
- 设置过期时间，让一些不是那么重要的消息到达指定时间之后就丢弃
- 将无法被消费的消息放到死信队列（DLQ）中

##### 如何解决消息被重复消费？

- 生产者发送消息的时候带上一个全局唯一的id，消费者拿到消息后，先根据这个id去 Redis 里查一下，之前有没消费过，没有消费过就处理，并且写入这个id到 Redis，如果消费过了，则不处理。
- 基于数据库的唯一键

### 十一、分布式应用程序协调服务——ZooKeeper

**ZooKeeper** 是一个开源的**分布式协调框架**，它的定位是为分布式应用提供一致性服务，是整个大数据体系的**管理员**。**ZooKeeper** 会封装好复杂易出错的关键服务，将高效、稳定、易用的服务提供给用户使用。

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/v2-ec4d237ed60d914e5d1915c99778f5a7_1440w-20240111095157623.webp)

##### 特点

1. **集群**：Zookeeper是一个领导者（Leader），多个跟随者（Follower）组成的集群。
2. **高可用性**：集群中只要有半数以上节点存活，Zookeeper集群就能正常服务。
3. **全局数据一致**：每个Server保存一份相同的数据副本，Client无论连接到哪个Server，数据都是一致的。
4. **更新请求顺序进行**：来自同一个Client的更新请求按其发送顺序依次执行。
5. **数据更新原子性**：一次数据更新要么成功，要么失败。
6. **实时性**：在一定时间范围内，Client能读到最新数据。
7. 从`设计模式`角度来看，zk是一个基于**观察者设计模式**的框架，它负责管理跟存储大家都关心的数据，然后接受观察者的注册，数据反生变化zk会通知在zk上注册的观察者做出反应。
8. Zookeeper是一个分布式协调系统，满足CP性，跟**[SpringCloud](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s%3F__biz%3DMzI4NjI1OTI4Nw%3D%3D%26mid%3D2247487072%26idx%3D1%26sn%3D3010908c4b668edef8fbd1f320343354%26scene%3D21%23wechat_redirect)**中的Eureka满足AP不一样。

##### 监听通知流程

- 在main线程中创建Zookeeper客户端，这时就会创建两个线程，一个负责网络连接通信（connet），一个负责监听（listener）。
- 通过connect线程将注册的监听事件发送给Zookeeper。
- 在Zookeeper的注册监听器列表中将注册的监听事件添加到列表中。
- Zookeeper监听到有数据或路径变化，就会将这个消息发送给listener线程。
- listener线程内部调用了process()方法。

#### 应用场景

##### **1. 数据发布/订阅**

当某些数据由几个机器共享，且这些信息经常变化数据量还小的时候，这些数据就适合存储到ZK中。

- **数据存储**：将数据存储到 Zookeeper 上的一个数据节点。
- **数据获取**：应用在启动初始化节点从 Zookeeper 数据节点读取数据，并在该节点上注册一个数据变更 **Watcher**
- **数据变更**：当变更数据时会更新 Zookeeper 对应节点数据，Zookeeper会将数据变更**通知**发到各客户端，客户端接到通知后重新读取变更后的数据即可。

2. ##### 分布式锁

主要是避免了羊群效应，临时节点已经预先存在，所有想要获得锁的线程在它下面创建临时顺序编号目录节点，编号最小的获得锁，用完删除，后面的依次排队获取。

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/v2-bbfaf9843eea699c518089ae8c8453ab_1440w.webp)

##### 3. 负载均衡

- 多个服务注册
- 客户端获取中间件地址集合
- 从集合中随机选一个服务执行任务

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/v2-530532fe3cb598e079483a33b069c13f_1440w.webp)

4. ##### 命名服务

利用 zk 创建一个全局唯一的路径，这个路径就可以作为一个名字，指向集群中的集群，提供的服务的地址，或者一个远程的对象等等。

5. ##### Leader选举

1. 利用ZooKeeper的**强一致性**，能够保证在分布式高并发情况下节点创建的全局唯一性，即：同时有多个客户端请求创建 /Master 节点，最终一定只有一个客户端请求能够创建成功。利用这个特性，就能很轻易的在分布式环境中进行集群选举了。
2. 就是动态Master选举。这就要用到 **EPHEMERAL_SEQUENTIAL**类型节点的特性了，这样每个节点会`自动被编号`。允许所有请求都能够创建成功，但是得有个创建顺序，每次选取序列号**最小**的那个机器作为**Master** 。

```markdown
## ZooKeeper集群节点个数一定是奇数个
在节点数量是奇数个的情况下， zookeeper集群总能对外提供服务（即使损失了一部分节点）；如果节点数量是偶数个，会存在zookeeper集群不能用的可能性（脑裂成两个均等的子集群的时候）。
```

### 十二、反向代理服务器——Nginx

支持热部署，几乎可以做到 7 * 24  小时不间断运行，即使运行几个月也不需要重新启动，还能在不间断服务的情况下对软件版本进行热更新。性能是 Nginx  最重要的考量，其占用内存少、并发能力强、能支持高达 5w 个并发连接数，最重要的是， Nginx 是免费的并可以商业化，配置使用也比较简单。

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/format,png.png)

##### 使用场景

1. 静态资源服务，通过本地文件系统提供服务；
2. 反向代理服务，延伸出包括缓存、负载均衡等；
3. API 服务， OpenResty。

```markdown
## 正向和反向代理区别
正向代理需要你主动设置代理服务器ip或者域名进行访问，由设置的服务器ip或者域名去获取访问内容并返回；
反向代理不需要你做任何设置，直接访问服务器真实ip或者域名，但是服务器内部会自动根据访问内容进行跳转及内容返回，你不知道它最终访问的是哪些机器。
## 使用场景
正向：VPN
反向：保护和隐藏原始资源服务器、加密和SSL加速、负载均衡、缓存静态内容、压缩、减速上传、安全、外网发布
```

### 十三、远程过程调用——RPC

- 客户端（Client）：服务调用方（服务消费者）
- 客户端存根（Client Stub）：存放服务端地址信息，将客户端的请求参数数据信息打包成网络消息，再通过网络传输发送给服务端
- 服务端存根（Server Stub）：接收客户端发送过来的请求消息并进行解包，然后再调用本地服务进行处理
- 服务端（Server）：服务的真正提供者

![RPC框架的实现原理，及RPC架构组件详解](https://gitee.com/xu_zuyun/picgo/raw/master/img/6155e42e5583999845effc201bcebf5a.jpeg)

![RPC框架的实现原理，及RPC架构组件详解](https://gitee.com/xu_zuyun/picgo/raw/master/img/1cbf540858e78191e813acba421a9467.jpeg)
