1. DELETE 后 TRUNCATE TABLE ;

   ​	然后重新分析一下

   ```sql
   	analyze table tablename compute statistics
   ```

查看表信息

```sql
select NUM_ROWS,BLOCKS,AVG_SPACE,AVG_ROW_LEN from user_tables where table_name=table_name;
```

2.Oracle 表删除大量数据后，即使表中只有几行记录，但用select count(*) from table 来查询发觉都不会马上出来，

原因是该表的空间大了，查询起来很慢。

解决的方法是把该表所占用的表空间缩小，或者说释放表空间。

alter table XXXX move; 这样处理后就释放了表空间了。

但是释放表空间后，表的行号rowid会发生变化，而基于rowid的索引则会变成无效。因此该操作后必须重建索引。

否则会 提示“ORA-01502: 索引'SMP.ITEMLOG_MID_IDX'或这类索引的分区处于不可用状态”

而重建索引的方法当然可以先drop掉再create ，但是这样太麻烦了，

用alter index XXX rebuild 这样最快了，不会改变原来的索引结构。

 

Oracle分析表简介

Oracle分析表是Oracle数据库管理的重要部分，下面就为您详细介绍Oracle分析表方面的知识，希望对您学习Oracle分析表方面能有所帮助。

 1、分析SQL

 

```sql
  analyze table tablename compute statistics
  //等同于 
  analyze table tablename compute statistics for table for all indexes for all columns
```

  for table的统计信息存在于视图：user_tables 、all_tables、dba_tables

  for all indexes的统计信息存在于视图： user_indexes 、all_indexes、dba_indexes

  for all columns的统计信息存在于试图：user_tab_columns、all_tab_columns、dba_tab_columns

  删除分析SQL:

```sql
analyze table tablename delete statistics 
```

会删除所有的statistics

  2、Oracle分析表的作用：为了使基于CBO的执行计划更加准确

  DBA_tables的数据有变化，可做对比。详见官方文档：

  Use the ANALYZE statement to collect non-optimizer statistics, for example, to:

  Collect or delete statistics about an index or index partition, table or table partition, index-organized

  table, cluster, or scalar object attribute.

  Validate the structure of an index or index partition, table or table partition, index-organized table,

  cluster, or object reference （REF）。

  Identify migrated and chained rows of a table or cluster.

  dbms_stats的作用主要是替代analyze的收集统计信息这一块的功能，且在这一方面做了相当大程度上的增强。

  以analyze table table_name compute statistics;这条为例，生成的统计信息会存在于user_tables这个视图，查看一下

```sql
 
select NUM_ROWS,BLOCKS,AVG_SPACE,AVG_ROW_LEN from user_tables where table_name=table_name;

```

 

观察一下NUM_ROWS,BLOCKS,AVG_SPACE,AVG_ROW_LEN几列你就会明白，这就是变化。