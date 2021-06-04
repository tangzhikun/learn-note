---
typora-copy-images-to: ./
---

# MySQL

#### 表

任何的列都可以作为主键，只要他满足以下条件：

1. 任意两行都不具有相同的主键值；
2. 每一行都必须具有一个主键值；
3. 主键值不允许修改或者更新；
4. 主键值不能重用（如果某行从表中删除，它的主键值不能赋给以后的新行）。

#### 登录MySQL

```mysql -h localhost -u root -p```

mysql为登录命令，-h（host）后面跟的参数是服务器的主机地址，-u（user）为登录数据库的用户，-p（password）表示为输入密码登录；

命令行使用 ；结束或者使用 \g 结束。

#### 新建与删除数据库

新建数据库：```create database database_name```

删除数据库：```drop database database_name```（无任何提示，直接删除）

#### 数据库存储引擎

数据库存储引擎是数据库底层的软件组件，数据库管理系统（DBMS）使用数据引擎进行创建、查询、更新和删除数据操作。MySQL的核心就是存储引擎。

# MySQL命令

## select:

``select * from table`` ：将table中所有元素取出

``select a, b from table ``: a，b为字段名，即第一行，对该列数据进行描述的名称，将table中的a，b这两列取出;

``select a, b * 12 from table`` ：将table表格中的a这一列，与b的这一列乘以12取出来，第二列的字段名为b * 12;

``select a, b * 12 annual_sal from table`` ：将table表格中的a这一列，与b这一列乘以12取出来，并将第二列命名为annual_sal。如果新的名称有空格，则需要添加为`` select a, b * 12  `annual sal` from table`` 

表格中所有NULL参与的计算结果均为NULL，如果需要在计算时需要将NULL更改为0，可以使用语句

``select a, b * 12 + (case when c is NULL then 0 else c end) as annual_sal from table``

#### 在select中使用distinct关键字

``select distinct a from table``：将a这一列中的重复元素去除后再取出。

``select distinct a, b from table``：将a，b这两列均重复的元素去除后再取出。

## where条件过滤

``select * from table where a = 10``：将a这一列等于10的元素的所有信息取出

如果需要将a这列不等于10的元素提取出来，则使用

``select * from table where a <> 10``

如果需要将a这一列中大于等于10，小于等于20的元素提取，

``select * from table where a >= 10 and a <= 20``或者

``select * from table where a between 10 and 20``

将a这一列中为空/不为空的元素取出，

``select * from table where a is null``

``select * from table where a is not null``

不可以使用该语句直接判断将a这一列中等于某个数的元素取出

``select * from table where a is 100``这样写会报错，

应该使用

``select * from table where a = 100``

如果是需要将a这一列中等于10和20的元素取出

``select * from table where a in (10, 20)`` 

如果a这一列存放的是时间，需要将a中大于某一时间的元素取出，

``select * from table where a > 2021-01-19``直接将时间按照格式传入即可。

#### 模糊条件过滤

当查询字符串时，无法得知具体的字符串，可以通过模糊条件过滤来查找，类似于Linux中的*.c之类的。

``select * from emp where a like '%A%'`` 这一句中``%``表示**0个或者多个字符**，这一句的作用是将a这一列中字符串里包含A的所有元素取出

``select * from emp where a like '%A%'`` 这一句中``_``表示**1个字符**，这一句的作用是将a这一列中的字符串中第二的字符为A的元素取出。

如果缺省，则认为是0个字符，例如``select * from emp where a like 'A%'``是将第一个字符为A的元素取出。

如果查找的字符串中包含``%`` 或者``_``，则在``%`` 或者``_``前加上反斜杠``\``即可。

## order by 排序

``select * from table order by a asc``将table中的所有元素取出，并且按照a这一列值的**升序排列**，asc缺省时默认为升序。如果需要更改为降序排列，则在后面跟一个desc

``select * from table order by a desc``

排序这一操作可以和where条件过滤同时使用

``select * from table where a > 10 order by a desc``将a中大于10的元素取出，并且按照降序排列。

如果存在多条排序原则，则先按照第一条排序，如果第一条相等，再按照第二条排序，以此类推

