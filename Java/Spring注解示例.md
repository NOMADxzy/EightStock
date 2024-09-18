### 示例

#### 1.@component

```java
// 定义一个服务接口和服务实现类：
package com.example.demo.service;

public interface MessageService {
    String getMessage();
}

// 服务实现类，使用@Component注解
package com.example.demo.service.impl;

import org.springframework.stereotype.Component;

@Component("messageService") // 这里可以指定Bean的名称，默认为类名首字母小写
public class MessageServiceImpl implements MessageService {
    @Override
    public String getMessage() {
        return "Hello, this is a message from a component!";
    }
}
```

```java
//为了能够自动发现并注册上面定义的Bean，我们需要确保Spring在启动时会扫描包含@Component注解的类所在的包。这通常在主配置类中使用@ComponentScan注解来完成：
// 主配置类
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ComponentScan;

@SpringBootApplication
@ComponentScan(basePackages = {"com.example.demo.service.impl"}) // 指定扫描的包路径
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

```java
最后，我们可以在其他需要使用这个服务的类中，通过@Autowired注解来注入这个Bean：
// 其他类中使用MessageService
package com.example.demo.controller;

import com.example.demo.service.MessageService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class DemoController {
    private final MessageService messageService;

    @Autowired
    public DemoController(MessageService messageService) {
        this.messageService = messageService;
    }

    @GetMapping("/getMessage")
    public String getMessage() {
        return messageService.getMessage();
    }
}
```

#### 2.@Bean

```java
// 接口
package com.example.demo.service;

public interface LogService {
    void logInfo(String message);
}
// 日志服务实现类
package com.example.demo.service.impl;

import com.example.demo.service.LogService;
import org.springframework.stereotype.Component;

@Component
public class ConsoleLogService implements LogService {
    @Override
    public void logInfo(String message) {
        System.out.println("INFO: " + message);
    }
}
```

```java
package com.example.demo.config;

import com.example.demo.service.LogService;
import com.example.demo.service.impl.ConsoleLogService;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Primary;
import org.springframework.beans.factory.annotation.Qualifier;

@Configuration
public class AppConfig {

    @Bean
    @Primary // 标记为首选Bean，如果有多个相同类型的Bean，此Bean将优先被注入
    public LogService logService() {
        return new ConsoleLogService();
    }
}
```

```java
// 使用Bean
package com.example.demo.controller;

import com.example.demo.service.LogService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class DemoController {

    private final LogService logService;

    @Autowired
    public DemoController(LogService logService) {
        this.logService = logService;
    }

    @GetMapping("/log")
    public String logSomething() {
        logService.logInfo("This is an info message.");
        return "Logged successfully!";
    }
}
```

#### 3.@Qualifier

```java
// 数据库连接池接口
public interface DataSource {
    void connect();
}
// 实现一
@Component("readDataSource")
public class ReadDataSource implements DataSource {
    @Override
    public void connect() {
        System.out.println("Connecting to the read database...");
    }
}
// 实现二

@Component("writeDataSource")
public class WriteDataSource implements DataSource {
    @Override
    public void connect() {
        System.out.println("Connecting to the write database...");
    }
}
```

```java
// 使用
@Service
public class DataService {
    private final DataSource readDataSource;
    private final DataSource writeDataSource;
    @Autowired
    public DataService(
            @Qualifier("readDataSource") DataSource readDataSource,
            @Qualifier("writeDataSource") DataSource writeDataSource) {
        this.readDataSource = readDataSource;
        this.writeDataSource = writeDataSource;
    }
    public void performReadWriteOperations() {
        readDataSource.connect(); // 使用读数据库连接池
        writeDataSource.connect(); // 使用写数据库连接池
    }
}
```

#### 4.装配方式——@Resource、@Autowired

##### 区别

默认装配方式：

- `@Autowired`默认按照**类型**（by-type）进行装配，如果容器中有多个相同类型的Bean，可能会导致歧义，此时需要配合`@Qualifier`注解指定具体Bean的名称来解决冲突。
- `@Resource`默认按照**名称**（by-name）进行装配，即它会尝试根据Bean的名称（通常是类名首字母小写）在Spring容器中查找匹配的Bean。如果找不到匹配的名称，`@Resource`也会回退到类型匹配（类似于`@Autowired`），但是这种行为取决于具体的容器实现。

必要性：

- `@Autowired`默认要求被注解的依赖必须存在，如果不希望抛出异常，可以设置其`required`属性为`false`。
- `@Resource`没有直接的required属性，但如果找不到匹配的Bean，同样会抛出异常。

使用位置：

- 两者都可以用在字段、setter方法和构造函数上，但是由于它们的特性不同，实际使用时可能会有偏好选择。例如，如果更倾向于按名称装配，可能会倾向于使用`@Resource`。

#### 5.@Configuration

##### - 作用

**组合配置**：配置类本身可以相互引用，通过`@Import`注解导入其他配置类，从而允许模块化和分层的配置结构。

**自动装配**：`@Configuration`类能被Spring Boot的自动配置机制识别，有助于简化配置过程。自动配置类通常会扫描带有特定注解（如`@Configuration`、`@Component`等）的类，并根据条件自动装配它们。

**条件化配置**：在配置类中，可以使用`@Conditional`注解来基于特定条件决定是否创建某个Bean，这为配置提供了灵活性和动态性。

**外部化配置支持**：与`@Value`、`@ConfigurationProperties`等注解结合使用时，可以轻松实现配置属性的外部化管理，使得应用配置更加灵活且易于部署到不同环境。

**生命周期管理**：配置类还可以控制Bean的生命周期，例如通过`@PostConstruct`、`@PreDestroy`注解或`InitializingBean`、`DisposableBean`接口来执行初始化和销毁方法。

##### - 示例

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {

    // 定义一个名为myService的Bean，Bean的名称默认为方法名（这里是myService）。
    @Bean
    public MyService myService() {
        return new MyServiceImpl();
    }

    // 定义另一个Bean，这个Bean依赖于myService
    @Bean
    public MyController myController() {
        return new MyController(myService()); //建立了Bean之间的依赖关系
    }
}
```

