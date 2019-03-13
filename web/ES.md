ES 的变量是弱类型，声明不是必需的（最好还是全部声明变量）。

ES关键字(不能用作变量名或函数)：
break
case
catch
continue
default
delete
do
else
finally
for
if
in
instanceof
new
return
switch
this
throw
try
typeof
var
void
while
with


 ECMAScript 的原始数据类型即 Undefined、Null、Boolean、Number和String型

ECMAScript提供了typeof运算符来判断一个值是否在某种类型的范围内。

Undefined类型具有唯一的值，即undefined。当声明的变量未初始化时，该变量的默认值是undefined。当函数无明确返回值时，返回的也是值"undefined"。

ndefined实际上是从值null派生来的，因此ECMAScript把它们定义为相等的。

    alert(null == undefined);  //输出 "true"

N是个奇怪的特殊值。一般说来，这种情况发生在类型（String、Boolean 等）转换失败时。

	alert(NaN == NaN);  //输出 "false"
	alert(isNaN("blue"));  //输出 "true"
	alert(isNaN("666"));  //输出 "false"

ECMAScript 的第三版为 Function 对象加入了两个方法，即 call() 和 apply().
	
	sayColor.call(obj, "The color is ", "a very nice color indeed.");
	sayColor.apply(obj, new Array("The color is ", "a very nice color indeed."));





