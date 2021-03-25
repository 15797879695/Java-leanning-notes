1. ##### 如何创建和删除数据库？

   craete  database   数据库名

   drop  database  数据库名 

2. MyISAM与InnoDB的区别？

   - InnoDB 支持事务；MyISAM 不支持事务
   - InnoDB 支持行级锁；MyISAM 支持表级锁
   - InnoDB 支持 MVCC(多版本并发控制)；MyISAM 不支持
   - InnoDB 支持外键，MyISAM 不支持
   - MySQL 5.6 以前的版本，InnoDB 不支持全文索引，MyISAM 支持；MySQL 5.6 及以后的版本，MyISAM 和 InnoDB 存储引擎均支持全文索引
   - InnoDB 不保存表的总行数，执行 select count(*) from table 时
     需要全表扫描；MyISAM 用一个变量保存表的总行数，查总行数速度很快
   - InnoDB 是聚集索引，数据文件是和索引绑在一起的，必须要有主键，
     通过主键索引效率很高。辅助索引需要两次查询，先查询到主键，再通过主键查询到数据。主键太大，其他索引也会很大；MyISAM 是非聚集索引，数据文件是分离的，索引保存的是数据文件的指针，主键索引和辅助索引是独立的

    

   总结：

   - InnoDB 存储引擎提供了具有提交、回滚、崩溃恢复能力的事务安全，与 MyISAM 比 InnoDB 写的效率差一些，并且会占用更多的磁盘空间以保留数据和索引
   - MyISAM 不支持事务、也不支持外键，优势是访问的速度快。对事务的完整性没有要求、以 SELECT 和 INSERT 为主的应用可以使用这个存储引擎

3. char与varchar的区别

   - char 是一种固定长度的字符串类型
   - varchar 是一种可变长度的字符串类型

4. 建表语句中varchar(50)中50的指是什么？

   字段最多存放 50 个字符

   如 varchar(50) 和 varchar(200) 存储 "ConstXiong" 字符串所占空间是一样的，后者在排序时会消耗更多内存

5. int(10)中10指什么？

   INT[(*M*)] [UNSIGNED] [ZEROFILL]  M 默认为11

   10 就是上述语句里的 M，指最大显示宽度，最大值为 255

   最大显示宽度意思是，如果是 int(10)，字段存的值是 10，则，显示会自动在之前补 8 个 0，显示为 0000000010

   int 类型在数据库里面存储占 4 个字节的长度

   - 有符号的整型范围是 -2147483648 ~ 2147483647
   - 无符号的整型范围是 0 ~ 4294967295

6. MySQL如何获取当前日期？

   ```SELECT CURRENT_DATE();javascript
   SELECT CURRENT_DATE();
   ```

7. 如何获取MySQL的版本？

   SELECT  VERSION()

8. 什么是触发器，MySQL都有哪些触发器？

   MySQL 数据库中有六种触发器：

   - Before Insert
   - After Insert
   - Before Update
   - After Update
   - Before Delete
   - After Delete

   使用场景：

   - 可以通过数据库中的相关表实现级联更改
   - 实时监控表中字段的更改做出相应处理

   注意：滥用会造成数据库及应用程序的维护困难

9. MySQL显示表前 50 行

   ```javascript
   SELECT * FROM tablename LIMIT 0,50;
   ```

10. 如何连接MySQL服务端、关闭连接？

    - 连接：使用指令 mysql -u -p -h -P (-u:指定用户名 -p:指定密码 -h:主机 -P:端口) 连接 MySQL 服务端
    - 关闭：使用指令 exit 或 quit

11. text 表示存储的数据类型是字符串类型

12. 说说你知道的MySQL存储引擎

    **InnoDB**

    - 默认事务型引擎，被广泛使用的存储引擎
    - 数据存储在共享表空间，即多个表和索引都存储在一个表空间中，可通过配置文件修改
    - 主键查询的性能高于其他类型的存储引擎
    - 内部做了很多优化，如：从磁盘读取数据时会自动构建hash索引，插入数据时自动构建插入缓冲区
    - 通过一些机制和工具支持真正的热备份
    - 支持崩溃后的安全恢复
    - 支持行级锁
    - 支持外键

     

    **MyISAM**

    - 拥有全文索引、压缩、空间函数
    - 不支持事务和行级锁、不支持崩溃后的安全恢复
    - 表存储在两个文件：MYD 和 MYI
    - 设计简单，某些场景下性能很好，例如获取整个表有多少条数据，性能很高

     

    其他表引擎：Archive、Blackhole、CSV、Memory

