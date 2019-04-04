# Spring security 

一个基于Spring的企业应用系统提供声明式的安全访问控制解决方式的安全框架，应用的安全性包括用户认证（Authentication）和用户授权（Authorization）两个部分。

springSecurity在我们进行用户认证以及授予权限的时候，通过各种各样的拦截器来控制权限的访问，从而实现安全。


主要过滤器：

	WebAsyncManagerIntegrationFilter 
	SecurityContextPersistenceFilter 
	HeaderWriterFilter 
	CorsFilter 
	LogoutFilter
	RequestCacheAwareFilter
	SecurityContextHolderAwareRequestFilter
	AnonymousAuthenticationFilter
	SessionManagementFilter
	ExceptionTranslationFilter
	FilterSecurityInterceptor
	UsernamePasswordAuthenticationFilter
	BasicAuthenticationFilter


![](./img/b.PNG)

### 核心组件

-  SecurityContextHolder：提供对SecurityContext的访问
-  SecurityContext：持有Authentication对象和其他可能需要的信息
-  AuthenticationManager 其中可以包含多个AuthenticationProvider
-  ProviderManager对象为AuthenticationManager接口的实现类
-  AuthenticationProvider 主要用来进行认证操作的类 调用其中的authenticate()方法去进行认证操作
- Authentication：Spring Security方式的认证主体
- GrantedAuthority：对认证主题的应用层面的授权，含当前用户的权限信息，通常使用角色表示
- UserDetails：构建Authentication对象必须的信息，可以自定义，可能需要访问DB得到
- UserDetailsService：通过username构建UserDetails对象，通过loadUserByUsername根据userName获取UserDetail对象 （可以在这里基于自身业务进行自定义的实现  如通过数据库，xml,缓存获取等）        


### 用户登录的验证和授权过程

用户一次完整的登录验证和授权，是一个请求经过 层层拦截器从而实现权限控制，整个web端配置为DelegatingFilterProxy（springSecurity的委托过滤其代理类 ）

DelegatingFilterProxy：它并不实现真正的过滤，而是所有过滤器链的代理类，真正执行拦截处理的是由spring 容器管理的个个filter bean组成的filterChain.

调用实际的FilterChainProxy 的doFilterInternal()方法 去获取所有的拦截器并进行过滤处理



