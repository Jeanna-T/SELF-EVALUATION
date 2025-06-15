# 数据库学习记录

## 一、数据库基本概念与操作(week1-week6)
#### 学习内容：
1. 了解什么是数据库和DBMS（Database-management system)
2. 学习关系数据库的结构、关系模式、码、关系代数等概念
3. 学习掌握关系数据库的DDL和DML语法，包括基本数据类型、模式定义、基本查询结构以及排序、字符串等操作。
4. 学习多表查询、null值处理、聚集函数、嵌套子查询等操作。
5. 学习修改数据库的语句，包括增删改查、主要约束含义以及排名操作。

#### 重点笔记：
1. 如何理解数据模式(schema)与实例(instance)？
   * 数据模式(schema)是对数据库结构的描述或设计，定义了数据库中有哪些表、每个表的属性、属性的数据类型、以及各种约束（如主键、外键等），模式是静态的，类比建筑的"设计蓝图"；
   * 实例(instance)是数据库在某一时刻实际存储的数据，是数据模式的具体体现，实例是动态的，类比根据图纸实际建造出来的房子。
2. 如何认识各种码(key)？
   * 超码：一个或多个属性的集合，可以唯一标识一个元组。超码是不唯一的。
   * 候选码：真子集不能构成超码的超码，即最小超码。候选码是不唯一的。比如学号/身份证都可以是候选码。
   * 主码：人工选定的唯一标识符。比如选定学号为主码。
   * 外码：建立表与表之间的联系，确保数据一致性，需引用另一个表的主码。
3. 单精度浮点与双精度浮点的区别？
   * 浮点数是计算机中表示实数的方式。单精度浮点存储大小为4字节，有效数字约6-7为十进制，计算速度较快；双精度浮点存储大小为8字节，有效数字约15-16为十进制，计算速度较慢；
