## SQL语句

#### 一、基本概念

- `数据库（database）` - 保存有组织的数据的容器（通常是一个文件或一组文件）。
- `数据表（table）` - 某种特定类型数据的结构化清单。
- `模式（schema）` - 关于数据库和表的布局及特性的信息。模式定义了数据在表中如何存储，包含存储什么样的数据，数据如何分解，各部分信息如何命名等信息。数据库和表都有模式。

```
SQL分类：
数据定义语言（DDL）：（Data Definition Language，DDL）是 SQL 语言集中负责数据结构定义与数据库对象定义的语言。
DDL 的主要功能是定义数据库对象。
DDL 的核心指令是 CREATE、ALTER、DROP。

数据操纵语言（DML）：（Data Manipulation Language, DML）是用于数据库操作，对数据库其中的对象和数据运行访问工作的编程语句。
DML 的主要功能是 访问数据，因此其语法都是以读写数据库为主。
DML 的核心指令是 INSERT、UPDATE、DELETE、SELECT。

事务控制语言（TCL）： (Transaction Control Language, TCL) 用于管理数据库中的事务。这些用于管理由 DML 语句所做的更改。它还允许将语句分组为逻辑事务。
TCL 的核心指令是 COMMIT、ROLLBACK。

数据控制语言（DCL）：(Data Control Language, DCL) 是一种可对数据访问权进行控制的指令，它可以控制特定用户账户对数据表、查看表、预存程序、用户自定义函数等数据库对象的控制权。
DCL 的核心指令是 GRANT、REVOKE。
```

#### 二、增删改查

- INSERT

  ```sql
  INSERT INTO user
  VALUES (10, 'root', 'root', 'xxxx@163.com'); // 插入完整的行
  
  INSERT INTO user(username, password, email)
  VALUES ('admin', 'admin', 'xxxx@163.com'); // 插入行的一部分
  ```

- DELETE

  ```sql
  DELETE FROM user
  WHERE username = 'robot'; // 删除表中的指定数据
  
  TRUNCATE TABLE user; // 清空表中的数据
  ```

- UPDATE

  ```
  UPDATE user
  SET username='robot', password='robot'
  WHERE username = 'root'; // 更新表中的记录
  ```

- SELECT

  `SELECT` 语句用于从数据库中查询数据。
  `DISTINCT` 用于返回唯一不同的值。它作用于所有列，也就是说所有列的值都相同才算相同。
  `LIMIT` 限制返回的行数。可以有两个参数，第一个参数为起始行，从 0 开始；第二个参数为返回的总行数。
  `ASC` ：升序（默认）
  `DESC` ：降序

  ```sql
  SELECT *
  FROM products; // 查询所有列
  
  SELECT prod_id, prod_name, prod_price
  FROM products; // 查询多列
  
  SELECT DISTINCT
  vend_id FROM products; // 查询不同的值
  
  // 限制结果行数
  -- 返回前 5 行
  SELECT * FROM mytable LIMIT 5;
  SELECT * FROM mytable LIMIT 0, 5;
  -- 返回第 3 ~ 5 行
  SELECT * FROM mytable LIMIT 2, 3;
  ```

#### 三、复杂使用

```sql
## 子查询
可以嵌套在 SELECT，INSERT，UPDATE 或 DELETE 语句内或另一个子查询中。
您可以使用比较运算符，如 >，<，或 =。比较运算符也可以是多行运算符，如 IN，ANY 或 ALL。
子查询必须被圆括号 () 括起来。
内部查询首先在其父查询之前执行，以便可以将内部查询的结果传递给外部查询。
SELECT cust_name, cust_contact
FROM customers
WHERE cust_id IN (SELECT cust_id
                  FROM orders
                  WHERE order_num IN (SELECT order_num
                                      FROM orderitems
                                      WHERE prod_id = 'RGAN01'));
```

