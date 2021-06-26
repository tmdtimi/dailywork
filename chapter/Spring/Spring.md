##Spring

Spring是一个轻量级的框架，是为了解决企业级开发的复杂性而创建的。

Spring是一个分层架构，由7个模块组成。

Spring的核心在于Spring 核心容器，可以在它的基础上整合其他模块，也可以整合第三方的框架。

Spring通过核心容器来实现IOC  --依赖倒置原则

IOC控制反转 ：将类得实例交给Spring容器进行管理，从而将类和类之间的关系解耦。 实现的方法是通过依赖注入。

IOC:

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

beanDefinition.setBeanClass(.class)  //设置原料

beanFactory.registerBeanDefinition(beanDefinition)  //注册到工厂中

beanFactory.getBean(.class) //可以生产了

BeanFactory是低级容器，只是需要我们先配置一些原料才能生产Bean.

AnnotationConfigApplicationContext(.xml) 直接将配置中的Bean自动加载成BeanDefinition原料，然后交给BeanFactory


