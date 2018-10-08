# Spring  事物

### 事物的配置方式

Spring配置文件中关于事务配置总是由三个组成部分，分别是DataSource、TransactionManager和代理机制这三部分，无论哪种配置方式，一般变化的只是代理机制这部分

 DataSource、TransactionManager这两部分只是会根据数据访问方式有所变化，比如使用hibernate进行数据访问时，DataSource实际为SessionFactory，TransactionManager的实现为HibernateTransactionManager


### 每个Bean都有一个代理

	 <?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
	    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	    xmlns:context="http://www.springframework.org/schema/context"
	    xmlns:aop="http://www.springframework.org/schema/aop"
	    xsi:schemaLocation="http://www.springframework.org/schema/beans
	           http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
	           http://www.springframework.org/schema/context
	           http://www.springframework.org/schema/context/spring-context-2.5.xsd
	           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.5.xsd">
	
	    <bean id="sessionFactory" 
	            class="org.springframework.orm.hibernate3.LocalSessionFactoryBean"> 
	        <property name="configLocation" value="classpath:hibernate.cfg.xml" /> 
	        <property name="configurationClass" value="org.hibernate.cfg.AnnotationConfiguration" />
	    </bean> 
	
	    <!-- 定义事务管理器（声明式的事务） --> 
	    <bean id="transactionManager"
	        class="org.springframework.orm.hibernate3.HibernateTransactionManager">
	        <property name="sessionFactory" ref="sessionFactory" />
	    </bean>
	   
	    <!-- 配置DAO -->
	    <bean id="userDaoTarget" class="com.bluesky.spring.dao.UserDaoImpl">
	        <property name="sessionFactory" ref="sessionFactory" />
	    </bean>
	   
	    <bean id="userDao" 
	        class="org.springframework.transaction.interceptor.TransactionProxyFactoryBean"> 
	           <!-- 配置事务管理器 --> 
	           <property name="transactionManager" ref="transactionManager" />    
	        <property name="target" ref="userDaoTarget" /> 
	         <property name="proxyInterfaces" value="com.bluesky.spring.dao.GeneratorDao" />
	        <!-- 配置事务属性 --> 
	        <property name="transactionAttributes"> 
	            <props> 
	                <prop key="*">PROPAGATION_REQUIRED</prop>
	            </props> 
	        </property> 
	    </bean> 
	</beans>



### 所有Bean共享一个代理基类


	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
	    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	    xmlns:context="http://www.springframework.org/schema/context"
	    xmlns:aop="http://www.springframework.org/schema/aop"
	    xsi:schemaLocation="http://www.springframework.org/schema/beans
	           http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
	           http://www.springframework.org/schema/context
	           http://www.springframework.org/schema/context/spring-context-2.5.xsd
	           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.5.xsd">
	
	    <bean id="sessionFactory" 
	            class="org.springframework.orm.hibernate3.LocalSessionFactoryBean"> 
	        <property name="configLocation" value="classpath:hibernate.cfg.xml" /> 
	        <property name="configurationClass" value="org.hibernate.cfg.AnnotationConfiguration" />
	    </bean> 
	
	    <!-- 定义事务管理器（声明式的事务） --> 
	    <bean id="transactionManager"
	        class="org.springframework.orm.hibernate3.HibernateTransactionManager">
	        <property name="sessionFactory" ref="sessionFactory" />
	    </bean>
	   
	    <bean id="transactionBase" 
	            class="org.springframework.transaction.interceptor.TransactionProxyFactoryBean" 
	            lazy-init="true" abstract="true"> 
	        <!-- 配置事务管理器 --> 
	        <property name="transactionManager" ref="transactionManager" /> 
	        <!-- 配置事务属性 --> 
	        <property name="transactionAttributes"> 
	            <props> 
	                <prop key="*">PROPAGATION_REQUIRED</prop> 
	            </props> 
	        </property> 
	    </bean>   
	  
	    <!-- 配置DAO -->
	    <bean id="userDaoTarget" class="com.bluesky.spring.dao.UserDaoImpl">
	        <property name="sessionFactory" ref="sessionFactory" />
	    </bean>
	   
	    <bean id="userDao" parent="transactionBase" > 
	        <property name="target" ref="userDaoTarget" />  
	    </bean>
	</beans>