``select * from table where a > 10 order by a desc, b asc``先按照a的降序排列，如果相同，再按照b的升序排列。

## 使用常用的函数

**lower**：将大写转换为小写

``select lower(a) from table``将表格中a这一列转换为小写后取出，并不会改变原表格内容；

**upper**：转换为大写，用法与lower相同。

**concat**：将字符串拼接起来

``select concat(a, '.', b, '-', c) as name from table``按照格式将字符串拼接起来。 

**char_length**：计算字符串长度

``select a, char_length(a) as strlen from  table order by strlen asc``将table中的a与a的长度以字段名为strlen取出，并以strlen升序排列。

**substring**：将字符串的子串取出

``select a, substring(a, 1, 3) as partofa from table``将a与a中的第1~3个字符的子串作为partofa取出。

 **ltrim/ rtrim/ trim**：将字符串的左边空格、右边空格、左右两边空格去除。

``select ltrim(a) from table``

**ceil/ floor**向上取整、向下取整、四舍五入

**round/ truncate**四舍五入或者直接截断不进位

``select round(3.14159, 3)``保留三位小数，结果为3.142

``select truncate(3.14159, 3)``截断，结果为3.141

**curdate()/ curtime()/ now()**：curdate为输出当前日期，格式为年-月-日，curtime输出当前时间，时-分-秒，now输出日期加上时间。

## 组函数

**max/ min/ avg/ sum/ **返回最大值、最小值、平均值、总和。

**count**返回记录的数量，count(*)返回表格中有多少条记录

## group by分组

按照条件进行分组，之后再对各个组进行操作

``select max(sal) from emp group by deptno``根据deptno进行分组，如果deptno相同则为一组，之后再对各个组求sal的最大值

## having语句

where语句是对单条语句的限制，having是对于分组的限制

``select avg(sal), deptno from emp group by deptno having avg(sal) > 2000;``取出平均工资大于2000的部门

## 子查询

把一个查询作为结果



## 表连接

**left join**：左外连接



## view视图

### 创建视图

create view创建一个临时的表格

```
create view vtemp as select avg(sal) as avg_sal, deptno from emp group by deptno;
select * from vtemp;
```

**对于视图的操作会同步到原表中。通过视图可以直接对原表进行修改操作。**

### **更新**视图

使用``create or replace view``创建或者更新分表，若分表不存在则创建，否则更新

使用``alter view``只更新分表

### 删除视图

``drop view if exist t_view``如果``t_view``存在则删除

## 创建表格

```mysql
create table t
(
    a int primary key,
    b varchar(10)
);
```

创建表格时，必须要指定主键，主键不能出现重复的值。

## limit分页

``select ename, sal from emp order by sal desc limit 5``取出薪水最高的5个员工，limit 5的作用是取出前5条

如果需要第3到第5条（从第0条记录开始）

``select ename, sal from emp order by sal desc limit 3, 3``

limit3, 3中第一个3表示第三条记录之后开始，（包含第0条记录），3表示取出记录的条数，即从第三条记录开始取出3条记录。

## 插入数据

插入一行记录

`` INSERT INTO TABLE_NAME(列名1，列名2，...) VALUES(值1，值2,...)``

插入多行记录
``INSERT INTO TABLE_NAME(列名1，列名2，...) VALUES(值1，值2，...), VALUES(值1，值2, ...)``

## 修改记录

``UPDATE table_name SET 字段名=字段值，... WHERE 条件``

example

``UPDATE test SET name=CONCAT(name, 'ok'), sex=DEFAULT WHERE id<=3;``

## 删除记录

``DELETE FROM table_name where 条件``



## 数据类型

MySQL中定义数据字段的类型对你数据库的优化是非常重要的。

MySQL支持多种类型，大致可以分为三类：数值、日期/时间和字符串(字符)类型。

**数值类型**：

