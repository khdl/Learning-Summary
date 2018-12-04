### sql标准

1. 数值函数
 - 求绝对值： ABS()</br>
 - 求指数： POWER(待求幂表达式,幂)</br>
 - 求平方根： SORT()</br>
 - 四舍五入： ROUND(数值,保留精度默认为0)
 - 去尾(去掉小数)： FLOOR()
 - 正弦、余弦、正切、余切、求符号、求自然对数等</br></br>

2. 字符串函数
  - 字符串长度：LENGTH()
  - 转小写:LOWER()
  - 转大写：UPPER()
  - 截空格：TRIM(),截左侧LTRIM(),截右侧RTRIM()
  - 截字符串：SUBSTRING(),oracel 为SUBSTR()，字符下标从1开始
  - 返回字符串的位置
  - 字符串替换: REPLACE(字符串,替换的字符,替换成的字符)
  - 返回字符的ASCII码： ASCII(),得到字符传的ASCII码：CHAR()</br></br>

3. 日期函数</br>
 date、time、dateTime、TimeStamp

4. 其它函数
 - 空值处理：COALESCE(表达式,VALUE1,VALUE2,...)函数</br></br>
 
 

            // case when then else end用法,exp 可省略
			(case  exp
			when  val1 then ..
			when  val2 then ..
			...
			else  ...
			end) as ..


### MySql

1. 数值函数
 - 求随机数：RAND()
 - 进1：CEILING()
 - π ： PI()
 - 求余数： MOD(除数，被除数)</br></br>

2. 日期函数</br>
  - 获取当前时间：NOW()、SYSDATE() 等
  - 日期增减：DATE_ADD(date, INTERVAl exper type),负数为减去时间段
  - 计算日期差：DATEDIFF(date1,date2)返回日期相差天数
  - 计算日期星期几：DAYNAME()
  - 日期格式装换： DATE_FORMAT(date,格式)</br></br>

3. 其它函数
 - 类型转换：cast()、CONVERT()
 - 判断是否为空，空返回val:IFNULL(exp,val)</br></br>

4. 独有函数 
 - 条件判断：IF(exp,val1,val2)
 - 进制转换：CONV()
 - 填充函数： LPAD()、RPAD()
 - 重复若干次组成字符串：REPEAT()
 - 反转字符串：REVERSE()
 - UUID()
 - 集合操作
 - 辅助函数、加解密、摘要算法

### SqlServer

1. 数值函数
 - 求随机数：RAND()
 - 进1：CEILING()
 - π ： PI()</br></br>

2. 日期函数</br>
 - 获取当前时间：GETDATE()
 - 日期增减：DATEADD()
 - 计算日期差：DATEDIFF()
 - 计算日期星期几：DAYNAME()，也用于格式转换</br></br>

3. 其它函数
  - 类型转换：cast()、CONVERT()
  - 判断是否为空，空返回val:ISNULL(exp,val)

唯一字符串函数：NEWID()

### oracle

1. 数值函数
 - 求随机数：dbms_random.value(low,high)
 - 随机字符串参数： dbms_random.string(选项参数,字符串长度)
 - 进1：CEIL()
 - π ： acos(-1)
 - 求余数： MOD(除数，被除数)</br></br>

2. 日期函数</br> 
 - 获取当前时间：select SYSDATE from dual,可以用TO_CHAR()函数转换日期格
 - 日期增减:使用 + 号和 - 号来进行日期天数的增减，使用ADD_MONTHS(日期,月数)来进行月数的增减
 - 计算日期差：两个日期相减得到想差的天数(带小数的精确数值)
 - 计算日期星期几：TO_CHAR(date,'DAY')，也用于格式化日期</br></br>

3. 其它函数
 - 类型转换：TO_CHAR()、TO_DATE()、TO_NUMBER(),转换为字符串、日期、数值类型
 - 判断是否为空，空返回val:NVL(exp,val)</br></br>

4. 独有函数

 - 填充函数： LPAD()、RPAD()
 - 返回当月的最后一天：LAST_DAY(exp)
 - 集合操作
 - 辅助函数： select USER from dual等