4. SQL字符串中注意事项：
   * 字符串在postgresql中区分大小写。
   * `%`表示匹配任意字符串；`_`表示匹配任意字符。如果要匹配`%`本身，需使用`\%`；如果要匹配`\`本身，需使用两个反斜杠`\\`。
   * 用`||`连接字符串。
5. 空值`null`的处理：
   * `null` 表示 "缺失的值" 或 "未知"，不是0、不是空字符串。
   * `null`对任何运算结果都为`null`。
   * `false`和`unknown`的结果均不会显示出来。
   * not unknown is unknown.
   * 聚集函数只有`count(*)`不会忽略`null`。
6. 主要约束有哪些，都有什么含义？
   * UNIQUE约束：确保某列或列组合中所有值都是唯一的，防止数据重复插入。
   * PRIMARY KEY约束：相当于UNIQUE+NOT NULL，唯一标识表中的每一行数据。
   * FOREIGN KEY约束：确保表中的数据与另一个表的数据匹配，维护表之间的关系。
   * CHECK约束：确保列中的值满足特定条件。
   * NOT NULL约束：确保列不能包含NULL值。
   * 为约束命名的作用是明确标识约束，便于管理维护，在需要修改删除约束时可以直接引用名称。
7. SQL查询的逻辑执行顺序？
   * from -> where -> group by -> having -> select -> order by -> limit
   * select字句中的列别名不能用于where子句。
 
#### 补充学习：
学习网课密歇根大学《面向所有人的 PostgreSQL 专项课程|postgresql-for-everybody》
[link](https://www.coursera.org/specializations/postgresql-for-everybody#courses) 

  * 学习记录：
    * 1. SQL is not a procedural language.
      2. SQL is a language that provides us opreations to Create,Read,Update,and Delete (CRUD) our data in a database.
      3. SQL has no real concept of loop. But delete has kind of an implied loop around it. `WHERE`is like a loop plus an if statement.
      4. `offset 1`is the second row,not the first row.
      5. The power of SQL comes when we have more than one tables and we can exploit the relationships between the tables.
      6.  `Indexes`:Indexes are shorcuts.As a table gets large,sacnning all the data to find a single row becomes very costly.There are techniques to shorten the scan as long as you create data structures and maintain those structures.`Hashes` and `Trees` are the most common indexes.
      7.  When add an index like primary key, you are actually telling the database store the data about where the data is.
      8.  `Database design`:The goal is to spread the data across multiple tables so end up with no replication of the data in the tables.
      9.  Drawing a picture of the data objects for our application and then figuring out how to represent the objects and their relationships.
      10.  Example:building a music data model: 
           `Track -> Album -> Artist -> Genre`
           
  * 学习感悟：
    * 课程内容由浅入深，讲授风格也很有趣，课程包括基本的SQL查询、数据建模、关系设计，到复杂的连接、聚合与子查询，层层递进，帮助我逐步建立起对数据库系统的整体认知。虽然目前只看到课程的第一部分，但是让我在掌握基础的SQL语法基础上，了解了SQL底层存储与设计逻辑，体会到数据背后的逻辑结构与管理思维。英文的讲解能够让我对数据库中的一些概念有了更好的理解，接下来会利用空余时间看完后续课程。。


## 二、中级SQL(week7-week8)
#### 学习内容：
1. 学习多表查询，理解 INNER JOIN、LEFT/RIGHT JOIN、FULL JOIN 的区别和使用方法。
2. 区分连接类型和连接条件。
3. 学习完整性约束知识和视图相关内容。

#### 重点笔记：
1. 如何理解连接类型和连接条件？
   * 连接类型决定：“哪些记录应该被保留在结果中”，包括`inner join`,`left outer join`,`right outer join`,`full outer join`.
   * 连接条件决定：“两张表之间哪些行应该被认为是匹配的”，包括`natural join`,`on`,`using`.
2. 不同的连接类型的区别？
   * `A inner join B`:只返回A和B两个表中匹配的行；
   * `A left join B`:返回A表的所有行，无论B表是否有匹配。如果B表没有匹配，则B的列显示NULL；
   * `A right join B`:返回B表的所有行，无论A表是否有匹配。如果A表没有匹配，则A的列显示NULL；
   * `A full join B`:返回两个表的所有行，无论是否有匹配。没有匹配的部分用NULL显示。
3. 不同的连接条件的区别？
   * `natural join`:隐式使用所有同名列；
   * `on`:`A join B on A.id = B.id`;
   * `using`:`A join B using(id)`;
   * 默认join就是inner join。
   * 自然连接输出只保留相同属性的一个。
4. 当使用显式join语法时，连接条件应该使用on子句而不是where子句。
5. where子句应该出现在所有join操作之后，用于过滤最终结果集。

#### 补充学习：
为了提升代码能力，完成`牛客网`的SQL快速入门的题目，用于上机练习。包括基础查询、条件查询、高级查询、多表查询、必会的常用函数以及综合练习，共42道题目。
![image](https://github.com/user-attachments/assets/9d3c9055-447c-4c81-9180-6953fb341b31)


## 三、高级SQL(week9-week11)
#### 学习内容：
1. 学习了解了高级数据类型，包括时间和日期类型，point类型、如何自定义数据类型等，以及数据类型间转换语句。
2. 学习SQL中的授权内容。
3. 学习SQL中如何定义和使用函数。
4. 了解触发器和如何使用程序语言访问SQL。
5. 了解SQL注入攻击内容。

#### 重点笔记：
1. time和timestamp两种日期数据类型的区别？
   * `time`：只表示时间（如：13:45:30），不含日期，默认精度为6位。
   * `timestamp`：表示日期 + 时间（如：2025-06-13 13:45:30），默认精度为6位。
2. 在对精度有要求的场景中，不要使用float、real、double precision，建议用numeric或decimal，后者是精确型的数据类型，适合金融类运算。
3. 如何理解SQL中的授权？
   * 在SQL中，“授权”（Authorization）指的是将特定操作权限赋予给用户或角色，控制他们对数据库中对象（如表、视图、函数等）的访问能力。
   * 常见权限包括：SELECT、INSERT、UPDATE、DELETE、ALL PRIVILEGES、CREATE、ALTER、DROP等。
   * 用户（User）是登录数据库的实体，角色（Role）是权限的集合，可以赋予多个用户共享。
     ```sql
     -- 创建角色并授予权限
     CREATE ROLE analyst;
     GRANT SELECT ON sales TO analyst;

     -- 将角色赋给具体用户
     GRANT analyst TO alice;