```java
// 使用
public class Application {

    public static void main(String[] args) {
        ApplicationContext context = 
            new AnnotationConfigApplicationContext(AppConfig.class);

        MyController controller = context.getBean(MyController.class);
        controller.handleRequest(); // 假设MyController有个handleRequest方法
    }
}
```

#### 6.生命周期注解

**`@PostConstruct`**: 已经提过，是Java EE规范中定义的注解，用于标记在依赖注入完成后立即执行的方法。在Spring框架中也得到了广泛支持。

**`@PreDestroy`**: 同样是Java EE规范的一部分，与`@PostConstruct`相对应，用于标记在Bean销毁之前执行的方法，进行清理工作。

```java
package com.example.demo;

import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;
import org.springframework.stereotype.Component;

@Component
public class MyService {

    public MyService() {
        System.out.println("MyService is being constructed.");
    }

    @PostConstruct
    public void init() {
        System.out.println("@PostConstruct: Initializing MyService...");
        // 这里可以执行一些初始化操作，如数据库连接初始化、资源加载等
    }

    @PreDestroy
    public void cleanup() {
        System.out.println("@PreDestroy: Cleaning up MyService...");
        // 这里执行清理操作，如关闭数据库连接、释放资源等
    }
}
```

#### 7.元注解

**`@Retention`：**

- 用途

  ：这个元注解用来定义被它修饰的注解的生命周期。生命周期有三种：

  - `RetentionPolicy.SOURCE`：注解只存在于源代码中，编译时会被忽略，不会保存在.class文件里。
  - `RetentionPolicy.CLASS`：默认值，注解会被编译器保留到字节码文件中，但JVM加载类时不会保留这些注解信息。
  - `RetentionPolicy.RUNTIME`：注解不仅会被编译器保留到字节码文件中，JVM加载类时也会保留这些注解信息，因此可以在运行时通过反射读取到这些注解信息。

- **示例**：`@Retention(RetentionPolicy.RUNTIME)` 表示该注解在运行时仍然可见，可以被反射机制访问。

**`@Target`：**

- **用途**：用来指定被它所注解的注解可以用在哪些类型的元素上。比如，可以用在类、方法、字段、参数等上。

- 取值

  ：

  ```
  ElementType
  ```

   枚举类型的值，包括：

  - `TYPE`：用于类、接口（包括注解类型）或枚举声明。
  - `METHOD`：用于方法声明。
  - `FIELD`：用于字段声明（包括枚举常量）。
  - `PARAMETER`：用于方法参数声明。
  - `CONSTRUCTOR`：用于构造器声明。
  - `LOCAL_VARIABLE`：用于局部变量声明。
  - `ANNOTATION_TYPE`：用于注解类型声明。
  - `PACKAGE`：用于包声明。

- **示例**：`@Target(ElementType.METHOD)` 表示该注解只能用于方法上。

#### 8.AOP注解

##### 目标业务类

```java
package com.example.demo;

public class BusinessService {
    
    public void doBusinessLogic() {
        System.out.println("Executing business logic...");
    }
}
```

##### 切面类

```java
package com.example.demo;

public class BusinessService {
    
    public void doBusinessLogic() {
        System.out.println("Executing business logic...");
    }
}
```

配置Spring启用AOP

```java
package com.example.demo;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

@Configuration
@EnableAspectJAutoProxy // 启用AspectJ自动代理
public class AppConfig {

    @Bean
    public BusinessService businessService() {
        return new BusinessService();
    }

    @Bean
    public LoggingAspect loggingAspect() {
        return new LoggingAspect();
    }
}
```

