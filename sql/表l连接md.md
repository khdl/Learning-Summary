### 表连接

1. inner join on：默认的数据库连接方式(大多数数据库中)，条件可以是不等值的连接
2. cross join：不存在on语句，将涉及到的所有表的所有记录包含在结果集中
3. 自连接：就是一张表中自己连接自己，取不同的别名
4. 外部连接：主要用来解决空值匹配的问题，分为 left join，right join， full outer join(mysql不z支持，union来模拟实现)