4. 函数返回结果，过程用于执行一些操作。
5. 函数基本要素包括：
   * 函数名：清晰、有意义，符合命名规范
   * 参数列表：指明输入变量及其数据类型
   * 返回类型：函数执行结果的数据类型
   * 函数体：包含逻辑、条件、返回值
   * 语言类型：指定使用的语言（如 SQL）
   * `$`表示函数体的定界符以及参数位置
6. 什么是URL？
   * URL（Uniform Resource Locator，统一资源定位符）是互联网上资源的地址，用于唯一标识并访问某个网络资源，比如网页、图片、视频、API 等。
   * 可以看作浏览器或程序在访问网络资源时的“地址说明书”。
   * 基本结构为：`协议://主机名:端口/路径?查询参数#片段`
   * 协议（Scheme）：定义资源访问的协议类型，如`http`，`https`
     主机名（Host）：标识资源所在的服务器，可以是域名或IP地址
     端口（Port）：指定服务的网络端口，HTTP默认80端口，HTTPS默认443端口
     路径（Path）：指定服务器上的资源位置，如`/index.html`
7. 如何解决SQL恶意注入问题？
   * 如果以字符串拼接的方式用SQL语句查询一名用户信息，如果用户输入的是：`' OR '1'='1`，拼接后变为`SELECT * FROM users WHERE username = '' OR '1'='1' `，则会返回所有用户的记录。如果用于登录验证，任何人都能“绕过密码”直接登录。
   * 使用 ORM（对象关系映射，Object-Relational Mapping）防止 SQL 注入，本质上是通过参数绑定自动构建安全的SQL查询，而不是将用户输入直接拼接进SQL语句中，从而避免注入攻击。比如`User.objects.get(username="alice")`，ORM框架会自动将字段和表映射成SQL语句，并将"alice"作为安全参数传入给SQL，而不是字符串拼接，等价于手动写的 PreparedStatement。
     
