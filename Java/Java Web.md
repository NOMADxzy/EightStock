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

> 1. 在 `pom.xml` 文件中加入 MyBatis 的 Maven 依赖：
>
>    ```java
>    <dependencies>
>        <dependency>
>            <groupId>org.mybatis</groupId>
>            <artifactId>mybatis</artifactId>
>            <version>3.5.6</version>
>        </dependency>
>    </dependencies>
>    
>    ```
>
> 2. 创建一个 `mybatis-config.xml` 配置文件，定义数据库连接信息、事务管理等：
>
>    ```java
>    <?xml version="1.0" encoding="UTF-8" ?>
>    <!DOCTYPE configuration
>            PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
>            "http://mybatis.org/dtd/mybatis-3-config.dtd">
>    <configuration>
>        <environments default="development">
>            <environment id="development">
>                <transactionManager type="JDBC"/>
>                <dataSource type="POOLED">
>                    <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
>                    <property name="url" value="jdbc:mysql://localhost:3306/yourdb"/>
>                    <property name="username" value="root"/>
>                    <property name="password" value="password"/>
>                </dataSource>
>            </environment>
>        </environments>
>        <mappers>
>            <mapper resource="org/mybatis/example/UserMapper.xml"/>
>        </mappers>
>    </configuration>
>    ```
>
> 3. 创建实体类
>
>    ```java
>    public class User {
>        private Integer id;
>        private String name;
>        private String email;
>    
>        // 省略构造方法、getter 和 setter 方法
>    }
>    ```
>
> 4. 编写 Mapper 接口
>
>    ```java
>    public interface UserMapper {
>        User selectUser(int id);
>    }
>    ```
>
> 5. 编写 Mapper XML文件
>
>    ```java
>    <?xml version="1.0" encoding="UTF-8"?>
>    <!DOCTYPE mapper
>            PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
>            "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
>    <mapper namespace="org.mybatis.example.UserMapper">
>        <select id="selectUser" resultType="User">
>          SELECT id, name, email FROM users WHERE id = #{id}
>        </select>
>    </mapper>
>    ```
>
> 6. 在应用程序中使用 MyBatis
>
>    ```java
>    import org.apache.ibatis.io.Resources;
>    import org.apache.ibatis.session.SqlSession;
>    import org.apache.ibatis.session.SqlSessionFactory;
>    import org.apache.ibatis.session.SqlSessionFactoryBuilder;
>                                                       
>    import java.io.Reader;
>                                                       
>    public class MyApp {
>        public static void main(String[] args) throws Exception {
>            Reader reader = Resources.getResourceAsReader("mybatis-config.xml");
>            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(reader);
>            SqlSession session = sqlSessionFactory.openSession();
>                                                       
>            try {
>                UserMapper mapper = session.getMapper(UserMapper.class);
>                User user = mapper.selectUser(1);
>                System.out.println(user.getName());
>            } finally {
>                session.close();
>            }
>        }
>    }
>    ```

##### 124.什么是mapper

定义了与数据库进行交互的方法，MyBatis 通过配置文件（XML）或注解来了解如何将这些接口方法映射到数据库操作上。当你调用一个 Mapper 接口的方法时，MyBatis 会为你动态生成该接口的实现代码，并且执行相应的 SQL 语句。

```java
public interface BlogMapper {
    Blog selectBlogById(int id);
    int insertBlog(Blog blog);
    int updateBlog(Blog blog);
    int deleteBlog(int id);
}
```

```java
// BlogMapper.xml中的内容
<mapper namespace="com.myapp.mapper.BlogMapper">
    
    <select id="selectBlogById" resultType="Blog">
        SELECT * FROM blog WHERE id = #{id}
    </select>
    
    <insert id="insertBlog" parameterType="Blog">
        INSERT INTO blog (title, content) VALUES (#{title}, #{content})
    </insert>

    <update id="updateBlog" parameterType="Blog">
        UPDATE blog SET title = #{title}, content = #{content} WHERE id = #{id}
    </update>
    
    <delete id="deleteBlog" parameterType="int">
        DELETE FROM blog WHERE id = #{id}
    </delete>
</mapper>
```

##### 125.mybatis 中 #{}和 ${}的区别是什么？

- 前者是预编译处理，MyBatis在处理#{}时，会将SQL语句中的#{}替换为?，即占位符，然后使用PreparedStatement的set方法来赋值。
- 后者是直接替换，MyBatis在处理${}时，会直接将SQL语句中的${}替换为参数的值。

```
由于${}是直接替换参数，不会给参数添加单引号，因此会导致SQL语句错误，如果非要使用${}，就需要手动对参数添加单引号，但这样又会带来SQL注入的问题（传入参数："' or 1 = '1"）
能使用#{}就使用#{}！如果非要使用${}，那么一定要进行参数校验。
```

##### 126.mybatis 有几种分页方式？

数据量大了的时候，一次性将所有数据查出来不现实，所以我们一般都是分页查询的，减轻服务端的压力，提升了速度和效率

1. Limit实现分页：mysql分页优化，灵活性高

2. RowBounds分页（不建议使用）：RowBounds中有2个字段offset和limit。这种方式获取所有的ResuitSet,从ResultSet中的offset位置开始获取limit个记录（不会查询所有结果），不指定参数的话默认查询所有行。（无论如何DB压力都大，因为结果暂存在db中了）

3. 利用数组Sublist分页

```java
@Override
    public List<Student> queryStudentsByArray(int currPage, int pageSize) {
        List<Student> students = studentMapper.queryStudentsByArray();
//        从第几条数据开始
        int firstIndex = (currPage - 1) * pageSize;
//        到第几条数据结束
        int lastIndex = currPage * pageSize;
        return students.subList(firstIndex, lastIndex);
    }
```

##### 4.使用MyBatis分页插件PageHelper

```markdown
## 1.拦截器机制
Mvbatis中的拦截器机制是指，在执行数据库操作时，可以通过自定义拦截器对SQL语句进行拦截和处理。Mvbatis提供了-个Interceptor接口，只要实现该接口并配置到mybatis-config.xml文件中即可使用
## 2.分页插件实现原理
在Mybatis中使用分页插件的原理如下:
(1)首先，在mybatis-config.xml文件中配置分页插件,
(2)当执行查询操作时，拦截器会对SQL语句进行拦截，并根据传入的参数自动将SQL语句进行改写，添加limit和offset关键字。
(3)最后将改写后的SQL语句交给Mybatis执行。
```

```markdown
## 物理分页和逻辑分页
物理分页就是数据库本身提供了分页方式，如MySQL的limit，好处是效率高，不好的地方就是不同数据库有不同的搞法。
逻辑分页利用游标分页，好处是所有数据库都统一，坏处就是效率低。使用 MyBatis 自带的 RowBounds 进行分页，它是一次性查询很多数据，然后在数据中再进行检索。
结论：物理分页总是优于逻辑分页，没有必要将属于数据库端的压力加诸到应用端来，就算速度上存在优势,然而其它性能上的优点足以弥补这个缺点
```

##### 129.mybatis 延迟加载的原理是什么？

在开发过程中很多时候我们并不需要总是在加载⽤户信息时就⼀定要加载他的订单信息。而是想用到订单信息时才会加载，延迟加载是基于嵌套查询来实现的。

原理：

1. **代理对象**： MyBatis 使用 CGLIB 或 JDK 动态代理技术创建目标类的代理对象。当你从 SqlSession 获取到一个对象时，如果该对象配置了延迟加载的属性，MyBatis 实际返回的是一个代理对象。

2. **触发加载条件**： 在访问需要延迟加载的属性时，代理对象内部会触发加载行为，此时代理对象才会执行相应的 SQL 语句去获取真正的数据填充该属性。

