## 1、Spring介绍

Spring是一个开源的轻量级控制反转（IOC）和面向切面（AOP）的容器框架。

**控制反转（IOC）**——Spring通过一种称作控制反转（IOC）的技术促进了松耦合。创建对象不再通过new的方式进行创建，而是交给Spring IOC容器进行创建，需要的时候从容器中去拿

**面向切面（AOP）**——允许通过分离应用的业务逻辑与系统级服务和事务进行内聚性的开发。可以不通过修改代码而改变逻辑

## 2、Spring IOC容器

**spring IOC容器有两种实现方式：**

1）通过xml配置文件

2）带有注解@Configuration的注解类

**IOC底层原理**

IOC底层原理使用技术：

- xml配置文件
- dom4j解析xml
- 工厂设计模式
- 反射

IOC底层原理实现：

第一步：创建xml配置文件，配置<bean>标签

第二步：创建工厂类，使用dom4j解析文件，获取标签值，通过反射技术创建类（Class clazz = Class.forName(className);clazz.newInstace()）

注：SPring框架已经封装了第二步

## 3、Spring 的bean管理（xml方式）

**bean实例化（在spring中通过配置我呢见创建对象）的三种方式**：

1、使用类的无参构造函数创建（**重点**）

`<bean id = "user" class="org.ltt.User">` </bean>

2、使用静态工厂创建

创建静态方法，返回类对象

`<bean id = "user" class="org.ltt.BeanFactory" factory-method="getBean"></bean>`

BeanFactory：创建的工厂类，getBean：在工厂类中创建的静态方法

3、使用实例化工厂创建

创建的不是静态方法，返回类对象

·`<bean id = "bean2Factory" class="org.ltt.Bean2Factory"></bean>`

<bean id="user" factory-bean="bean2Factory" factory-method="getBean2">

Bean2Factory：创建的工厂类；getBean2：创建的方法·

**bean标签常用属性**

1. id属性：唯一标识
2. class属性
3. name属性：和id属性功能相同，但是id属性值不包含特殊符号，name属性可以使用特殊符合（下划线等）
4. scope属性：
   - singleton：默认值，单例
   - prototype：多例
   - request：创建对象把对象放到request域里面
   - session：创建对象把对象放到session域里面
   - globalSession：创建对象把对象放到globalSession域里面

**属性注入：**

​	创建对象的时候，向类里面的属性设置值，这个过程称为属性注入

属性注入的三种方式（java）

1. 使用set get方法注入

2. 使用有参构造函数注入

3. 使用接口注入

   ```java
   public interface Dao{
   ​	public void delete(String name);
   }
   
   public class DaoInpl implements Dao {
   ​	private String name;
   ​	public void delete(String name) {
   ​		this.name = name;
   ​	}
   }
   ```

在spring框架中，支持前两种方式属性注入

1. set方法注入（重点）

   ```
   <bean id="user" class = "org.ltt.User">
   	<!-- name:属性字段   value：属性值 -->
   	<property name="userName" value="小张"></property>
   </bean>
   ```

   

2. 有参构造注入

   ```
   <bean id="user" class = "org.ltt.User">
   	<constructor-arg name="userName" value="小张"></constructor-arg>
   </bean>
   ```

   

注入复杂类型的属性(数组、list、map、properties)：

Java类中定义这些属性，并生成set方法

```
<bean id="test" class="org.com.ltt">
	<!-- 数组 -->
	<property name="arrays">
		<list>
			<value>小明</value>
			<value>小张</value>
			<value>小强</value>
		</list>
	</property>
	<!-- list -->
	<property>
		<list>
			<value>test1</value>
			<value>test2</value>
			<value>test3</value>
		</list>
	</property>
	<!-- map -->
	<property>
		<map>
			<entry key="aa" value="test1"></entry>
			<entry key="bb" value="test2"></entry>
			<entry key="cc" value="test3"></entry>
		</map>
	</property>
	<!-- properties -->
	<property>
		<props>
			<prop key="driverclass">com.mysql.jdbc.Driver</prop>
			<prop key="username">root</prop>
		</props>
	</property>
</bean>
```

## **4、IOC和DI区别**

1）IOC：控制反转，把对象创建给spring进行配置

2）DI：依赖注入，向类里面的属性设置值

3）IOC和DI关系：依赖注入不能单独存在，需要在ioc基础之上完成操作

IOC更像是一种思想，改变了A对象获取B对象的方式，不在在A中主动new B，而是反转控制。将创建B对象的控制权交给第三方容器（spring IOC容器），ioc思想中，动态的向某个对象提供它所属的对象，这一点是通过DI来实现的，A需要B，我们只需要告诉spring，A需要B，至于B怎么构造，何时构造，A不需要知道，spring会在何时的时候制造一个B，然后注入到A中，这样就完成了各个对象之间关系的控制。