|   类型    |          大小           | 范围(有符号) | 范围（无符号） |
| :-------: | :---------------------: | :----------: | :------------: |
|  TINYINT  |          1byte          | (-128, 127)  |    (0, 2^8)    |
| SMALLINT  |         2bytes          |              |   (0, 2^16)    |
| MEDIUMINT |         3bytes          |              |   (0, 2^24)    |
|    INT    |         4bytes          |              |   (0, 2^32)    |
|  BIGINT   |         8bytes          |              |   (0, 2^64)    |
|   FLOAT   |         4bytes          |              |                |
|  DOUBLE   |         8bytes          |              |                |
|  DECIMAL  | 如果M>D为M+2，否则为D+2 |              |                |

DECIMAL存储格式为``DECIMAL(P,D)``,P为有效数字的位数，D为小数的位数，例如DECIMAL（6,2）存储范围为-9999.99至9999.99。

#### FLOAT数据类型在存储时的方式：

一个float类型浮点数由3部分组成：符号位s（1位）、指数位e（8位）和底数m（23位），格式为

``SEEE EEEE EMMM MMMM MMMM MMMM MMMM MMMM``

符号位表示浮点数的正负。

指数可正可负，因此必须减去127之后才是真正的指数，范围为-126至127，对于全0和全1的另做特殊处理

底数实际上是一个24bit的数，但是最高位始终为1，因为转换需要先转换为科学计数法，所以最高位始终为1，因此最高位不存储，只需要存储23bit。

17.625转换为2进制为10001.101，使用科学计数法为1.0001101 * 2^45，因此符号位为0，底数为10001101，但是最高位1不用存储。指数为4，但是必须加上127，即为131（1000 0011）最终存储的形式是：

0100 0001 1000 1101 0000 0000 0000 0000 

## 索引

索引是帮助MySQL高效获取数据的**排好序**的**数据结构**。

#### 1. 什么是索引？有什么用？

索引就相当于一本书的目录，通过目录可以快速的找到对应的资源。在数据库方面，查询一张表的时候有两种检索方式：
第一种方式是全表扫描（效率低），另一种方式是根据索引检索（效率很高）。

索引虽然可以提高检索效率，但是不能随意添加索引，因为索引也是数据库当中的对象，也需要数据库不断的维护。表中数据经常被修改就不适合添加索引。

#### 2. 什么时候考虑给字段添加索引？

* 数据量庞大。
* 该字段很少的DML操作。（因为字段进行修改操作，索引需要维护）
* 该字段经常出现在where语句中。（需要经常被查找）

**主键**和**具有unique约束的字段**会自动添加索引。

unique约束是指所有记录中字段的值不能重复出现。

#### 3. 如何给字段添加或删除索引

1. 创建索引

``create index username_index on user(username)``

create index + index名 + on + 表名（字段名）

2. 删除索引

   drop index 索引名 on 表名

#### 4. 索引的分类

1. 单一索引：给单个字段添加索引；
2. 复合索引：给多个字段联合起来添加一个索引；
3. 主键索引：主键上会自动添加索引
4. 唯一索引：有unique约束的字段上会自动添加索引。

#### 5.索引什么时候失效？

``select * from emp where ename like '%A%';``

模糊查询的时候，第一个通配符使用的%，会使得索引失效。

#### 聚集索引B+树

叶节点包含完整的数据记录称为聚集索引（``InnoDB``），否则为非聚集索引（``MyISAM``）。

![](.\7338b901ac083ffb7f2a950371a0397c.png)

非聚簇索引B树

![](C:\Users\Administrator\Desktop\learning\cpp\23913bfe60f6fa7319ad58bd46475b38.png)

### 事务

#### 1. 事务具有四个特征ACID

1. **原子性（Atomicity）**：整个事务中所有的操作，必须作为一个单元全部成功完成，或全部失败完成。
   实现方式：undo log
2. **一致性（Consistency）**：在事务开始之前与结束之后，数据库都保持一致状态。
3. **隔离性（Isolation）**：一个事务不会影响其他事务的运行。
   实现方式：锁
4. **持久性（Durability)**：持久性是指一个事务一旦被提交，它对数据库的影响是永久性的，接下来即使数据库发生故障也不应该对其有任何影响。
   实现方式：redo log

其中，原子性、隔离性、持久性都是为了保证一致性。