### 使用拦截器


	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
	    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	    xmlns:context="http://www.springframework.org/schema/context"
	    xmlns:aop="http://www.springframework.org/schema/aop"
	    xsi:schemaLocation="http://www.springframework.org/schema/beans
	           http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
	           http://www.springframework.org/schema/context
	           http://www.springframework.org/schema/context/spring-context-2.5.xsd
	           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.5.xsd">
	
	    <bean id="sessionFactory" 
	            class="org.springframework.orm.hibernate3.LocalSessionFactoryBean"> 
	        <property name="configLocation" value="classpath:hibernate.cfg.xml" /> 
	        <property name="configurationClass" value="org.hibernate.cfg.AnnotationConfiguration" />
	    </bean> 
	
	    <!-- 定义事务管理器（声明式的事务） --> 
	    <bean id="transactionManager"
	        class="org.springframework.orm.hibernate3.HibernateTransactionManager">
	        <property name="sessionFactory" ref="sessionFactory" />
	    </bean> 
	  
	    <bean id="transactionInterceptor" 
	        class="org.springframework.transaction.interceptor.TransactionInterceptor"> 
	        <property name="transactionManager" ref="transactionManager" /> 
	        <!-- 配置事务属性 --> 
	        <property name="transactionAttributes"> 
	            <props> 
	                <prop key="*">PROPAGATION_REQUIRED</prop> 
	            </props> 
	        </property> 
	    </bean>
	     
	    <bean class="org.springframework.aop.framework.autoproxy.BeanNameAutoProxyCreator"> 
	        <property name="beanNames"> 
	            <list> 
	                <value>*Dao</value>
	            </list> 
	        </property> 
	        <property name="interceptorNames"> 
	            <list> 
	                <value>transactionInterceptor</value> 
	            </list> 
	        </property> 
	    </bean> 
	 
	    <!-- 配置DAO -->
	   <bean id="userDao" class="com.bluesky.spring.dao.UserDaoImpl"> 
            <property name="sessionFactory" ref="sessionFactory" />    
       </bean>
       </beans>


### 使用tx标签配置的拦截器

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
	    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	    xmlns:context="http://www.springframework.org/schema/context"
	    xmlns:aop="http://www.springframework.org/schema/aop"
	    xmlns:tx="http://www.springframework.org/schema/tx"
	    xsi:schemaLocation="http://www.springframework.org/schema/beans
	           http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
	           http://www.springframework.org/schema/context
	           http://www.springframework.org/schema/context/spring-context-2.5.xsd
	           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.5.xsd
	           http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-2.5.xsd">
	
	    <context:annotation-config />
	    <context:component-scan base-package="com.bluesky" />
	
	    <bean id="sessionFactory" 
	            class="org.springframework.orm.hibernate3.LocalSessionFactoryBean"> 
	        <property name="configLocation" value="classpath:hibernate.cfg.xml" /> 
	        <property name="configurationClass" value="org.hibernate.cfg.AnnotationConfiguration" />
	    </bean> 
	
	    <!-- 定义事务管理器（声明式的事务） --> 
	    <bean id="transactionManager"
	        class="org.springframework.orm.hibernate3.HibernateTransactionManager">
	        <property name="sessionFactory" ref="sessionFactory" />
	    </bean>
	
	    <tx:advice id="txAdvice" transaction-manager="transactionManager">
	        <tx:attributes>
	            <tx:method name="*" propagation="REQUIRED" />
	        </tx:attributes>
	    </tx:advice>
	   
	    <aop:config>
	        <aop:pointcut id="interceptorPointCuts"
	            expression="execution(* com.bluesky.spring.dao.*.*(..))" />
	        <aop:advisor advice-ref="txAdvice"
	            pointcut-ref="interceptorPointCuts" />       
	    </aop:config>     
	</beans>

### 全注解


	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
	    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	    xmlns:context="http://www.springframework.org/schema/context"
	    xmlns:aop="http://www.springframework.org/schema/aop"
	    xmlns:tx="http://www.springframework.org/schema/tx"
	    xsi:schemaLocation="http://www.springframework.org/schema/beans
	           http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
	           http://www.springframework.org/schema/context
	           http://www.springframework.org/schema/context/spring-context-2.5.xsd
	           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.5.xsd
	           http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-2.5.xsd">
	
	    <context:annotation-config />
	    <context:component-scan base-package="com.bluesky" />
	
	    <tx:annotation-driven transaction-manager="transactionManager"/>
	
	    <bean id="sessionFactory" 
	            class="org.springframework.orm.hibernate3.LocalSessionFactoryBean"> 
	        <property name="configLocation" value="classpath:hibernate.cfg.xml" /> 
	        <property name="configurationClass" value="org.hibernate.cfg.AnnotationConfiguration" />
	    </bean> 
	
	    <!-- 定义事务管理器（声明式的事务） --> 
	    <bean id="transactionManager"
	        class="org.springframework.orm.hibernate3.HibernateTransactionManager">
	        <property name="sessionFactory" ref="sessionFactory" />
	    </bean>
	   
	</beans>



