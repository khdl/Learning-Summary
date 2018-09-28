# Ognl 简介
  OGNL是Object-Graph Navigation Language的缩写，它是一种功能强大的表达式语言，通过它简单一致的表达式语法，可以存取对象的任意属性，调用对象的方法，遍历整个对象的结构图，实现字段类型转化等功能。它使用相同的表达式去存取对象的属性

  OGNL的API看起来就是两个简单的静态方法：

	  public static Object getValue( Object tree, Map context, Object root ) throws OgnlException;
	  public static void setValue( Object tree, Map context, Object root, Object value ) throws OgnlException

### Ognl 基本介绍

  - OGNL可以让我们用非常简单的表达式访问对象层，例如，当前环境的根对象为user1，则表达式person.address[0].province可以访问到user1的person属性的第一个address的province属性。
  - webwork2和现在的Struts2.x中使用OGNL取代原来的EL来做界面数据绑定，所谓界面数据绑定，也就是把界面元素（例如一个textfield,hidden)和对象层某个类的某个属性绑定在一起，修改和显示自动同步。
  - 和struts1.x的formbean相比，这样做的好处非常明显：在webwork中不需要为每个页面专门写formbean，可以直接利用对象层的对象。例如在对象设计中，我们的User和Person是分开的，而一个注册用户界面需要填写两者的内容，在webwork中，就可以保持后台的对象结构，把属于用户属性的界面元素用user.person.xxx绑定，把属于账号属性的界面元素用user.xxx绑定。
###Ognl 重要的3个符号：#、%、$

 "#"主要有三种用途：

  - 访问非根对象属性，例如#session.msg表达式，由于Struts 2中值栈被视为根对象，所以访问其他非根对象时，需要加#前缀。实际上，#相当于ActionContext. getContext()；#session.msg表达式相当于ActionContext.getContext().getSession(). getAttribute("msg") 。
  - 用于过滤和投影（projecting）集合
  - 用来构造Map

"%"符号的用途是在标志的属性为字符串类型时，计算OGNL表达式的值，这个类似js中的eval 

"$"符号主要有两个方面的用途。

  - 在国际化资源文件中，引用OGNL表达式，例如国际化资源文件中的代码：reg.agerange=国际化资源信息：年龄必须在${min}同${max}之间
  - 在Struts 2框架的配置文件中引用OGNL表达式

###Ognl 与 Struts2 

  - OGNL表达式的计算是围绕OGNL上下文进行的。
   OGNL上下文实际上就是一个Map对象，由ognl.OgnlContext类表示。它里面可以存放很多个JavaBean对象。它有一个上下文根对象。上下文中的根对象可以直接使用名来访问或直接使用它的属性名访问它的属性值。否则要加前缀“#key”。
  - Struts2的标签库都是使用OGNL表达式来访问ActionContext中的对象数据的。如：<s:propertyvalue="xxx"/>。
  - 值栈(ValueStack) ：
   可以在值栈中放入、删除、查询对象。访问值栈中的对象不用“#”。
   Struts2总是把当前Action实例放置在栈顶。所以在OGNL中引用Action中的属性也可以省略“#”。
  - 调用ActionContext的put(key,value)放入的数据，需要使用#访问。

### MyBatis中的OGNL

  MyBatis中可以使用OGNL的地方有两处：

  1.     动态SQL表达式中
  2.    ${param}参数中

MyBatis常用OGNL表达式：

	e1 or e2
	e1 and e2
	e1 == e2,e1 eq e2
	e1 != e2,e1 neq e2
	e1 lt e2：小于
	e1 lte e2：小于等于，其他gt（大于）,gte（大于等于）
	e1 in e2
	e1 not in e2
	e1 + e2,e1 * e2,e1/e2,e1 - e2,e1%e2
	!e,not e：非，求反
	e.method(args)调用对象方法
	e.property对象属性值
	e1[ e2 ]按索引取值，List,数组和Map
	@class@method(args)调用类的静态方法
	@class@field调用类的静态字段值

 MyBatis中的OGNL例子:

1.动态SQL表达式中例子 mysql中的like查询,test的值会使用OGNL计算结果。

	<select id="xxx" ...>
	    select id,name,... from country
	    <where>
	        <if test="name != null and name != ''">
	            name like concat('%', #{name}, '%')
	        </if>
	    </where>
	</select>

2.通用 like 查询,这里<bind>的value值会使用OGNL计算。

	<select id="xxx" ...>
	    select id,name,... from country
	    <bind name="nameLike" value="'%' + name + '%'"/>
	    <where>
	        <if test="name != null and name != ''">
	            name like #{nameLike}
	        </if>
	    </where>
	</select>

3.${param}参数中的例子

	 <select id="xxx" ...>
	    select id,name,... from country
	    <where>
	        <if test="name != null and name != ''">
	            name like '${'%' + name + '%'}'
	        </if>
	    </where>
	</select>

这里注意写的是${'%' + name + '%'}，而不是%${name}%，这两种方式的结果一样，但是处理过程不一样。
在MyBatis中处理${}的时候，只是使用OGNL计算这个结果值，然后替换SQL中对应的${xxx}，OGNL处理的只是${这里的表达式}。
这里表达式可以是OGNL支持的所有表达式，可以写的很复杂，可以调用静态方法返回值，也可以调用静态的属性值。