##### 1.1 undo log

undo log中保存的是跟执行操作相反的操作。

#### 2. 和事务相关的语句

和事务相关的语句只有DML（Data Manipulation Language select）语句（insert delete update）， 因为他们这三个语句都是和数据库表当中的数据相关的。**事务的存在是为了保证数据的完整性，安全性**。

#### 3. 脏读、幻读、不可重复读

1. **脏读**：脏读就是指当一个事务正在访问数据，并且对数据进行了修改，而这种修改还没有提交到数据库中，这时，另一个事务也访问了这个数据，然后使用了这个数据。
   **导致的问题：**如果前一个事务回滚会导致第二个事务在此之前读取的数据就是“脏数据”。
2. **幻读：**幻读是指同样一笔查询在整个事务过程中多次执行后，查询所得的结果集是不一样的。幻读针对的是多笔记录。例如第一个事务对一个表中的数据进行了修改，这种修改涉及到表中的全部数据行。同时，第二个事务也修改这个表中的数据，这种修改是向表中插入一行新数据。那么，以后就会发生操作第一个事务的用户发现表中还有没有修改的数据行，就好象发生了幻觉一样。
3. **不可重复读**：是指在一个事务内，多次读同一数据。在这个事务还没有结束时，另外一个事务也访问该同一数据。那么，在第一个事务中的两次读数据之间，由于第二个事务的修改，那么第一个事务两次读到的的数据可能是不一样的。这样就发生了在一个事务内两次读到的数据是不一样的，因此称为是不可重复读。

#### 4. 事务的隔离级别

事务隔离性存在隔离级别，理论上隔离级别包括4个：

1. **第一级别：读未提交（read uncommitted）**：对方事务还未提交，我们当前事务就可以读到对方为提交的数据。
   **读未提交存在脏读现象。**

2.  **第二级别：读已提交（read committed）**：对方事务已提交之后的数据可以读到。

   解决了脏读现象。
   读已提交存在的问题是：不可重复读。

3. **第三级别：可重复读（repeatable read）**：永远读的是事务开始时的数据，
   存在的问题：幻读。实际上数据可能已经被删改。

4. **第四级别：序列化/串行化读（serializable）****：当一个事务进行时，不允许其他事务进行，解决了所有问题。
   存在的问题是：效率低，事务需要排队。

MySQL默认的隔离级别是**可重复读**。

### 数据库设计三范式

1. 任何一张表都应该有主键，且每一个字段原子不可再分。
2. 所有非主键字段完全依赖主键，不能只产生部分依赖。
   利用关系表，外键实现多对多的关系。
3. 所有非主键字段，直接依赖主键字段，不能产生传递依赖。
   通过加外键实现一对多关系。

### MySQL中的四种索引

MySQL中有四种索引，分别是主键索引，普通索引，唯一索引和全文索引。

全文索引只能在表的数据库引擎为MyISAM时生效。



#### 聚簇索引（clustered index)与普通索引/二级索引/辅助索引(secondary index)

聚簇索引叶子节点存放所有行的数据记录信息，即数据即索引，索引即数据，所以只有idb一个文件。

普通索引在叶子节点在叶子节点不包含所以行数据只会在叶子节点存自己本身的键值和主键的值，索引数据时通过索引上的叶子节点的主键来获取查找行数据记录

#### 回表查询

当查找存在普通索引的字段时，先使用普通索引查找到该记录信息的主键值，再通过聚簇索引查询该行的记录。

![img](.\webp)

#### 索引覆盖

如果一个索引包含（或覆盖）所有需要查询的字段的值，称为‘覆盖索引’。即只需要扫描索引而无须回表。
优点：

1. 索引条目通常远小于数据行大小，如果只读取索引，MySQL就会极大地减少数据访问量。
2. 索引按照列值顺序存储，对于I/O密集的范围查询会比随机从磁盘中读取每一行数据的I/O要少很多。
3. InnoDB的辅助索引（亦称二级索引）在叶子节点中保存了行的主键值，如果二级索引能够覆盖查询，则可不必对主键索引进行二次查询了。

#### 最左前缀匹配原则