13. 说一说MySQL中的锁机制

    按粒度分：

    - 表级锁：粒度最大的一种锁，表示对当前操作的整张表加锁。开销小，加锁快；不会出现死锁；锁定粒度大，发生锁冲突的概率最高，并发度最低
    - 行级锁：粒度最小的一种锁，表示只针对当前操作的行进行加锁。开销大，加锁慢；会出现死锁；锁定粒度最小，发生锁冲突的概率最低，并发度最高
    - 页级锁：粒度介于行级锁和表级锁中间的一种锁。开销、加锁时间和并发度界于表锁和行锁之间；会出现死锁

     

    按操作分：

    - 读锁(共享锁)：针对同一份数据，多个读取操作可以同时进行，不互相影响
    - 写锁(排它锁)：当前写操作没有完成前，会阻断其他写锁和读锁

     

    **MySQL 不同的存储引擎支持不同的锁机制**

    - InnoDB 存储引擎既支持行级锁(row-level locking)，也支持表级锁，但默认情况下是采用行级锁
    - MyISAM 和 MEMORY 存储引擎采用的是表级锁(table-level locking)
    - BDB 存储引擎采用的是页面锁(page-level locking)，也支持表级锁

14. 说一说MySQL中的事务

    **事务具有四大特性（ACID）：**

    - 原子性(Atomic)：一个事务中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环节。事务在执行过程中发生错误，会被回滚到事务开始前的状态，就像这个事务从来没有执行过一样
    - 一致性(Consistency)：在事务开始之前和事务结束以后， 数据库的完整性没有被破坏
    - 隔离性(Isolation)：数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。事务隔离分为不同级别，包括读未提交（Read uncommitted）、读提交（read committed）、可重复读（repeatable read）和串行化（Serializable）
    - 持久性(Durability)：事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失

     

    **事务控制：**

    - BEGIN 或 START TRANSACTION 显式地开启一个事务
    - COMMIT 会提交事务，使已对数据库进行的所有修改成为永久性的
    - ROLLBACK 回滚，会结束用户的事务，并撤销正在进行的所有未提交的修改
    - SAVEPOINT identifier，SAVEPOINT 允许在事务中创建一个保存点，一个事务中可以有多个 SAVEPOINT
    - RELEASE SAVEPOINT identifier 删除一个事务的保存点，没有指定的保存点执行该语句会抛出一个异常
    - ROLLBACK TO identifier 把事务回滚到标记点
    - SET TRANSACTION 用来设置事务的隔离级别

     

    **MYSQL 事务处理的方法：**

    1、用 BEGIN, ROLLBACK, COMMIT来实现

    - BEGIN 开始事务
    - ROLLBACK 事务回滚
    - COMMIT 事务确认

     

    2、直接用 SET 来改变提交模式:

    - SET AUTOCOMMIT=0 禁止自动提交
    - SET AUTOCOMMIT=1 开启自动提交

     

    **注意：**

    - MySQL 的事务控制是表引擎上处理，有些引擎是不支持事务的
    - 不支持事务的表上执行事务操作，MySQL不会发出提醒，也不会报错

15. MySQL中TEXT数据类型的最大长度

    - TINYTEXT：256 bytes
    - TEXT：65,535 bytes(64kb)
    - MEDIUMTEXT：16,777,215 bytes(16MB)
    - LONGTEXT：4,294,967,295 bytes(4GB)

16. MySQL中有哪些时间字段？

    **占用空间**

    - DATETIME：8 bytes
    - TIMESTAMP：4 bytes
    - DATE：4 bytes
    - TIME：3 bytes
    - YEAR：1 byte 

    **日期格式**

    - DATETIME：YYYY-MM-DD HH:MM:SS
    - TIMESTAMP：YYYY-MM-DD HH:MM:SS
    - DATE：YYYY-MM-DD     
    - TIME：HH:MM:SS       
    - YEAR：YYYY   

    ​     
    **最小值**

    - DATETIME：1000-01-01 00:00:00   
    - TIMESTAMP：1970-01-01 00:00:01 UTC
    - DATE：1000-01-01       
    - TIME：-838:59:59       
    - YEAR：1901   

    ​        
    **最大值**

    - DATETIME：9999-12-31 23:59:59   
    - TIMESTAMP：2038-01-19 03:14:07 UTC
    - DATE：9999-12-31       
    - TIME：838:59:59        
    - YEAR：2125 

    ​         
    **零值**

    - DATETIME：0000-00-00 00:00:00
    - TIMESTAMP：1970-01-01 00:00:01 UTC
    - DATE：0000-00-00
    - TIME：00:00:00
    - YEAR：0000