```sql
## WHERE
WHERE 子句用于过滤记录，即缩小访问数据的范围。
WHERE 后跟一个返回 true 或 false 的条件。

## AND、OR、NOT
AND 操作符表示左右条件都要满足。
OR 操作符表示左右条件满足任意一个即可。
NOT 操作符用于否定一个条件。

SELECT prod_id, prod_name, prod_price
FROM products
WHERE vend_id = 'DLL01' AND prod_price <= 4;
```

```sql
IN 操作符在 WHERE 子句中使用，作用是在指定的几个特定值中任选一个值。
BETWEEN 操作符在 WHERE 子句中使用，作用是选取介于某个范围内的值。

SELECT *
FROM products
WHERE vend_id IN ('DLL01', 'BRS01');

SELECT *
FROM products
WHERE prod_price BETWEEN 3 AND 5;
```

```sql
## Like
LIKE 操作符在 WHERE 子句中使用，作用是确定字符串是否匹配模式。
只有字段是文本值时才使用 LIKE。
LIKE 支持两个通配符匹配选项：% 和 _。
不要滥用通配符，通配符位于开头处匹配会非常慢。
% 表示任何字符出现任意次数。
_ 表示任何字符出现一次。

SELECT prod_id, prod_name, prod_price
FROM products
WHERE prod_name LIKE '%bean bag%';
```

#### 四、连接和组合

##### 连接

连接用于连接多个表，使用 `JOIN` 关键字，并且条件语句使用 `ON`

```sql
SELECT vend_name, prod_name, prod_price
FROM vendors INNER JOIN products
ON vendors.vend_id = products.vend_id; // 内连接
```

```sql
内连接 = 等值连接
1、自然连接一定是等值连接，但等值连接不一定是自然连接。
2、内连接提供连接的列，而自然连接自动连接所有同名列，即等值连接要求相等的分量，不一定是公共属性；而自然连接要求相等的分量必须是公共属性。
3、等值连接不把重复的属性除去；而自然连接要把重复的属性除去。(例如表A(a,b) 和 表B(b,c)连接后，自然连接得到C(a,b,c)，等值连接得到C(a,b,b1,c))

// 自然连接
SELECT *
FROM Products
NATURAL JOIN Customers;
```

```sql
内连接: 合并具有同一列的两个以上的表的行, 结果集中不包含一个表与另一个表不匹配的行
select  员工表.id,部门表.department
from 员工表，部门表
where 员工表.部门id=部门表.部门id；     
缺点：如果我们想要把不满足条件的数据（例如：员工表中有的员工他没有部门）也查询出来，内连接就做不到。

// 于是我们引入外连接：除了返回满足连接条件的行以外还返回左（或右）表中不满足条件的行
1. 外连接返回一个表中的所有行，并且仅返回来自次表中满足连接条件的那些行
2. 连接可以替换子查询，并且比子查询的效率一般会更快。

SELECT customers.cust_id, orders.order_num
FROM customers LEFT JOIN orders
ON customers.cust_id = orders.cust_id; // 左外连接
```

##### 组合

- `UNION` 运算符将两个或更多查询的结果组合起来，并生成一个结果集，其中包含来自 `UNION` 中参与查询的提取行。
- 每个查询中涉及表的列的数据类型必须相同或兼容。
- 通常返回的列名取自第一个查询。

应用场景：

- 在一个查询中从不同的表返回结构数据。
- 对一个表执行多个查询，按一个查询返回数据。

```sql
SELECT cust_name, cust_contact, cust_email
FROM customers
WHERE cust_state IN ('IL', 'IN', 'MI')
UNION
SELECT cust_name, cust_contact, cust_email
FROM customers
WHERE cust_name = 'Fun4All';
```

JOIN vs UNION

- `JOIN` 中连接表的列可能不同，但在 `UNION` 中，所有查询的列数和列顺序必须相同。
- `UNION` 将查询之后的行放在一起（垂直放置），但 `JOIN` 将查询之后的列放在一起（水平放置），即它构成一个笛卡尔积。