3. **SqlSession 的控制**： 延迟加载依赖于原始 SqlSession 的存在。如果调用 `SqlSession.close()` 方法关闭了 SqlSession，那么之后再尝试触发延迟加载将会抛出异常，因为无法执行新的数据库查询。

   ```
   在映射文件中，针对那些需要延迟加载的属性，使用 `<association>` 或 `<collection>` 标签，并设置 `fetchType="lazy"` 来配置延迟加载。
   ```

优点：先从单表查询，需要时再从关联表去关联查询，⼤⼤提⾼数据库性能，因为查询单表要⽐关联查询多张表

缺点：因为只有当需要⽤到数据时，才会进⾏数据库查询，这样在⼤批量数据查询时会很慢。

所以：在多表中：⼀对多，多对多——通常情况下采⽤延迟加载，⼀对⼀（多对⼀）——通常情况下采⽤⽴即加载

##### 130.说一下 mybatis 的一级缓存和二级缓存？

①、一级缓存是SqlSession级别的缓存。在操作数据库时需要构造sqlSession对象，在对象中有一个数据结构（HashMap）用于存储缓存数据。不同的sqlSession之间的缓存数据区域（HashMap）是互相不影响的。

②、二级缓存是mapper级别的缓存，多个SqlSession去操作同一个Mapper的sql语句，多个SqlSession可以共用二级缓存，二级缓存是跨SqlSession的。

区别：

1. **作用域**：SqlSession 缓存针对的是单个 `SqlSession`；Mapper 缓存则是应用级别的，可以跨 `SqlSession`。
2. **共享性**：SqlSession 缓存不能跨会话共享数据；Mapper 缓存可以在多个会话之间共享数据。
3. **配置**：SqlSession 缓存默认开启，不需要特别配置；Mapper 缓存需要在 MyBatis 配置文件中显式启用。

```java
// 获取 SqlSession
SqlSession sqlSession = sqlSessionFactory.openSession();
try {
    // 获取 Mapper 接口
    BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);
    
    // 第一次查询，会从数据库中获取数据并将结果缓存在 SqlSession 中
    Blog blog = mapper.selectBlogById(1);
    
    // 第二次查询相同的数据，由于使用的是同一个 SqlSession，
    // 因此会直接从一级缓存中获取数据，不会再次执行 SQL 查询
    Blog sameBlog = mapper.selectBlogById(1);
    
    // 这时候 sameBlog 和 blog 是指向同一个对象的引用
    
} finally {
    sqlSession.close();
}
```

```java
// 获取第一个 SqlSession
SqlSession sqlSession1 = sqlSessionFactory.openSession();
try {
    BlogMapper mapper1 = sqlSession1.getMapper(BlogMapper.class);
    // 第一次查询，会从数据库中获取数据并缓存到二级缓存中
    Blog blog1 = mapper1.selectBlogById(1);
} finally {
    sqlSession1.close(); // 关闭 SqlSession，这样二级缓存才会被使用
}

// 获取第二个 SqlSession
SqlSession sqlSession2 = sqlSessionFactory.openSession();
try {
    BlogMapper mapper2 = sqlSession2.getMapper(BlogMapper.class);
    // 此时，即使是新的 SqlSession，也不会再次执行查询，
    // 数据会从二级缓存中获取
    Blog blog2 = mapper2.selectBlogById(1);
} finally {
    sqlSession2.close();
}
```

##### 131.mybatis 和 hibernate 的区别有哪些？

Hibernate的优缺点：

- 优点：面向对象开发，不需要自己写sql语句。如果进行数据库迁移不需要修改sql语句，只需要修改一下方言。
- 缺点：hibernate维护数据表关系比较复杂。完全是有hibernate来管理数据表的关系，对于我们来说完全是透明的，不易维护。Hibernate自动生成sql语句，生成sql语句比较复杂，比较难挑错。Hibernate由于是面向对象开发，不能开发比较复杂的业务。

Mybatis的优缺点：

- Mybatis只需要程序员关注sql本身，不需要过多的关注业务，开发简单。MyBatis由于所有sql都是依赖数据库书写的，所以扩展性、迁移性比较差（Hibernate与数据库具体的关联在XML中）。

##### 132.mybatis 有哪些执行器（Executor）？

1. SimpleExecutor(简单执行器)：这是默认的执行器类型。它每次执行都会创建一个Statement对象，并立即执行SQL语句。这种执行器不支持事务，每次都会关闭Statement对象，适用于简单的查询场景。
2. ReuseExecutor(重用执行器)：这种执行器重用预处理的Statement对象。它会缓存Statement对象，当需要执行相同的SQL语句时，会直接使用缓存的Statement对象，而不是每次都创建新的对象。这种执行器也不支持事务。
3. BatchExecutor(批处理执行器)：这种执行器用于批量操作，可以一次执行多个SQL语句。它会将相同类型的SQL语句分组，并使用JDBC的批处理功能执行。这种执行器可以提高性能，尤其适用于需要执行大量相同类型SQL语句的场景，如批量插入或更新操作。】

##### 133.Mybatis和MybatisPlus的区别？

- MyBatis
  1. **灵活性和控制度**：MyBatis 允许你完全控制 SQL 语句的编写，它不会自动生成 SQL 语句。较高控制度，适合需要精细管理 SQL 细节的场景。
  2. **配置**：需要手动编写较多的配置信息，包括 SQL 语句、映射声明等。
- MyBatis Plus
  1. **自动CRUD**：MyBatis-Plus 提供了一套 CRUD 的方法，这些方法已经内置实现，无需用户自己定义。
  2. **代码生成**：MyBatis-Plus 包含了代码生成器，能够根据数据库表生成实体类、映射文件及相应的 Mapper 接口。
  3. **内置方法**：MyBatis-Plus 提供了多种内置的方法来支持例如分页查询、条件构造器等高级操作。
  4. **扩展性**：虽然提供了很多便利功能，但 MyBatis-Plus 也允许你像使用 MyBatis 那样自定义复杂的 SQL 查询。

### 二、动态web 资源开发技术——Servlet

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
### 三、动态的网页技术——JSP

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

### 四、Web服务器——Tomcat

#### 结构：

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

#### 流程：

在Tomcat容器中，当接收到HTTP请求时，会创建相应的Servlet类实例来处理该请求。Tomcat使用了Java的反射机制来创建Servlet实例，并依赖于Servlet规范的生命周期方法来管理Servlet的生命周期。

```markdown
## 初始化阶段：
Tomcat在启动时会扫描应用程序的部署描述符（如web.xml文件）来获取Servlet的配置信息。
根据配置信息，Tomcat会使用Java的反射机制动态加载Servlet类，并调用其无参构造方法创建Servlet实例。
创建Servlet实例后，Tomcat会调用Servlet的init()方法将Servlet初始化，并传递一个ServletConfig对象，其中包含了Servlet的配置参数。

## 请求处理阶段：
当接收到HTTP请求时，Tomcat会根据请求的URL匹配到对应的Servlet。
Tomcat会为每个请求创建一个独立的线程，该线程负责处理该请求。
Tomcat会调用Servlet的service()方法来处理请求，将请求和响应对象作为参数传递给service()方法。

## 生命周期管理：
在Servlet的整个生命周期中，Tomcat会根据Servlet规范的要求调用相应的生命周期方法。
在Servlet实例创建后，Tomcat会调用init()方法进行初始化。
当Tomcat关闭或重新加载Web应用程序时，会调用Servlet的destroy()方法进行销毁。
在运行期间，Tomcat会根据需要调用service()方法来处理请求。
```

使用Java的反射机制，Tomcat可以动态地加载和实例化Servlet类，而不需要直接在代码中进行显式的实例化。通过调用Servlet的生命周期方法，Tomcat能够管理Servlet的状态和资源，并在需要时创建、初始化和销毁Servlet实例。这种基于反射和生命周期方法的机制使Tomcat能够有效地处理Servlet请求，并提供高性能和可扩展性的Web应用程序容器。

#### 优化性能：