17. MySQL中DATETIME和TIMESTAMP的区别

    区别：

    - DATETIME 的日期范围是 1001——9999 年；TIMESTAMP 的时间范围是 1970——2038 年
    - DATETIME 存储时间与时区无关；TIMESTAMP 存储时间与时区有关，显示的值也依赖于时区
    - DATETIME 的存储空间为 8 字节；TIMESTAMP 的存储空间为 4 字节
    - DATETIME 的默认值为 null；TIMESTAMP 的字段默认不为空(not null)，默认值为当前时间(CURRENT_TIMESTAMP)

18. MySQL的数据类型有哪些？

    1、整数类型： TINYINT、SMALLINT、MEDIUMINT、INT、BIGINT

    分别占用 1 字节、2 字节、3 字节、4 字节、8 字节；任何整数类型都可以加上 UNSIGNED 属性，表示数据是无符号的，即非负整数；整数类型可以被指定长度，即为显示长度，不影响存储占用空间

     

    2、实数类型： FLOAT、DOUBLE、DECIMAL

    DECIMAL 可以用于存储比 BIGINT 还大的整型，能存储精确的小数；FLOAT 和 DOUBLE 有取值范围，支持使用标准的浮点进行近似计算

     

    3、字符串类型： CHAR、VARCHAR、TEXT、BLOB

    CHAR 是定长的，根据定义的字符串长度分配足够的空间；VARCHAR 用于存储可变长字符串；TEXT 存大文本；BLOB 存二进制数据

     

    4、枚举类型：ENUM

    把不重复的数据存储为一个预定义的集合，可以替代常用的字符串类型

     

    5、日期和时间类型：YEAR、TIME、DATE、TIMESTAMP、DATETIME

    分别占用 1 byte、3 bytes、4 bytes、4 bytes、8 bytes

19. FLOAT和DOUBLE的区别是什么？

    - FLOAT 类型数据可以存储至多 8 位十进制数，占 4 字节
    - DOUBLE 类型数据可以存储至多 18 位十进制数，占 8字节

20. Mysql驱动程序是什么？

    - Mysql 提供给 Java 编程语言的驱动程序就是这样 mysql-connector-java-5.1.18.jar 包
    - 针对不同的数据库版本，驱动程序包版本也不同
    - 不同的编程语言，驱动程序的包形式也是不一样的
    - 驱动程序主要帮助编程语言与 MySQL 服务端进行通信，如果连接、关闭、传输指令与数据等

21. Innodb引擎有什么特性？

    - 插入缓冲(insert buffer)
    - 二次写(double write)
    - 自适应哈希索引(ahi)
    - 预读(read ahead)

22. 使用过MySQL的存储过程吗？介绍一下

    - 存储过程(Stored Procedure)是数据库中一种存储复杂程序，供外部程序调用的一种数据库对象
    - 是一段 SQL 语句集，被编译保存在数据库中
    - 可命名并传入参数来调用执行
    - 可在存储过程中加入业务逻辑和流程
    - 可在存储过程中创建表，更新数据，删除数据等
    - 可通过把 SQL 语句封装在容易使用的单元中，简化复杂的操作

23. 什么是索引？什么场景使用？

    索引是对数据库表中一列或多列的值进行排序的一种结构，使用索引可快速访问数据库表中的特定信息

    使用索引目的是加快检索表中数据

    使用场景：

    - 中到大数据量表适合使用索引
    - 小数据量表，大部分情况全表扫描效率更高
    - 特大数据量表，建立和使用索引的代价会随之增大，适合使用分区或分库

24. 索引的种类有哪些？

    - 普通索引：最基本的索引，没有任何约束限制。
    - 唯一索引：和普通索引类似，但是具有唯一性约束，可以有 null
    - 主键索引：特殊的唯一索引，不允许有 null，一张表最多一个主键索引
    - 组合索引：多列值组成一个索引，用于组合搜索，效率大于索引合并
    - 全文索引：对文本的内容进行分词、搜索
    - 覆盖索引：查询列要被所建的索引覆盖，不必读取数据行

25. 索引如何创建与删除？

    - 创建单个字段索引的语法：CREATE INDEX 索引名 on 表名(字段名)
    - 创建联合索引的语法：CREATE INDEX 索引名 on 表名(字段名1，字段名2)
    - 索引命名格式一般可以这样：idx_表名_字段名。注意有长度限制
    - 删除索引：DROP INDEX 索引名 ON 表名

    如：

    给 id 创建索引：CREATE INDEX idx_t1_id on t1(id);

    给 username 和 password 创建联合索引：CREATE index idx_t1_username_password ON t1(username,password)

     

    index 替换成 unique 或 primary key，分别代表唯一索引和主键索引