#### 补充学习：
完成`牛客网`的SQL热题的题目20道。
![image](https://github.com/user-attachments/assets/894286df-8475-4246-aa76-0bb4c618f5b8)

## 四、数据库设计(week12-week13)
#### 学习内容：
1. 学习了E-R图的概念、如何建立E-R图、如何将E-R图转化为关系模式。
2. 学习关系数据库的范式，理解什么是“好的模式”。
3. 学习模式分解、函数依赖的概念以及BCNF和3NF两种范式。
4. 自主探索draw.io的使用。
   
#### 重点笔记：
1. 为什么E-R图中可以出现复合属性和多值属性，关系模型却要求原子性？
   * E-R图用于概念建模，帮助设计者理解和表达现实世界中的数据结构，允许更丰富的属性类型是为了更自然描述现实世界。
   * 关系模型用于逻辑实现，是数据库实际存储和操作的数据结构，要求原子性、不能有表的嵌套关系、每个单元格只能有一个值。
   * 在将E-R模型转换为关系模型时，必须“拆分”或“规范化”复合属性和多值属性。
     ```text
     原E-R属性：name { first_name, middle_name, last_name }
     转换后关系模型属性：first_name, middle_name, last_name
2. 注意映射基数在E-R图中如何表示：
   * 箭头从 many 指向 one 的一方。
   * 两条线代表实体的全参与。
3. 如何确定联系集的主码？
   * 联系集的主码是能唯一标识每一个“联系”实例的一组属性。通常由参与联系的实体集的主码（或部分主码）组成，视联系的参与度和联系类型（多对多、一对多、一对一）而定。
   * 如果是 one-to-many ，则主码 = “多”方实体的主码。
   * 如果是 many-to-many，则主码 = 所有关联实体的主码的组合。
4. 如何理解弱实体集？
   * 自身没有足够的属性来唯一标识其实体的实体集，它依赖于另一个实体集（称为“强实体集”）来提供标识性。
   * 在E-R图中以双线表示，转化为关系模型时需引入外键，并构造组合主键。
6. E-R图转关系模式需要注意的问题：
   * 复合属性要拆分为多个原子属性（满足第一范式）
   * 多值属性要建立一个新关系，包括实体主码 + 属性值
   * 弱实体集要引入强实体的主码作为外键
7. 怎么理解范式？
   * 范式（Normal Form）是关系数据库设计的一种形式规范，目标是减少数据冗余，避免插入、删除、更新异常，保持数据一致性和可维护性。
   * 范式包括：1NF、2NF、3NF、BCNF
   * 对于过大的模式，可能会存在数据冗余，更新异常，插入/删除异常，维护难度大等缺点。
   * 对于过小的模式，可能会存在查询复杂，需要多表JOIN，连接多时可能影响性能的问题。
8. BCNF和3NF的区别：
   * BCNF
     > 具有函数依赖集 F 的关系模式 R 属于 BCNF 的条件是，
     > 对 F⁺ 中所有形如 α → β 的函数依赖，下面至少一个成立： 
     > 
     > - α → β 是平凡的函数依赖
     > - α 是 R 的一个超码
   * 3NF
     > 具有函数依赖集 F 的关系模式 R 属于 3NF 的条件是，
     > 对 F⁺ 中所有形如 α → β 的函数依赖，下面至少一个成立： 
     > 
     > - α → β 是平凡的函数依赖
     > - α 是 R 的一个超码
     > - β - α 的每个属性都属于R的某个候选码。

   *BCNF 是比 3NF 更严格的范式，可能有函数依赖满足 3NF 但不满足 BCNF。

## 五、数据库存储、索引、查询与事务(week14-week16)
#### 学习内容：
学习如何实现数据库的底层逻辑，包括数据存储、索引机制、查询处理与优化和事务管理。

#### 重点笔记：
1. DBMS 如何在磁盘中存储数据？
   * DBMS 通过将表中的数据组织为 Page 结构存储到磁盘文件中。
   * DBMS 可以按照不同方式组织 Pages：堆文件组织，树文件组织，顺序文件组织，哈希文件组织。
   * PostgreSQL 使用行存储：将一个元组的所有属性在 Page 里连续存储。
2. DBMS 如何管理内存并与磁盘来回移动数据？
   * 数据库在运行时，先从磁盘将数据页读入内存，在内存中进行所有的读写操作，必要时将“脏页”（被修改的页）写回磁盘。
   * 为了避免因突然崩溃导致数据丢失，DBMS 采用 WAL（预写日志）机制：修改数据之前，先将操作记录写入日志文件。
4. 查询的基本步骤：
   * 解析（将 SQL 语句转换为内部表示） -> 优化（生成查询计划） -> 执行（执行查询计划）
5. 如何理解事务？
   * 事务是数据库管理系统中一组逻辑上的操作单元，要么全部成功执行，要么全部失败回滚，不可中间停止或只执行部分。
   * 事务的四大特性（ACID）：原子性、一致性、隔离性、持久性。
   * 原子性:事务不可再分割（要么全做，要么全不做）
   * 一致性:执行后数据库处于一致状态（比如转账后，账目总额保持不变）
   * 隔离性:并发事务互不干扰（不同用户操作不会互相“看见”中间结果）
   * 持久性:事务执行成功后效果永久保存（崩溃也不会丢失数据（依靠日志/WAL））
6. 树索引 VS 哈希索引：
   * 树索引是基于有序结构（通常是 B+ 树）实现的索引方式，用于支持范围查找、排序和范围扫描，操作的平均时间复杂度为对数级。
   * 哈希索引基于哈希表结构，将键值通过哈希函数映射到固定位置，支持非常快速的等值查询，适用于精确查找，不支持范围查找或排序。

# 自我评分
#### 总分：46/50
#### 分数分布:
1. 课堂学习（20）：18
2. 作业情况（10）：10
3. 课后学习（10）：8
4. 学习态度（10）：10
   