1. 调整Tomcat线程池大小：Tomcat默认的线程池大小为200，如果在高并发的情况下，可能会导致线程池资源不足，可以通过修改Tomcat的配置文件，调整线程池大小。
2. 调整JVM内存分配：Tomcat在启动时会使用JVM内存来加载应用程序和处理请求。如果JVM内存分配不合理，可能会导致内存不足或内存泄漏，可以通过调整JVM内存分配参数，如-Xms和-Xmx。
3. 使用缓存：Tomcat支持多种缓存技术，如内存缓存、磁盘缓存和分布式缓存等。通过使用缓存，可以减少应用程序对数据库和其他资源的访问，提高响应速度和并发访问量。
4. 使用 CDN 加速静态资源：可以使用 CDN（内容分发网络）来加速静态资源的传输，减少服务器的负载，提高网站的访问速度。
5. 启用静态资源缓存：可以通过在 web.xml 中配置 filter，启用静态资源缓存，减少服务器的负载，提高访问速度。

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

#### Core 核心层

Core 核心层是 Netty 最精华的内容，它提供了底层网络通信的通用抽象和实现，包括可扩展的事件模型、通用的通信 API、支持零拷贝的 ByteBuf 等。

##### ByteBuf

`ByteBuf` 是 Netty 中用来存储字节数据的主要数据结构，可以存在于 JVM 堆内存中，也可以配置为使用堆外（直接）内存以优化性能。特点：

- **读写索引**：ByteBuf 维护两个独立的指针，分别是读索引和写索引，分别记录读取和写入操作的位置。
- **池化**：Netty 支持对 ByteBuf 的池化，这意味着可以重用 ByteBuf 实例减少内存分配的开销。
- **零拷贝**：ByteBuf 提供了多种零拷贝操作，例如 slice、duplicate 和 compositeByteBuf 等。

##### 事件循环

Netty 的事件模型是指在网络应用中，采用`异步事件驱动`的编程方式。在这个模型中，程序的执行流程主要由外部事件触发，而不是固定的顺序。这种模型的设计能够有效提高系统的并发性和性能。

EventLoop：

- 一个 Netty 应用通常会包含一个或多个 EventLoop，每个 EventLoop 都在自己的线程中运行。
- EventLoop 内部维护了一个任务队列，它是一个双向链表。任务队列中存放着要执行的任务（例如，当一个 Channel 接收到数据时，EventLoop 会将读取数据的任务添加到任务队列中。）
- EventLoop 在不断地轮询任务队列，从队列中取出任务并执行。由于 EventLoop 是单线程的，因此不需要担心线程安全的问题。

Channel：在使用 Selector 之前，需要将 Channel 注册到 Selector 上，以便 Selector 监听该通道上的事件。注册了通道后，可以通过不断轮询的方式监听事件并进行处理。

