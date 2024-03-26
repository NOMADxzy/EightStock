## Spring框架

### 一、Spring基础

1. **IoC容器（Inversion of Control）**:
   - Spring 的核心是其控制反转（IoC）容器，它负责管理对象的生命周期和相互之间的依赖关系。通过依赖注入（Dependency Injection），Spring能够在运行时自动装配组件间的依赖关系。
2. **AOP（面向切面编程）**:
   - AOP 允许开发者定义跨越多个点的横切关注点（例如日志、安全等）。这样，这些关注点可以独立于业务逻辑被模块化，减少了代码重复并提高了可维护性。
3. **事务管理**:
   - Spring 提供了一致的编程式和声明式事务管理，支持编程式事务管理以及声明式事务管理，简化了对事务操作的处理。
4. **MVC框架**:
   - Spring MVC 是一个富有表现力的Web框架，允许将请求映射到处理器函数，并提供了多种视图技术以展示数据。
5. **安全**:
   - Spring Security 提供了身份验证和授权的全面和可扩展的解决方案，易于与Spring应用集成。
6. **数据访问/集成**:
   - Spring 模块化地支持对JDBC、JPA、JMS等进行抽象化，简化了数据库交互和消息传递。
7. **测试**:
   - Spring 提供了一套丰富的测试功能，旨在通过Spring TestContext Framework简化JUnit或TestNG测试的编写。

概括：**Spring 是一个轻量级的控制反转（IoC）和面向切面（AOP）的开源容器（框架）**

优点：

**轻量：**Spring 是轻量的，基本的版本大约2MB。
**控制反转：**Spring通过控制反转实现了松散耦合，对象们给出它们的依赖，而不是创建或查找依赖的对象们。
面向切面的编程(AOP)：Spring支持面向切面的编程，并且把应用业务逻辑和系统服务分开。
**容器：**Spring 包含并管理应用中对象的生命周期和配置。
MVC：框架：Spring的WEB框架是个精心设计的框架，是Web框架的一个很好的替代品。
**事务管理：**Spring 提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务（JTA）
**异常处理：**Spring 提供方便的API把具体技术相关的异常（比如由JDBC，Hibernate or JDO抛出的）转化为一致的unchecked 异常。

#### 1、控制反转(IOC)

控制反转是一种软件设计模式，反转指获得依赖对象的过程被反转了。IoC把传统模式中需要自己通过 new  实例化构造函数，或者通过工厂模式实例化的任务交给容器。通俗的来理解，就是本来当需要某个类（构造函数）的某个方法时，自己需要主动实例化变为被动，不需要再考虑如何实例化其他依赖的类，实现对象之间的解耦。

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/v2-2a1fa4b04fe10f0f377561b4056bdaf4_1440w.png)

引进了中间位置的“第三方”，也就是IOC容器，使得所有对象间没有了耦合关系，齿轮之间的传动全部依靠“第三方”了，全部对象的控制权全部上缴给“第三方”IOC容器，所以，IOC容器成了整个系统的关键核心

依赖注入(DI)和控制反转(IOC)是从不同的角度的描述的同一件事情，就是指通过引入IOC容器，利用依赖关系注入的方式，实现对象之间的解耦。

##### 价值：

1. 提升开发效率
2. 提高模块化
3. 便于单元测试

##### 流程：

当我们创建汽车 **(Car)** 这个类的时候，我们需要依赖轮子，发动机

现在引擎和轮胎的创建不再直接依赖它们的构造函数，而是通过 IoC 容器 (container) 来创建，使得 Car 类 和  Engine，Tires 没有了强耦合关系。代码中不再依赖于具体，而是依赖于 container  抽象容器，即要针对接口编程，不针对实现编程。过去思维中想要什么依赖，需要自己去 “拉” 改为抽象容器主动 “推”  给你，你只管使用实体就可以了。这是**依赖倒转**  (DIP) 的一种表现形式。

```java
import { Engine } from 'path/to/engine';
import { Tires } from 'path/to/tires';
import { Container } from 'path/to/container';

const container = new Container();
container.bind('engine', Engine);
container.bind('tires', Tires);

class Car {
  private engine;
  private tires;

  constructor() {
    this.engine = container.get('engine');
    this.tires = container.get('tires');
  }
}
```

#### 2、Spring框架

Spring framework是很多模块的集合，使用这些模块可以很方便地协助我们进行开发，比如说 Spring 支持 IoC（Inverse of Control:控制反转） 和 AOP(Aspect-Oriented  Programming:面向切面编程)、可以很方便地对数据库进行访问、可以很方便地集成第三方组件（电子邮件，任务，调度，缓存等等）、对单元测试支持比较好、支持 RESTful Java 应用程序的开发。

##### 组件：

核心的思想：不重新造轮子，开箱即用，提高开发效率。

- **spring-core** ：Spring 框架基本的核心工具类。
- **spring-beans** ：提供对 bean 的创建、配置和管理等功能的支持。
- **spring-context** ：提供对国际化、事件传播、资源加载等功能的支持。
- **spring-aop** ：提供了面向切面的编程实现。
- **spring-jdbc** ：提供了对数据库访问的抽象 JDBC。不同的数据库都有自己独立的 API 用于操作数据库，而 Java 程序只需要和 JDBC API 交互，这样就屏蔽了数据库的影响。
- **spring-tx** ：提供对事务的支持。
- **spring-jms** : 消息服务。自 Spring Framework 4.1 以后，它还提供了对 spring-messaging 模块的继承。
- **spring-web** ：对 Web 功能的实现提供一些最基础的支持。
- **spring-webmvc** ： 提供对 Spring MVC 的实现。
- **spring test：**Spring 的测试模块对 JUnit（单元测试框架）、TestNG（类似 JUnit）、Mockito（主要用来 Mock 对象）比较友好

Spring MVC ：Spring 中的一个很重要的模块，主要赋予 Spring 快速构建 MVC 架构的 Web 程序的能力。MVC  是模型(Model)、视图(View)、控制器(Controller)的简写，其核心思想是通过将业务逻辑、数据、显示分离来组织代码。

Spring Boot ：旨在简化 Spring 开发（减少配置文件，开箱即用！）

Spring IoC：将原本在程序中手动创建对象的控制权，交由 Spring 框架来管理。