26. 索引对性能有哪些影响？

    优点：

    - 减少数据库服务器需要扫描的数据量
    - 帮助数据库服务器避免排序和临时表
    - 将随机 I/O 变顺序I/O
    - 提高查询速度
    - 唯一索引，能保证数据的唯一性

     

    缺点：

    - 索引的创建和维护耗时随着数据量的增加而增加
    - 对表中数据进行增删改时，索引也要动态维护，降低了数据的维护速度
    - 增大磁盘占用

27. MySQL创建和使用索引的注意事项？

    - 适合创建索引的列是出现在 WHERE 或 ON 子句中的列，而不是出现在 SELECT 关键字后的列
    - 索引列的基数越大，数据区分度越高，索引的效果越好
    - 对字符串列进行索引，可制定一个前缀长度，节省索引空间
    - 避免创建过多的索引，索引会额外占用磁盘空间，降低写操作效率
    - 主键尽可能选择较短的数据类型，可减少索引的磁盘占用，提高查询效率
    - 联合索引遵循前缀原则
    - LIKE 查询，%在前不到索引，可考虑使用 ElasticSearch、Lucene 等搜索引擎
    - MySQL 在数据量较小的情况可能会不使用索引，因为全表扫描比使用索引速度更快
    - 关键词 or 前面的条件中的列有索引，后面的没有，所有列的索引都不会被用到
    - 列类型是字符串，查询时一定要给值加引号，否则索引失效
    - 联合索引要遵从最左前缀原则，否则不会用到索引

28. 创建MySQL联合索引应该注意什么？

    - 联合索引要遵从最左前缀原则，否则不会用到索引
    - Mysql从左到右的使用索引中的字段，一个查询可以只使用索引中的一部份，但只能是最左侧部分。
    - 如索引是 index (a,b,c)，可以支持 a 或 a,b 或 a,b,c 3种组合进行查找，但不支持 b,c 进行查找

29. 列值为NULL时，查询是否会用到索引？

    MySQL 中存在 NULL 值的列也是走索引的

    计划对列进行索引，应尽量避免把它设置为可空，因为这会让 MySQL 难以优化引用了可空列的查询，同时增加了引擎的复杂度

30. 以下语句是否会使用索引？

     不会，因为列涉及到运算，不会使用索引 

31. 以下三条sql 如何只建一条索引？

    ```javascript
    WHERE a=1 AND b=1
    WHERE b=1
    WHERE b=1 ORDER BY time DESC
    ```

     

    以顺序 b，a，time 建立联合索引，CREATE INDEX idx_b_a_time ON table(b,a,time)。

    新 MySQL 版本会优化 WHERE 子句后面的列顺序，以匹配联合索引顺序

32. 与Oracle相比，Mysql有什么优势

    - Mysql 是开源软件、无需付费
    - 操作简单、部署方便，用户可以根据应用的需求去定制数据库
    - Mysql 的引擎是插件式

