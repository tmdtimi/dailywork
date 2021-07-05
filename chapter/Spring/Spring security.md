##Spring security

Spring security + JWT +缓存

解决一个认证、授权问题。 

###登录认证

 	Authencation


Spring Security的Web基础是Filters,通过一层层的Filters对web请求做处理

一个web请求会经过一条过滤器链，在经过过滤器链的过程中完成认证和授权，如果中间发现这条请求未认证或者未授权，会根据被保护API的权限抛出异常，然后由异常处理器去处理这些异常。

![](../pics/s3.png)

![](../pics/s4.png)

在WebSecurityConfigurerAdapter子类SpringSecurityConfig中配置configure方法配置过滤链

formlogin:对应form表单认证方式，即UsernamePasswordAuthenticationFilter

httpBasic 对应basic认证方式，即BasicAuthenticationFilter

配置了这两种认证方式，过滤器链中才会加入它们，否则它们是不会被加到过滤器链中去的。


Spring Security自带的过滤器中是没有针对JWT这种认证方式的，所以我们的demo中会写一个JWT的认证过滤器，然后放在绿色的位置进行认证工作。


![](../pics/s5.png)



**几个关键类**

SecurityContext:上下文对象，Authentication对象会放在里面。

SecurityContextHolder:用于拿到上下文对象的静态工具类

Authentication:认证接口，定义了认证对象的数据形式

AuthenticationManager:用于校验Authentication，返回一个认证完成后的Authentication对象。


Web系统中登录认证（Authentication）的核心就是凭证机制，无论是Session还是JWT，都是在用户成功登录时返回给用户一个凭证，后续用户访问接口需携带凭证来标明自己的身份。后端会对需要进行认证的接口进行安全判断，若凭证没问题则代表已登录就放行接口，若凭证有问题则直接拒绝请求。这个安全判断都是放在过滤器里统一处理的


登录认证是对用户的身份进行确认，权限授权（Authorization）是对用户能否访问某个资源进行确认，授权发生都认证之后。

LoginFilter先进行登录认证判断，认证通过后再由AuthFilter进行权限授权判断，一层一层没问题后才会执行我们真正的业务逻辑。

Spring Security默认会启用很多过滤器：


UsernamePasswordAuthenticationFilter负责登录认证，FilterSecurityInterceptor负责权限授权。


在实际开发中，这些默认配置好的功能往往不符合我们的实际需求，所以我们一般会自定义一些配置。


	@EnableWebSecurity
	public class SecurityConfig extends WebSecurityConfigurerAdapter {
	}

在该类中重写WebSecurityConfigurerAdapter的方法就能对Spring Security进行自定义配置

通过 SecurityContext 来获取Authentication

SecurityContext就是我们的上下文对象

上下文对象则是交由 SecurityContextHolder 进行管理，你可以在程序任何地方使用它

	Authentication authentication = SecurityContextHolder.getContext().getAuthentication();

可以看到调用链路是这样的：SecurityContextHolder SecurityContext Authentication。

SecurityContextHolder使用ThreadLocal来保证一个线程中传递同一个对象

Authentication：存储了认证信息，代表当前登录用户

SeucirtyContext：上下文对象，用来获取Authentication

SecurityContextHolder：上下文管理对象，用来在程序任何地方获取SecurityContext

![](../pics/s6.png)

Authentication中那三个玩意就是认证信息：

Principal：用户信息，没有认证时一般是用户名，认证后一般是用户对象

Credentials：用户凭证，一般是密码

Authorities：用户权限



查询用户数据 判断账号密码是否正确 正确则将用户信息存储到上下文中 上下文中有了这个对象则代表该用户登录了

// 账号密码正确了才将认证信息放到上下文中,用户权限需要再从数据库中获取

	Authentication authentication = new UsernamePasswordAuthenticationToken(用户名, 用户密码, 用户的权限集合);
	SecurityContextHolder.getContext().setAuthentication(authentication);