selector：Selector 的实现基于操作系统提供的多路复用机制，例如 Linux 中的 select、poll，以及更[高效的](https://so.csdn.net/so/search?q=高效的&spm=1001.2101.3001.7020) epoll。Netty 采用了事件通知的方式，通过操作系统提供的底层机制，在通道上发生事件时通知 EventLoop 进行处理。

```
Selector ——允许一个线程同时监听多个通道的事件，减少线程的开销，提高系统的并发处理
```

EventLoopGroup：`EventLoopGroup` 用于管理 EventLoop ，包括将 EentLoop 分配给 Channel ，包括 EventLoop 的协同

#### Protocol Support 协议支持层

协议支持层基本上覆盖了主流协议的编解码实现，如 HTTP、SSL、Protobuf、压缩、大文件传输、WebSocket、文本、二进制等主流协议，此外 Netty  还支持自定义应用层协议。Netty 丰富的协议支持降低了用户的开发成本，基于 Netty 我们可以快速开发 HTTP、WebSocket  等服务。

#### Transport Service 传输服务层

传输服务层提供了网络传输能力的定义和实现方法。它支持 Socket、HTTP 隧道、虚拟机管道等传输方式。Netty 对 TCP、UDP 等数据传输做了抽象和封装，用户可以更聚焦在业务逻辑实现上，而不必关系底层数据传输的细节。

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

![b9a62febc4d142e4b55c8721a1c08151](https://gitee.com/xu_zuyun/picgo/raw/master/img/b9a62febc4d142e4b55c8721a1c08151.png)

1. 外部

   - #####  Producer (生产者)：生产者是发送消息的应用程序。它创建消息，并且将其发布到RabbitMQ中的交换器。

   - #####  Consumer (消费者)：消费者是接收消息的应用程序。它通过订阅队列来等待和处理消息。

   - Channel：网络信道，读写都是在Channel中进行（NIO的概念），包括对MQ进行的一些操作（例如clear queue等）都是在Channel中进行，客户端可建立多个Channel，每个Channel代表一个会话任务

2. 内部

   Message：由properties（有消息优先级、延迟等特性）和Body（消息内容）组成

   Routing key：路由规则，在消息中

   

   Queue：	

   ​    **队列名称**：保存队列的名称，用于唯一标识不同的队列。

   ​	**队列属性**：例如持久性、自动删除等配置选项。

   ​	**绑定信息**：队列与交换器之间的绑定关系。

   

   Exchange：交换器负责接收生产者发送的消息，并根据消息的routing key和队列的绑定规则转发这些消息到相应的队列中。

   

   Binding：是一个链接，它告诉交换器如何根据路由键、绑定键或头信息将消息路由到正确的队列。

   

   VHost：RabbitMQ 通过虚拟主机来实现逻辑分组和资源隔离，一个虚拟主机就是一个小型的 RabbitMQ 服务器，拥有独立的队列、交换器和绑定关系。保证`业务之间的隔离性和数据安全`。

交换器、队列 和 绑定健之间的关系：

```python
# 声明交换器
channel.exchange_declare(exchange='logs_direct', exchange_type='direct')
# 声明队列
channel.queue_declare(queue='error_logs')
channel.queue_declare(queue='info_logs')
# 绑定队列到交换器
channel.queue_bind(exchange='logs_direct', queue='error_logs', routing_key='error')
channel.queue_bind(exchange='logs_direct', queue='info_logs', routing_key='info')
```

- 优点：
  1. 解除业务系统之间的**耦合**，降低系统之间的依赖关系
  2. 实现消息的**异步**处理，无需同步等待
  3. **削峰填谷**，将流量从高峰期引到低谷期进行处理
- 缺点：
  1. 系统的可用性比原先要低，MQ挂了以后，整个系统也会崩溃.
  2. 消费端数据一致性问题
- 使用场景
  - 内容订阅
  - 日志采集
  - 埋点
  - 订单

```markdown
基于消息的模型，关心的是“通知”，而非“处理”.
在下单时库存系统不能正常使用。也不影响正常下单，因为下单后，订单系统写入消息队列就不再关心其他的后续操作了。实现订单系统与库存系统的应用解耦.
`为了保证库存肯定有，可以将队列大小设置成库存数量`
```

#### 同步方式

- 元数据信息在集群内部通过**Erlang分布式数据库**（Mnesia）存储和管理。当一个节点变更了这些配置或者元数据时，**变更会被自动同步**到所有其他节点。
- 当新节点加入到集群或者已有节点恢复在线后，RabbitMQ会**同步队列的状态和元数据信息到该节点**。这使得节点能够迅速更新到集群当前的状态。

#### 两种节点（Node）

RabbitMQ中有两种节点类型：磁盘节点（disk nodes）和内存节点（ram nodes）。这些分类主要影响的是**元数据**而不是消息本身。

- **磁盘节点**会定期将元数据从内存同步到磁盘。这些元数据包括队列的定义、交换器的定义、绑定以及用户权限等。由于这些信息被写入到磁盘，即使所有的节点都宕机，集群依然能够在重新启动后恢复到之前的状态。至少需要一个磁盘节点来保持集群的元数据，确保元数据的持久性和一致性。

- **内存节点**不会把所有元数据写入磁盘，而是保留在内存中。这可以提供更快的读写性能，但是如果没有任何磁盘节点，并且所有内存节点同时失效的话，集群可能会丢失配置信息，因此内存节点不适合用于存储关键数据。内存节点通常用于提高集群的消息传递性能。

  ```markdown
  是否将消息保存到磁盘是由消息发送时的持久化设置决定的，而不是由节点类型决定。磁盘节点确保元数据能够被持久化存储，而对于消息的持久化，则需要发送者在发布消息时将其`标记为持久化`，并确保消息被路由到`持久化队列`中。
  
  如果将`非持久化消息发送到持久化队列`中，那么这些消息在服务器重启后仍然会丢失。
  反之，如果将`持久化消息发送到非持久化队列`，那么队列将不会在服务器重启后恢复，而且其中的消息也会丢失，无论这些消息在重启前是否已经被写入磁盘。
  ```

#### 四种交换机(Exchange)

- i. 直连交换机，Direct exchange：带路由功能的交换机，根据routing_key（消息发送的时候需要指定）直接绑定到队列，只有当队列的绑定键和消息的Routing Key严格相等时，才能收到消息。⼀个交换机也可以通过过个routing_key绑定多个队列。

  ![](https://gitee.com/xu_zuyun/picgo/raw/master/img/16f7a40302a0db52~tplv-t2oaga2asx-jj-mark 3024 0 0 0 q75.png)

  1. 创建一个名为 `direct_logs` 的 Direct Exchange。
  2. 生产者发布消息到 `direct_logs` 交换器，并为每条消息指定一个 `routing key`，比如 "error"。
  3. 队列通过 `binding key` 绑定到交换器。例如，一个队列可能只关心错误日志，因此它使用 "error" 作为 `binding key`。
  4. 当 `direct_logs` 收到带有 "error" `routing key` 的消息时，它就会将消息路由到绑定了 "error" `binding key` 的队列。

  ```java
  // 示例代码：在生产者端设置 routing key 发送消息
  channel.basicPublish("direct_logs", "error", null, message.getBytes());
  ```

- ii. 扇形交换机，Fanout exchange：⼴播消息，适合于发布/订阅模型

  在扇形交换机的场景中，一个生产者发布的消息会被路由到所有与之绑定的队列，而不管绑定时使用的routing key是什么。这样就实现了消息的广播功能。

  ![16f7a4014628ba4c~tplv-t2oaga2asx-jj-mark 3024 0 0 0 q75](https://gitee.com/xu_zuyun/picgo/raw/master/img/16f7a4014628ba4c~tplv-t2oaga2asx-jj-mark%203024%200%200%200%20q75.png)

- iii. 主题交换机，Topic exchange：发送到主题交换机上的消息需要携带指定规则的routing_key（依据字符串模式，`*`代表一个词，`#`代表零个或多个词），主题交换机会根据这个规则将数据发送到对应的(多个)队列上。

  ![16f7a4015f452d5e~tplv-t2oaga2asx-jj-mark 3024 0 0 0 q75](https://gitee.com/xu_zuyun/picgo/raw/master/img/16f7a4015f452d5e~tplv-t2oaga2asx-jj-mark%203024%200%200%200%20q75.png)

  - 队列A关注所有来源的错误日志，因此它使用 `*.error` 作为绑定键。
  - 队列B关注来自认证系统的所有日志，因此它使用 `auth.*` 作为绑定键。

  ```java
  // 示例代码：在生产者端设置 routing key 为 "error.db.mysql" 发送消息
  channel.basicPublish("topic_logs", "mysql.error", null, message.getBytes());
  ```

- iv. ⾸部交换机，Headers exchange：⾸部交换机是忽略routing_key的⼀种路由⽅式。路由器和交换机路由的规则是通过Headers信息来交换的，这个有点像HTTP的Headers。将⼀个交换机声明成⾸部交换机，绑定⼀个队列的时候，定义⼀个Hash的数据结构，消息发送的时候，会携带⼀组hash数据结构的信息，当Hash的内容匹配上的时候，消息就会被写⼊队列。

```markdown
在RabbitMQ集群中的节点只有两种类型：`内存节点/磁盘节点`，单节点系统只运行磁盘类型的节点。而在集群中，可以选择配置部分节点为内存节点。

内存节点将所有的队列，交换器，绑定关系，用户，权限，和vhost的元数据信息`保存在内存`中。

磁盘节点将这些信息保存在磁盘中，但是内存节点的性能更高，为了保证集群的高可用性，必须保证集群中有`两个以上的磁盘节点`，来保证当有一个磁盘节点崩溃了，集群还能对外提供访问服务
```

#### 两种模式

##### 1.经典队列模式（高性能）

- **单个队列存储**：在经典模式下，每个队列通常存储在一个 RabbitMQ 节点上，这个节点被称为“主节点”（Master Node）。所有的生产者和消费者都通过该节点进行消息的发送和接收。
- **优缺点：**如果存放消息的节点宕机了，就等待该节点恢复；由于不需要同步消息数据所以性能较高。

```markdown
## 消费流程
当消息进入A节点的Queue中后，consumer从B节点拉取时，RabbitMQ会临时在A、B间进行消息传输，`把A中的消息实体取出并经过B发送给consumer`，所以consumer应平均连接每一个节点，从中取消息。
## 故障处理
该模式存在一个问题就是当A节点故障后，B节点无法取到A节点中还未消费的消息实体。
1. 如果做了队列持久化或消息持久化，那么得`等A节点恢复，然后才可被消费`，并且在A节点恢复之前其它节点不能再创建A节点已经创建过的持久队列；
2. 如果没做队列持久化或消息持久化，重新连接到集群里的其他节点，并重新创建队列。


    （这种模式更适合非持久化队列，只有该队列是非持久的，客户端才能重新连接到集群里的其他节点，并重新创建队列。假如该队列是持久化的，那么唯一办法是将故障节点恢复起来。）
```

##### 2.镜像模式：高可用性

消息实体会主动在镜像节点（可以选择部分节点作为镜像，也可以选择所有节点作为镜像）间同步，而不是在consumer取数据时临时拉取。

1. 声明队列A的节点上拥有一个主副本（master），该主副本将处理所有A消息的接收和发送。通过设置策略来指定其镜像属性（ha-mode），主副本上的当前状态将同步到所有镜像副本。这包含了以下几个部分：

- 消息内容：队列中现有的消息被复制到镜像节点。
- 队列操作：随后对主队列的所有操作（比如新消息的入队、消息的确认和删除）也会同步到所有镜像节点。

2. 在正常运行期间，所有的写操作（发布消息、确认等）都首先在主副本上执行，然后异步地传播到所有镜像副本。这确保所有节点上的队列副本始终保持一致。

3. 如果主副本所在的节点发生故障，RabbitMQ会自动从镜像副本中选择一个作为新的主副本。这个过程称为故障转移（failover），它允许队列继续服务，即使原来的主节点不可用。

4. 当故障的节点恢复后，它会成为新主副本的镜像节点，并重新同步队列的状态。

**优缺点：**集群内部的网络带宽将会被这种同步通讯大大消耗掉。对可靠性要求较高的场合中适用

#### 推和拉

推模式：当消息到达RabbitMQ时，RabbitMQ会自动地、不断地投递消息给匹配的消费者

```
优点：实时性好、吞吐量高
缺点：消费者必须设置一个缓冲区缓存这些消息
```

拉模式：在消费者需要时才去[消息中间件](https://so.csdn.net/so/search?q=消息中间件&spm=1001.2101.3001.7020)拉取消息，这段网络开销会明显增加消息延迟，降低系统吞吐量

```
优点：消费速度可控、避免消息积压
缺点：实时性差、吞吐量低
```

#### 搭建注意

- 各节点之间使用“–link”连接，此属性不能忽略。
- 各节点使用的 erlang cookie 值必须相同，此值相当于“秘钥”的功能，用于各节点的认证。
- 整个集群中必须包含一个磁盘节点。

#### 问题

⚠如何保证MQ中消息不会丢失？

- RabbitMQ允许将消息`标记为持久化`，这意味着消息将会被写入磁盘而不是仅保存在内存中。这样，在消息发送到队列之后，即使RabbitMQ服务器发生故障或重启，消息也能够存储在磁盘上，并在恢复后仍然可用。

- 生产者发生异常没有把消息成功发送给MQ，MQ成功接收到消息之后发生宕机了，消息未被成功，消费消费端要设置`签收机制`为手动签收，只有当消息最终被处理，才告诉MQ已经消费，此时MQ再去删除这条消息。

  ```
  消息预取（Message Prefetch）：RabbitMQ允许消费者一次从队列中获取多个消息，并将它们存储在本地缓冲区中。这样可以提高消费者的效率，并减少消费者与RabbitMQ服务器之间的通信次数。
  ```

```java
具体如下：
1.声明交换机Exchange的时候设置 durable=true；
2.声明队列Queue的时候设置 durable=true；(意味着即使RabbitMQ服务重启，队列也仍然存在)
3.发送消息的时候设置消息的 deliveryMode = 2；(表示该消息是持久化的)
    
//public TopicExchange(String name, boolean durable, boolean autoDelete)
return new TopicExchange(EXCHANGE_NAME,true,false);
//durable：是否将队列持久化 true表示需要持久化 false表示不需要持久化
return new Queue(QUEUE_NAME, false);

new MessageProperties() --> DEFAULT_DELIVERY_MODE = MessageDeliveryMode.PERSISTENT --> deliveryMode = 2;
```

###### 如何解决消息堆积？

- 启动多个消费者微服务
- 优化消费者处理消息的能力，提高消费消息的性能
- 调整RabbitMQ的队列容量
- 设置过期时间，让一些不是那么重要的消息到达指定时间之后就丢弃
- 将无法被消费的消息放到死信队列（DLQ）中
- 自定义消息队列逻辑：最短任务优先、优先级调度...

###### 如何解决消息被重复消费？

- 生产者发送消息的时候带上一个全局唯一的id，消费者拿到消息后，先根据这个id去 Redis 里查一下，之前有没消费过，没有消费过就处理，并且写入这个id到 Redis，如果消费过了，则不处理。
- 消费者端幂等性：设计消费者端的处理逻辑具有幂等性。即无论消息被处理多次，最终结果都保持一致。这样，即使消息被重复消费，也不会对最终结果产生影响。

###### RabbitMQ 中 vhost 的作用是什么？

vhost：每个 RabbitMQ 都能创建很多 vhost，我们称之为虚拟主机，每个虚拟主机其实都是 mini 版的RabbitMQ，它拥有自己的队列，交换器和绑定，拥有自己的权限机制，主要是为了隔离，vhost 不仅消除了为基础架构中的每一层运行一个RabbitMq服务器的需要, 童谣避免为每一层创建不同的集群.

###### RabbitMQ 集群中唯一一个磁盘节点崩溃了会发生什么情况？

如果唯一磁盘的磁盘节点崩溃了，
唯一磁盘节点崩溃了，集群是**可以保持运行**的，但你**不能更改任何东西**。因为配置更改和其他管理操作需要访问存储在磁盘节点上的持久化数据。

###### RabbitMQ 每个节点是其他节点的完整拷贝吗？为什么？

不是，原因有以下两个：

- 存储空间的考虑：如果每个节点都拥有所有队列的完全拷贝，这样新增节点不但没有新增存储空间，反而增加了更多的冗余数据；

- 性能的考虑：如果每条消息都需要完整拷贝到每一个集群节点，那新增节点并没有提升处理消息的能力，最多是保持和单节点相同的性能甚至是更糟

###### rabbitMQ节点的关闭顺序？

在关闭 RabbitMQ 集群时，应该先关闭从节点，然后再关闭主节点。这是因为从节点的状态是依赖于主节点的，如果先关闭主节点，可能会导致从节点无法正常工作。

#### 代码

```go
package main

import (
	"github.com/streadway/amqp"
	"log"
)

func failOnError(err error, msg string) {
	if err != nil {
		log.Fatalf("%s: %s", msg, err)
		panic(fmt.Sprintf("%s: %s", msg, err))
	}
}

func main() {
	conn, err := amqp.Dial("amqp://guest:guest@localhost:5672/")
	failOnError(err, "Failed to connect to RabbitMQ")
	defer conn.Close()

	ch, err := conn.Channel()
	failOnError(err, "Failed to open a channel")
	defer ch.Close()

	q, err := ch.QueueDeclare(
		"hello", // name
		false,   // durable
		false,   // delete when unused
		false,   // exclusive
		false,   // no-wait
		nil,     // arguments
	)
	failOnError(err, "Failed to declare a queue")

	body := "Hello World!"
	err = ch.Publish(
		"",     // exchange
		q.Name, // routing key
		false,  // mandatory
		false,  // immediate
		amqp.Publishing{
			ContentType: "text/plain",
			Body:        []byte(body),
		})
	failOnError(err, "Failed to publish a message")
	log.Printf(" [x] Sent %s", body)
}
```

```go
package main

import (
	"github.com/streadway/amqp"
	"log"
)

func failOnError(err error, msg string) {
	if err != nil {
		log.Fatalf("%s: %s", msg, err)
		panic(fmt.Sprintf("%s: %s", msg, err))
	}
}

func main() {
	conn, err := amqp.Dial("amqp://guest:guest@localhost:5672/")
	failOnError(err, "Failed to connect to RabbitMQ")
	defer conn.Close()

	ch, err := conn.Channel()
	failOnError(err, "Failed to open a channel")
	defer ch.Close()

	q, err := ch.QueueDeclare(
		"hello", // name
		false,   // durable
		false,   // delete when unused
		false,   // exclusive
		false,   // no-wait
		nil,     // arguments
	)
	failOnError(err, "Failed to declare a queue")

	msgs, err := ch.Consume(
		q.Name, // queue
		"",     // consumer
		true,   // auto-ack
		false,  // exclusive
		false,  // no-local
		false,  // no-wait
		nil,    // args
	)
	failOnError(err, "Failed to register a consumer")

	forever := make(chan bool)

	go func() {
		for d := range msgs {
			log.Printf("Received a message: %s", d.Body)
		}
	}()

	log.Printf(" [*] Waiting for messages. To exit press CTRL+C")
	<-forever
}
```

### 十一、分布式应用程序协调服务——ZooKeeper

**ZooKeeper** 是一个开源的**分布式协调框架**，它的定位是为分布式应用提供一致性服务，是整个大数据体系的**管理员**。**ZooKeeper** 会封装好复杂易出错的关键服务，将高效、稳定、易用的服务提供给用户使用。

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/v2-ec4d237ed60d914e5d1915c99778f5a7_1440w-20240111095157623.webp)

##### 特点

1. **集群**：Zookeeper是一个领导者（Leader），多个跟随者（Follower）组成的集群。

2. **高可用性**：集群中只要有半数以上节点存活，Zookeeper集群就能正常服务。（当发起一个变更请求时，领导者需要从过半的节点（包括自己）得到确认才能认为该操作被提交（这意味着至少写入了过半节点的日志中）

3. **一致性**：每个Server保存一份相同的数据副本，Client无论连接到哪个Server，数据都是一致的。

4. **有序性：**

   全局唯一递增的事务ID（zxid）：每个更新请求都被赋予一个全局唯一的递增的事务ID。这个ID反映了所有事务的顺序。

   更新请求顺序进行：来自同一个Client的更新请求按其`发送顺序依次执行`（通过事务id递增实现）。

5. **原子性**：一次数据更新要么成功，要么失败。

6. **可见性**：在一定时间范围内，Client能读到最新数据。

   ```
   原子性、有序性、可见性
   ```

7. 从`设计模式`角度来看，zk是一个基于**观察者设计模式**的框架，它负责管理跟存储大家都关心的数据，然后接受观察者的注册，数据发生变化zk会通知在zk上注册的观察者做出反应。

```markdown
Zookeeper 有三种部署模式：
    单机部署：一台集群上运行；
    集群部署：多台集群运行；
    伪集群部署：一台集群启动多个 Zookeeper 实例运行。
Zookeeper 保持主从节点的同步？
    Zookeeper 的核心是原子广播机制，这个机制保证了各个 server 之间的同步。实现这个机制的协议叫做 Zab 协议。Zab 协议有两种模式，它们分别是恢复模式和广播模式。
为什么要有主节点？
    在分布式环境中，有些业务逻辑只需要集群中的某一台机器进行执行，其他的机器可以共享这个结果，这样可以大大减少重复计算，提高性能
```

- **C（一致性）**：ZooKeeper保证强一致性。它使用原子广播协议（如Zab协议）来确保集群内所有副本在完成变更前都达成一致。
- **A（可用性）**：在没有网络分区的情况下，ZooKeeper提供高可用性。但是，在出现网络分区时，如果不能与过半节点通信，为了保持一致性，ZooKeeper会牺牲部分可用性——该集群将停止对外提供写服务，直到能够重新建立过半的节点连接。
- **P（分区容忍性）：**zookeeper牺牲了该特性（一个具有分区容忍性的系统能够继续提供服务，确保系统的可用性和数据的可靠性，即使在部分节点无法相互通信的情况下。）

##### 原子广播

> 特点：
>
> - 所有的写操作（比如创建、删除、更新 znodes）确实需要经过 Leader 节点的协调和管理
> - 如果请求是发送给 Follower 节点的，那么 Follower 会将请求**转发给 Leader** 节点
>
> 步骤：
>
> 1. **启动阶段**:
>    - 当 ZooKeeper 服务启动时，如果没有现存的 Leader，集群的服务器将进入领导者选举阶段。
>    - 所有服务器参与领导者选举，基于它们的事务日志中的最新事务ID（ZXID）和其他一些信息（比如服务器ID）进行投票选择 Leader。
> 2. **领导者选举**:
>    - 通过一系列投票轮次，直到集群中超过半数的节点同意某个特定的节点作为 Leader。
>    - 一旦选举出 Leader，该节点就开始对 Follower 进行同步，以确保整个集群的状态一致。
> 3. **恢复阶段**:
>    - Leader 收集所有 Follower 的状态信息，并判断最新的系统状态。
>    - 任何落后于当前系统状态的 Follower 都会接收到来自 Leader 的更新，以便他们能够追上最新状态。
> 4. **提案广播**:
>    - 在恢复阶段结束后，Leader 节点就可以开始处理客户端的写请求了。
>    - 当 Leader 收到一个修改系统状态的请求（例如创建、删除或更新 znode），它会创建一个 "提案" 并将其分发给所有的 Follower 节点。
> 5. **确认阶段**:
>    - Follower 接收到提案后，将其写入到`事务日志`，然后向 Leader 发送一个 "ACK"（确认响应）。
>    - 一旦 Leader 收到了来自超过半数节点的 ACKs，它就认为这个提案已经被提交。
> 6. **提交与应用**:
>    - Leader 向所有 Follower 发送一个 "commit" 消息，指示他们应用这个提案。
>    - Follower 节点在收到 "commit" 消息后，会正式将提案应用到它们自己的状态中，并向客户端返回操作成功的响应。
> 7. **客户端响应**:
>    - 客户端最终从 Leader 或 Follower 节点获得操作结果。
>
> ZAB 协议确保了即使在出现`网络分区、服务器崩溃`等问题的情况下，ZooKeeper 也能够保证数据的一致性和正确的顺序。当系统处于稳定状态，且没有选举发生时，ZAB 协议以高效的方式达成数据的一致性。当需要进行新的领导者选举时，ZAB 也包含了保证集群状态不丢失并快速恢复的机制。

##### ZooKeeper文件系统

ZooKeeper文件系统（ZooKeeper File System，ZFS）是ZooKeeper提供的一种类似于文件系统的数据模型，可以帮助用户通过树形节点结构，对ZooKeeper中的数据进行管理和访问。

在ZFS中，数据存储以节点（ZNode）的形式组织，每个ZNode可以保存一个Byte数组，同时也可以是一个目录节点，包含了子节点的信息。用户可以通过ZFS的API接口，对节点进行创建、读取、更新和删除等操作。

ZFS的根节点是"/"，通过这个节点，用户可以访问整个ZooKeeper文件系统。节点名字可以是任何的字符串，但是不能重复，节点名字可以有多级，中间用"/ "隔开。例如，/myapp/config是一个典型的ZFS节点路径。

##### ZNode节点四种类型：

- 持久节点（persistent）：这是最常见的节点类型，一旦创建，将一直存在于ZooKeeper的目录结构中，直到显式删除。
- 临时节点（ephemeral）：临时节点是指在创建客户端会话期间存在的节点。当客户端会话结束（由于某种原因）时，节点将被自动删除。临时节点非常有用，它们可以用于表示客户端会话是否处于特定状态。
- 有序节点（sequential）：有序节点还可以与上述两种节点类型结合使用，这样可以创建名称自动带有序增量的节点。有序节点用于排序构造出一种递增的节点名称集合。
- 临时顺序节点（ephemeral sequential）：这是一种组合节点类型，是临时节点和顺序节点的组合。这种节点将在客户端会话期间存在，并带有一个顺序号。当会话终止时，顺序号将自动从数据库中删除。

##### 监听通知流程

- 在main线程中创建Zookeeper客户端，设置Watcher，这时就会创建两个线程，一个负责网络连接通信（connet），一个负责监听（listener）。

- 通过connect线程将注册的监听事件发送给Zookeeper。

- 在Zookeeper的注册监听器列表中将注册的监听事件添加到列表中。

- Zookeeper监听到有数据或路径变化，就会将这个消息发送给listener线程。

- listener线程内部调用了`process()`方法。

  

- 在 ZooKeeper 中，watcher 是一次性的：每个 watcher 只会触发一次通知。如果客户端需要持续监听，那么在每次接收并处理完通知之后，都需要重新设置 watcher。

  ```java
  import org.apache.zookeeper.WatchedEvent;
  import org.apache.zookeeper.Watcher;
  import org.apache.zookeeper.ZooKeeper;
  
  public class ZooKeeperWatcherExample {
      private static final String ZK_SERVER_URL = "localhost:2181";
      
      public static void main(String[] args) throws Exception {
          // 创建一个与服务器的连接
          ZooKeeper zk = new ZooKeeper(ZK_SERVER_URL, 1000, new Watcher() {
              // 监听器的回调方法
              public void process(WatchedEvent event) {
                  System.out.println("已经触发了" + event.getType() + "事件！");
                  // 可以在这里添加更多逻辑来响应不同类型的事件
              }
          });
  
          // 假设我们要监听的znode路径为 "/my_service"
          byte[] data = zk.getData("/my_service", true, null);
          // 执行其他操作...
  
          // 关闭与服务器的连接
          zk.close();
      }
  }
  ```

#### 不适合做的事

- **1. 大规模数据存储：** ZooKeeper不是为了作为一个数据库设计的，因此不适合存储大量数据。它的节点（znode）有数据大小的限制，默认情况下是1MB。
- **2. 实时查询或处理：** ZooKeeper的读写请求延迟比一般数据库高，不适合需要快速、实时查询或处理的应用。

#### 应用场景

##### 1. 数据发布/订阅

当某些数据由几个机器共享，且这些信息经常变化数据量还小的时候，这些数据就适合存储到ZK中。

- **数据存储**：将数据存储到 Zookeeper 上的一个数据节点。
- **数据获取**：应用在启动初始化节点从 Zookeeper 数据节点读取数据，并在该节点上注册一个数据变更 **Watcher**
- **数据变更**：当变更数据时会更新 Zookeeper 对应节点数据，Zookeeper会将数据变更**通知**发到各客户端，客户端接到通知后重新读取变更后的数据即可。

##### 2. 分布式锁

主要是避免了羊群效应，临时节点已经预先存在，所有想要获得锁的线程在它下面创建临时顺序编号目录节点，编号最小的获得锁，用完删除，后面的依次排队获取。

优点：

1. 临时顺序节点实现了`公平锁`
2. 无论连接到哪个 ZooKeeper 服务器节点上，客户端都能读取到最新的数据，确保同一时刻`只有一个客户端持有锁`
3. ZooKeeper 的 Watcher 机制使得客户端能够订阅节点的变更事件，这样就可以有效地等待锁的释放，而`无需轮询检查`。这减少了网络通信量和响应时间。
4. 如果客户端崩溃或者由于某种原因失去与服 务器的连接，ZooKeeper 可以`自动清理`该客户端建立的临时节点，从而释放锁。这对于避免死锁情况非常重要。
5. “大多数”机制 确保 发生分区时不会出现 锁被多个客户端`同时认为自己持有` 的情况

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/v2-bbfaf9843eea699c518089ae8c8453ab_1440w.webp)

##### 3. 负载均衡

- 多个服务注册
- 客户端获取中间件地址集合
- 从集合中随机选一个服务执行任务

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/v2-530532fe3cb598e079483a33b069c13f_1440w.webp)

4. ##### 命名服务

利用 zk 创建一个全局唯一的路径，这个路径就可以作为一个名字，指向集群中的集群，提供的服务的地址，或者一个远程的对象等等。

5. ##### Leader选举

   利用ZooKeeper的**强一致性**，能够保证在分布式高并发情况下节点创建的全局唯一性，即：同时有多个客户端请求创建 /Master 节点，最终一定只有一个客户端请求能够创建成功。利用这个特性，就能很轻易的在分布式环境中进行集群选举了。

   就是动态Master选举。这就要用到 **EPHEMERAL_SEQUENTIAL**类型节点的特性了，这样每个节点会`自动被编号`。允许所有请求都能够创建成功，但是得有个创建顺序，每次选取序列号**最小**的那个机器作为**Master** 。

```markdown
## ZooKeeper集群节点个数一定是奇数个
在节点数量是奇数个的情况下， zookeeper集群总能对外提供服务（即使损失了一部分节点）；如果节点数量是偶数个，会存在zookeeper集群不能用的可能性（脑裂成两个均等的子集群的时候）。
```

```golang
func main() { // 创建和删除节点
	// ...（前略）

	// 创建一个临时顺序节点
	path, err := conn.Create("/myapp", []byte("data"), zk.FlagEphemeral|zk.FlagSequence, zk.WorldACL(zk.PermAll))
	if err != nil {
		fmt.Printf("Create returned error: %+v\n", err)
		return
	}
	fmt.Printf("Created node %s\n", path)

	// 获取节点数据
	data, stat, err := conn.Get("/myapp0000000001") // 假定节点名为 /myapp0000000001
	if err != nil {
		fmt.Printf("Get returned error: %+v\n", err)
		return
	}
	fmt.Printf("Node data: %s, version: %d\n", string(data), stat.Version)

	// 删除节点
	err = conn.Delete("/myapp0000000001", -1) // 版本号设置为-1可以匹配任何版本，从而无条件删除节点
	if err != nil {
		fmt.Printf("Delete returned error: %+v\n", err)
		return
	}
	fmt.Println("Deleted node /myapp0000000001")

	// ...（后略）
}
```

```go
package main // 观察节点变化

import (
	"fmt"
	"github.com/go-zookeeper/zk"
	"time"
)

func main() {
	// 连接到ZooKeeper服务器
	conn, _, err := zk.Connect([]string{"localhost:2181"}, time.Second*5)
	if err != nil {
		panic(err)
	}
	defer conn.Close()

	// 定义观察函数
	watchNode := func(path string) {
		for {
			// GetW 用于获取节点数据，并为该节点的后续变化设置一个watcher
			_, _, ch, err := conn.GetW(path)
			if err != nil {
				fmt.Printf("GetW failed on path %s: %v\n", path, err)
				return
			}

			// 等待事件通道上的事件
			event := <-ch
			fmt.Printf("Received event: %+v\n", event)

			// 根据事件类型进行处理
			switch event.Type {
			case zk.EventNodeCreated:
				fmt.Printf("Node %s was created\n", path)
			case zk.EventNodeDeleted:
				fmt.Printf("Node %s was deleted\n", path)
				return // 如果节点被删除，则退出循环
			case zk.EventNodeDataChanged:
				fmt.Printf("Node %s data changed\n", path)
			case zk.EventNodeChildrenChanged:
				fmt.Printf("Node %s children changed\n", path)
			}
		}
	}

	// 启动goroutine以观察指定的节点
	go watchNode("/example_node")

	// 保持程序运行
	select {}
}
```

### 十二、反向代理服务器——Nginx

支持热部署，几乎可以做到 7 * 24  小时不间断运行，即使运行几个月也不需要重新启动，还能在不间断服务的情况下对软件版本进行热更新。性能是 Nginx  最重要的考量，其占用内存少、并发能力强、能支持高达 5w 个并发连接数，最重要的是， Nginx 是免费的并可以商业化，配置使用也比较简单。

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/format,png.png)

##### 使用场景

1. 静态资源服务，通过本地文件系统提供服务；
2. 反向代理服务，延伸出包括缓存、负载均衡等；

```markdown
## 正向和反向代理区别
正向代理需要你主动设置代理服务器ip或者域名进行访问，由设置的服务器ip或者域名去获取访问内容并返回；
反向代理不需要你做任何设置，直接访问服务器真实ip或者域名，但是服务器内部会自动根据访问内容进行跳转及内容返回，你不知道它最终访问的是哪些机器。
## 使用场景
正向：VPN
反向：保护和隐藏原始资源服务器、加密和SSL加速、负载均衡、缓存静态内容、压缩、减速上传、安全、外网发布
```

```nginx
# 主要的全局设置
user www-data; # 指定 Nginx 进程运行用户
worker_processes auto; # 工作进程数量，auto 代表自动按照可用的 CPU 核心数
pid /run/nginx.pid; # 用于存储主进程 PID 的文件

events {
  worker_connections 768; # 单个工作进程可以支持的最大连接数
  # 其他事件相关的设置...
}

http {
  # http 块内的全局 HTTP 设置
  include       /etc/nginx/mime.types; # 包含 MIME 类型映射表
  default_type  application/octet-stream; # 默认文件类型

  # 日志格式定义
  log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                    '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for"';

  # 访问日志和错误日志路径
  access_log  /var/log/nginx/access.log  main;
  error_log  /var/log/nginx/error.log  warn;

  sendfile        on; # 启用 sendfile 优化文件传输
  tcp_nopush     on; # 减少网络报文的数量
  tcp_nodelay    on; # 禁用 Nagle 算法，减少延迟

  keepalive_timeout  65; # keep-alive 超时时间

  # 包含额外的配置文件，通常用于虚拟主机
  include /etc/nginx/conf.d/_*.conf;
  include /etc/nginx/sites-enabled/_*;

  服务器块示例
  server {
    listen       80 default_server; # 监听端口和默认服务器设置
    server_name  localhost; # 服务器域名或 IP

    # 文档根目录
    root   /usr/share/nginx/html;

    # 默认请求处理器
    location / {
      index  index.html index.htm; # 默认首页文件
    }

    # 图片文件缓存配置
    location ~* \.(gif|jpg|jpeg|png)$ {
      expires 30d; # 设置静态图片资源的 HTTP 缓存有效期为 30 天
    }
    
    location /api/ { #location /api/ 块定义了所有以 /api 开头的请求都会被转发到 http://localhost:8080/
            # 反向代理的配置
            proxy_pass http://localhost:8080/; # 将请求转发到后端服务
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade; #proxy_set_header 用于确保正确的 HTTP 头部信息被转发给后端服务
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
            # 可以根据需要添加更多的 proxy_set_header 指令
        }
    
    # 其他 location 块...

  }

  # 更多 server 块...
}
```

### 十三、远程过程调用——RPC

#### 原理

- 客户端（Client）：服务调用方（服务消费者）
- 客户端存根（Client Stub）：存放服务端地址信息，将客户端的请求参数数据信息打包成网络消息，再通过网络传输发送给服务端
- 服务端存根（Server Stub）：接收客户端发送过来的请求消息并进行解包，然后再调用本地服务进行处理
- 服务端（Server）：服务的真正提供者

```markdown
关键步骤：
## 一、 接口定义：
RPC 开始于为所需服务定义明确的接口。这通常使用接口定义语言（Interface Definition Language, IDL）来完成，它定义了可供远程客户端调用的过程和函数、参数以及数据类型。

## 二、客户端存根（Stub）：
负责将过程调用的所有参数“序列化”成一串能够通过网络发送的字节流

> 同步调用：客户端等待直至获得响应才继续执行。
  异步调用：客户端不用等待响应就可以继续执行其他任务。

## 三、通信：
 TCP 或 UDP

## 四、服务器存根：
反序列化，调用得到结果，序列化返回
```

![RPC框架的实现原理，及RPC架构组件详解](https://gitee.com/xu_zuyun/picgo/raw/master/img/6155e42e5583999845effc201bcebf5a.jpeg)

![RPC框架的实现原理，及RPC架构组件详解](https://gitee.com/xu_zuyun/picgo/raw/master/img/1cbf540858e78191e813acba421a9467.jpeg)

#### 服务发现

##### 为什么gRPC仍然需要服务发现？

- **动态性**：在生产环境中，服务实例可能会因为部署、扩展、故障转移等原因动态地增加或减少。服务发现机制可以帮助客户端找到当前可用的服务实例。
- **负载均衡**：服务发现通常与负载均衡结合使用。当有多个服务实例可用时，客户端需要知道向哪个实例发送请求，以便合理分配负载，并避免某些实例过载。
- **抽象和解耦**：服务发现使客户端无需硬编码服务实例的地址。这降低了微服务架构中各服务之间的耦合度，提高了系统的灵活性和可维护性。
- **故障检测和处理**：服务注册表通常具备健康检查的功能，可以从注册表中自动剔除故障的服务实例，保证客户端总是连接到健康的实例上。

##### 服务发现步骤

1. **服务注册**：在服务启动时，将其地址和元数据注册到服务发现系统中，如Etcd或Zookeeper。
2. **服务健康检查**：注册的服务应定期发送心跳或通过其他机制来维持其在服务发现系统中的状态。
3. **服务解析器**：gRPC客户端使用一个服务解析器来查询服务发现系统，以获得最新的服务实例列表。
4. **负载均衡**：gRPC客户端根据获取到的服务实例列表执行负载均衡算法，决定将请求发送到哪个实例。

### 十四、JWT——轻量级的身份验证和授权机制

Json Web Token

#### 组成

- Header（标头），通常由两部分组成：令牌的类型 和 所用的加密算法，然后将该JSON对象数据进行`Base64 URL`编码，得到的字符串就是JWT令牌的第一部分。

- Payload（载荷），有效数据存储区，主要定义自定义字段和内置字段数据。通常会把用户信息和令牌过期时间放在这里，同样也是一个JSON对象，里面的key和value可随意设置，然后经过`Base64 URL`编码后得到JWT令牌的第二部分

  ```
  Audience (aud): Token的目标受众，通常是API或资源服务器的标识符，表明Token是给谁用的。
  Expiration Time (exp): Token的过期时间。这个时间之后，Token将不再有效。
  User Identifiers: 可能包括用户名、电子邮件地址、电话号码等用户身份信息，标记用户的身份。
  User Roles/Permissions: 用户的角色或权限信息，它们定义了用户在应用程序中可以执行哪些操作。
  Scope (scope): 指示Token允许的操作范围或访问级别，如read, write, admin等。
  Authentication Method (amr): 用户认证方法，比如密码、生物识别、多因素认证(MFA)等。
  ```

- Signature（签名）, 使用头部Header定义的加密算法，对前两部分Base64编码拼接的结果进行加密，加密时的秘钥服务私密保存，加密后的结果在通过Base64Url编码得到JWT令牌的第三部分。

- JWT令牌由Header、Payload、Signature三部分组成，每部分字符串中间用. 拼接。JWT令牌的最终格式是这样的： 第一部分.第二部分.第三部分

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/69d0be675f494f11a4a6570e1ad3c208~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.awebp)

#### 流程

1. 应用在用户认证后不需要把用户信息保存在内存中，而是将用户信息编码成JWT TOKEN，返回
2. 前端可以把token保存在localStorage或sessionStorage中,退出登录时删除Token
3. 每次请求时，前端应把Token放在HTTP请求Header的Authorization字段中，并且后端验证Token的有效性和正确性
4. 如果Token被验证通过，后端可以根据Token中的用户信息进行其他权限或业务操作，并返回相应的结果

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/867393af1fbb4d12a0fa520e8780f802~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.awebp)

#### 代码

```java
package com.example.mydreams.jwt;

import com.auth0.jwt.JWT;
import com.auth0.jwt.JWTVerifier;
import com.auth0.jwt.algorithms.Algorithm;
import com.auth0.jwt.interfaces.DecodedJWT;

import java.util.Calendar;
import java.util.HashMap;

public class JwtDemo {
    public static void main(String[] args) {
        String token = generateJwtToken();
        System.out.println("生成的JWT的Token为:");
        System.out.println(token);
        //创建验证对象
        JWTVerifier jwtVerifier = JWT.require(Algorithm.HMAC256("Periqi")).build();
        DecodedJWT verify = jwtVerifier.verify(token);
        System.out.println("验证传入的用户信息为:userId:"+verify.getClaim("userId")+",username:"+verify.getClaim("username")+",过期时间:"+verify.getExpiresAt());
    }

    //生成JWTtoken
    public static String generateJwtToken() {
        HashMap<String,Object> map = new HashMap();
        Calendar instance = Calendar.getInstance();
        instance.add(Calendar.SECOND,1000);
        String token = JWT.create()
                .withHeader(map)    //Header（可以不用设置，因为Header有默认值：{"alg":"HS256", "typ":"JWT" }）
                .withClaim("userId",100)  //payload
                .withClaim("username","码农佩奇") //payload
                .withExpiresAt(instance.getTime())  //指定令牌过期时间
                .sign(Algorithm.HMAC256("Periqi"));  //Signature
        return token;
    }
}

程序运行结果:
生成的JWT的Token为:
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE3MDE0NDI0OTYsInVzZXJJZCI6MTAwLCJ1c2VybmFtZSI6IueggeWGnOS9qeWlhyJ9.qAik5UQsDV_mjr10C3cyCqpLBvhI_IXZDRczQQy2mCk
验证传入的用户信息为:userId:100,username:"码农佩奇",过期时间:Fri Dec 01 22:54:56 CST 2023
```

#### 验证

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/7ab1ca061b3942eda9e49f4d8d1974e5~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.awebp)

服务器收到token后，取出前2部分使用私钥签名得到结果，与token第三部分比较，看是否相同

- SignatureVerificationException        签名不一致异常
- TokenExpiredException                 令牌过期异常（在payload里面设置）
- InvalidClaimException                 失效的payload异常（编码错误等）

缺点：不支持注销、Token过期机制存在缺陷、Token数量过多可能导致性能问题