## 事务隔离等级

1、Serializable：最严格的级别，事务串行执行，资源消耗最大；

2、REPEATABLE READ：保证了一个事务不会修改已经由另一个事务读取但未提交（回滚）的数据。避免了“脏读取”和“不可重复读取”的情况，但是带来了更多的性能损失。

3、READ COMMITTED:大多数主流数据库的默认事务等级，保证了一个事务不会读到另一个并行事务已修改但未提交的数据，避免了“脏读取”。该级别适用于大多数系统。

4、Read Uncommitted：保证了读取过程中不会读取到非法数据。隔离级别在于处理多事务的并发问题。

### 读数据问题

1： Dirty reads--读脏数据。也就是说，比如事务A的未提交（还依然缓存）的数据被事务B读走，如果事务A失败回滚，会导致事务B所读取的的数据是错误的。

2： non-repeatable reads--数据不可重复读。比如事务A中两处读取数据-total-的值。在第一读的时候，total是100，然后事务B就把total的数据改成 200，事务A再读一次，结果就发现，total竟然就变成200了，造成事务A数据混乱。

3： phantom reads--幻象读数据，这个和non-repeatable reads相似，也是同一个事务中多次读不一致的问题。但是non-repeatable reads的不一致是因为他所要取的数据集被改变了（比如total的数据），但是phantom reads所要读的数据的不一致却不是他所要读的数据集改变，而是他的条件数据集改变。比如Select account.id where account.name="ppgogo*",第一次读去了6个符合条件的id，第二次读取的时候，由于事务b把一个帐号的名字由"dd"改成"ppgogo1"，结果取出来了7个数据。

	
		             Dirty reads	non-repeatable reads	phantom reads
	Serializable	    不会	              不会	               不会
	REPEATABLE READ	    不会	              不会	               会
	READ COMMITTED	    不会	              会	                    会
	Read Uncommitted	会	               会	                 会





   数据库提供了四种事务隔离级别, 不同的隔离级别采用不同的锁类开来实现. 

　　在四种隔离级别中, Serializable的级别最高, Read Uncommited级别最低. 

　　大多数数据库的默认隔离级别为: Read Commited,如Sql Server , Oracle. 

　　少数数据库默认的隔离级别为Repeatable Read, 如MySQL InnoDB存储引擎 

　　即使是最低的级别,也不会出现 第一类 丢失 更新问题 . 



 1. 脏读(事务没提交，提前读取) ：脏读就是指当一个事务正在访问数据，并且对数据进行了修改，而这种修改还没有提交到数据库中，这时，另外一个事务也访问这个数据，然后使用了这  个数据。 
 2. 不可重复读(两次读的不一致) ：是指在一个事务内，多次读同一数据。在这个事务还没有结束时，另外一个事务也访问该同一数据。那么，在第一个事务中的两次读数据之间，由于第二个事务的修改，那么第一个事务两次读到的的数据可能是不一样的。这样就发生了在一个事务内两次读到的数据是不一样的，因此称为是不可重复读
 3. 幻读 : 是指当事务不是独立执行时发生的一种现象，例如第一个事务对一个表中的数据进行了修改，这种修改涉及到表中的全部数据行。同时，第二个事务也修改这个表中的数据，这种修改是向表中插入一行新数据。那么，以后就会发生操作第一个事务的用户发现表中还有没有修改的数据行，就好象发生了幻觉一样。

### 补充 : 基于元数据的 Spring 声明性事务 : 
Isolation 属性一共支持五种事务设置，具体介绍如下： 

DEFAULT 使用数据库设置的隔离级别 ( 默认 ) ，由 DBA 默认的设置来决定隔离级别 . 

READ_UNCOMMITTED 会出现脏读、不可重复读、幻读 ( 隔离级别最低，并发性能高 ) 

READ_COMMITTED  会出现不可重复读、幻读问题（锁定正在读取的行） 

REPEATABLE_READ 会出幻读（锁定所读取的所有行） 

SERIALIZABLE 保证所有的情况不会发生（锁表） 

