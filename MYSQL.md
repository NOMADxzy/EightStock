## MYSQL

连接层：
               最上层是一些客户端链接服务，主要完成一些类似于连接处理、授权认证、及相关的安全方案。服务器也会为安全接入的每个客户端验证它所具有的操作权限。
服务层：
               第二层架构主要完成大多数的核心服务功能，如 SQL 接入，并完成缓存的查询，SQL 的分析和优化，部分内置函数的执行。所有跨存储引擎的功能也在这一层实现，如 过程、函数等。
引擎层：
               存储引擎真正地负责了 MySQL 中数据的存储和提取，服务器通过 API 和存储引擎通信。不同的存储引擎具有不同的功能，这样我们可以根据自己的需要，来选取合适的**存储引擎**。
存储层：
                主要是将数据存储在文件系统之上，并完成与**存储引擎**的交互。

![https://img-blog.csdnimg.cn/a5caacfe5fef4255a9f74d2a57c98f63.png](https://gitee.com/xu_zuyun/picgo/raw/master/img/a5caacfe5fef4255a9f74d2a57c98f63.png)

存储引擎就是存储数据、建立索引、更新 / 查询数据等技术的实现方式，存储引擎是基于表的，而不是基于库的，所以存储引擎也可被称为表类型。（InnoDB 引擎、MyISAM 引擎、Memory 引擎）

 InnoDB 是一种兼顾高可靠性和高性能的通用存储引擎，在 MySQL 5.5 之后，InnoDB 是默认的 MySQL 存储引擎。 InnoDB 对每张表在磁盘中的存储以 xxx.ibd 后缀结尾，innoDB 引擎的每张表都会对应这样一个表空间文件，用来存储该表的表结构（frm、sdi）、数据和索引。

InnoDB 的逻辑[存储结构](https://so.csdn.net/so/search?q=存储结构&spm=1001.2101.3001.7020)：表空间、段、区、页、行

![https://img-blog.csdnimg.cn/95540eb1ea7349109d388ff6fc7f7cd7.png](https://gitee.com/xu_zuyun/picgo/raw/master/img/95540eb1ea7349109d388ff6fc7f7cd7.png)

InnoDB 还会自动的给我们添加三个隐藏字段及其含义分别是：
    DB_TRX_ID
    最近修改事务 ID ，记录插入这条记录或最后一次修改该记录的事务 ID 。
    DB_ROLL_PTR
    回滚指针，指向这条记录的上一个版本，用于配合  undo log ，指向上一个版本。
    DB_ROW_ID
    隐藏主键，如果表结构没有指定主键，将会生成该隐藏字段。如果已存在主键，将不会生成该字段。
#### 一、可重复读实现原理

MVCC，直译多版本并发控制，它的全称是Multi-Version Concurrency Control， 直白说就是在同一时刻同一条记录在系统中可以存在多个版本，这就是数据库的多版本并发控制。本质上是一种乐观锁，用于实现提交读(READ COMMITTD)和可重复读(REPEATABLE READ)这两种隔离级别

![https://img-blog.csdnimg.cn/d992db6af2fa48588cc3ddc4e45e5c19.png](https://gitee.com/xu_zuyun/picgo/raw/master/img/d992db6af2fa48588cc3ddc4e45e5c19.png)

![https://img-blog.csdnimg.cn/828b5b7c754b4465844f7d1676e1273a.png](https://gitee.com/xu_zuyun/picgo/raw/master/img/828b5b7c754b4465844f7d1676e1273a.png)

最终我们发现，不同事务或相同事务对同一条记录进行修改，会导致该记录的 undolog 生成一条 记录版本链表，链表的头部是最新的旧记录，链表尾部是最早的旧记录。

 **ReadView（读视图）**是 快照读 SQL 执行时 MVCC 提取数据的依据，记录并维护系统当前活跃的事务 （未提交的）id。 ReadView 中包含了四个核心字段：可以通过这个列表来判断某一个版本是否对当前事务可见

![image-20231223204345236](https://gitee.com/xu_zuyun/picgo/raw/master/img/image-20231223204345236.png)

 readview 中就规定了版本链数据的访问规则： trx_id 代表当前 undolog 版本链对应事务 ID

![image-20231223203456559](https://gitee.com/xu_zuyun/picgo/raw/master/img/image-20231223203456559.png)

READ COMMITTED ：在事务中每一次执行快照读时生成 ReadView。

REPEATABLE READ：仅在事务中第一次执行快照读时生成 ReadView，后续复用该 ReadView。

### 二、锁

MySQL有三种锁的级别：页级、表级、行级。

- 表级锁：开销小，加锁快；不会出现死锁；锁定粒度大，发生锁冲突的概率最高,并发度最低。
- 行级锁：开销大，加锁慢；会出现死锁；锁定粒度最小，发生锁冲突的概率最低,并发度也最高。
- 页面锁：开销和加锁时间界于表锁和行锁之间；会出现死锁；锁定粒度界于表锁和行锁之间，并发度一般



**死锁的解决办法?**

1. 查出的线程杀死 kill
   SELECT trx_MySQL_thread_id FROM information_schema.INNODB_TRX;
2. 设置锁的超时时间
   Innodb 行锁的等待时间，单位秒。可在会话级别设置，RDS 实例该参数的默认值为 50（秒）。生产环境不推荐使用过大的  innodb_lock_wait_timeout参数值该参数支持在会话级别修改，方便应用在会话级别单独设置某些特殊操作的行锁等待超时时间，如下：
   set innodb_lock_wait_timeout=1000; —设置当前会话 Innodb 行锁等待超时时间，单位秒。
3. 指定获取锁的顺序

**临时表**：*MySQL用于存储一些中间结果集的表，临时表只在当前连接可见，当关闭连接时，Mysql会自动删除表并释放所有空间。*

 **悲观锁：**

- 共享锁（Shared Lock）：也称为读锁，允许多个事务同时获取共享锁，但不允许事务获取排他锁。共享锁用于保护数据的一致性读操作，不阻塞其他共享锁的获取，但会阻塞排他锁的获取。

  **For Update**：将获取排他锁

  ![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/v2-c673a214bb7309f9ae353d6c682f2e12_1440w.jpg)

- 排他锁（Exclusive Lock）：也称为写锁，只允许一个事务获取排他锁，其他事务无法同时获取共享锁或排他锁。排他锁用于保护数据的写操作，阻塞其他锁的获取，包括共享锁和排他锁。

  **Lock in Share Mode**：获取共享锁

  ![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/v2-1fc105cb7f7583389f1ae95f4dbcde90_1440w.jpg)

**乐观锁：**

- 乐观锁不实际上对数据进行锁定。它基于一种假设，即数据在被修改期间不会被其他事务修改。在乐观锁中，你通常会使用版本号或时间戳等机制，以确保在提交修改之前，数据没有被其他事务修改。如果检测到冲突，事务将回滚。