![三大核心组件.png](https://gitee.com/xu_zuyun/picgo/raw/master/img/1240.png)

##### 设计模式：

- **工厂设计模式** : Spring 使用工厂模式通过 `BeanFactory`、`ApplicationContext` 创建 bean 对象。
- **代理设计模式** : Spring AOP 功能的实现。
- **单例设计模式** : Spring 中的 Bean 默认都是单例的。
- **模板方法模式** : Spring 中 `jdbcTemplate`、`hibernateTemplate` 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。
- **包装器设计模式** : 我们的项目需要连接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源。
- **观察者模式:** Spring 事件驱动模型就是观察者模式很经典的一个应用。
- **适配器模式** : Spring AOP 的增强或通知(Advice)使用到了适配器模式、spring MVC 中也是用到了适配器模式适配`Controller`。

#### 3、Spring Bean

Bean 代指的就是那些被 IoC 容器所管理的对象，我们需要告诉 IoC 容器帮助我们管理哪些对象，这个是通过配置元数据来定义的。配置元数据可以是 XML 文件、注解或者 Java 配置类。

```markdown
Bean的创建方式
1. XML 配置文件： 传统上，使用 XML 文件来定义和配置 beans。
2. 注解（Annotation-based configuration）： 使用如 @Component, @Service, @Repository, @Controller 等注解来自动注册 bean。
3. Java 配置类（Java-based configuration）： 使用 @Configuration 和 @Bean 注解来定义配置类和方法。
```

##### **将一个类声明为 Bean 的注解有哪些?**

- @Component ：通用的注解，可标注任意类为 Spring 组件。如果一个 Bean 不知道属于哪个层，可以使用@Component 注解标注。
- @Repository : 对应持久层即 Dao 层，主要用于数据库相关操作。
- @Service : 对应服务层，主要涉及一些复杂的逻辑，需要用到 Dao 层。
- @Controller : 对应 Spring MVC 控制层，主要用户接受用户请求并调用 Service 层返回数据给前端页面。

##### @Component 和 @Bean 的区别是什么？

- @Component 注解作用于类，而@Bean注解作用于方法。
- @Component 通常是通过类路径扫描来自动侦测以及自动装配到 Spring 容器中，@Bean 注解通常是我们在标有该注解的方法中定义产生这个 bean,@Bean告诉了 Spring  这是某个类的实例，当我需要用它的时候还给我。
- @Bean 注解比 @Component 注解的自定义性更强，而且很多地方我们只能通过 @Bean 注解来注册 bean。比如当我们引用第三方库中的类需要装配到 Spring容器时，则只能通过 @Bean来实现。

```java
@Configuration
public class AppConfig {
    @Bean
    public TransferService transferService() {
        return new TransferServiceImpl();
    }

}
```

##### Bean 的作用域有哪些?

**singleton** 单例 : IoC 容器中只有唯一的 bean 实例。Spring 中的 bean 默认都是单例的，是对单例设计模式的应用。

**prototype** 原型: 每次获取都会创建一个新的 bean 实例。也就是说，连续 getBean() 两次，得到的是不同的 Bean 实例。

**request** 请求（仅 Web 应用可用）: 每一次 HTTP 请求都会产生一个新的 bean（请求 bean），该 bean 仅在当前 HTTP request 内有效。

**session** 会话（仅 Web 应用可用） : 每一次来自新 session 的 HTTP 请求都会产生一个新的 bean（会话 bean），该 bean 仅在当前 HTTP session 内有效。

```xml
## 配置作用域
xml 方式：
<bean id="..." class="..." scope="singleton"></bean>
```

##### **单例 Bean 的线程安全问题了解吗？**

大部分时候我们并没有在项目中使用多线程，所以很少有人会关注这个问题。单例 Bean 存在线程问题，主要是因为当多个线程操作同一个对象的时候是存在资源竞争的。

常见的有两种解决办法：

1. 在 Bean 中尽量避免定义可变的成员变量。
2. 在类中定义一个 ThreadLocal 成员变量，将需要的可变成员变量保存在 ThreadLocal 中（推荐的一种方式）。

不过，大部分 Bean 实际都是无状态（没有实例变量）的（比如 Dao、Service），这种情况下， Bean 是线程安全的。

#### 4、Spring AoP

AOP(Aspect-Oriented  Programming:面向切面编程)能够将那些与业务无关，却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可拓展性和可维护性。

> - ### AOP组成结构：
>
> - **切面（Aspect）：** 一个关注点的模块化，这个关注点可能会横切多个对象。切面可以包含通知和切点。
>
> - **连接点（Join Point）：** 程序执行的某个特定位置，如方法调用或异常抛出的地方。在 Spring AOP 中，一个连接点总是表示一个方法的执行。
>
> - 通知（Advice）：
>
>    
>
>   切面在特定连接点上执行的动作。主要有以下类型：
>
>   - 前置通知（Before advice）：在某连接点之前执行（但不影响连接点的执行）。
>   - 后置通知（After returning advice）：在某连接点正常完成后执行。
>   - 异常通知（After throwing advice）：在方法抛出异常退出时执行。
>   - 最终通知（After advice）：无论连接点退出的方式如何都将执行的通知。
>   - 环绕通知（Around advice）：围绕一个连接点的通知，可以在方法调用前后自定义行为，甚至可以完全替换方法。
>
> - **切点（Pointcut）：** 匹配连接点的断言，在 AOP 语法中，通知与一个切点表达式关联，并在满足切点的连接点上运行。
>
> - **目标对象（Target Object）：** 被一个或多个切面所通知的对象。
>
> ```java
> import org.aspectj.lang.JoinPoint;
> import org.aspectj.lang.annotation.*;
> import org.aspectj.lang.ProceedingJoinPoint;
> import org.springframework.stereotype.Component;
> 
> @Aspect
> @Component
> public class LoggingAspect {
> 
>     // 定义切点：匹配com.example.service包下所有类的所有方法
>     @Pointcut("execution(* com.example.service.*.*(..))")
>     public void serviceLayer() {
>     }
> 
>     // 前置通知：在目标方法执行前调用
>     @Before("serviceLayer()")
>     public void logBefore(JoinPoint joinPoint) {
>         System.out.println("前置通知：即将执行方法: " + joinPoint.getSignature().getName());
>     }
> 
>     // 后置通知：在目标方法正常执行后调用
>     @AfterReturning(pointcut = "serviceLayer()", returning = "result")
>     public void logAfterReturning(JoinPoint joinPoint, Object result) {
>         System.out.println("后置通知：方法执行完成: " + joinPoint.getSignature().getName() + ", 返回值：" + result);
>     }
> 
>     // 异常通知：在目标方法抛出异常后调用
>     @AfterThrowing(pointcut = "serviceLayer()", throwing = "error")
>     public void logAfterThrowing(JoinPoint joinPoint, Throwable error) {
>         System.out.println("异常通知：方法执行异常: " + joinPoint.getSignature().getName() + ", 异常信息：" + error.getMessage());
>     }
> 
>     // 最终通知：无论目标方法如何结束都会执行
>     @After("serviceLayer()")
>     public void logAfter(JoinPoint joinPoint) {
>         System.out.println("最终通知：无论方法如何执行完毕都会调用");
>     }
> 
>     // 环绕通知：可以在目标方法前后自定义行为，也可以阻止方法的执行
>     @Around("serviceLayer()")
>     public Object logAround(ProceedingJoinPoint joinPoint) throws Throwable {
>         System.out.println("环绕通知开始：方法名：" + joinPoint.getSignature().getName());
>         try {
>             Object result = joinPoint.proceed();
>             System.out.println("环绕通知成功结束，结果是：" + result);
>             return result;
>         } catch (Throwable e) {
>             System.out.println("环绕通知捕获到异常：" + e.getMessage());
>             throw e; // 可以决定是否重新抛出异常
>         } finally {
>             System.out.println("环绕通知结束");
>         }
>     }
> }
> ```

Spring AOP 就是基于动态代理的，如果要代理的对象，实现了某个接口，那么 Spring AOP 会使用 **JDK Proxy**，去创建代理对象，而对于没有实现接口的对象，, Spring AOP 会使用 **Cglib** 生成一个被代理对象的子类来作为代理

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/v2-3438696c64b8eac308dab82c2b355ed3_1440w.webp)