33. MySQL有哪些常用函数？

    **数值型函数**

    - ABS：计算绝对值
    - SQRT：计算二次方根
    - MOD：计算余数
    - CEIL、CEILING：返回不小于参数的最小整数，即向上取整
    - FLOOR：向下取整，返回值转化为一个 BIGINT
    - RAND：生成一个 0~1 之间的随机数
    - ROUND：四舍五入
    - SIGN：返回参数的符号
    - POW、POWER：参数次方的值
    - SIN：计算正弦值
    - ASIN：计算反正弦值
    - COS：计算余弦值
    - ACOS：计算反余弦值
    - TAN：计算正切值
    - ATAN：计算反正切值
    - COT：计算余切值

     

    **字符串函数**

    - LENGTH：返回字符串的字节长度
    - CONCAT：合并字符串，返回结果为连接参数产生的字符串，参数可以使一个或多个
    - INSERT：替换字符串
    - LOWER：将字符串中的字母转换为小写
    - UPPER：将字符串中的字母转换为大写
    - LEFT：从左侧字截取符串，返回字符串左边的若干个字符
    - RIGHT：从右侧字截取符串，返回字符串右边的若干个字符
    - TRIM：删除字符串左右两侧的空格
    - REPLACE：字符串替换，返回替换后的新字符串
    - SUBSTRING：截取字符串，返回从指定位置开始的指定长度的字符换
    - REVERSE：字符串反转，返回与原始字符串顺序相反的字符串

     

    **日期和时间函数**

    - CURDATE、CURRENT_DATE：返回当前系统的日期值
    - CURTIME、CURRENT_TIME：返回当前系统的时间值
    - NOW、SYSDATE：返回当前系统的日期和时间值
    - UNIX_TIMESTAMP：获取 UNIX 时间戳函数，返回一个以 UNIX 时间戳为基础的无符号整数
    - FROM_UNIXTIME：将 UNIX 时间戳转换为时间格式
    - MONTH：获取指定日期中的月份
    - MONTHNAME：获取指定日期中的月份英文名称
    - DAYNAME：获取指定曰期对应的星期几的英文名称
    - DAYOFWEEK：获取指定日期对应的一周的索引位置值
    - WEEK：获取指定日期是一年中的第几周
    - DAYOFYEAR：获取指定曰期是一年中的第几天，返回值 1~366
    - DAYOFMONTH：获取指定日期是一个月中是第几天，返回值 1~31
    - YEAR：获取年份
    - TIME_TO_SEC：将时间参数转换为秒数
    - SEC_TO_TIME：将秒数转换为时间
    - DATE_ADD、ADDDATE：向日期添加指定的时间间隔
    - DATE_SUB、SUBDATE：向日期减去指定的时间间隔
    - ADDTIME：时间加法运算，在原始时间上添加指定的时间
    - SUBTIME：时间减法运算，在原始时间上减去指定的时间
    - DATEDIFF：获取两个日期之间间隔，返回参数 1 减去参数 2 的值
    - DATE_FORMAT：格式化指定的日期，根据参数返回指定格式的值
    - WEEKDAY：获取指定日期在一周内的对应的工作日索引

     

    **聚合函数**

    - MAX：查询指定列的最大值
    - MIN：查询指定列的最小值
    - COUNT：统计查询结果的行数
    - SUM：求和，返回指定列的总和
    - AVG：求平均值，返回指定列数据的平均值

     

    **流程控制函数**

    - IF：判断是否为 true
    - IFNULL：判断是否为空
    - CASE：分支判断

34. MyISAM索引与InnoDB索引的区别？

    - InnoDB 索引是聚簇索引，MyISAM 索引是非聚簇索引
    - InnoDB 的主键索引的叶子节点存储着行数据，主键索引非常高效
    - MyISAM 索引的叶子节点存储的是行数据地址，需要再寻址一次才能得到数据
    - InnoDB 非主键索引的叶子节点存储的是主键和其他带索引的列数据，因此查询时做到覆盖索引会非常高效

35. Mysql的体系结构是什么样的？

    - 连接者：不同语言的代码程序和 Mysql 的交互
    - 连接池：认证、线程管理、连接限制、内存校验、部分缓存
    - 管理服务和工具组件：系统管理和控制工具，例如备份恢复、Mysql 复制、集群等 
    - SQL接口：接受用户的 SQL 命令，并且返回用户需要查询的结果
    - 查询解析器：SQL 命令传递到解析器的时候会被解析器验证和解析(权限、语法结构)
    - 查询优化器：SQL 语句在查询之前会使用查询优化器对查询进行优化
    - 缓存：如果查询缓存有命中的查询结果，查询语句就可以直接去查询缓存中取数据
    - 插入式存储引擎：对数据存储、更新、查询数据等操作的管理，支持选择使用不同的存储引擎

36. Mysql中exists和in的区别

    下面将主查询的表称为外表；子查询的表称为内表。exists 与 in 的主要区别如下：

    - 子查询使用 exists，会先进行主查询，将查询到的每行数据循环带入子查询校验是否存在，过滤出整体的返回数据；子查询使用 in，会先进行子查询获取结果集，然后主查询匹配子查询的结果集，返回数据
    - 外表内表相对大小情况不一样时，查询效率不一样：两表大小相当，in 和 exists 差别不大；内表大，用 exists 效率较高；内表小，用 in 效率较高。
    - 不管外表与内表的大小，not exists 的效率一般要高于 not in，跟子查询的索引访问类型有关。