#### 五、函数、排序和分组

##### 函数

```
mysql> SELECT NOW();
2018-4-14 20:25:11
```

```
SELECT AVG(DISTINCT col1) AS avg_col
FROM mytable
```

##### ORDER BY

- `ORDER BY` 用于对结果集进行排序。
- `ASC` ：升序（默认）
- `DESC` ：降序
- 可以按多个列进行排序，并且为每个列指定不同的排序方式

```sql
SELECT * FROM products
ORDER BY prod_price DESC, prod_name ASC;
```

##### GROUP BY

- `GROUP BY` 子句将记录分组到汇总行中。
- `GROUP BY` 为每个组返回一个记录。
- `GROUP BY` 通常还涉及聚合：COUNT，MAX，SUM，AVG 等。

```sql
SELECT cust_name, COUNT(cust_address) AS addr_num
FROM Customers GROUP BY cust_name
ORDER BY cust_name DESC;
```

##### HAVING

- `HAVING` 用于对汇总的 `GROUP BY` 结果进行过滤。
- `WHERE` 和 `HAVING` 都是用于过滤。
- `HAVING` 适用于汇总的组记录；而 WHERE 适用于单个记录。

```sql
SELECT cust_name, COUNT(*) AS num
FROM Customers
WHERE cust_email IS NOT NULL
GROUP BY cust_name
HAVING COUNT(*) >= 1;
```

##### LIMIT OFFSET

优化大表分页查询性能：大表LIMIT 1000000, 10该怎么优化?

1. 使用索引覆盖扫描（Covering Index Scan）：如果你的查询可以被一个索引完全覆盖，那么MySQL可以只读取索引，而不需要读取实际的行。这可以大大提高查询速度。

   查询可以完全通过索引来获取所需数据，而不需要访问表中的实际数据行。这种策略能提高查询效率

   ```sql
   CREATE INDEX idx_users_id_username_email ON users(id, username, email);
   SELECT id, username, email FROM users ORDER BY id LIMIT 1000000, 10;
   这样，MySQL可以只读取索引，而不需要读取实际的行。
   ```

2. 记住上次查询的最后一个ID：如果你的表有一个递增的ID列，你可以在每次查询时记住上次查询的最后一个ID，然后在下一次查询时使用这个ID（Where ID > ?）来限制结果。

   ```sql
   SELECT * FROM users WHERE id > 10 ORDER BY id LIMIT 10。
   ```

3. 使用分区表：如果你的表非常大，你可以考虑使用分区表。这样，你的查询可以只扫描一个分区，而不是整个表。

   ```sql
   CREATE TABLE users (
     id INT NOT NULL,
     username VARCHAR(30) NOT NULL,
     email VARCHAR(30) NOT NULL,
     PRIMARY KEY(id)
   )
   PARTITION BY RANGE (id) (
     PARTITION p0 VALUES LESS THAN (1000000),
     PARTITION p1 VALUES LESS THAN (2000000),
     PARTITION p2 VALUES LESS THAN MAXVALUE
   );
   ```

   ```sql
   public class UserDao {
       private JdbcTemplate jdbcTemplate;
    
       public UserDao(JdbcTemplate jdbcTemplate) {
           this.jdbcTemplate = jdbcTemplate;
       }
    
       public List<User> getUsers(int partition, int limit) {
           String sql = "SELECT * FROM users PARTITION (p" + partition + ") ORDER BY id LIMIT ?";
           return jdbcTemplate.query(sql, new Object[]{limit}, (rs, rowNum) ->
                   new User(rs.getLong("id"), rs.getString("username"), rs.getString("email")));
       }
   }
   ```

#### 六、数据定义

##### 数据表

1、普通创建

