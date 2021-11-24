### Mysql优化

#### 1. mysql中的数据库存储引擎-InnoDB

是mysql的直接产品，是mysql5.5以上的默认引擎。支持事物外建。效率也比较高

#### 2. mysql中的数据库存储引擎-MyISAM

Myisam是mysql的isam扩展格式数据库引擎，读取速度非常快。但是不支持事物，不支持外建。但是增删改慢。

#### 3. 如何选用呢？

如果读取比较多、不需要事物可以考虑myisam，否则用innoDB。

#### **4.**   查询数据库操作的基本命令

查询数据库存储引擎

show engines

查看当前数据库存储引擎是哪个

show variables like ‘%engine%’

查看数据库表的存储引擎

Show create table ‘表名’

改变存储引擎

Alter table ‘表名’ engine=’innodb’

#### **5.**  数据库中的索引

为什么要使用索引？

这是因为使用索引可以大大提高查询速度。

##### 5.1． 使用索引的优点

1. 创建唯一索引，可以保证数据库表中每一行数据的唯一性。

2. 可以大大加快数据的检索速度，这也是创建索引的主要原因。

3. 可以加速表和表之间的连接，特别是实现数据参考完整性方面特别有意义。

4. 在使用分组和排序子句进行数据检索时，同样可以显著减少查询中分组和排序的时间。

5. 通过使用索引，可以在查询过程中使用查询优化器，提高我们系统性能。

##### 5.2 使用索引的缺点

1. 创建索引和维护索引要浪费时间，这种时间随着数据量的增加而增加。

2. 索引占用物理空间，除了数据占数据空间外，每一个索引还需要占用物理空间如果建立聚簇索引，那么要的空间会更大。

3. 当对表中的数据进行新增、删除、修改的时候。索引也要动态维护。这样也就降低了维护速度。

##### 5.3 什么样的字段适合建立索引

1. 在经常需要搜索的字段上，添加索引（如：一张表中有20个字段，其中有5个字段需要经常查询，那么可以添加索引）

2. 在作为主键列上，强制该列的唯一性和组织表中的数据排列结构。

3. 在经常用的在连接的列上，这些列一定是外建，可以加快连接速度。

4. 在经常需要根据范围进行搜索的列上创建索引，因为索引已经排序，其指定的范围是连续的。

5. 在经常需要排序的列上添加索引，因为索引已经排序，这样查询可以利用索引排序，加快排序查询的时间。

6. 经常在where字句中的列上创建索引，加快查询条件判断速度。

7. 在建立索引时，如select ** from **where f1 and f2.这种情况下，单纯在f1上加索引是没有用的，得两个同时加才能生效。

#### 6. Mysql中的sql优化策略

##### 6.1  避免全表扫描

对查询进行优化，应尽量避免全表扫描，首先考虑where 及 orderby 涉及列上的索引。

##### 6.2. 避免判断null值

尽量避免在where字句中进行null判断，否则将导致存储引擎放弃使用索引而进行全表扫描，如：

Select id from t_user where num is null.

修改：可以给num字段设置默认值为0

select id from t_user where num=0

##### 6.3 避免不等值判断

尽量避免在where字句中出现！=或者<>操作符，否则会索引失效进行全表扫描。

##### 6.4  避免使用or逻辑

尽量避免在where子句中使用or来连接条件，否子会索引失效进行全表扫描。如：

Select id from t_user where num=1 or num=2

可以修改为union all 来连接

Select id from t_user where num=1

Union all

Select id from t_user where num=2



##### 6.5. 慎用in 和not in 

逻辑  in和not in 也要慎用，否则可能会导致全表扫描，如：

Select id from t_user where num in(select id from t_dept where id >10)

此时外层查询会全表扫描，不使用索引，可以修改为：

Select id from t1 t1,(select id from t_user where id>10) t2 where t1.id = t2.id

##### **6.6** 注意模糊查询

模糊查询尽量使用like ‘条件%’，不要使用like ‘%条件%’，如必须是查询条件，可    以考虑使用solr等搜索引擎。

##### 6.7 查询条件字段中不要使用计算

如：select * from t_user where num/2 = 100;

修改为：select * from t_user where num = 100x2

##### 6.8 使用exists关键字

很多时候利用exists代替in是一种很好的选择

如：select number from t_user where number in(select number from t_dept)

修改为：select number from t_user where exists(select 1 from t_dept where number = t_user.number)