37. MySQL如何进行慢SQL优化？

    思路：

    - 通过慢查询日志去寻找哪些 SQL 执行效率低
    - 使用 explain 获取低效率 SQL 的执行计划
    - 结合 SQL 与执行计划，进行分析与优化

     

    引起 SQL 查询很慢的原因与解决办法：

    1、没有索引。解决办法：

    - 根据 where 和 order by 使用比较频繁的字段创建索引，提高查询效率
    - 索引不宜过多，单表最好不要超过 6 个。索引过多会导致占用存储空间变大；insert、update 变慢
    - 删除未使用的索引

     

    2、索引未生效。解决办法：

    - 避免在 where 子句中对字段进行 null 值判断，创建表默认值是 NULL。尽量使用 NOT NULL，或使用特殊值，如 0、-1
    - 避免在 where 子句中使用 != 或 <> 操作符， MySQL 只有对以下操作符才使用索引：<、<=、=、>、>=、BETWEEN、IN、非 % 开头的 LIKE
    - 避免在 where 子句中使用 or 来连接条件，可以使用 UNION 进行连接
    - 能用 union all 就不用 union，union 过滤重复数据要耗费更多的 CPU 资源
    - 避免部分 like 查询，如 '%ConstXiong%'
    - 避免在索引列上使用计算、函数
    - in 和 not in 慎用，能用 between 不要用 in
    - select 子句中避免使用 *

     

    3、单表数据量太大。解决办法：

    - 分页查询(在索引上完成排序分页操作、借助主键进行关联)
    - 单表数据过大，进行分库分表
    - 考虑使用非关系型数据库提高查询效率
    - 全文索引场景较多，考虑使用 ElasticSearch、solr

     

    提升性能的一些技巧：

    - 尽量使用数字型字段
    - 只需要一行数据时使用 limit 1
    - 索引尽量选择较小的列
    - 不需要的数据在 GROUP BY 之前过滤掉
    - 大部分时候 exists、not exists 比 in、not in 效率（除了子查询是小表的情况使用 in 效率比 exists 高）
    - 不确定长度的字符串字段使用 varchar/nvarchar，如使用 char/nchar 定长存储会带来空间浪费
    - 不要使用 select *，去除不需要的字段查询
    - 避免一次性查询过大的数据量
    - 使用表别名，减少多表关联解析时间
    - 多表 join 最好不超过 5 个，视图嵌套最好不超过 2 个
    - or 条件查询可以拆分成 UNION 多个查询
    - count(1) 比 count(*) 有效
    - 判断是否存在数据使用 exists 而非 count，count 用来获取数据行数

38. 说说一些索引失效的情况

    - 如果条件中有 or，即使其中有部分条件是索引字段，也不会使用索引
    - 复合索引，查询条件不使用索引前面的字段，后续字段也将无法使用索引
    - 以 % 开头的 like 查询
    - 索引列的数据类型存在隐形转换
    - where 子句里对索引列有数学运算
    - where 子句里对索引列使用函数
    - MySQL 引擎估算使用全表扫描要比使用索引快，则不使用索引

39. 这种情况，ID 是几？

    一张自增表里总共有 7 条数据，删除了最后 2 条，重启 MySQL 数据库，又插入了 1 条，此时 ID 是几？

    ------

    参考答案

    表的存储引擎如果是 MyISAM，ID = 8
    表的存储引擎如果是 InnoDB，ID = 6

    InnoDB 表只会把自增主键的最大 ID 记录在内存中，所以重启之后会导致最大 ID 丢失

40. 说一说 MySQL 的行锁和表锁

    MyISAM 只支持表锁；InnoDB 支持表锁和行锁，默认为行锁。

     

    - 表锁：开销小，加锁快，不会出现死锁。锁粒度大，发生锁冲突的概率最高，并发量最低。
    - 行锁：开销大，加锁慢，会出现死锁。锁粒度小，发生锁冲突的概率小，并发度最高。

41. 说一说MySQL的乐观锁和悲观锁？

    乐观锁：每次去获取数据的时候都认为别人不会修改，不会上锁，但是在提交修改的时候会判断一下在此期间别人有没有修改这个数据。
    悲观锁：每次去获取数据的时候都认为别人会修改，每次都会上锁，阻止其他线程获取数据，直到这个锁释放。

    MySQL 的乐观锁需要自己实现。一般在表里面添加一个 version 字段，每次修改成功值加 1；每次其他字段值的时候先对比一下，自己拥有的 version 和数据库现在的 version 是否一致，如果不一致就可以返回失败也可以进行重试。

    MySQL 的悲观锁，以 Innodb 存储引擎为例，将 MySQL 设置为非 autoCommit 模式

    ```javascript
    begin;
    select * from table where id = 1 for update;
    insert ...
    update ...
    commit;
    ```

    当上面语句未 commit，id = 1 的数据是被锁定的，即其他事务中的查到这条语句会被阻塞，直到事务提交。

    数据的锁定还涉及到索引的不同可能是行锁、表锁的问题。