| 术语              | 含义                                                         |
| ----------------- | ------------------------------------------------------------ |
| 目标(Target)      | 被通知的对象                                                 |
| 代理(Proxy)       | 向目标对象应用通知之后创建的代理对象                         |
| 连接点(JoinPoint) | 目标对象的所属类中，定义的所有方法均为连接点                 |
| 切入点(Pointcut)  | 被切面拦截 / 增强的连接点（切入点一定是连接点，连接点不一定是切入点） |
| 通知(Advice)      | 增强的逻辑 / 代码，也即拦截到目标对象的连接点之后要做的事情  |
| 切面(Aspect)      | 切入点(Pointcut)+通知(Advice)                                |

##### Spring AOP 和 AspectJ AOP 有什么区别？

**Spring AOP 属于运行时增强，而 AspectJ 是编译时增强。** Spring AOP 基于代理(Proxying)，而 AspectJ 基于字节码操作(Bytecode Manipulation)。如果我们的切面比较少，那么两者性能差异不大。但是，当切面太多的话，最好选择 AspectJ ，它比 Spring AOP 快很多。

##### AspectJ 定义的通知类型有哪些？

- **Before**（前置通知）：目标对象的方法调用之前触发
- **After** （后置通知）：目标对象的方法调用之后触发
- **AfterReturning**（返回通知）：目标对象的方法调用完成，在返回结果值之后触发
- **AfterThrowing**（异常通知） ：目标对象的方法运行中抛出 / 触发异常后触发。AfterReturning 和 AfterThrowing 两者互斥。如果方法调用成功无异常，则会有返回值；如果方法抛出了异常，则不会有返回值。
- **Around**： （环绕通知）编程式控制目标对象的方法调用。环绕通知是所有通知类型中可操作范围最大的一种，因为它可以直接拿到目标对象，以及要执行的方法，所以环绕通知可以任意的在目标对象的方法调用前后搞事，甚至不调用目标对象的方法

##### 多个切面的执行顺序如何控制？

```java
// 通常使用@Order 注解直接定义切面顺序,值越小优先级越高
@Order(3)
@Component
@Aspect
public class LoggingAspect implements Ordered {

// 实现Ordered 接口重写 getOrder 方法
@Component
@Aspect
public class LoggingAspect implements Ordered {

    // ....

    @Override
    public int getOrder() {
        // 返回值越小优先级越高
        return 1;
    }
}
```

#### 5、Spring MVC

传统Web模式：

1. Model:系统涉及的数据，也就是 dao 和 bean。
2. View：展示模型中的数据，只是用来展示。
3. Controller：处理用户请求都发送给 ，返回数据给 JSP 并展示给用户。

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/v2-bf377aa840ff80122a3ca306e76e2602_1440w.png)

随着 Spring 轻量级开发框架的流行，Spring 生态圈出现了 Spring MVC 框架， Spring MVC 是当前最优秀的 MVC 框架。相比于 Struts2 ， Spring MVC 使用更加简单和方便，开发效率更高，并且 Spring MVC 运行速度更快。

Spring MVC 下我们一般把后端项目分为 Service 层（处理业务）、Dao 层（数据库操作）、Entity 层（实体类）、Controller 层(控制层，返回数据给前台页面)。

##### 组件：

- **DispatcherServlet** ：**核心的中央处理器**，负责接收请求、分发，并给予客户端响应。
- **HandlerMapping** ：**处理器映射器**，根据 uri 去匹配查找能处理的 Handler ，并会将请求涉及到的拦截器和 Handler 一起封装。
- **HandlerAdapter** ：**处理器适配器**，根据 HandlerMapping 找到的 Handler ，适配执行对应的 Handler；
- **Handler** ：**请求处理器**，处理实际请求的处理器。
- **ViewResolver** ：**视图解析器**，根据 Handler 返回的逻辑视图 / 视图，解析并渲染真正的视图，并传递给 DispatcherServlet 响应客户端

##### 流程：

1. 客户端（浏览器）发送请求， DispatcherServlet拦截请求。
2. DispatcherServlet 根据请求信息调用 HandlerMapping 。HandlerMapping 根据 uri 去匹配查找能处理的  Handler（也就是我们平常说的 Controller 控制器） ，并会将请求涉及到的拦截器和 Handler 一起封装。
3. DispatcherServlet 调用 HandlerAdapter适配执行 Handler 。
4. Handler 完成对用户请求的处理后，会返回一个 ModelAndView 对象给DispatcherServlet，ModelAndView 顾名思义，包含了数据模型以及相应的视图的信息。Model 是返回的数据对象，View 是个逻辑上的 View。
5. ViewResolver 会根据逻辑 View 查找实际的 View。
6. DispaterServlet 把返回的 Model 传给 View（视图渲染）。
7. 把 View 返回给请求者（浏览器）