在MySQL**建立联合索引时**会遵守最左前缀匹配原则，即最左优先，在检索数据时从联合索引的最左边开始匹配。即排序按照，最左的字段最优先的原则进行排序。

![img](.\v2-57ac6c797deaee5518920c064c1b8387_720w.jpg)

#### 索引下推

如果存在某些被索引的列的判断条件时，MySQL服务器将这一部分判断条件传递给存储引擎，然后由存储引擎通过判断索引是否符合MySQL服务器传递的条件，只有当索引符合条件时才会将数据检索出来返回给MySQL服务器 。即在查询到字段时先进行判断满足条件之后再返回。

### MVCC（Multi-Version Concurrency Control)

MVCC只适用于读已提交与可重复读两种隔离级别，MVCC在MySQL中的实现依赖的是undo log与read view。

**当前读：**读取的是记录的最新版本，读取时还要保证其他并发事务不能修改当前记录，会对读取的记录加锁。

**当事务对某条记录进程更改时，执行的顺序**：

1. 首先使用排它锁锁定该行记录，其他事务无法对该条记录进行访问；
2. 记录undo log日志；
3. 将更改前的该行记录的值复制到undo log 中去；
4. 对该行记录进行修改，并且将DB_ROLL_PTR中存放一个指向undo log中该行记录修改前的DB_ROW_ID的指针。

每行记录都会有三条隐式字段，分别是DB_ROW_ID，DB_TRX_ID，DB_ROLL_PTR。

1. DB_ROW_ID：6个字节，隐含的自增ID，如果表中没有主键，InnoDB会自动以DB_ROW_ID产生一个聚簇索引。
2. DB_TRX_ID：6个字节，每处理一次则加一。最近修改/插入事务ID：记录创建这条记录/最后一次修改该记录的事务ID。
3. DB_ROLL_PTR：7个字节，指向undo log中的一个指针。指向这条记录的上一个版本。

undo log主要分为两种：

  insert undo log

​      代表事务在insert新记录时产生的undo log, 只在事务回滚时需要，并且在事务提交后可以被立即丢弃

​    update undo log

​      事务在进行update或delete时产生的undo log; 不仅在事务回滚时需要，在快照读(select，当读的过程中有写的十事务开始和提交，会造成读数据的脏读、不可重复读、幻读等)时也需要；所以不能随便删除，只有在快速读或事务回滚不涉及该日志时，对应的日志才会被purge线程统一清除。

### ReadView

  什么是Read View，说白了Read View就是事务进行快照读(select * from)操作的时候生产的读视图(Read View)，在该事务执行的快照读的那一刻，会生成事务系统当前的一个快照，记录并维护系统当前活跃事务(未提交事务)的ID(当每个事务开启时，都会被分配一个ID, 这个ID是递增的，所以最新的事务，ID值越大)。

ReadView中主要包含4个比较重要的内容：

read view中活跃就是指未提交的事务

​    1. m_ids：表示在生成ReadView时当前系统中活跃的读写事务的事务id列表。

​    2. min_trx_id：表示在生成ReadView时当前系统中活跃的读写事务中最小的事务id，也就是m_ids中的最小值。

​    3. max_trx_id：表示生成ReadView时系统中应该分配给下一个事务的id值。

    4. creator_trx_id：表示生成该ReadView的快照读操作产生的事务id。

#### 基于RR可重复读隔离级别实现基本原理

 在select读数据的过程中，m_ids首次发现未提交的事务信息不会因在查找过程中其他事务id提交而把该事务id排除在外，直至查询到该事务链中最后提交的事务

#### 基于RC读已提交隔离级别实现基本原理

保存事务系统中的活跃的事务id，基于m_ids中的id信息在事务链中直到查找到非活跃的事务id(已提交的事务，不管事务提交是否在read view生成后)，此时就认为是该事务id查询的信息

**RC隔离级别是在执行sql时生成read view，RR隔离级别是在事务开始生成read view。**

这样在访问某条记录时，只需要按照以下的步骤判断记录的某个版本是否可见：