```sql
CREATE TABLE user (
  id int(10) unsigned NOT NULL COMMENT 'Id',
  username varchar(64) NOT NULL DEFAULT 'default' COMMENT '用户名',
  password varchar(64) NOT NULL DEFAULT 'default' COMMENT '密码',
  email varchar(64) NOT NULL DEFAULT 'default' COMMENT '邮箱'
) COMMENT='用户表';
```

2、约束类型

- `NOT NULL` - 指示某列不能存储 NULL 值。
- `UNIQUE` - 保证某列的每行必须有唯一的值。
- `PRIMARY KEY` - NOT NULL 和 UNIQUE 的结合。确保某列（或两个列多个列的结合）有唯一标识，有助于更容易更快速地找到表中的一个特定的记录。
- `FOREIGN KEY` - 保证一个表中的数据匹配另一个表中的值的参照完整性。
- `CHECK` - 保证列中的值符合指定的条件。
- `DEFAULT` - 规定没有给列赋值时的默认值。
- AUTO_INCREMENT， 不是标准SQL约束，但许多数据库系统提供了这种功能，它用于在新行插入表时自动生成唯一的数值（通常用于主键）。

3、级联

```sql
ALTER TABLE child_table
ADD CONSTRAINT fk_name // 外键名称
FOREIGN KEY (child_column) REFERENCES parent_table(parent_column)
ON UPDATE CASCADE
ON DELETE CASCADE;
```

```sql
CREATE TABLE child_table (
    child_column INT,
    ...
    FOREIGN KEY (child_column) REFERENCES parent_table(parent_column)
    ON UPDATE CASCADE
    ON DELETE CASCADE
);
```

##### 视图

定义：
视图是基于 SQL 语句的结果集的可视化的表。视图是虚拟的表，本身不包含数据，也就不能对其进行索引操作。对视图的操作和对普通表的操作一样。
作用：

- **安全性**：可以限制用户对特定数据的访问。
- **简便性**：可以封装复杂的查询，让用户不必编写复杂的 SQL 语句。
- **抽象化**：可以隐藏底层数据的复杂性，例如多表连接、计算字段等。
- **逻辑数据独立性**：如果底层表结构变化，只需要修改视图定义而不影响依赖于视图的应用程序。

缺点：因为数据是实时生成的，没有进行物理存储。因此，如果视图背后的查询非常复杂，或者涉及大量数据，那么使用视图可能会导致速度慢于直接查询实际的数据表。

```sql
CREATE VIEW ActiveEmployees AS
SELECT EmployeeID, FirstName, LastName, Department
FROM Employees
WHERE Status = 'Active';

SELECT * FROM ActiveEmployees;
DROP VIEW ActiveEmployees;
```

##### 索引

作用：
通过索引可以更加快速高效地查询数据。
用户无法看到索引，它们只能被用来加速查询。
注意：
更新一个包含索引的表需要比更新一个没有索引的表花费更多的时间，这是由于索引本身也需要更新。因此，理想的做法是仅仅在常常被搜索的列（以及表）上面创建索引。

```
CREATE INDEX user_index
ON user (id); // 创建索引

CREATE UNIQUE INDEX user_index
ON user (id); // 创建唯一索引

ALTER TABLE user
DROP INDEX user_index;
```

如何选择索引？

```markdown
## 单列/复合索引
当你经常需要根据一个列/多个列 进行Where查找时，单列索引最为有效。

## 全文索引：
在文本搜索中非常有用，特别是对大量文本数据进行搜索时，并非所有数据库系统都支持全文索引。

## 部分索引：
CREATE INDEX idx_partial_columnname ON tablename (columnname) WHERE condition;
这种类型的索引在只有少数行满足查询条件的情况下非常有用，因为它可以缩小索引大小并提高效率。

## 唯一索引：
唯一索引不仅加速查询，还强制列中的值是唯一的。（当试图在具有唯一索引的列中插入重复字段时，数据库将不允许该操作，并且会抛出一个错误）
ERROR: duplicate key value violates unique constraint "idx_unique_columnname"
DETAIL: Key (columnname)=(value) already exists.
```

