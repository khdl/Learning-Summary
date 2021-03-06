### sql

### 基础

可变长度字符串大字段：clob ， 存储二进制文件：blob </br>
oracle 存储整数类型：  number(3,3) </br>
外键创建:  foreign(字段名)  references 表名(字段名) (插入数据时，如果关联的主键数据不存在会报错)</br>
表中添加列: alter table 表名 add  字段名  字段类型 </br>
表中删除列: alter table 表名 drop  字段名 </br>
删除表： drop table 表名 （如果有外键关联到删除的表，须先删除外键关联）</br>
插入数据： insert into 表名 values</br>
更新数据：update 表名 set (用“,”分开修改多个列)</br>

### 数据检索基础

as： 定义别名，可以省略，支持中文列名才能定义中文别名</br>
count(字段名)： 统计的是字段不为空的总记录数</br>
ASC是默认的排序方式可以省略：order by id</br>
多个排序规则： order by age desc,salary desc</br>
通配符： 

			1. 单字符：_   (like '_ad':表示第一个字符任意，第二三字符为ad的字符）  
			2. 多字符：%   
			3. 单字符和多字符可以一起使用  
			//使用通配符的时候会对全表进行扫描，能避免使用就避免

对空值的检测使用 is null(is not null) 来处理，字符用 =null 来查询的时候可能查不出数据来。 </br>
sql提供的表示不等于： <>  </br>
对表达式取反：not(表达式) (!取反在不少数据库不支持)</br>
范围查询 ： in  、 between  and，数据库对between and做了优化，性能好，是闭区间查询</br>

where 1=1: 有这个过滤条件后数据库就无法使用索引等查询优化策略，数据库将会被迫进行全表扫描，数据量很大的时候执行速度就会很慢。</br>

分组语句(group by)经常和聚合函数一起使用。没有出现在group by 后的列是不能出现在select后的(聚合函数除外)。group by 后跟多个字段指定多个分组规则。查询的结果是以最末一级的分组来进行输出的</br>
having 对分组进行过滤，having语句中不能包含未分组的列名

oracle定义表别名的时候不能使用as。</br>
sqlserver和oracel 支持窗口函数：ROW_NUMBER(): 用法ROW_NUMBER() OVER(规则)</br>
oracle虽然支持ROW_NUMBER()，但提供了更方便的特性来计算行号。Oracle为每个结果集都增加了一个默认表示行号的列，这个列的名称为rownum。**注意rownum是结果集的行号**</br>

distinct 是对整个结果集进行数据重复抑制的</br>
查询中设置常量字段</br>

oracle：获取字段长度函数LENGTH(),截取字符串的长度函数SUBSTR()</br>
函数可以嵌套使用，不同数据库提供的函数差异非常大</br>

所有的数据库都支持字符串用单引号包围，有些数据库的双引号有其它含义(比如列的别名)</br>

mysql使用CONCAT()函数来拼接字符串，它的 **+** 号会转换成整形(转换失败默认为0)进行计算，sqlserver使用 **+** 号，oracle支持使用 **||** 和CONTAC()函数（oracel中最多只支持两个参数，但可以函数嵌套）进行字符串的拼接。

Update T_User set **age = age+1**</br>

mysql和SqlServer允许使用不带from字句的select语句来查询不属于任何实体表的数据：select 1；还可以在不带from字句中使用函数</br>
oracel不允许使用这种不带任何from字句的select语句，不过可以使用oracel的系统表来作为from字句中的表名，最常用的系统表为DUAl：select 1,LENGTH('abc')  from dual;</br>

UNION去重了，UNION ALL 不去重