## RESTful API

REST（英文：Representational State  Transfer，简称REST，直译过来表现层状态转换）是一种软件架构风格、设计风格，而不是标准，只是提供了一组设计原则和约束条件。它主要用于客户端和服务器交互类的软件。

一套**层次结构清晰、便于理解、便于扩展、易于实现缓存**让大部分人都能够理解接受的接口风格

![image-20240310195652876](https://gitee.com/xu_zuyun/picgo/raw/master/img/image-20240310195652876.png)

### 特征

**以资源为基础** ：资源可以是一个图片、音乐、一个XML格式、HTML格式或者JSON格式等网络上的一个实体，除了一些二进制的资源外普通的文本资源更多以JSON为载体、面向用户的一组数据(通常从数据库中查询而得到)。

```markdown
资源是REST系统的核心概念。 所有的设计都是以资源为中心
## 什么资源？
1.商品加入购物车 购物车
2.提交订单 订单
3.创建用户 用户

围绕资源进行 添加，获取，修改，删除，以及对符合特定条件的资源进行列表操作 。针对资源设计接口
每个资源都拥有一个资源标识，每个资源的资源标识可以用来唯一地标明该资源

## 哪种方式更符合RESTful?
/api/users/1
/api/user/1
第一种方式 /api/users/1 更常见且稍微更符合RESTful命名约定。它使用复数形式的“users”表明这是“用户”这一资源集合的一部分，1是该资源集合中特定用户的唯一标识符。
```

**统一接口 使用状态码**:  对资源的操作包括获取、创建、修改和删除，这些操作正好对应HTTP协议提供的GET、POST、PUT和DELETE方法。换言而知，使用RESTful风格的接口但从接口上你可能只能定位其资源，但是无法知晓它具体进行了什么操作，需要具体了解其发生了什么操作动作要从其HTTP请求方法类型上进行判断。具体的HTTP方法和方法含义如下：

- GET（SELECT）：从服务器取出资源（一项或多项）。
- POST（CREATE）：在服务器新建一个资源。
- PUT（UPDATE）：在服务器更新资源（客户端提供完整资源数据）。
- PATCH（UPDATE）：在服务器更新资源（客户端提供需要修改的资源数据）。
- DELETE（DELETE）：从服务器删除资源。

```
GET
安全且幂等
    获取表示
    变更时获取表示（缓存）

    200（OK） - 表示已在响应中发出

    204（无内容） - 资源有空表示
    301（Moved Permanently） - 资源的URI已被更新
    303（See Other） - 其他（如，负载均衡）
    304（not modified）- 资源未更改（缓存）
    400 （bad request）- 指代坏请求（如，参数错误）
    404 （not found）- 资源不存在
    406 （not acceptable）- 服务端不支持所需表示
    500 （internal server error）- 通用错误响应
    503 （Service Unavailable）- 服务端当前无法处理请求
POST
    不安全且不幂等
    使用服务端管理的（自动产生）的实例号创建资源
    创建子资源
    部分更新资源
    如果没有被修改，则不过更新资源（乐观锁）
    200（OK）- 如果现有资源已被更改
    201（created）- 如果新资源被创建
    202（accepted）- 已接受处理请求但尚未完成（异步处理）
    301（Moved Permanently）- 资源的URI被更新
    303（See Other）- 其他（如，负载均衡）
    400（bad request）- 指代坏请求
    404 （not found）- 资源不存在
    406 （not acceptable）- 服务端不支持所需表示
    409 （conflict）- 通用冲突
    412 （Precondition Failed）- 前置条件失败（如执行条件更新时的冲突）
    415 （unsupported media type）- 接受到的表示不受支持
    500 （internal server error）- 通用错误响应
    503 （Service Unavailable）- 服务当前无法处理请求
PUT
    不安全但幂等
    用客户端管理的实例号创建一个资源
    通过替换的方式更新资源
    如果未被修改，则更新资源（乐观锁）
    200 （OK）- 如果已存在资源被更改
    201 （created）- 如果新资源被创建
    301（Moved Permanently）- 资源的URI已更改
    303 （See Other）- 其他（如，负载均衡）
    400 （bad request）- 指代坏请求
    404 （not found）- 资源不存在
    406 （not acceptable）- 服务端不支持所需表示
    409 （conflict）- 通用冲突
    412 （Precondition Failed）- 前置条件失败（如执行条件更新时的冲突）
    415 （unsupported media type）- 接受到的表示不受支持
    500 （internal server error）- 通用错误响应
    503 （Service Unavailable）- 服务当前无法处理请求
DELETE
    不安全但幂等
    删除资源
    200 （OK）- 资源已被删除
    301 （Moved Permanently）- 资源的URI已更改
    303 （See Other）- 其他，如负载均衡
    400 （bad request）- 指代坏请求
    404 （not found）- 资源不存在
    409 （conflict）- 通用冲突
    500 （internal server error）- 通用错误响应
    503 （Service Unavailable）- 服务端当前无法处理请求
```



**无状态**：服务器不能保存客户端的信息， 每一次从客户端发送的请求中，要包含所有必须的状态信息，会话信息由客户端保存， 服务器端根据这些状态信息来处理请求。  当客户端可以切换到一个新状态的时候发送请求信息， 当一个或者多个请求被发送之后, 客户端就处于一个状态变迁过程中。  每一个应用的状态描述可以被客户端用来初始化下一次的状态变迁。

```markdown
为什么要无状态？
1. 可伸缩性（Scalability）
由于服务端不需要保存客户端的状态，它可以更容易地处理大量并发的请求。服务端不需要维护用户状态信息，因此资源可以被自由地重定向到处理新的请求。这使得负载均衡器能够将来自同一客户端的任何请求分配给集群中的任何服务器，而不会导致会话数据问题。
2. 简化服务器设计（Simplicity of Server Design）
无状态协议简化了服务器的设计，因为服务器只需考虑单个请求。他没有必要担心请求之间的上下文如何影响服务器的响应。这意味着服务器变得更加简洁，易于实现，且更少出错。
3. 易于缓存（Facilitates Caching）
由于每个响应都是自包含的，正确标记的响应更容易被缓存处理。缓存机制可以根据每个请求的信息来处理和重新使用响应，这提高了效率，减轻了服务器负担。
```

### 问题

1. RESTful必须是http的吗，必须是同步的吗？

- 它适用于任何具有请求/响应模型的协议，例如HTTP, HTTPS, CoAP等。
- 它通常是基于同步通信模型的，这意味着发送请求后阻塞等待直到收到服务器的响应。但是，这个模型并不限制异步通信的使用。在某些情况下，为了提高性能或避免长时间等待，可以采取异步的方式实现。例如，一个RESTful接口可能会立即返回一个确认消息（比如HTTP状态码202 Accepted），随后通过不同的机制（例如回调URL、WebSocket、Server-Sent Events或轮询）来告知客户端操作完成的结果。

### 举例

```
GET /api/users : 获取所有用户列表
GET /api/users/1 : 获取特定用户信息

POST /api/users : 创建新用户
PUT /api/users/1 : 更新特定用户信息
Content-Type: application/json
body : {
  "name": "John Doe",
  "email": "john.doe@example.com"
}
DELETE /api/users/1 : 删除特定用户

GET /api/users/1/articles : 获取特定用户的所有文章
POST /api/users/1/articles : 为特定用户创建文章
```

```markdown
RESTful API的无状态性意味着每个请求都应包含所有必须的信息，使得服务器能够理解和处理该请求。服务器不会保存任何客户端请求之间的状态信息。这样，每个请求都可以独立于其他请求进行处理，提高了API的可靠性、可扩展性和简化了服务器设计。

## 登录
POST /api/login
用户通过发送用户名和密码到登录端点进行认证。服务器验证凭据后，响应中返回一个令牌（token），客户端将使用这个令牌来访问需要认证的资源。
GET /api/users/1
Authorization: Bearer abcdefghij
即使之前已经进行了登录，服务器也不会“记住”这个用户；每次请求都需要提供令牌作为认证的一部分。

## 访问
如果客户端试图访问同样的资源但没有提供令牌，服务器将不能识别用户，并且会返回错误响应。
401 Unauthorized
Content-Type: application/json
body : {
  "error": "Authentication required"
}

```

### 参考

https://zhuanlan.zhihu.com/p/334809573

https://chatgate.ai/

https://www.jianshu.com/p/1cd7ad39f124