42. 举例说明数据库死锁

    加锁顺序不一致可能会导致死锁：

    1、事务 1 持有 id = 1 的行锁，更新 id = 2 的行数据；事务 2 持有 id = 2 的行锁，更新 id = 1的行数据

    ![img](C:\Users\LYM\Desktop\知识总结\image__20200725145411.png)

     

    2、在范围查询更新时，加锁是一条记录一条记录挨个加锁的，数据行被加锁顺序不一样也会导致死锁

    ```javascript
    事务1                                        
    update table set name = 'A' where id <100;
    
    事务2
    update table set name = 'B' where age > 25;
    ```

43. MySQL中如何避免死锁？

    - 尽量以相同的顺序来访问索引记录和表
    - 业务上能够接受幻读和不可重复读，考虑降低锁的级别到 Read committed，降低死锁发生的概率
    - 添加合理的索引，走索引避免为每一行加锁，降低死锁的概率
    - 在事务中一次锁定所需要的所有资源，如 MyISAM 引擎的表锁
    - 避免大事务，尽量将大事务拆成多个小事务来处理
    - 尽量避免同时并发对同一表进行读写操作，特别是执行加锁且操作数据量较大的语句
    - 设置锁等待超时参数

44. truncate和delete的异同？

    - truncate 命令永久地从表中删除所有数据；delete 命令从一个表中删除某一行或多行数据
    - truncate 和 delete 都可以将数据实体删掉，truncate 的操作并不记录到 rollback 日志，操作速度较快，删除数据不能恢复
    - delete 操作不释放表空间
    - truncate 不能对视图等进行删除；delete 可以删除单表的视图数据(本质是对表数据的删除)
    - truncate 是数据定义语言(DDL)；delete 是数据操纵语言(DML)

45. 脏读、幻读、不可重复读指什么？

    - 脏读：一个事务读取另外一个事务还没有提交的数据。
    - 幻读：指在一个事务内两次读取同一条件的数据，两次读取的数据条数不同。
    - 不可重复读：指在一个事务内，多次读同一条件的数据，数据条数相同但数据的值发生了改变。

     

    幻读与不可重复读的区别就是：幻读是由于其他事务 insert 或 delete 导致的；不可重复读是由于其他事务 update 导致的。

46. 事务有哪些隔离级别？

    - 读未提交（Read Uncommitted）：是最低的事务隔离级别，它允许另外一个事务可以看到这个事务未提交的数据。会出现脏读，幻读，不可重复读，所有并发问题都可能遇到。
    - 读已提交（Read Committed）：保证一个事物提交后才能被另外一个事务读取。另外一个事务不能读取该事物未提交的数据。不会出现脏读现象，但是会出现幻读，不可重复读。
    - 可重复读（Repeatable Read）：这种事务隔离级别可以防止脏读，不可重复读，但是可能会出现幻象读。它除了保证一个事务不能被另外一个事务读取未提交的数据之外还避免了不可重复读。
    - 序列化（Serializable）：这是花费最高代价但最可靠的事务隔离级别。事务被处理为顺序执行。防止脏读、不可重复读、幻象读。

47. delete、drop、truncate区别

    - truncate 和 delete 只删除数据，不删除表结构；drop 删除表结构
    - 表空间：delete 不释放；truncate 不一定释放；oracle 数据库的 drop 将表删除到回收站，可以被彻底删除也可以被还原
    - 删除数据的速度：drop > truncate > delete
    - delete 属于 DML 语言，需要事务管理，commit 之后才能生效；drop 和 truncate 属于 DDL 语言，操作立刻生效，不可回滚
    - 使用场合：不再需要表时使用 drop 语句; 保留表删除所有记录用 truncate 语句; 删除部分记录用 delete 语句

48. 数据库的三范式是什么？有什么作用？

    - 列不可分，确保表的每一列都是不可分割的原子数据项。作用：方便字段的维护、查询效率高、易于统计。
    - 属性字段完全依赖（完全依赖指不能存在仅依赖主键的部分属性）于主键。作用：保证每行数据都是按主键划分的独立数据。
    - 任何非主属性字段不依赖于其它非主属性字段。作用：减少表字段与数据存储，让相互依赖的非主键字段单独成为一张关系表，记录被依赖字段即可。

    
    三大范式只是一般设计数据库的基本理念，可以设计冗余较小、存储查询效率高的表结构。

    但不能一味的去追求数据库设计范式，数据库设计应多关注需求和性能，重要程度：需求 - 性能 - 表结构。比如有时候添加一个冗余的字段可以大大提高查询性能。

