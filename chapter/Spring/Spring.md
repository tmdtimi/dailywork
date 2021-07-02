##Spring

Spring是一个轻量级的框架，是为了解决企业级开发的复杂性而创建的。

Spring是一个分层架构，由7个模块组成。

Spring的核心在于Spring 核心容器，可以在它的基础上整合其他模块，也可以整合第三方的框架。

###IOC

Spring通过核心容器来实现IOC  --依赖倒置原则

IOC控制反转 ：将类得实例交给Spring容器进行管理，从而将类和类之间的关系解耦。 实现的方法是通过依赖注入。

IOC: 对象的耦合，依赖程度降低 资源更加容易管理。

1 .通过java config(@Component) 注解、xml的方式<bean> 配置bean

2 .加载Spring容器： 

	XML:new ClassPathXmlApplicationContext(.xml) 
	
	javaConfig:new AnnotationConfigApplicationContext(.config)

3 .getBean("bean")


调用getBean("") ---> 是使用BeanFactory.get(bean)的

BeanFactory 核心接口，用于生产Bean  ---简单工厂设计模式


![](../pics/s1.png)


ClassPathXmlApplicationContext/AnnotationConfigApplicationContext  ApplicationContext --- BeanFactory


BeanFactory：生产Bean 

beanDefinition.setBeanClass(.class)  //设置原料  --

beanFactory.registerBeanDefinition(beanDefinition)  //注册到工厂中

beanFactory.getBean(.class) //可以生产了

BeanFactory  需要我们配置原料，registerBeanDefinition 注册入工厂才能生产。

BeanFactory是低级容器，只是需要我们先配置一些原料才能生产Bean.

BeanFactory 适用于资源受限的设备作内嵌的组件生产Bean


AnnotationConfigApplicationContext(@Component) ClassPathXmlApplicationContext（.xml）

采用不同的读取方式直接将配置中的Bean自动加载成BeanDefinition原料，然后交给BeanFactory

ApplicationContext自动配置了BeanDefinition生产原料 然后交给BeanFactory生产Bean


BeanDenifition参数： ObjectClass , Scope作用范围 ，  LazyInit是否懒加载

可以修改BeanDefinition参数来修改生产Bean的配置

首先将每个Bean加载成BeanDefinition 的Map中 注册到BeanFactory 

getBean通过BeanFactory生产Bean , 

1.通过Class.newInstance(ObjectClass)反射实例化这个Bean

2.实例化后进行属性注入 @AutoWired

3.初始化 

3.将Bean放入Map中。 Map<beanName,bean实例> ，这个Map成为一级缓存 或 单例池。



加载配置文件，解析成BeanDefinition 放入Map里。

当调用getBean的时候，从BeanDefinition所属的Map里，拿出Class对象进行实例化

同时如果由依赖关系，将递归调用getBean方法，完成依赖注入。



循环依赖问题：

	Class A{
		@Autowired
		 B b;
	}
	
	Class B{
		@Autowired
		 A a;
	}



在属性注入时会出现死循环。 - 使用三级缓存解决。


singletonObjects 一级缓存，用于保存实例化、注入、初始化完成的bean实例

earlySingletonObjects 二级缓存，用于保存实例化完成的bean实例

singletonFactories 三级缓存，用于保存bean创建工厂，以便于后面扩展有机会创建代理对象。

![](../pics/s2.png)


扩展接口：


BeanFactoryPostProcessor --修改BeanDefinition
					

BeanFactoryPostProcessor 子接口 BeanDefinitionRegisterPostProcessor - 提供BeanDefinition注册器


BeanPostProcessor :  反射实例化、属性注入、初始化调用9次。



###配置

注入方法： 构造器注入（指定参数）/ set方法注入（set参数，可以部分注入）

**bean作用域：**

singleton : 单例

当一个 bean 的作用域为 singleton，那么Spring IoC容器中只会存在一个共享的 bean 实例，并且所有对 bean 的请求，只要 id 与该 bean 定义相匹配，则只会返回bean的同一实例。

singleton 是单例类型(对应于单例模式)，就是在创建起容器时就同时自动创建了一个bean的对象，不管你是否使用，

但我们可以指定Bean节点的 lazy-init=”true” 来延迟初始化bean，这时候，只有在第一次获取bean时才会初始化bean，即第一次请求该bean时才初始化。 每次获取到的对象都是同一个对象。

@scope("singleton") 或 bean配置中<bean scope="singleton">

prototype——多例 每次请求都会创建一个新的 bean 实例

 prototype 作用域的 bean 会导致在每次对该 bean 请求（将其注入到另一个 bean 中，或者以程序的方式调用容器的 getBean() 方法）时都会创建一个新的 bean 实例


request / session / global session三种作用域仅在基于web的应用中使用

