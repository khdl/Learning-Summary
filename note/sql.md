### sql

可变长度字符串大字段：clob  存储二进制文件：blob </br>
oracle 存储整数类型：  number(3,3) </br>
外键创建:  foreign(字段名)  references 表名(字段名) </br>
表中添加列: alter table 表名 add  字段名  字段类型 </br>
表中删除列: alter table 表名 drop  字段名 </br>
删除表： drop table 表名 （如果有外键关联到删除的表，须先删除外键关联）</br>