49. 左连接、右连接、内连接和全外连接的区别

    - 左连接(left join)：返回包括左表中的所有记录和右表中连接字段相等的记录。
    - 右连接(right join)：返回包括右表中的所有记录和左表中连接字段相等的记录。
    - 内连接(inner join)：只返回两个表中连接字段相等的记录。
    - 全外连接(full join)：返回左右表中连接字段相等的记录和剩余所有记录。

50. 常用命令

     insert  into  ... values 

     delete  ... from 

     update ... set 

     select...from 

     去除重复查询，select distinct  name from school

    

    在 SQL 中增加 HAVING 子句原因是，WHERE 关键字无法与合计函数一起使用。

    ### SQL HAVING 语法

    ```
    SELECT column_name, aggregate_function(column_name)
    FROM table_name
    WHERE column_name operator value
    GROUP BY column_name
    HAVING aggregate_function(column_name) operator value
    ```

    备份

    导出数据

    1、mysqldump  -uroot -ppassword  数据库 表名 >d:/lym.sql

    2、mysqldump  -uroot  -ppassword 数据库 表一，表二，表三   >d:/lym.sql

    3、mysqldump  -uroot -ppassword  数据库  >d:/lym.sql

    导入数据

    source  lym.sql

    

51. 视图

    什么是视图：
        视图是一个虚拟表，其内容由查询定义。同真实的表一样，视图包含一系列带有名称的列和行数据。但是，视图并不在数据库中以存储的数据值集形式存在。行和列数据来自由定义视图的查询所引用的表，并且在引用视图时动态生成。
        视图具有表结构文件，但不存在数据文件。
        对其中所引用的基础表来说，视图的作用类似于筛选。定义视图的筛选可以来自当前或其它数据库的一个或多个表，或者其它视图。通过视图进行查询没有任何限制，通过它们进行数据修改时的限制也很少。
        视图是存储在数据库中的查询的sql语句，它主要出于两种原因：安全原因，视图可以隐藏一些数据，如：社会保险基金表，可以用视图只显示姓名，地址，而不显示社会保险号和工资数等，另一原因是可使复杂的查询易于理解和使用。

    ```
    CREATE VIEW [Current Product List] AS
    SELECT ProductID,ProductName
    FROM Products
    WHERE Discontinued=No
    ```

    ```
    SELECT * FROM [Current Product List]
    ```

     drop  view  viewname

52. x /* 锁表 */表锁定只用于防止其它客户端进行不正当地读取和写入MyISAM 支持表锁，InnoDB 支持行锁-- 

    锁定    LOCK TABLES tbl_name [AS alias]-- 

    解锁    UNLOCK TABLES

    

    

53. 存储过程

    创建:create  procedure  过程名（参数列表）

    begin

    过程体

    end

    存储过程是一段可执行性代码的集合。相比函数，更偏向于业务逻辑。
    调用：CALL 过程名
    -- 注意
    - 没有返回值。
    - 只能单独调用，不可夹杂在其他语句中
    -- 参数
    IN|OUT|INOUT 参数名 数据类型
    IN      输入：在调用过程中，将数据输入到过程体内部的参数
    OUT     输出：在调用过程中，将过程体处理完的结果返回到客户端
    INOUT   输入输出：既可输入，也可输出

54. 触发器

    create trigger  trigger-name  trigger_time trigger_event  ON   tablename  FRO   EACH  ROW  trigger _stmt

    参数：
        trigger_time是触发程序的动作时间。它可以是 before 或 after，以指明触发程序是在激活它的语句之前或之后触发。
        trigger_event指明了激活触发程序的语句的类型
            INSERT：将新行插入表时激活触发程序
            UPDATE：更改某一行时激活触发程序
            DELETE：从表中删除某一行时激活触发程序
        tbl_name：监听的表，必须是永久性的表，不能将触发程序与TEMPORARY表或视图关联起来。
        trigger_stmt：当触发程序激活时执行的语句。执行多个语句，可使用BEGIN...END复合语句结构

    删除触发器

     drop trigger trigger—name

55. Mybatis 缓存

    1、一级缓存，Mybatis 默认开启的是一级缓存，一级缓存是sqlsession 级别的当有相同的sql查询时 可以直接使用缓存数据，使用hashmap 进行存储

    2、二级缓存，默认不开启，可以在全局配置文件Mybatis-config 中配置配置开启，eacheable =true ，也可以单独在mapper 中开启，eacheable =true  ，也可以单独在 sql 标签中使用 useCache =false 关闭二级缓存。默认是开启的，二级缓存是跨sqlsession的。



