Ext JS是一个JavaScript框架，它具有面向对象编程的功能。</br>
Ext是封装Ext JS中所有类的命名空间。

Ext.define（）用于在Ext JS中定义类。

    Ext.define(class name, class members/properties, callback function);

### Ext.js 集装箱

Ext JS中的容器是我们可以添加其他容器或子组件的组件。Ext.container.Container是Ext JS中所有容器的基类。

有各种类型的容器Ext.panel.Panel，Ext.form.Panel，Ext.tab.Panel和Ext.container.Viewport是Ext JS中经常使用的容器

### Ext.js 布局

1. 绝对：此布局允许使用容器中的XY坐标定位项目。 layout: 'absolute' 
2. 手风琴：这种布局允许将所有项目以堆叠方式（一个在另一个之上）放在容器内。  layout: 'accordion' 
3. 锚点：此布局给予用户给出每个元素相对于容器大小的大小的特权。 layout: 'anchor' 
4. 边框：在此布局中，各种面板嵌套并由边框分隔。  layout: 'border' 
5. 自动：这是默认布局，根据元素数量决定元素的布局。  layout: 'auto' 
6. card TabPanel：此布局允许使用容器中的XY坐标定位项目。   layout: 'layout-cardtabs' 
7. 列：此布局用于在容器中显示多个列。 我们可以定义列的固定宽度或百分比宽度。 百分比宽度将基于容器的完整大小计算。  layout: 'column' 
8. 适合：在此布局中，容器用单个面板填充，当没有与布局相关的特定要求时，则使用此布局。 layout: 'fit' 
9. 表：由于名称意味着此布局以HTML表格式在容器中排列组件。  layout: 'table' 
10. vbox：此布局允许元素以垂直方式分布。 这是最常用的布局之一。  layout: 'vbox' 
11. hbox：这种布局允许元素以水平方式分布。  layout: 'hbox' 


### Ext.js 组件

网格：这个组件是显示数据的简单组件，它是以表格格式存储在Ext.data.Store中的记录的集合。

	 Ext.create('Ext.grid.Panel',{
	 grid properties..
	 columns : [Columns]
	 });

表单：在大多数Web应用程序表单是最重要的小部件从用户获取信息，如登录表单/反馈表单，以便该值可以保存在数据库中

	{ 	
	   xtype: 'textfield',
	   fieldLabel: 'Text field'  
	} 
	{
	   xtype: 'datefield',
	   fieldLabel: 'Date picker'
	}
	{
	   xtype: 'button', 
	   text : 'Button'
	}

Ext.js 饼图
	
	 Ext.create('Ext.chart.PolarChart', {
	 series: [{
	   Type: 'pie'
	   ..
	   }]
	   render and other properties.
	 });

Ext.js 折线图

	Ext.create('Ext.chart.CartesianChart',{
	   series: [{
	      type: 'line',
	      xField: 'name',
	      yField: 'G1'
	      }]
	      render, legend and other properties
	});

Ext.js 条形图

	Ext.create('Ext.chart.CartesianChart',{
	   series: [{
	      type: 'bar',
	      xField: 'name',
	      yField: 'G1'
	      }]
	      render, legend and other properties
	});