## 5、spring整合web项目原理

1、加载spring核心配置文件

```
ApplicationContext context = new ClassPathXmlApplicationContext("application.xml");
```

然后，new对象，功能上可以实现，效率很低

2、实现思想

把加载配置文件和创建对象过程，在服务器启动时候完成

3、实现原理

（1）ServletContext对象

（2）监控器

（3）具体使用：

- 在服务启动时候，为每个项目创建一个ServletContext对象

- 在ServletContext对象创建的时候，使用监听器可以监听到ServletContext对象在什么时候创建

- 使用监听器监听到ServletContext对象创建的时候，加载spring配置文件，把配置文件配置对象创建

- 把创建出来的对象放到Servlet Context域对象里面（setAttribute方法）

- 获取对象的时候，到ServletContext域得到（getAttribute方法）

  ## Spring 的bean管理（注解）

**spring注解开发的准备工作**

1、引入jar包

（1）导入基本jar包

comons-loggin-XXX.jar

log4j-XXX.jar

spring-bean-XXX.jar

spring-context-XXX.jar

spring-core-XXX.jar

spring-expression-XXX.jar

（2）导入aop的jar包

spring-aop-XXX.jar

2、创建，创建方法



3、创建spring配置文件，引入约束

（1）做ioc基本功能，引入beans约束

（2）做spring的IOC注解开发，引入context约束

4、开启注解扫描

```
<!-- 开启注解扫描 到包里面扫描类、方法、属性行是否有注解-->
<context:component-scan base-package="org.ltt"></context:component-scan>

<!-- 扫描属性上面的注解 -->
<context:annotation-config></context:annotation-config>
```

**注解创建对象**

1、创建对象有四种注解

- @Controller：web层

- Service： service层

- Respository:持久层

- Component

  例子：@Service(value="user")

创建对象是单实例还是多实例

@Scope(value="prototype")

**注解注入属性**

1. @Autowired(value="")

根据类名找到类对应的对象

 2.@Resource(name="")

name属性值写@Component注解中value属性值,即bean的id

**配置文件和注解混合使用**



## 6、AOP

**1、AOP概念**

1）aop：面向切面（方面）编程：扩展功能不修改源代码实现

2）AOP采取横向抽取机制，取代了传统纵向继承体系重复性代码

**2、AOP原理**

纵向抽取机制：

![image-20201116221817445](C:\Users\10852\AppData\Roaming\Typora\typora-user-images\image-20201116221817445.png)

AOP横向抽取机制，有接口情况

![image-20201116222513884](C:\Users\10852\AppData\Roaming\Typora\typora-user-images\image-20201116222513884.png)

AOP横向抽取机制，无接口情况



![image-20201116222953370](C:\Users\10852\AppData\Roaming\Typora\typora-user-images\image-20201116222953370.png)

**AOP操纵术语**

- Joinpoint(连接点):所谓连接点是指那些被拦截到的点，在spring中，这些点指的是方法，因为spring只支持方法类型的连接点。（类李米娜那些方法可以被增强，这些方法成为连接点）

- Pointcut(切入点)：所谓切入点是指我们要对那些Joinpoint进行拦截的定义。（在类中实际被增强的方法称为切入点，比如类中只增强了add方法，则add方法就是切入点）

- Advice(通知/增强)：所谓通知是指拦截到Joinpoint之后要做的事情就是通知（比如扩展日志功能，这个日志功能称为增强），通知分为五种方式

  前置通知：在方法之前执行

  后置通知：在方法之后执行，不管方法是否抛出异常都会执行

  异常通知：方法出现异常执行

  最终通知：在后置之后执行

  环绕通知：在方法之前和之后执行

- Aspect(切面)：把增强应用到具体方法上面的过程称为切面（把增强用到切入点的过程）。
- Target(目标对象)：代理的目标对象（要增强方法所在的类）
- Weaving(织入)：把增强应用到目标对象的过程（把advice应用到target的过程）。
- Proxy(代理)：一个类被AOP织入增强后，就产生一个结果代理类。

## 7、spring的AOP操作

1）在spring里面进行aop操作，使用aspectj实现

- aspectj不是spring的一部分，aspectj和spring一起使用进行aop操作
- spring2.0后新增了对Aspectj的支持

2）使用aspectj实现aop有两种方式

- 基于aspectj的xml配置
- 基于aspectj的注解方式

**AOP操作的基础准备（xml方式）**

1. 导入jar包

   除基础jar包外，添加aopaliance-1.0.jar、aspectjweaver-1.8.7.jar、spring-aop-4.2.4.jar、spring-aspects-4.2.4.jar

2. 创建spring核心配置文件，导入aop约束