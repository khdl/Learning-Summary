###  语法差异

语法差异主要体现：数据类型的差异、运算符的差异、函数的差异、常用sql的差异、取元数据信息的差异

oracel数据数值类型只有number。oracel使用  || 来拼接字符串。oracel为每个结果集提供了一个默认表示行号的列rownum。

oracle获得当前登录名：select user from  dual</br>
oracel获得所有表名： select  Object_Name from all_objects where Object_Type = 'table'</br>
orccel表all_objects中的owner字段代表Schema</br>
oracel表all_tab_columns表示系统中所有字段的定义，其中table_name为表名