![原理.png](https://gitee.com/xu_zuyun/picgo/raw/master/img/fce7ba08d9819bdc214d03f382f00cb6~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.png)

##### 层次结构

>1. **Model 层（Model）**
>   - **数据访问对象（Data Access Object, DAO）**：与数据库的交互，负责提供数据操作的方法。
>   - **服务层（Service Layer）**：封装业务逻辑，进行事务管理。
>   - **数据传输对象（Data Transfer Object, DTO）**：在各层之间传输数据。
>2. **视图层（View）**
>   - **视图技术**：如 JSP、FreeMarker、Thymeleaf 等，用来渲染界面。
>   - **静态资源**：如 HTML、CSS、JavaScript 文件。
>3. **控制层（Controller）**
>   - **DispatcherServlet**: 作为前端控制器，所有的请求都会先经过它。它负责将请求路由到相应的处理器，并处理其他如多视图解析、国际化、主题解析等。
>   - **控制器（Controllers）**：处理用户请求，将请求映射到对应的处理逻辑，并返回响应或视图名给前端。
>   - **表单对象（Form Object）/命令对象（Command Object）**：封装客户端提交数据。
>
>除了这三个主要层次外，在实际的Spring MVC应用中，还可能有以下组件或层次：
>
>1. **Web 层**
>   - **过滤器（Filters）**：进行请求的预处理和响应的后处理。
>   - **拦截器（Interceptors）**：在控制器执行前后或视图渲染前执行特定逻辑。
>2. **配置层**
>   - **配置类或XML文件**：定义 Spring Bean、数据源、事务管理器等配置信息。
>3. **安全层**
>   - **Spring Security**：提供认证和授权的机制来保护应用程序。

DTO和DAO？

- DTO 主要用于传输跨层或跨系统边界的数据。

- DAO 专注于提供数据访问技术的抽象，例如数据库操作。

      目的：DTO 主要用于封装从一个系统到另一个系统的数据传输，是一种简单的Java对象，它用于聚合多个数据项以便于传输。
      作用范围：DTO 通常用于服务层与控制层之间或者不同应用程序/服务之间的数据传递。DTO 的目的是减少数据交换时的网络开销，并且可以定制所需展现给客户端的数据结构。
      内容：DTO 不包含任何业务逻辑，仅包含数据字段及其getter和setter方法。有时候，DTO 也可能包含一些简单的转换逻辑或校验逻辑。
      示例：如果需要展示用户信息和他们的订单数量，可以创建一个 UserOrdersDTO 类来封装这两类信息，即使用户信息和订单数量来自不同的源头。
      
      目的：DAO 是指访问数据源的对象，它封装了对数据源CRUD（创建、检索、更新、删除）操作的实现。
      作用范围：DAO 直接与数据存储（如数据库）打交道。它抽象和封装了所有与数据持久化相关的操作，使上层业务逻辑不直接依赖于底层的数据持久化细节。
      内容：DAO 通常会提供各种方法来访问数据源，如 findById, findAll, save, update, 和 delete 等。
      示例：有一个 UserDAO 类，提供了获取用户信息、保存新用户等与用户表直接关联的数据库操作方法。

DispatcherServlet？

- **请求路由**：`DispatcherServlet` 根据请求的URL来确定选择哪个控制器（Controller）进行处理，并且将请求转发给相对应的控制器。
- **控制器选择**：通过使用处理器映射（Handler Mapping）来识别URL请求所对应的控制器。
- **调用适配器**：使用处理器适配器（Handler Adapter）来执行控制器中相应的方法。
- **模型和视图解析**：控制器处理完请求后，返回一个模型和视图（ModelAndView）对象，`DispatcherServlet` 会根据这个对象来选择相应的视图技术进行渲染。

##### 软件架构

> 在软件架构中，持久层（Persistence Layer）、业务层（Business Layer）和表示层/视图层（Presentation Layer）是三个主要的层次结构，它们各自负责不同的功能区域，并且通常相互分离，以实现关注点分离（Separation of Concerns）。以下是每一层的简单介绍：
>
> ### 持久层（Persistence Layer）
>
> 持久层，也称为数据访问层（Data Access Layer），负责与数据库进行交互，执行CRUD操作（创建、读取、更新、删除）。它提供了一个抽象层使得业务逻辑可以访问数据库操作所需的数据。在Java应用中，这一层可以通过使用ORM框架如Hibernate，或者使用数据访问技术如JDBC、JPA、MyBatis来实现。
>
> ### 业务层（Business Layer）
>
> 业务层包含应用程序的业务逻辑，其核心功能是实现应用程序的业务规则。业务层协调应用程序的工作流程，处理用户请求，并对持久层发送的数据执行业务决策。在复杂应用中，业务层可能还会处理事务、授权和其他核心功能。常见的Spring注解`@Service`就是用来标记业务层的组件。
>
> ### 表示层/视图层（Presentation Layer）
>
> 表示层是用户看到并与之交互的界面，它负责数据的展示和用户输入的处理。在Web应用中，表示层可以由HTML、CSS和JavaScript组成，而在服务器端，Spring MVC框架中的`@Controller`注解被用来处理HTTP请求，并返回对应的视图名称或响应体。表示层通常从业务层获取数据，并将用户输入传送到业务层进行处理。
>
> 每层都专注于其职责范围内的特定任务，从而实现了代码的模块化和清晰的架构设计。通过这种分层，开发人员能够独立地开发和测试每个层次，提高了应用程序的维护性和可扩展性。在Spring框架中，这些层通常由不同的Spring组件来实现，比如使用@Repository注解的类实现持久层，@Service注解的类实现业务层，@Controller或@RestController注解的类实现表示层。

##### 常用Bean

![特殊bean.png](https://gitee.com/xu_zuyun/picgo/raw/master/img/705ca6f84bc8ea5a98b590c4f65263ea~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.png)

```java
<!-- 对模型视图名称的解析,即在模型视图名称添加前后缀 --> ResourceViewResolver
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/jsp/" />
    <property name="suffix" value=".jsp" />
</bean>
```

```java
<!-- 上传限制 --> MultipartResolver
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
     <!-- 上传文件大小限制为31M，31*1024*1024 -->
     <property name="maxUploadSize" value="32505856"/>
</bean>
  
  //当解析器MultipartResolver完成处理时，请求便会像其他请求一样被正常流程处理。
  @RequestMapping(path = "/form", method = RequestMethod.POST)
 public String handleFormUpload(@RequestParam("name") String name, 
            @RequestParam("file") MultipartFile file) {

   if (!file.isEmpty()) {
          byte[] bytes = file.getBytes();
          // store the bytes somewhere
          return "redirect:uploadSuccess";
    }
    return "redirect:uploadFailure";
}
```

##### 统一异常处理怎么做？

使用注解的方式统一异常处理，具体会使用到 @ControllerAdvice + @ExceptionHandler 这两个注解 。这种异常处理方式下，会给所有或者指定的 Controller 织入异常处理的逻辑（AOP），当 Controller 中的方法抛出异常的时候，由被@ExceptionHandler 注解修饰的方法进行处理。

![异常处理方式.png](https://gitee.com/xu_zuyun/picgo/raw/master/img/00a7ff39f13b0fb6757567d58045d9bb~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.png)

```java
@ExceptionHandler(Exception.class)
public Object exceptionHandler(Exception ex, HttpServletResponse response, 
          HttpServletRequest request) throws IOException {
    String url = "";
    String msg = ex.getMessage();
    Object resultModel = null;
    try {
        if (ex.getClass() == HttpRequestMethodNotSupportedException.class) {
            url = "admin/common/500";
            System.out.println("--------毛有找到对应方法---------");
        } else if (ex.getClass() == ParameterException.class) {//自定义的异常

        } else if (ex.getClass() == UnauthorizedException.class) {
            url = "admin/common/unauth";
            System.out.println("--------毛有权限---------");
        }

        String header = req.getHeader("X-Requested-With");
        boolean isAjax = "XMLHttpRequest".equalsIgnoreCase(header);
        String method = req.getMethod();
        boolean isPost = "POST".equalsIgnoreCase(method);

        if (isAjax || isPost) {
            return Message.error(msg);
        } else {
            ModelAndView view = new ModelAndView(url);
            view.addObject("error", msg);
            view.addObject("class", ex.getClass());
            view.addObject("method", request.getRequestURI());
            return view;
        }
    } catch (Exception exception) {
        logger.error(exception.getMessage(), exception);
        return resultModel;
    } finally {
        logger.error(msg, ex);
        ex.printStackTrace();
    }
}
```

### 二、Spring使用

#### 资源

Spring 框架内部使用 Resource 接口作为所有资源的抽象和访问接口，在上一篇文章的示例代码中的配置文件是通过ClassPathResource 进行封装的，ClassPathResource 是 Resource 的一个特定类型的实现，代表的是位于 classpath 中的资源。

对不同来源的资源文件 Spring 都提供了相应的实现：文件（FileSystemResource ）、ClassPath资源（ClassPathResource）、URL资源（UrlResource）、InputStream资源（InputStreamResource）、ByteArray资源（ByteArrayResource）等。

#### 注解

![spring mvc注解.png](https://gitee.com/xu_zuyun/picgo/raw/master/img/4a0c5875318183bd36274805b1793a43~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.png)

###### 宏观：

①Service

`@Service` 注解是用来声明服务层(Service Layer)组件，通常包含业务逻辑，作为数据访问层(Data Access Layer)和控制器(Controller Layer)之间的桥梁。

- 它是一个**立即加载**的注解，这意味着当 Spring 应用程序启动时，Spring 容器就会创建标注了 `@Service` 的类的实例。
- `@Service` 是一个特殊类型的 `@Component` 注解。它允许自动检测通过类路径扫描，为其创建 Bean 定义并注册到 Spring 容器中。
- 它有助于区分 Spring 组件的表示层(Controller)、服务层(Service)和数据访问层(Repository)。

②Controller

用于标记一个类作为 Spring MVC 控制器组件。控制器负责处理由 DispatcherServlet 分发的来自浏览器或其他客户端的 HTTP 请求。这个注解会将类识别为一个 Bean，并注册到 Spring 应用上下文中。

- 通常与 `@RequestMapping` 或其他基于 HTTP 方法的注解(`@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`, `@PatchMapping`)配合使用，以定义访问该控制器方法的 URL 模式和请求类型。
- 允许通过依赖注入引入其他层级的服务或组件，比如服务层 (`@Service`) 或数据访问层 (`@Repository`)。
- 与视图技术（如 Thymeleaf、JSP 等）协同工作，可以返回 String 类型的视图名，由视图解析器进一步处理。
- 当使用 Spring Boot 和 RESTful Web 服务时，通常会结合 `@RestController` 注解来使用，它是 `@Controller` 和 `@ResponseBody` 的组合体，这意味着控制器的所有响应默认都会转换成 JSON 或 XML 格式返回给客户端。

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.ui.Model;

@Controller
public class MyController {

    @RequestMapping("/greet")
    public String greet(Model model) {
        model.addAttribute("message", "Hello, World!");
        return "greeting"; // 返回视图名称，通常是一个 HTML 页面
    }
}
```

③Repository

用于标识持久层组件（如DAO组件）的特殊化 `@Component` 注解。当你在类上使用 `@Repository` 注解时，它会告知 Spring 容器该类是一个 Bean，并且用于封装数据访问异常，将底层数据访问技术抛出的异常转换为 Spring 的 `DataAccessException`。

```java
// 告诉Spring，让Spring创建一个名字叫“userDao”的UserDaoImpl实例。
@Repository(value="userDao")
public class UserDaoImpl extends BaseDaoImpl<User> {
………
}

// 注入userDao，从数据库中根据用户Id取出指定用户时需要用到
@Resource(name = "userDao")
private BaseDao<User> userDao;
```

④Configuration

`@Configuration` 是表明某个类是用来作为 bean 定义的源的。被 `@Configuration` 注解的类通常包含了一个或多个标记有 `@Bean` 注解的方法。Spring 容器会在运行时自动调用这些方法，将返回对象注册为容器中的 bean。

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {

    @Bean
    public MyService myService() {
        return new MyServiceImpl();
    }
}
// AppConfig 类被标注为 @Configuration，这意味着它可以包含 bean 定义。myService 方法上的 @Bean 注解告诉 Spring，这个方法将返回一个对象，该对象需要被注册为应用程序上下文中的一个 bean。默认情况下，bean 的名称与方法名相同，但可以通过 @Bean 注解的 name 属性来自定义。

// 这种配置方式实现了代码的模块化，并且取代了传统的 XML 配置文件，使得配置更加清晰和类型安全
```

⑤Resource

- `@Resource` 默认按照 by-name 自动装配，即根据 bean 的名称进行匹配；如果没有找到与名称匹配的 bean，则会退回到 by-type 自动装配（首先尝试依据属性名作为 bean 名称查找，如果失败则按类型装配）。而 `@Autowired` 默认按照 by-type 自动装配。
- `@Resource` 所标注的自动装配过程是在 bean 属性设置完成之后、初始化方法（如 `@PostConstruct` 注解的方法）之前进行的，而 `@Autowired` 则是在构造器、字段、setter 方法或其他任意带有参数的方法上使用。

```java
import javax.annotation.Resource;
@Component
public class MyComponent {
    // By name
    @Resource(name = "myBean")
    private MyBean myBeanByName;

    // By type (当存在多个相同类型的 bean 时，可能需要指定 name 来避免冲突)
    @Resource
    private MyBean myBeanByType;

    public void doSomething() {
        // 使用 myBeanByName 和 myBeanByType 完成一些操作...
    }
}
// @Resource 注解有两个重要的属性：name 和 type：
name: 指定要注入的 bean 的名称。
type: 指定要注入的 bean 的类型。
```



###### 微观

① Async：将方法标注为异步执行

在一个普通的方法或者类中调用了由 @Async 注解标记的方法，那么当前线程会立即返回，而实际执行的过程会在另外的线程中进行，当前线程将继续执行自身的任务。

```java
@Service
public class MyService {
  
  @Autowired
  private EmailService emailService;

  @Async
  public void sendEmails(List<String> recipients, String content) {

    for (String recipient : recipients) {
      emailService.sendEmail(recipient, content);
    }

    System.out.println("All emails sent!");
  }
}
```

②Autowired：自动装配依赖的 bean。

@Autowired 注解是 Spring Framework 中的一个依赖注入注解，它可以自动将容器中已经创建好的 bean 对象装配到需要他们的类或者方法中。**默认先按byType进行匹配，如果发现找到多个bean，则又按照byName方式进行匹配，如果还有多个，则报出异常**

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class MyComponent {

    private final Dependency dependency;

    // 构造器注入，如果容器中不存在Dependency类型的Bean，应用将无法启动
    @Autowired(required = true)
    public MyComponent(Dependency dependency) {
        this.dependency = dependency;
    }
    
    // 如果容器中不存在OptionalDependency类型的Bean，则dependency字段将为null，但应用依旧会启动
    @Autowired(required = false)
    private OptionalDependency optionalDependency;
}
```

③Bean：声明一个需要被 Spring 管理的 bean

```java
@Configuration // 添加了 @Configuration 注解来标识这是一个配置类，因此 Spring 容器在扫描时会识别并加载该类。
public class MyConfiguration {
  @Bean
  public UserDao userDao() {
    return new JdbcUserDao(dataSource());
  }
  @@Bean(“myBean”) // 这样可以在装配其他依赖项时方便地引用该 bean 对象。
  public DataSource dataSource() {
    // 创建并配置数据源对象
    return new HikariDataSource(config);
  }
}
```

在上面的代码中，我们定义了一个配置类 MyConfiguration，并在其中定义了两个 bean 对象：userDao() 和 dataSource()。其中 userDao() 方法返回 JdbcUserDao 对象，而 dataSource() 方法返回一个 Hikari 数据源对象。这两个方法都标记了 @Bean 注解，这意味着它们会被 Spring 容器扫描到并将创建的对象注册到容器中。

④Cacheable：增加缓存支持

@Cacheable 是 Spring Framework 中的一个注解，用于实现方法级别的缓存。使用 @Cacheable 注解的方法在调用时，将会检查缓存中是否存在与该方法所需参数相对应的缓存项。如果存在，则直接返回缓存结果，不再执行被注解的方法；如果不存在，则执行该方法，并将方法的返回结果添加到缓存中。

```
@Service
public class UserService {

  private final UserMapper userMapper;

  public UserService(UserMapper userMapper) {
    this.userMapper = userMapper;
  }

  @Cacheable(value = "users", key = "#userId")
  public User getUserById(int userId) {
    return userMapper.getUserById(userId);
  }

}
```

⑤ Conditional

Conditional用于根据特定条件动态决定是否创建某个 Bean 实例，Spring 还提供了一些预定义的条件注解，如 @Profile、@ConditionalOnMissingClass、@ConditionalOnProperty 等，它们可以方便地满足一些常见的判断场景，减少了编写自定义条件类的工作量。

```java
@Configuration
public class MyConfiguration {
  @Bean
  @Conditional(MyCondition.class)
  public MyBean myBean() {
    return new MyBean();
  }
}

public class MyCondition implements Condition {
  @Override
  public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
    // TODO: 自定义条件判断逻辑
    return true; // 当结果为 true 时，表示条件成立，按照要求创建 Bean；否则，则不会创建这个 Bean。
  }
}
```

```java
使用 @Profile 注解可以确保只有相关的 beans 在特定的环境配置下被创建，这样可以避免在不适合的环境中运行可能引起冲突或异常的 beans，并有助于维护清晰的环境特定配置。
@Profile("development")
@Profile("production")

根据设置系统属性: -Dspring.profiles.active=development
或者使用环境变量设置 SPRING_PROFILES_ACTIVE=development等来切换profile
```

⑥ConstructorBinding

用于表示在使用基于构造器的依赖注入时，应该将配置属性绑定到构造器参数上

```java
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.context.properties.ConstructorBinding;

@ConstructorBinding
@ConfigurationProperties(prefix = "app") //指定哪个前缀的配置属性应被绑定到该类的字段上,Spring Boot 将会查找 app.name 和 app.threadPoolSize 配置项，并通过构造器自动注入相应的值。
public class AppConfigProperties {

    private final String name;
    private final int threadPoolSize;

    public AppConfigProperties(String name, int threadPoolSize) {
        this.name = name;
        this.threadPoolSize = threadPoolSize;
    }

    // Getters for the fields
    public String getName() {
        return name;
    }

    public int getThreadPoolSize() {
        return threadPoolSize;
    }
}
```

⑦ControllerAdvice

@ControllerAdvice 是 Spring MVC 框架中的注解，用于定义一个全局性的异常处理器类。可以对 Controller 层面和全局异常进行统一的处理。

```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler({NullPointerException.class, IllegalArgumentException.class}) //当捕获到空指针异常或者非法参数异常时，会执行这个方法，将异常信息添加到 request 中，并返回 “error” 视图。
    public String handleException(Exception e, HttpServletRequest request) {
        request.setAttribute("error", e);
        return "error";
    }
}
```

⑧CrossOrigin

同源策略阻止一个域下的文档或脚本与另一个域下的资源进行交互。这有助于保护用户免受恶意网站的攻击，但也限制了合法的跨源请求。

```
例如，如果你的页面在 https://www.example.com 上运行，而尝试通过 AJAX 请求从 https://api.another-site.com 获取数据，这将被默认视为跨域请求，并可能遭到浏览器的阻止，除非目标服务器明确允许来自原始域的请求。
```

```java
@RestController
@RequestMapping("/api")
public class ApiController {

    @CrossOrigin(origins = "http://localhost:8080")
    @GetMapping("/users/{id}")
    public User getUserById(@PathVariable Long id) {
        // ...
    }
}
```

⑨EnableAsync

在处理一些 IO 密集型操作时，非常有用

```java
@Configuration
@EnableAsync // @EnableAsync 注解需要与异步方法搭配使用，异步方法需要用 @Async 标记。
public class AppConfig {

    @Bean
    public Executor taskExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(10);
        executor.setMaxPoolSize(20);
        executor.setQueueCapacity(30);
        executor.initialize();
        return executor;
    }
}
```

⑩RequestMapping

`@RequestMapping` 的专门化版本提供了更简洁的语法。这些包括 `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`, `@PatchMapping`。每个都对应于一个 HTTP 方法，使得代码更加简洁易读。

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("/api")
public class MyController {

    @GetMapping("/hello") // 等同于 @RequestMapping(value = "/hello", method = GET)
    @ResponseBody
    public String helloWorld() {
        return "Hello, World!";
    }

    @PostMapping("/submit") // 等同于 @RequestMapping(value = "/submit", method = POST)
    @ResponseBody
    public String submitForm() {
        // 方法实现
        return "Form submitted";
    }
}

```

###### 其他

①lazy

@Lazy 是 Spring 框架中的注解之一，它用于控制 Bean 的实例化时间。通过在容器中标记一个 Bean 为 @Lazy，可以让 Spring 容器在第一次使用这个 Bean 时再进行实例化。相对应的，如果不加 @Lazy 注解，则默认情况下 Spring 容器会在启动时就实例化该 Bean。
```java
@Service
@Lazy // 使用 @Lazy 注解，Spring 容器将在第一次使用 UserService 的实例时才进行实例化而不是在启动时就实例化
public class UserService {
    // ...
}
```

②Import

允许我们在一个配置类中引入其他配置类或普通的 Java 类

```java
@Configuration
@Import({DataSourceConfig.class, RedisConfig.class, ServiceUtils.class})
public class AppConfig {
    // ...
}
// 在创建 Spring ApplicationContext 时，这三个配置类都会被加载并注册为 Spring 的 bean。
// 可以在 AppConfig 中使用 ServiceUtils 类中的方法或属性
```

③元注解

- `@Target`：指定注解可以应用于 Java 的哪些元素（如类、方法、字段等）。
- `@Retention`：指定注解在什么级别可用（源码、类文件或运行时）。
- `@Documented`：指定注解是否应该被 javadoc 工具记录。
- `@Inherited`：指定注解是否可以被子类继承。
- `@Component`：Spring 特有的元注解，用于声明一个类是 Spring 组件。其它注解如 `@Service`, `@Repository`, `@Controller` 都是用 `@Component` 注解的例子。

④Valid

在控制器方法的参数列表中使用 `@Valid` （ @NotNull、@Min、@Max、@Size、@Email...）注解来实现请求参数的校验。

```java
@RestController
@RequestMapping("/users")
public class UserController {
    @PostMapping
    public User createUser(@Valid @RequestBody User user) {
        // 保存用户信息到数据库
    }

    // ...
}

// 校验的规则通常定义在 User 类的属性上
public class User {
    @NotBlank
    private String username;

    @Pattern(regexp = "^[a-zA-Z0-9]+@[a-zA-Z0-9]+\\.[a-zA-Z]{2,}$", message = "Email 格式错误")
    private String email;
}
```

###### 第三方

1. ### `@Data`

   `@Data`注解是Lombok库的一部分，非Spring框架本身的一部分。当你在一个类上使用`@Data`注解，Lombok会自动生成以下代码：

   - 所有字段的getter方法
   - 所有非最终字段的setter方法
   - `toString()`方法
   - `equals()`和`hashCode()`方法
   - 一个包含所有非静态、非瞬态字段的构造函数

   这大大减少了样板代码的数量，使得类的定义更加简洁

2. ### `@Mapper`

   `@Mapper`注解是MyBatis框架的一部分，它主要用于标记一个接口作为数据库操作的映射接口。MyBatis通过这个接口关联XML配置文件或者注解中定义的数据库操作。

   ```java
   @Mapper
   public interface UserMapper {
       User selectUserById(Long id);
   }
   
   // 在Spring Boot项目中，通常还会配合使用@MapperScan注解来指定需要扫描的@Mapper接口所在的包路径。
   @SpringBootApplication
   @MapperScan("com.example.project.mapper")
   public class Application {
       public static void main(String[] args) {
           SpringApplication.run(Application.class, args);
       }
   }
   ```

#### 事务

声明式事务管理是一种基于 AOP 的方法，它将事务管理逻辑从业务代码中抽象出来。在 Spring 中，声明式事务管理通常是通过使用 `@Transactional` 注解来实现的。

可以设置多个属性以定制事务的行为，如 `isolation` (隔离级别), `propagation` (传播行为), `readOnly` (是否只读), `timeout` (超时时间)等

```java
import org.springframework.transaction.annotation.Transactional;

@Service
public class MyTransactionalService {

    @Transactional
    public void performBusinessOperation() {
        // 执行需要被事务化保护的操作 ,被包裹在一个事务中执行。如果方法成功完成，事务将自动提交；如果方法抛出运行时异常，事务将自动回滚。
    }
}
```

事务管理器：Spring 提供了不同类型的事务管理器，以支持不同类型的数据访问资源。例如 `DataSourceTransactionManager` 适用于 JDBC 操作，而 `JpaTransactionManager` 适用于 JPA 操作，还有其他特定技术栈的事务管理器。

```java
事务管理器通常会被定义为 Spring 应用程序上下文中的一个 bean，并通过依赖注入与其它组件（如服务）结合使用。
@Bean
public PlatformTransactionManager transactionManager(DataSource dataSource) {
    return new DataSourceTransactionManager(dataSource);
}
```

#### 例子

>### 步骤1:创建项目（（https://start.spring.io/）
>
>### 步骤2: 配置application.properties
>
>在src/main/resources目录下的`application.properties`文件中配置数据库连接、MyBatis和Redis。假设MySQL数据库已经安装在本地并且有一个名为`test_db`的数据库，Redis服务也运行在本地默认端口。
>
>```properties
># DataSource配置
>spring.datasource.url=jdbc:mysql://localhost:3306/test_db
>spring.datasource.username=root
>spring.datasource.password=yourpassword
>
># MyBatis配置
>mybatis.mapper-locations=classpath:mappers/*.xml
>mybatis.type-aliases-package=com.example.demo.entity
>
># Redis配置
>spring.redis.host=localhost
>spring.redis.port=6379
>```
>
>确保替换`yourpassword`为你自己的数据库密码。
>
>### 步骤3: 实体类
>
>在src/main/java/com/example/demo/entity目录中，创建一个名为`User.java`的实体类。
>
>```java
>package com.example.demo.entity;
>
>import lombok.Data;
>
>@Data
>public class User {
>    private Long id;
>    private String name;
>    private String email;
>}
>```
>
>### 步骤4: Mapper接口
>
>创建一个Mapper接口用于定义数据库操作。在src/main/java/com/example/demo/mapper目录中，创建一个名为`UserMapper.java`的接口。
>
>```java
>package com.example.demo.mapper;
>
>import com.example.demo.entity.User;
>import org.apache.ibatis.annotations.Mapper;
>import org.apache.ibatis.annotations.Select;
>
>@Mapper
>public interface UserMapper {
>    @Select("SELECT * FROM users WHERE id = #{id}")
>    User findById(Long id);
>}
>```
>
>请注意，`users`应该是你数据库中对应的表名。
>
>### 步骤5: Mapper XML
>
>在src/main/resources/mappers目录中，创建一个名为`UserMapper.xml`的MyBatis映射文件。
>
>```xml
><!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
>"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
><mapper namespace="com.example.demo.mapper.UserMapper">
>    <select id="findById" resultType="com.example.demo.entity.User">
>        SELECT * FROM users WHERE id = #{id}
>    </select>
></mapper>
>```
>
>### 步骤6: Service层
>
>创建一个Service类来处理业务逻辑。在src/main/java/com/example/demo/service目录中，创建一个名为`UserService.java`的类。
>
>```java
>package com.example.demo.service;
>
>import com.example.demo.entity.User;
>import com.example.demo.mapper.UserMapper;
>import org.springframework.beans.factory.annotation.Autowired;
>import org.springframework.cache.annotation.Cacheable;
>import org.springframework.stereotype.Service;
>
>@Service
>public class UserService {
>
>    private final UserMapper userMapper;
>
>    @Autowired
>    public UserService(UserMapper userMapper) {
>        this.userMapper = userMapper;
>    }
>
>    @Cacheable(value = "users", key = "#id")
>  // value = "users"：指定缓存组件的名称，在配置中引用。这里的"users"代表这个缓存组件的唯一标识。Spring会检查缓存区域（Cache）是否存在一个名为"users"的缓存。如果不存在，Spring会自动创建这个缓存。
>  
>  > 在Redis中，"users"只是一个逻辑概念，用于标识存储在Redis服务器上特定数据集的键的前缀或命名空间。实际存储的键可能看起来像users::1234，其中1234是用户的ID，而且通常Redis并不会有一个物理的结构或容器叫做"users"。相反，所有的键都存储在全局的键空间中，但是由Spring管理，它在内部使用合适的键名前缀来模拟区域的概念。
>  
>  // key = "#id"：表示缓存数据时所用的键值。这里使用了SpEL（Spring Expression Language），#id表明传入的id参数将作为缓存的键。
>    public User getUserById(Long id) {
>        return userMapper.findById(id);
>    }
>}
>```
>
>### 步骤7: Controller层
>
>创建Controller类来处理HTTP请求。在src/main/java/com/example/demo/controller目录中，创建一个名为`UserController.java`的类。
>
>```java
>package com.example.demo.controller;
>
>import com.example.demo.entity.User;
>import com.example.demo.service.UserService;
>import org.springframework.beans.factory.annotation.Autowired;
>import org.springframework.web.bind.annotation.*;
>
>@RestController
>@RequestMapping("/users")
>public class UserController {
>
>    private final UserService userService;
>
>    @Autowired
>    public UserController(UserService userService) {
>        this.userService = userService;
>    } //1. 构造函数注入： 这种方法的优点是可以创建不可变的对象，并且注入的依赖在对象创建时就已确定，无法更改。这也被视为最推荐的注入方式。
>      //2. Setter 方法注入: 适用于可选依赖和可变对象
>      //3. 字段注入: @Autowired 
>                  //private Dependency dependency;
>      //   它使单元测试变得更加困难，并增加了类的耦合性
>      //4. @Resource 或 @Inject 替换 @Autowired
>
>    @GetMapping("/{id}")
>    public User getUserById(@PathVariable Long id) {
>        return userService.getUserById(id);
>    }
>}
>```
>
>### 步骤8: 启用缓存
>
>在主应用类上添加`@EnableCaching`注解以启用缓存。
>
>```java
>package com.example.demo;
>
>import org.springframework.boot.SpringApplication;
>import org.springframework.boot.autoconfigure.SpringBootApplication;
>import org.springframework.cache.annotation.EnableCaching;
>
>/* 
>SpringBootApplication包含以下三个：
>1. @Configuration：标注一个类作为bean定义的源。
>2. @EnableAutoConfiguration：告诉Spring Boot根据添加的jar依赖猜测你可能需要配置的beans。例如，如果spring-webmvc在classpath上，它会假定你正在开发一个web应用，并相应地对Spring进行设置。
>3. @ComponentScan：告诉Spring去搜索其他组件、配置和服务，在路径上，寻找带有@Component、@Service、@Controller等注解的类。
>*/
>@SpringBootApplication
>@EnableCaching 
>public class DemoApplication {
>    public static void main(String[] args) {
>        SpringApplication.run(DemoApplication.class, args);
>    }
>
>}
>```
>
>### 步骤9: 运行应用
>
>编译并运行应用。现在你可以向`http://localhost:8080/users/{id}`发送GET请求来测试你的应用，它应该返回相应的用户对象，并且结果会被缓存在Redis中。

### 三、问题

#### 1、gin的context和spring的model有什么区别？

- **设计**：Gin的`Context`是针对每个请求的上下文，提供了关于当前HTTP请求的全部相关信息和操作；而Spring MVC的`Model`仅仅是关于视图渲染所需数据的存储和访问。

- **职责范围**：Gin的`Context`范围更广，涉及请求的处理和响应的构建；Spring MVC的`Model`主要关注于持有和传递控制器生成的数据，以便提供给视图层。

  ```java
  @Controller
  public class GreetingController {
  
      @GetMapping("/greet")
      public String greet(@RequestParam(name = "name", defaultValue = "Guest") String name, Model model) {
          // 向模型添加属性
          model.addAttribute("message", "Hello " + name);
          // 返回视图的名称
          return "greeting";
      }
  }
  ```

  ```go
  package main
  
  import "github.com/gin-gonic/gin"
  
  func main() {
      r := gin.Default()
  
      r.GET("/hello", func(c *gin.Context) {
          name := c.DefaultQuery("name", "Guest")
          // 设置HTTP响应信息
          c.JSON(200, gin.H{
              "message": "Hello " + name,
          })
      })
  
      r.Run(":8080")
  }
  ```


#### 2、Spring和SpringBoot的区别是什么？

Spring Framework 是一个开源的Java平台，它最初被设计用来**简化企业级应用的开发**。它是一个轻量级、综合性的框架，提供了以下特性：

- **依赖注入（DI）**：允许对象定义它们所依赖的对象，而不是创建它们。这促进了松耦合。
- **面向切面编程（AOP）**：支持将方法拦截器和切点声明为逻辑独立于程序的主体部分的方面。
- **事务管理**：提供一致的编程模型来管理事务。
- **MVC框架**：Web开发者可以使用Spring的模型-视图-控制器实现。
- **各种辅助功能**：如数据访问、消息传递、测试和更多。

Spring 的强大功能使得开发复杂的应用成为可能，但同时也导致了更高的配置复杂性。使用Spring需要明确配置大量的XML或注释驱动的配置项。

Spring Boot 是基于Spring构建的，旨在使创建Spring应用变得更快、更容易。它主要提供了以下优势：

- **自动配置**：根据类路径下的jar包、Spring Boot会自动配置你的应用。
- **起步依赖**：通过Maven或Gradle等构建工具，你可以快速添加起步依赖来简化构建配置。
- **内嵌Tomcat, Jetty或Undertow**：不需要部署WAR文件，可以直接运行应用。
- **没有代码生成和XML配置**：不再需要繁琐的XML配置，只需要很少的配置来启动应用。
- **应用监控**：提供了多种监控端点，以检查应用状态。

简言之，Spring Boot 实际上不替代Spring Framework，它是为了简化Spring应用的初始搭建及开发过程。Spring Boot 允许开发者更快地启动和运行新的Spring应用，并减少了开发者需要写的样板代码、XML配置和注解。通过Spring Boot，开发者能够更专注于业务逻辑的开发，而不是花费时间在配置上。