request:每一次HTTP请求都会产生一个新的bean，该bean仅在当前HTTP request内有效

request只适用于Web程序，每一次 HTTP 请求都会产生一个新的bean，同时该bean仅在当前HTTP request内有效，当请求结束后，该对象的生命周期即告结束。	


session——每一次HTTP请求都会产生一个新的 bean，该bean仅在当前 HTTP session 内有效

在global session 作用域中定义的 bean 被限定于全局portlet Session的生命周期范围内。


**Bean的生命周期**

单例对象： /出生：当容器创建时出生 /活着：只要容器还在，对象一直或者  /死亡：容器销毁，对象消亡  /单例对象的生命周期和容器相同

多例对象： /出生：当我们要使用对象时，Spring框架为我们创建  /活着：对象只是在使用过程就一直活着 
	
/死亡：当对象长时间不用，且没有对象引用时，由垃圾回收器回收



**相关注解**

@Autowired 是根据类型自动装配的  @Autowired + @Qualifier：根据byName的方式自动装配

@Resource 先byName，找不到再byType

@Component 衍生注解： @Controller @Service @Repository

@Configuration :代表是配置类  @Bean:通过方法注册一个bean,beanName为方法name，Bean为方法返回值

@RequestMapping @ResponseBody @PathVariable ...



**AOP**

@Aspect

@Before : 在目标业务方法执行之前执行

@After : 在目标业务方法执行之后执行

@AfterReturning : 在目标业务方法返回结果之后执行

@AfterThrowing : 在目标业务方法抛出异常之后

@Around : 环绕通知 功能强大，可代替以上四种通知，还可以控制目标业务方法是否执行以及何时执行

@Pointcut


在不改变原有业务逻辑的情况下，增加了横切逻辑代码，实现解耦合，避免横切的代码逻辑重复。


![](../pics/a1.png)

AbstractAutoProxyCreator : Bean初始化前调用所有的后置处理器

在Bean初始化前调用 ： 查找切面，解析切面

在Bean初始化后调用 ： 代理生成


aop 其实就是，在系统中随便找个一个包下的 某些文件 或者 某个类的方法 或者 加了某个注解的类 进行拦截对这些类进行代理，将Spring原有的例Bean转换成自己的代理Bean,以至于达到自己可控的目的，代理之后，

该类的方法都走 一个叫做 invoke的方法。然后就达到了自己可控的操作，就可以为所欲为了。 在走这个 invoke的时候，先把自己的前置增强 @Before执行起来，然后就又到 该代理类的目标方法，目标方法结束后，执行 @After后置增强，执行目标方法的时候 try一下，目标方法异常了，就走 @AfterThrowing 异常方法，否则就正常执行@AfterReturning方法。


**静态代理模式 / 动态代理模式**

静态代理：

	public interface Subject{
		public void doSomething();
	}

	public Class RealSubject implements Subject{
		public void doSomething(){
			sout("call something()");
		}
	}

	public Class SubjectProxy implements Subject{
		Subject sub=new RealSubject();
		
		public void doSomething(){
			sub.doSomething();
		} 
	}

	public class Test{
		public static void main(String[] args){
			Subject subject=new SubjectProxy();
			sub.doSomething();
		}
	}



动态代理：

	public interface Subject{
		public void doSomething();
	}

	public class RealSubject implements Subject{
		public void doSomething(){
			sout("call something()");
		}
	}

	public class proxyHandler implements InvocationHandler{
		private Object tar;
	
		public Object bind(Object tar){
			this.tar=tar;
			return Proxy.newProxyInstance(tar.getClass().getClassLoader(),
										  tar.getClass().getInterfaces(),
										  this);
		}

		public Object invoke(Object proxy,Method method,Object[] args) throw Throwable{
			Object result=null;
			// before : 在调用具体方法前，执行功能处理
			result=method.invoke(tar,args);
			// after: 在调用具体方法后，执行功能处理
			return result;
		}
	
	}

	public class proxyTest{
		public static void main(String[] args){
			ProxyHandler proxy=new ProxyHandler();
			Subject subject=(Subject)proxy.bind(new RealSubject());
			subject.doSomething();
		}
	}


静态代理： 代理类可以在不改变原有类的情况下，对其功能进行扩展。

 -扩展性好 -可维护性低：实现统一接口，接口改变都得变 可重用性差


动态代理：

在Proxy.getProxyInstance()方法中，传入需要被代理的类对象，将传入的Object实例化，并取其类加载器，接口得到代理类对象。

对代理类对象方法的调用，都会转化为对invoke方法的调用，可以在invoke方法中对代码进行扩展。

代码可重用性强 / 代理类和被代理类完全解耦

不够灵活，必须执行invoke里的