##### 事务处理

`START TRANSACTION` - 指令用于标记事务的起始点。
`SAVEPOINT` - 指令用于创建保留点。
`ROLLBACK TO` - 指令用于回滚到指定的保留点；如果没有设置保留点，则回退到 `START TRANSACTION` 语句处。
`COMMIT` - 提交事务。

```
-- 开始事务START TRANSACTION;-- 插入操作 A
INSERT INTO `user`VALUES (1, 'root1', 'root1', 'xxxx@163.com');-- 创建保留点 updateASAVEPOINT updateA;-- 插入操作 BINSERT INTO `user`VALUES (2, 'root2', 'root2', 'xxxx@163.com');-- 回滚到保留点 updateAROLLBACK TO updateA;-- 提交事务，只有操作 A 生效COMMIT;
```

##### 权限控制

GRANT 和 REVOKE 可在几个层次上控制访问权限：
整个服务器，使用 GRANT ALL 和 REVOKE ALL；
整个数据库，使用 ON database.*；
特定的表，使用 ON database.table；

```
CREATE USER myuser IDENTIFIED BY 'mypassword';
SHOW GRANTS FOR myuser;
GRANT SELECT, INSERT ON *.* TO myuser;
REVOKE SELECT, INSERT ON *.* FROM myuser;
```

#### 七、高级使用

##### TRIGGER

1. 数据验证

   ```sql
   -- 在员工表中插入数据前，检查年龄是否符合规定（假设法定工作年龄不得低于18岁）
   CREATE TRIGGER CheckAgeBeforeInsert
   BEFORE INSERT ON Employees
   FOR EACH ROW
   BEGIN
     IF NEW.age < 18 THEN
       SIGNAL SQLSTATE '45000'
         SET MESSAGE_TEXT = 'Employee must be at least 18 years old.';
     END IF;
   END;
   ```

2. 自动填充数据

   ```sql
   -- 当新订单被创建时，自动填充创建时间戳字段
   CREATE TRIGGER SetOrderTimestamp
   BEFORE INSERT ON Orders
   FOR EACH ROW
   SET NEW.creation_timestamp = NOW();
   ```

3. 审计和日志记录

   ```sql
   -- 每当员工信息被更新时，记录变更到一个审计日志表中
   CREATE TRIGGER EmployeeAuditLog
   AFTER UPDATE ON Employees
   FOR EACH ROW
   BEGIN
     INSERT INTO Employees_Audit (employee_id, changed_by, change_timestamp, action)
     VALUES (OLD.employee_id, CURRENT_USER(), NOW(), 'UPDATE');
   END;
   ```

4. 同步多个表

   ```sql
   -- 当产品价格在产品表中更新时，同时更新一个存储所有历史价格的表
   CREATE TRIGGER SyncProductHistory
   AFTER UPDATE OF price ON Products
   FOR EACH ROW
   BEGIN
     INSERT INTO ProductPriceHistory(product_id, new_price, change_date)
     VALUES (NEW.product_id, NEW.price, NOW());
   END;
   ```

5. 约束实施

   ```sql
   -- 确保订单总额不超过客户的信用额度
   CREATE TRIGGER CheckCreditLimitBeforeInsertOrder
   BEFORE INSERT ON Orders
   FOR EACH ROW
   BEGIN
     DECLARE credit_limit DECIMAL(10,2);
     SELECT credit_limit INTO credit_limit FROM Customers WHERE customer_id = NEW.customer_id;
     IF NEW.order_total > credit_limit THEN
       SIGNAL SQLSTATE '45000'
         SET MESSAGE_TEXT = 'Order total exceeds customer credit limit.';
     END IF;
   END;
   ```

##### 查看执行性能

```sql
EXPLAIN SELECT * FROM your_table WHERE your_column = 'some_value';
SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY);
```

