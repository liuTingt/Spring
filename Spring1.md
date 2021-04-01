Spring

1、spring介绍

Spring是一个开源的轻量级控制反转（IOC）和面向切面（AOP）的容器框架。

**控制反转（IOC）**——Spring通过一种称作控制反转（IOC）的技术促进了松耦合。创建对象不再通过new的方式进行创建，而是交给Spring IOC容器进行创建，需要的时候从容器中去拿

**面向切面（AOP）**——允许通过分离应用的业务逻辑与系统级服务和事务进行内聚性的开发。可以不通过修改代码而改变逻辑

2、bean的创建过程

通过dom4j解析配置文件，获取<bean>标签属性，再通过反射技术获取类，newInstance()实例化对象

3、ioc容器初始化

Spring IOC容器有两种实现方式：

1）通过xml配置文件

2）带有注解@Configuration的注解类

**IOC底层原理**

ioc底层原理使用技术

（1）xml配置文件

（2）dom4j解析xml

（3）工厂设计模式

（4）反射

IOC底层原理实现：
第一步：创建xml配置文件，配置<bean>标签

第二步：创建工厂类，使用dom4j解析文件，获取标签值，通过反射技术创建类（Class clazz = Class.forName(className);clazz.newInstance()）

注意：Spring框架已经封装了第二步

4、spring依赖注入

5、SPring 的bean管理（xml方式）
bean实例化的方式：
 1、在spring里面通过配置文件创建对象

 2、bean实例化三种方式
 第一种：使用类的构造函数创建（重点）

 第二种：使用静态工厂创建

 第三种：使用实例工厂创建

**bean标签常用属性**
	(1) id属性
	

	(2)class属性
	
	(3)name属性：
	
	(4)scope属性
	- singleton：默认值，单例
	- prototype：多例
	- request：创建对象把对象放到request域里面
	- session：创建对象把对象放到session域里面
	- globalSession：创建对象把对象放到globalSession域里面

属性注入：
	创建对象时候，向类里面的属性设置值
	

	2、属性注入方式（普通java）：
	第一种：使用set方法注入
		
	第二种：使用有参构造函数注入
	第三种：使用接口注入
	
	3、在spring框架中，支持前两种方式
	(1)set方法注入（重点）
	(2)有参构造注入


​	
​	

Spring 面试

1、Spring中的bean/对象的类型（scope）有那些？

​	取决于所使用的具体容器，如果使用的是基本容器，例如classPathXmlApplicationContext，那么只有single和prototype可用，如果使用的是Web容器，例如WebXmlApplicationContext，那么就有single、prototype、request、session、global session等可用，另外scope还可以自定义。

2、为什么要使用Spring？

​	它提供了一个web应用程序的基本框架，它是工厂和容器，提供了IOC。
​	总结如下：
​	1、spring提供一个工厂/容器，统一管理模块的定义，根据需要创建。
​	2、把模块的配置参数统一管理，模块不需要自行读取配置。
​	3、Spring提供依赖注入，把依赖的模块自动推送进来，不需要模板自动拉取。
​	4、spring提供对很多第三方框架的集成功能，减少样板代码

3、BeanFactory – BeanFactory 实 现 举 例 

Bean工厂是工厂模式的一个实现，提供了控制反转功能，用来把应用的配置和依赖从真正的代码中分离，最常用的BeanFactory实现是XmlBeanFactory类。