1）如果被访问版本的trx_id属性值与ReadView中的creator_trx_id值相同，意味着当前事务在访问它自己修改过的记录，所以该版本可以被当前事务访问。

​    2）如果被访问版本的trx_id属性值小于ReadView中的min_trx_id值，表明生成该版本的事务在当前事务生成ReadView前已经提交，所以该版本可以被当前事务访问。

​    3）如果被访问版本的trx_id属性值大于ReadView中的max_trx_id值，表明生成该版本的事务在当前事务生成ReadView后才开启，所以该版本不可以被当前事务访问。

​    4）如果被访问版本的trx_id属性值在ReadView的min_trx_id和max_trx_id之间，那就需要判断一下trx_id属性值是不是在m_ids列表中，如果在，说明创建ReadView时生成该版本的事务还是活跃的，该版本不可以被访问；如果不在，说明创建ReadView时生成该版本的事务已经被提交，该版本可以被访问

### 日志

#### 1. 重做日志（redo log)

redo log包括两部分：一个是内存中的日志缓存（redo log buffer），另一个是磁盘上的日志文件（redo log file）。

mysql每执行一条DML语句，先将记录写入redo log buffer，后续某个时间段再一次性将多个操作记录写到redo log file。这种先写日志，再写磁盘的技术就是mysql里经常说到的WAL（Write Ahead Logging）。



#### 2. 二进制日志（bin log)

Binlog在MySQL的Server层实现，Binlog是逻辑日志，记录的是一条语句的原始逻辑，Binlog不限大小，追加写入，不会覆盖以前的日志。

Binlog用于记录数据库执行的写入性操作信息，以二进制的形式保存在磁盘中。

**binlog的使用场景**：主要有两个使用场景，**主从复制**和**数据恢复**。

1. 主从复制：在master端开启binlog，然后将binlog发送到给个slava端，slave重放binlog从而达到主从数据一致。
2. 数据恢复：通过使用mysqlbinlog工具来恢复数据。

对于InnoDB来说，只有在事务提交时才会记录binlog，通过sync_binlog参数空间binlog写入到磁盘的时间，

* 0：不去强制要求，有系统自行判断何时写入磁盘；
* 1：每次commit的时候都要将binlog写入磁盘；
* N：每N个事务完成一次将binlog写入磁盘。



#### 3. undo log

undo log主要记录了数据的逻辑变化，比如一条insert语句，对应一条delete的undo log，对于每个update语句，对应一条相反的update的undo log，这样在发生错误的时候，就能回滚到事务之前的数据状态。

undo log是MVCC实现的关键。

### MySQL常见面试题

#### 1. InnoDB与MyISAM的区别

1. InnoDB支持事务，MyISAM不支持。
2. InnoDB支持外键，MyISAM不支持。
3. InnoDB支持表锁和行锁，MyISAM只支持表锁。
4. InnoDB在5.6版本后支持全文索引。
5. InnoDB使用聚集索引，MyISAM使用非聚集索引，即InnoDB的索引的叶子节点直接村发放数据，而MyISAM存放的是数据的地址。

#### 2. MySQL的索引一般有几层？

一般情况下，3到4层就足以支持千万级别的表查询。

创建索引的字段长度较小更好，一个节点可以存放个更多的记录。

#### 3. 在分布式应用场景中，自增id还使用吗？

雪花算法，snowflake，自定义id生成器。

4. 一条SQL语句在MySQL中执行过程全解析
   查询过程：权限校验-》查询缓存-》分析器-》优化器-》权限校验-》执行器-》引擎
   更新等过程：分析器-》权限校验-》执行器-》引擎-》redolog prepare-》binlog-》redolog commit
5. MySQL组成
   MySQL主要分为Server层和存储引擎层：
   Server层：主要包括连接器、查询缓存、分析器、优化器、执行器等，所有跨存储引擎的功能都在这一层实现，比如存储过程，触发器，视图，函数等，还有一个通用的日志模块binlog日志模块。
   存储引擎：主要负责数据的存储和读取，采用可以替换的插件式架构。
6. innodb叶子节点单向还是双向的？
   数据页之间是双向的，单条数据之间是单向的



