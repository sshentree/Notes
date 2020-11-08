# Spring 笔记

## 介绍 Spring 框架

### Spring 概述

1. [参考地址](https://www.w3cschool.cn/wkspring/pesy1icl.html)
2. [下载参考](https://www.jianshu.com/p/bd98ddfa93be)
3. Spring 轻量级意义
   - 不完全依赖于 Spring，可以根据需求回收 Spring 管理权
4. IOC（Spring 核心）
   - Inverse of Control （控制反转）
   - 控制反转是一种编程思想：将在程序中手动创建对象的控制权，交由 Spring 框架来管理
   - __这种管理权，可以收回（轻量级）__
5. AOP（Spring 重要功能）
   - Aspect Oriented Program（面向切面编程），OOP 面向对象编程

### 搭建 Spring 运行环境

1. 导入 JAR 包
   - Spring 自身 JAR 包 `spring-framework-4.0.0.RELEASE/libs`
     1. `spring-beans-4.0.0.RELEASE.jar`
     2. `spring-context-4.0.0.RELEASE.jar`
     3. `spring-core-4.0.0.RELEASE.jar`
     4. `spring-expression-4.0.0.RELEASE.jar`
   - Spring 日志工具
     1. `commons-logging-1.2.jar`
2. 在 Spring Tool Suite 工具中通过以下步骤创建 Spring 配置文件

### Spring 简单示例一

1. 导包（上述的 JAR 包）

2. 创建 Bean 层

   - 创建 `Person` 类

3. 创建配置文件 `applicationContext.xml` 将对象管理交由 Spring 管理 (STS 有快捷方式 `Spring Bean Configuration File` ）

   - 将类全名按照标签要求书写 `<bean class="Bean.Person"></bean>`

   - __XML 的命名空间__ （`xmlns/xmlns:xsi...`） ，规定配置文件书写格式，并有提示信息。

   - XML 配置

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
         <bean class="Bean.Person"></bean>
     </beans>
     ```

   - 通过子标签对 __对象__ 属性赋值

     ```xml
     <bean id="person" class="Bean.Person">
         <property name="id" value="100"></property>
         <property name="name" value="zhangsan"></property>
     </bean>
     ```

4. 测试类是否创建

   - 创建测试类

     ```java
     // 获取初始化容器 IOC
     // 为什么直接写文件名，因为 conf(使用 source folder 创建)编译后会加载到类路径下
     ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
     
     // 获得对象(通过 id 值)
     // 返回 Object 
     // Person person = (Person) ac.getBean("person");
     
     // 可是使用 类.class 获取对象（与 id 无关），但是配置文件中只能管理一个类
     // Person person = ac.getBean(Person.class);
     
     //通过 id 和 类.class 获取对象，建议使用此方法
     Person person = ac.getBean("person01", Person.class);;
     
     System.out.println(person); // Person [id=100, name=zhangsan]
     ```

## IOC 容器和 Bean 配置

### IOC 和 DI

1. IOC 反转控制
   - 对对象创建来说，程序员将不需要在考虑对象创建的问题，只需要使用时，获取即可。
     1. 如单类涉及，Spring 只需在配置文件中要设置 `scope` 为单类即可。（方便快捷）
2. DI（Dependency Injection）：依赖注入
   - DI 是 IOC 的一种具体表现形式
   - __依赖__ 便是 B 类中属性为 A 类
   - __注入__ 等于赋值操作
     1. 如：通过子标签对 __对象__ 属性赋值
3. IOC 容器在 Spring 中实现
   - IOC 容器在底层实质上是一个对象工厂
   - Spring 提供了两种实现 IOC 容器的方式
     1. `BeanFactory` ：IOC 容器的基本实现，是 Spring 内部方式，是面向 Spring 本身的，不是提供给开发者使用的
     2. `ApplicationContext` ：是 `BeanFactory` 的子接口，提供了更高级的特性，是面向 Spring 的使用者，集合所有场合都是用 `ApplicationContext` ，而不是 `BeanFactroy`
4. __ApplicationContext 的主要实现类__
   - `ClassPathXmlApplicationContext` ：对应类路径下的 XML 格式配置文件
     1. 使用 __相对路径__，因为其配置文件编译后在类路径下（Source Folder 创建）
   - `FileSystemXmlApplicationContext` ：对应文件系统中 XML 格式配置文件
     1. 使用 __绝对路径__
   - 初始化时创建单例 bean，也可以通过配置文件创建多例 bean
5. `ConfigurableApplicationContext` （接口）
   - 是 `ApplicationContext` 的子接口，包含一些扩展方法
   - `refesh()` 和 `close()` 让 `ApplicationContext` 具有启动、刷新和关闭功能

### bean 属性赋值

1. 依赖注入的方式（都依赖配置文件），有两种
   - `setXxx` 方法注入
   - 构造器注入
2. 通过 bean 的 `setXxx()` 方法注入
   - 实际上配置文件中 `<property name="age" value="29"></property>` 属性 `name` 就是寻找的 `setXxx` 给对象赋值的
3. 通过构造方法注入
   - 在配置文件中 `<constructor-arg value="11001" index="0" type="java.lang.Integer"></constructor-arg>`
   - 直接按照构造器的参数顺序书写即可
   - 如果有需要指定参数类型及位置时，使用 `type` 和 `index` 

### p 命令空间

1. __XML 文件中命令空间即为规定用什么标签__

2. p 命令空间，引入 `xmlns:p="http://www.springframework.org/schema/p"`

3. 使用实例

   - 直接在 `<bean>` 中使用，__属于 `setXxx` 方式__ 

     ```xml
     <bean id="s2" class="Bean.Student" p:id="100" p:name="wangwu" p:age="21"></bean>
     ```

### 为属性赋值时，是可以使用是什么值

1. 字面量

   - 可以使用字符串表示的值，可以通过 `value` 属性或其子节点方式指定

     1. 子节点赋值

        ```xml
        <property name="id">
            <value>10001</value>
        </property>
        ```

   - 基本数据类型及其封装类、String 都可以采用字面量注入

   - 如字面值含有特殊字符，可以使用 `<![CDATA[]]>`

2. 属性也为对象时，如何赋值

   - __如：学生类中，有老师类，如何赋值__

   - __如：老师中有名字属性如何赋值__

   - 在 XML 中管理 Teacher 类

   - 在学生中，使用 `ref` 属性指定老师类的 `id` 

     ```xml
     <bean id="student" class="Bean.Student">
         <property name="id" value="102"></property>
         <property name="name" value="lisi"></property>
         <property name="teacher" ref="teacher"></property>
         <property name="teacher.name" value="xiaohong"></property>
     </bean>
     
     <bean id="teacher" class="Bean.Teacher">
         <property name="id" value="102"></property>
         <property name="name" value="lisi"></property>
     </bean>
     ```

3. 内部 bean

   - 内部 bean ，定义在某个 bean 的内部，只能在当前的 bean 中使用

     1. `id` 属性可以不写（没有用处）

        ```xml
        <bean id="student" class="Bean.Student">
            <property name="id" value="102"></property>
            <property name="name" value="lisi"></property>
            <property name="teacher">
                <bean id="teacher" class="Bean.Teacher">
                    <property name="id" value="102"></property>
                    <property name="name" value="lisi"></property>
                </bean>
            </property>
        </bean>
        ```

### 注入集合属性

1. [参考地址](https://www.w3cschool.cn/wkspring/kp5i1ico.html)

2. 集合中存储引用数据类型

   - 样式一

     ```xml
     <property name="gf">
         <list>
             <ref bean="id值"/>
             <ref bean="id值"/>
         </list>
     </property>
     ```

   - 样式二

     1. 创建内部 bean

        ```xml
        <property name="gf">
            <list>
                <bean></bean>
            </list>
        </property>
        ```

   - 样式三

     1. 引用另一个命令空间 `xmlns:util="http://www.springframework.org/schema/util"`

     2. 属性引用集合（这就是表示集合）

        ```xml
        <property name="gf" ref="student"></property>
        
        <util:list id="student">
            <value>test</value>
        </util:list>
        ```

3. 数组和 `List` 一样

   - 数组也可以使用 `List` 标签

## FactoryBean（对象工厂）

### FactoryBean

1. Spring 中有两种类型的 bean ，一种普通 bean ，另一种 bean ，就是 FactoryBean

   - 工厂 bean 与普通 bean 不同，其返回的对象不是指定的实例，其返回的是该工厂 bean 的 `getObject` 方法返回的对象
   - 工厂 bean 必须实现 `org.springframework.beans.factory.FactoryBean` 接口

2. 实现 `FactoryBean` 接口的三个抽象方法

   - 获取工厂创建的对象 
     1. `public Car getObject() throws Exception` 
   - 获取创建对象的类型
     1. `public Class<?> getObjectType()`
   - 是否是单例
     1. `public boolean isSingleton() `
     2. `true` 为单例 ，默认工厂创建的为 __多例__
     3. IOC 管理的对象默认是 __单例__

3. 在配置文件中

   - 只需要书写 __工厂类__ 即可

     1. 实例

        ```xml
        <bean id="factory" class="factoryBean.MyFactory"></bean>
        ```

4. 创建对象

   - 获取容器，创建对象（创建的不是工厂对象）

     1. 代码（强转）

        ```java
        Car car = (Car) ac.getBean("factory");
        ```

## Bean 知识

### Bean 作用域

1. 配置文件实例

   - XML

     ```xml
     <bean id="student" class="test01.Student" scope="">
         <property name="id" value="10"></property>
         <property name="name" value="zhangsan"></property>
     </bean>
     ```

2. 单例和多例

   - 单例 `scope="singleton"`
   - 多例 `scope="prototype"`
   - __初始化容器时：如果是单例（默认单例）初始化容器时便创建对象__

### Bean 生命周期

1. Spring IOC 容器可以管理 bean 的生命周期，Spring 允许在 bean 生命周期内特定时间点执行指定的任务
2. Spring IOC 容器对 bean 管理的生命周期的过程
   - 通过构造器或工厂创建 bean 实例
   - 为 bean 的属性设置值和引用其他 bean
   - 调用 bean 的初始化方式
   - bean 可以使用
   - 当容器关闭时，调用 bean 的销毁方法
3. 在配置 bean 时，通过 `init-method` 和 `destroy-method` 属性为 bean 指定初始化和销毁
   - 当指定方法后，就可以在特定时间点执行
   - `<bean init-method="init" destroy-method="destroy"></bean>`

### Bean 的后置处理器

1. 将 bean 的生命周期从 5 步，增加到 7 步
   - bean 的后置处理器允许调用 __初始化方法前、后__ 对 bean 进行额外的处理
2. bean 后置处理器对 IOC 容器例所有 bean 实例逐一处理，而非单一实例
   - 其典型应用：检查 bean 属性的正确性；根据特定标准修改 bean 属性
3. bena 后置处理器需要实现接口
   - 实现 `org.springframework.beans.factory.config.BeanPostProcessor` 
   - 在初始化前后，Spring 将每个 bean 实例分别传递给上述接口的以下两个方法
     1. `Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException`
     2. `Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException`
4. __添加后置处理器__
   - 注册 __后置处理器__ ，__注册就完了__
5. 添加 bean 的后置处理器后 bean 的生命周期

## 引用外部属性文件

### 导入 JAR 包

1. `druid.jar` 数据库连接池
2. `mysql-connection-java.jar`  连接 mysql 数据库

### 使用 XML 配置信息

1. 使用 `druid.xml`

   - 实例

     ```xml
     <bean id="datasource" class="com.alibaba.druid.pool.DruidDataSource">
         <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
         <property name="url" value="jdbc:mysql://localhost:3306/bookstore"></property>
         <property name="username" value="root"></property>
         <property name="password" value="123"></property>
     </bean>
     ```

2. 测试

   - 代码

     ```java
     ApplicationContext ac = new ClassPathXmlApplicationContext("druid.xml");
     // 获取连接池
     DruidDataSource dataSource = ac.getBean("datasource", DruidDataSource.class);
     DruidPooledConnection connection = dataSource.getConnection();
     ```

### 加载 .property 资源文件

__说明：使用 `Source Floder` 创建目录，在创建 `.properties` 文件 `druid.properties`__

1. 使用 `bean` 标签

   - 加载资源文件

     ```xml
     <bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
         <property name="location" value="druid.properties"></property>
     </bean>
     ```

2. 使用 `xmlns:context="http://www.springframework.org/schema/context"` 命令空间

   - 加载资源文件

     ```xml
     <context:property-placeholder location="druid.properties"/>
     ```

3. 引用资源文件内容

   - 使用 `${}` 获取内容

     ```xml
     <bean id="datasource" class="com.alibaba.druid.pool.DruidDataSource">
         <property name="driverClassName" value="${jdbc.driverClassName}"></property>
         <property name="url" value="${jdbc.url}"></property>
         <property name="username" value="${jdbc.username}"></property>
         <property name="password" value="${jdbc.password}"></property>
     </bean>
     ```


## 自动装配

### 自动装配概述

1. 手动装配：以 `value` 或者 `ref` 的方式指定属性值都是手动装配 __（就是依赖注入）__
2. 自动装配：根据指定的装配规则，不需要明确指定，Spring 自定将匹配的属性注入 bean 中
   - __自动装配属性是非字面量的属性__
   - __依赖于 `setXxx()` 方法__
   - __允许装配的属性为 null__
3. 属性 `autowire` 
   - 默认值 `no` ，没有自动装配

### 属性 `autowire`

1. 值 `byName` 
   - 通过属性名，与 Spring 容器中 bean 的 id 匹配，若一致直接赋值
2. 值 `byType`
   - 通过 spring 容器中 bean 的类型，为兼容性的属性赋值
   - 在 Spring 容器中，只能存在一个类型兼容 bean
3. 选用建议
   - __都不推荐__

### 通过 `byName`

1. 例如：

   - 定义 A 类，存在一个属性为 B 类

     ```java
     public class A {
         private B b; // B 类型属性
     }
     ```

     

2. `byName` 匹配原则为 A 类中的 B 类型属性的变量名，要与 B 类在 XML 的 id 相同（id 是唯一的）

3. 例（`autowrie`）

   - XML 代码（bean B 的 id 要和 A 的属性名对应）

     ```xml
     <bean id="" class="A" autowire="byName">
         <property name="" value=""></property>
     </bean>
     
     <bean id="b" class="B"></bean>
     ```

### 通过 `byType`

1. 如上理解：只要是类型匹配即可
   - 接口的实现类
   - 类的子类
2. 这种匹配不唯一性，寻找类型与寻找 id ，可想而知。

## 通过注解配置 bean

### 概述

1. 概述
   - 相对于 XML 而言，通过注解方式配置 bean 将会更加优雅，而且和 MVC 组件化开发理念更加契合，是开发中常用的方式
2. 注解理解：
   - 相当于在配置文件中使用 `bean` 标签（实际上 Spring 会自动生成 bean 标签）
   - __Spring 管理的只能是可以实例化的类（抽象类、接口不可以），所以说注解也只能加载可以实例化的类上__
3. 导入 Spring 的JAR 包
   - `spring-aop-4.0.0.RELEASE.jar`

### 使用注解标识组件

1. 普通组件 `@Component`

   - 标识一个受 Spring IOC 容器管理组件

2. 持久化层组件 `@Repository`

   - 标识一个受 Spring IOC 容器管理的持久层组件

3. 业务逻辑层组件 `@Service`

   - 标识一个受 Spring IOC 容器管理的业务逻辑层组件

4. 表述层控制组件 `@Controller`

   - 标识一个受 Spring IOC 容器管理

5. 组件命令规则

   - 默认情况：bean 会以类的首字母小写为 id 值
   - __使用组件注解的 value 属性指定 bean 的 id __
     1. `@Controller(value="id")` ，当只有 value 一个属性值时，可以 `@Controller("id")`
   - 事实上 Spring 没有能力识别一个组件是否是他所标识的，即使将 `@Repository` 注解在表述控制层组件上，也不会产生任何错误，所以注解的不同类型，只为开发人员便是

6. __配置文件书写方式__

   - 使用命令空间

     1. 添加命名空间 `xmlns:context="http://www.springframework.org/schema/context"`

   - 使用标签：__扫描组件（组件就是组成 Spring 的 bean 部分 [管理类]）__

     1. 标签

        ```xml
        <context:component-scan base-package="加入注解的A包, B包 "></context:component-scan>
        ```

     2. 可以使用逗号 `,` 分割两个包

7. 实现过程

   - 添加注解
   - 配置文件（2 步）
   - 加载时
     1. 作为 Spring 组件进行加载，会自动在 Spring 的配置文件中生成相对应的 bean
     2. __bean 会以类的首字母小写为 id 值__

### 扫描包含包和排除包

1. Java 包的概述

   - 包的命令规则：一般全为小写
   - 包的使用：每一个 `.` ，代表一层包
     1. `com.spring.test` ，这就代表 3 层包

2. __过滤属性__

   - 默认情况 `use-default-filters="true"` ，表示全部包含

3. 包含过滤

   - 将属性 `use-default-filters=""` 设置 `false` ，表示全部不包含，在使用包含过滤器，指定包含的

     ```xml
     <context:component-scan base-package="加入注解的包A" use-default-filters="true">
         <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
     </context:component-scan>
     ```

   - 上面实例是根据注解类型进行扫描

   - 根据指定类的限定名

     ```xml
     <context:exclude-filter type="assignable" expression="test03.Test"/>
     ```

4. 不包含过滤

   - 将属性 `use-default-filters="true"` 设置 `true` ，表示全部包含，使用过滤去，排除不包含的

     ```xml
     <context:component-scan base-package="加入注解的包A" use-default-filters="true">
         <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Component"/>
     </context:component-scan>
     ```

### 基于注解的自动装配

1. 使用 `@Autowired` 注解，自动装配（默认使用 `byType`）

   - 在属性上使用此注解，就不要手动赋值了
   - 不需要使用 `setXxx()` 方法
   - 默认不允许装配对象为 `null` ，可以通过 `@Autowired(required=true)` 设置
   - 默认使用 `byType` 可是使用 `@Qualifier(value="id")` 指定装配的 id
     1. 也可以作用于方法（参数）
   - XML 的配置（命令空间）

2. __好处__

   - 可以指定某一特定的字面量属性赋值
   - bean 的 `autowire` 属性，是针对类中所有字面量属性的赋值

3. __使用何种策略（默认使用 `byType`）__

   - `byName` 
     1. 注解标识组件生成的 id 为类名的首字母小写
     2. 注解自动装配的属性名，不太可能与 id 相同
   - `byType`
     1. 要求只有一个类型可以赋值的（不唯一报错）
     2. 可以赋值的（接口的实现类、子类都可以）

4. 注解自动装配可以自动切换

   - `byType` 不符合条件时（多个符合条件的），将自动切换到 `byName`（前提时属性名与 id 相同）

5. 属性（`@Autowired(required=true)`） 

   - 自动装配失败，将会抛出异常
   - `false` ：装配失败 Spring 也不会抛出异常，默认赋值 `null`

6. __指定自动装箱类__

   - 使用 `qualifier(value="id")` ，指定装箱的对象的 id

     ```java
     @Autowired
     @Qualifier(value="id")
     ```

   - 也可以作用在方法上（`setXxx()`），注解作用在方法参数上

     ```java
     @Autowired
     @Qualifier(value="id")
     public void setXxx(Object obj) {
         this.obj = obj;
     }
     ```

## AOP（面向切面编程）

### 动态代理

1. JDK 代理介绍

   - [参考地址](https://cloud.tencent.com/developer/article/1429932)
   - 依赖于接口，没有接口则不能实现 JDK 动态代理
   - __代理只能代理接口__ ，就是代理的目标类，必须实现接口，而代理类只能代理接口中的方法
     1. 代理类实现了，代理目标类实现的接口，__本质上代理类与代理目标类是兄弟关系__

2. 实现过程必要知识

   - 接口 `InvocationHandler` 作用：调用动态代理类实现代理目标接口的方法

     1. 代码

        ```java
        public interface InvocationHandler {
            /**
             * @param proxy 代理类对象
             * @param methon 标识具体调用的是代理类的哪个方法
             * @param args 代理类方法的参数
             */
            public Object invoke(Object proxy, Method method, Object[] args)
                throws Throwable;
        }
        ```

   - Java 提供了 `Proxy` 的静态方法生成动态代理类

     1. 代码，以及参数

        ```java
        Proxy.newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h);
        ```

     2. `loader` ：类加载器，加载代理对象所属类

     3. `interfaces`  接口类型数组，代理目标实现的接口（也是代理类要实现的接口）

     4. `h` ：调用处理器，调用代理类实现的接口方法

3. 创建动态代理类

   - 代码

     ```java
     public class ProxyUtil {
         // 代理目标对象
         private Object obj;
     	// 借助构造器赋值
         public ProxyUtil(Object obj) {
             super();
             this.obj = obj;
         }
     
         public Object getProxy() {
     		// 获取类加载
             ClassLoader loader = this.getClass().getClassLoader();
             // 获取代理目标实现接口（代理类需要实现）
             Class<?>[] interfaces = obj.getClass().getInterfaces();
     		// 借助 Proxy 获取动态代理类
             // new InvocationHandler() 调用处理代理对象实现的方法
             return Proxy.newProxyInstance(loader, interfaces, new InvocationHandler() {
     
                 @Override
                 public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {				
                     // 可以再次处理异常（try/catch.finally）
                     Object result = method.invoke(obj, args);
                     return result;
                 }
             });
         }
     
     }
     
     ```

4. 调用代理类对象

   - 代理目标类 `A` ，其实现的接口 `IB,IC` 
     1. 接口 `IB` ，抽象方法 `bMethod()`
     2. 接口 `IC` ，抽象方法 `cMethod()`

   - 测试类

     ```java
     public class MainJavaProxy {
         public static void main(String[] args) {
             ProxyUtil pu = new ProxyUtil(A);
             // 向上转型
             IB ib = (IB) pu.getProxy(); // 返回 Object，实际上返回的是接口的实现类（与代理目标是兄弟）
             ib.bMethod();
             // 向上转型
             IC ic = (IC) pu.getProxy(); // 返回 Object，实际上返回的是接口的实现类（与代理目标是兄弟）
             ib.cMethod();
         }
     
     ```

### AOP 概述

1. 介绍
   - [参考地址](https://www.w3cschool.cn/wkspring/izae1h9w.html)
   - OOP：纵向继承
   - AOP：横向抽取
   - __横切关注点__：带抽取的功能（Java 中表现形式为方法）
   - __切面__：`aspect` 将抽取的功能整合在一起（Java 中表现形式为类，且依赖于动态代理）
2. AOP 术语
   - [参考地址](https://www.w3cschool.cn/wkspring/izae1h9w.html)

### AspectJ 介绍

1. 介绍
   - AspectJ 是 Java 社区中最完整、最流行的 AOP 框架
   - 可以继承关系，也可以实现代理
   - 在 Spring 2.0 以上，可以使用基于 AspectJ 注解或基于 XML 配置的 AOP
2. 在 Spring 中启用 AspectJ 注解支持
   - 导入 JAR 包 [下载地址](https://mvnrepository.com/)
   - `com.springsource.net.sf.cglib-2.2.0.jar`
     1. __在没有接口的情况下，转到继承上（Java 所有类继承  `Object`） ，所以 Java 的所有类都可以代理__
   - `com.springsource.org.aopalliance-1.0.0.jar`
   - `com.springsource.org.aspectj.weaver-1.6.8.RELEASE.jar`
   - Spring 中
     1. `aop.jar`
     2. `aspects.jar`
3. __配置文件__
   - 引入命令空间 `aop`
   - 标签（开启 aspectj 的自动代理）
     1. `aop:aspectj-autoproxy></aop:aspectj-autoproxy>`
   - __注意__ ：AOP 依赖于 IOC，所以加注解的需要进行扫描
     1. `<context:component-scan base-package="test01"></context:component-scan>`
     2. 目标、代理都需要 Spring 管理

### AspectJ 实例一

1. AOP 是依赖于 IOC 的，所以都需要 Spring 进行管理

   - 开启 Aspectj 自动代理
     1. 配置文件设置 `aop:aspectj-autoproxy></aop:aspectj-autoproxy>`

   - 目标：被通知或被代理的类
     1. Spring 进行管理，注解 `@Component`
     2. 设置为代理，注解 `Aspect`

   - 切面：存储横切关注点的的类
     1. Spring 进行管理，注解 `@Component`
   - 通知：与横向关注点本质上是同一物体（横向关注点被抽象成方法放入切面中就叫做通知）
     1. 通知注解 `@Before(value = "execution(public double test01.MathIImpl.add(double, double))")`
     2. `value`：切入点表达式（是对目标方法的定位）
     3. `JoinPoint` ：封装切入点信息，如方法名、方法参数……

2. 切面类

   - 注解的使用，目标：`test01` 包下的 `MathIImpl` 类的 `add()` 方法

     1. 代码

        ```java
        @Component
        @Aspect
        public class MyLoggerAspect {
        
            /**
        	 * @Before:将方法设置为前置通知
        	 * 必须设置 value:其值为切入点表达式(作用在连接点上：方法的某一个位置)
        	 * @param joinPoint:切入点(封装切入点信息)
        	 * 	方法名、方法参数……
        	 */
            @Before(value = "execution(public double test01.MathIImpl.add(double, double))")
            public void beforeMethod(JoinPoint joinPoint) {
                Object[] args = joinPoint.getArgs(); // 获取参数
                String methodName = joinPoint.getSignature().getName(); // 方法名
        
                System.out.println("methodName:" + methodName + "; args:" + Arrays.toString(args));
        
            }
        }
        ```

3. 测试类的编写

   - IOC 容器其加载，获取的实际上是切面的对象，所以 `.class` 需要时实现的接口

     1. __这里已经实现了自动代理，所以切面也实现了目标类的接口，所以获取的切面对象（是目标对象的兄弟）__

     2. __在没有接口时，继承也可以（类都继承 `Object`） ，换句话说任何类都可以代理__

        - 获取对象 `ac.getBean()` 返回代理类即可
     
     3. 代码
     
   ```java
        ApplicationContext ac = new ClassPathXmlApplicationContext("aop.xml"); // 获取 IOC容器
        MathI mathI = ac.getBean("mathIImpl", MathI.class); // 获取的切面类
   ```
   

### 切面表达式

1. 通知不可能之作用于一个方法，所以 `value` 表达式需要有兼容性
2. 例
   - `value = "execution(public double test01.MathIImpl.add(double, double))"`
     1. 表示：`test01` 包下的 `MathIImpl` 类的 `public double add(double, double)` 方法
3. 兼容
   - `public double` ：访问权限和返回值用一个 `*` 表示
   - `add` ：方法名用一个 `*` 表示
   - `MathIImpl` ：类也可用一个 `*` 表示
   - 任意方法 `*(..)`
4. 例
   - `value = "execution(* test01.*.*(..))"`
     1. `test01` 下所有类的所有方法

### 前置、后置通知

1. 注解

   - 前置通知：`@Before(value="execution=()")`

   - 后置注解 `@After(value="execution=()")`

2. 前置：作用于方法执行之前

   - 传递参数 `JoinPoint`

   - 获取方法名，及参数

3. 后置：作用于方法的 `finally` ，不管方法发生什么，总会执行

   - 不怎么需要传递参数

   - 可以关闭一些资源

### 返回通知

1. 注解：`AfterReturning(value="")`

2. __没有异常则会执行，发生异常则不会执行__

   - 获取方法名，及参数
   - 传递参数 `JoinPoint`

   - 即可以后的返回值的想法

3. __获取方法返回值__

   - 注解设置属性 `AfterReturning(value="", returning="result")`
   - 将属性当作形参传递给返回通知（方法执行完，将结果赋值给 `Oject` 类型的 `result`）
     1. 返回通知方法 `afterReturning(JoinPoint joinPonit, Object result)`
     2. 获得 `System.out.println(result);`  输出结果

### 异常通知

1. 注解：`@AfterThrowing(value="", throwing="ex")`
2. 执行时间
   - 方法异常时，执行
3. 操作异常信息，作为参数传递
   - 异常通知方法 `afterThrowing(Exception ex)`
   - 可以指定处理异常类型，就可以将 `Exception` 换成要处理的异常类型
     1. 如处理空指针异常 `NullException ex` ，异常通知将只处理空指针异常，其他异常将不会触发 __异常通知执行__

### 环绕通知

1. 注解 `Around(value="")`

2. 可以在方法任何时候执行（可控）

   - 环绕通知可以控制代理方法的执行，和动态代理的 `invoke()` 相同，要保证返回数据一致性
   - 可以与其他通知公用

3. 返回代理

   - 代码

     ```java
     @Around(value="execution()")
     public Object around(ProceeJoinPoint pJoinPoit) {
         Object result = null;
         try{
             //前置通知
             result = pJoinPoint.procee(); // 执行代理方法
             // 返回通知
             return result；
         } catch {
             //异常通知
         } finally {
             // 后置通知
         }
         return -1
     }
     ```

   - `ProceeJoinPoint` 是 `JoinPoint ` 的子接口，功能更强大

   - 代理的是 `public int add(int a, int b)` 方法，保持数据的一致性，所以有返回值

### 公共切入点和切面的优先级

1. 注解 `@Pointcut` 

2. 用法

   - 代码

     ```java
     @Pointcut(value="execution(* test01.*.*(..)))")
     public void test(){}
     ```

   - 其他的通知就可以简写

     ```java
     @Before(value="test()")
     public before(){
         
     }
     ```

3. 切面的优先级

   - 首先的有多个切面作用一个目标

   - 原本适合配置文件的加载顺序有关

   - 注解 `@Order(value)` ，越小值越高，0 最高。默认值为 `int` 的最大值

   - 例

     ```java
     @Component
     @Aspect
     @Order(1)
     public class A {}
     ```

### 以 XML 的形式配置切面

1. 注解 `Aspect` 以 XML 配置

2. XML 的书写

   - XML 

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <beans>
         <!-- 扫描注解定义的组件 -->
         <context:component-scan base-package=""></context:component-scan>
     
         <!-- 不需要自动配置，因为使用的 Spring 原生的 AOP -->
         <!-- <aop:aspectj-autoproxy></aop:aspectj-autoproxy> -->
     
         <aop:config>
             <aop:aspect ref="首字母小写的类名作为 id 的切面">
                 <aop:after method="方法名" pointcut="execution()"/>
             </aop:aspect>
         </aop:config>
     </beans>
     ```

   - 公共切入点

     ```xml
     <aop:config>
         <aop:aspect ref="首字母小写的类名作为 id">
             <aop:pointcut expression="execution()" id="cut"/>
             <aop:after method="方法名" pointcut-ref="cut"/>
         </aop:aspect>
     </aop:config>
     ```

## JDBCTemplate

### 介绍

1. 小型的持久层框架（JDBC 工具类）

   - [参考地址](https://www.w3cschool.cn/wkspring/zjs51h9z.html)

2. 导入 JAR 包（与 AOP 无关）

   - Spring 的 IOC 需要的包
     1. `spring-beans-4.0.0.RELEASE.jar`
     2. `spring-context-4.0.0.RELEASE.jar`
     3. `spring-core-4.0.0.RELEASE.jar`
     4. `spring-expression-4.0.0.RELEASE.jar`
   - JDBCTemplate 需要的包
     1. `spring-jdbc-4.0.0.RELEASE.jar`
     2. `spring-orm-4.0.0.RELEASE.jar`
     3. spring-tx-4.0.0.RELEASE.jar

   - Spring 日志工具
     1. `commons-logging-1.2.jar`
   - 数据库驱动和连接池
     1. `druid.jar`
     2. `mysql-connector-java.jar`

3. 配置文件

   - 将 `JdbcTemplate` ，交给 Spring 管理
     1. `<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate"></bean>`

### 简单实例一

1. XML 配置文件

   - 将 `JdbcTemplate` 类，交给 Spring 的 IOC 进行管理

     1. 属性为数据源
     2. 如何进行赋值，创建 `DruidDataSource`

   - 数据源使用 `druid` ，将 `DruidDataSource` 交给 Spring 管理

     1. 必要属性：驱动、`url` 、用户名、密码
     2. 如何进行赋值，使用外部资源文件

   - 在 XML 中引用外部资源 `druid.properties`

   - 配置文件实例

     1. `.properties`

        ```properties
        jdbc.driver=com.mysql.cj.jdbc.Driver
        jdbc.url=jdbc:mysql://localhost:3306/java
        jdbc.username=root
        jdbc.password=123
        ```

     2. XML

        ```xml
        <!-- 引入 druid 资源文件 -->
        <bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
            <property name="location" value="druid.properties"></property>
        </bean>
        
        <!-- 引入 context 命令空间 -->
        <!-- <context:property-placeholder location="druid.properties"/> -->
        
        <!-- druid 创建的数据源 -->
        <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
            <property name="driverClassName" value="${jdbc.driver}"></property>
            <property name="url" value="${jdbc.url}"></property>
            <property name="username" value="${jdbc.username}"></property>
            <property name="password" value="${jdbc.password}"></property>
        </bean>
        
        <!-- 根据 druid 配置 JDBCTemplate -->
        <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
            <property name="dataSource" ref="dataSource"></property>
            <property name=""></property>
        </bean>
        ```

2. 从 IOC 容器获取 `JdbcTemplate` 类

   - 使用 `update(String sql)` 更新

   - 使用 `update(String sql, Object... args)` 更新，占位符 `?`

   - 代码

     ```java
     ApplicationContext ac = new ClassPathXmlApplicationContext("jdbc.xml");
     
     JdbcTemplate jdbcTemplate = ac.getBean("jdbcTemplate", JdbcTemplate.class);
     
     @Test
     public void testUpdate() {
         //		System.out.println(jdbcTemplate);
     
         //		String sql = "insert into emp values(null, 'zhangsan', 12, 'man')";
         //		jdbcTemplate.update(sql);
     
         String sql = "insert into emp values(null, ?, ?, ?)";
         jdbcTemplate.update(sql, "lisi", 23, "feman");
     }
     ```

### 批量操作

1. 数据库的批量操作

   - SQL 语句

     ```sql
     insert into table values(value1, value2),(value3, value4),(value5, value6);
     ```

2. `JdbcTemplate` 批量操作

   - 使用 `batchUpdate(String sql, List<Objct[]> list)`
     1. `sql` 跟新的数据库 SQL 语句
     2. `list` 每次参数为一个 `Object` 数组
     
   - 测试
   
     ```java
     @Test
     public void testBatchUpdate() {
         String sql = "insert into emp values(null, ?, ?, ?)";
         List<Object[]> list = new ArrayList<>();
         list.add(new Object[] {"wangwu", 12, "man"});
         list.add(new Object[] {"zhaoliu", 25, "feman"});
         
         jdbcTemplate.batchUpdate(sql, list); // 执行批量操作
     }
     ```

### 查询操作

1. 查询单条实例或单例（字段）
   
- 查询单例
  
     1. `queryForObject(String sql, Class<T> requiredType)`
     
     2. 测试（不能使用基本数据类型）
     
        ```java
        @Test
        public void testqueryForObject() {
            String sql = "select count(*) from emp";
            Integer count = jdbcTemplate.queryForObject(sql, Integer.class);
            System.out.println(count);
        }
        ```
     
     - 查询单一实例 
     
     1. `queryForObject(String sql,Object[] args, RowMapper<T> rowmapper)`
     
     2. 测试
     
        ```java
        @Test
        public void testqueryForObject() {
            String sql = "select eid, ename, age, sex where eid = ?";
            RowMapper<Emp> rowMapper = new BeanPropertyRowMapper<>(Emp.class);
            Emp emp = jdbcTemplate.queryForObject(sql, new Object[] {1}, rowMapper); // 执行
        }
        ```

2. 查询所有实例，返回对象值

   - SQL 语句

     1. `select eid, ename, age, sex from emp`

   - 使用 `query(String sql, RowMapper<T> rowmapper)`

     1. `RowMapper<T>` 接口可以完成查询后 __字段与属性映射（自动赋值）__
     - `org.springframework.jdbc.core.RowMapper` 
        - 实现类 `org.springframework.jdbc.core.BeanPropertyRowMapper`
   
   - 测试
   
     ```java
     @Test
     public void testQuery() {
         String sql = "select eid, ename, age, sex from emp";
         RowMapper<Emp> rowMapper = new BeanPropertyRowMapper<Emp>(Emp.class);
     
         List<Emp> list = jdbcTemplate.query(sql, rowMapper);
     }
     ```

### 事务

__说明：AOP 的重要应用__

#### 事务介绍

1. JDBC 事务操作流程
   - 获取数据库连接 `Connection` 对象
   - 取消事务自动提交
   - 执行操作
   - 正常完成操作，手动提交
   - 异常，事务回滚
   - 关闭相关资源
   
2. 事务嵌入到业务逻辑层中

3. __事务管理类介绍__

   - Spring 顶层类（抽象类） `org.springframework.transaction.support.AbstractPlatformTransactionManager`
   - Spring 原生 JDBC 事务管理类 `org.springframework.jdbc.datasource.DataSourceTransactionManager`
   - 持久层框架 `Hibernate` 事务管理类 org.springframework.orm.hibernate4.HibernateTransactionManager`

4. 将事务管理类交给 IOC 管理

   - 配置文件 

     ```xml
     <!-- 事物管理器，管理的是数据源的连接 Connection -->
     <bean id="dataSourceTransactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
         <property name="dataSource" value="dataSource"></property>
     </bean>
     
     <!-- 开启注解驱动，即对事物有关的注解扫描，解析含义执行功能 -->
     <tx:annotation-driven transaction-manager="transactionManager"/>
     ```

   - 其中 `transaction-manager="transactionManager"` 是默认值，没有什么实际意义，就是如果自定义的事务管理器 `id=transactionManager` 时，`<tx:annotation-driven>` 可以不写

5. 注解使用

   - 注解 `@Transactional` 
   - 说明：事务一般在业务逻辑层 `Service` ，而持久层 `dao` 是关于数据库操作的。
     1. 疑问：事务是数据库操作，为什么不在持久层中呢？
     2. 解答：持久层的方法都 __一次性__ 的操作数据库，事务是多个一次性的集合，所以在业务逻辑层调用 `dao` 层的方法
     3. 将事务注解 `@Transactional` 用在 `Service` 的业务方法中
   - __`@Transactional` 也可以使用在类上__
     1. 对类中，所有方法都有效果

#### @Transactional 的属性

__说明：事务在业务逻辑层书写__

1. __`propagation` 事务的传播行为（事务发生嵌套时）__
   - A 方法和 B 方法都有事务，A 中调用 B，则 A 将事务传播给 B，B 对事务的处理方式则是事务的传播行为
   - `@Transactional(propagation = Propagation.REQUIRED)` ：使用调用者事务 __默认情况__。
     1. 将会使用 A 的事务，要么全部执行，要么全不执行
   - `Propagation.REQUIRES_NEW` ：不使用调用者事务
     1. 将调用者事务挂起，执行 B 的事务
2. __`isolation` 事务的隔离级别（并发请求时）__
   - 读未提交：会发生脏读（针对字段，0）
   - 读已提交：会发生不可重复读（针对字段，2）
   - 可重复性：会发生幻读（针对一行数据，4）
   - 串行化：性能下降（8）
   - 可以不进行设置，默认情况和数据库隔离级别相同（mysql 默认为 __可重复读__）
3. __`timeout` 在事务强制回滚前最多可以执行（等待）的时间__
   - 需要估算出，处理请求的时间，将此时间设为强制回滚时间
   - `@Transactional(timeout=3)`
4. __`readOnly` 指定当前事务中一系列操作是否为只读__
   - 默认为 `false` 
   - 设置为 `true` 后，mysql 就会对请求访问数据，不加锁，提高性能
   - 当事务操作全部为只读操作时，可以设为 `true` ，不加锁
   - 当事务中有写和读操作时，语法上可以设置 `true` ，但是在 mysql 中修改数据不上锁，会出现问题

#### 事务回滚条件

__说明：会因为什么异常回滚或者不会因为发生什么异常回滚__

1. 会因为发生什么异常回滚
   - `rollbackFor` 类型 `class<>[]` 写异常 `类.class`
   - `rollbackForClassName` 类型 `String []` 写全限定类名
2. `noRollbackFor`
3. `noRollbackForClassName`

#### XML 事务配置

1. 事务与 AOP 关系

   - 事务是 AOP 的重要运行（日志也是），事务管理器是 Spring 已经封装完成的 AOP，在 `org.springframework.jdbc.datasource.DataSourceTransactionManage` 中
   - AOP 面向切面编程
     1. 切面
     2. 通知
     3. 切入点表达式

2. 事务管理器 `DataSourceTransactionManage` 与 AOP 对应关系

   - 事务管理器 --》切面
   - 业务逻辑层（Service） ，便是代理目标

3. XML 配置

   - 不需要开启注解驱动

     ```xml
     <!-- 开启注解驱动，即对事物有关的注解扫描，解析含义执行功能 -->
     <tx:annotation-driven transaction-manager="transactionManager"/>
     ```

   - 代码

     ```xml
     <!-- 引入 druid 资源文件 -->
     <!-- druid 创建的数据源 -->
     <!-- 根据 druid 配置 JDBCTemplate -->
     <!-- 事物管理器，管理的是数据源的连接 Connection -->
     <!-- 事务管理器："dataSourceTransactionManager" -->
     
     <!-- 配置事物通知 -->
     <tx:advice id="tx" transaction-manager="dataSourceTransactionManager">
         <tx:attributes>
             <!-- 在以配置好的切面表达式下，再次对事物设置以确定目标代理方法 -->
             <tx:method name="method" timeout="-1"/>
             <tx:method name="method"/>
         </tx:attributes>
     </tx:advice>
     
     <!-- 配置切入点表达式 -->
     <aop:config>
         <aop:pointcut expression="execution(定位到业务逻辑层)" id="pointCut"/>
         <aop:advisor advice-ref="tx" pointcut-ref="pointCut"/>
     </aop:config>
     ```

# SpringMVC

## SpringMVC 概述

### SpringMVC  介绍

1. 介绍
   - Spring 为展现层提供基于 MVC （model view controller，模型 视图 控制器）设计理念的优秀 Web 框架
   - SpringMVC 是通过 MVC 注解，让 POJO （Plain Ordinary Java Object，普通 Java 对象）成为处理请求的控制器，而无需实现任何接口
   - 轻量级、基于MVC 的 Web 层应用框架，偏前端而不是基于业务逻辑层
   - 支持 `REST` 风格的 URL 请求。`Restful`
   - 采用了松散耦合可插拔组件结构，比其他 MVC 框架更具扩展性和灵活性
2. 特点
   - 天生与 Spring 框架集成
   - 支持 `Restful` 风格
   - 进行简洁的 Web 层开发
   - 支持灵活的 URL 到页面控制器的映射
   - 容易与其他视图技术集成，如 Velocity，FreeMarker 等等
     1. 原因：因为模型数据不存放的特定 API 里，而是放在一个 Model 里（Map 数据结构实现，容易被其他框架使用，Map Java 的原生类型）
     2. 原因：灵活的数据验证、格式化和数据绑定，能使用任何数据对象进行数据绑定，不必实现特定框架 API
     3. __API ：应用核心接口__ ，不会使用 SpringMVC 特有的类型记录数据
   - 强大的异常处理、对静态资源的支持（`CSS`）

### 搭建 SpringMVC

1. 创建动态的 Web 项目

2. 导入 JAR 包
   - Spring 中的 JAR 包
     1. `spring-beans-4.0.0.RELEASE.jar`
     2. `spring-context-4.0.0.RELEASE.jar`
     3. `spring-core-4.0.0.RELEASE.jar`
     4. `spring-expression-4.0.0.RELEASE.jar`
     5. `spring-aop-4.0.0.RELEASE.jar`
     6. `spring-web-4.0.0.RELEASE.jar`
     7. `spring-webmvc-4.0.0.RELEASE.jar`
   2. Spring 日志工具
      1. `commons-logging-1.2.jar`
   3. 导入 JSTL 包
   
3. 配置 `web.xml`

   - 配置 __核心控制器 __ `DispatcherServlet` ，就是配置一个 `servlet`
   - 作用：会自动加载 SpringMVC 的默认位置和名称的配置文件
     1. 默认名称及位置 `WEB-INF/springMVC-servlet.xml`
     2. 理解：就是对应 `springMVC` 加一个 `-servlet.xml`

   - 实例

     1. `/` 表示只有请求才会被处理，访问页面（资源）是不会被处理

        ```xml
        <servlet>
            <servlet-name>springMVC</servlet-name>
            <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        </servlet>
        <servlet-mapping>
            <servlet-name>springMVC</servlet-name>
            <url-pattern>/</url-pattern>
        </servlet-mapping>
        ```

4. 配置 `springMVC-servle.xml`

   - 上面提到，springMVC 可以将一个普通类作为一个控制层

   - 添加命令空间 `context` 

   - 扫描组件，将加上 `@Controller` 注解的类，作为控制层

   - 配置视图解析器（命令前缀、后缀）

   - 实例

     ```xml
     <!-- 扫描组件，将加上 @Controller 注解的类作为 springMVC 的控制层 -->
     <context:component-scan base-package="test"></context:component-scan>
     <!-- 配置视图解析器 -->
     <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
         <!-- 命令前缀、后缀 -->
         <property name="prefix" value="/WEB-INF/"></property>
         <property name="suffix" value=".jsp"></property>
     </bean>
     ```

   - spring 加载 `springMVC-servlet.xml` 配置文件，会根据扫描组件的位置，找到控制组件

5. 创建普通类（POJO），加上 `@Controller` 注解，springMVC 将此类作为控制层，让其处理请求与响应

   - 创建方法（方法名最好与请求参数名相同，但是不绝对要求）

   - 加上 `@RequestMapping(value="请求参数")`

   - 实例

     ```java
     /**
      * 访问路径 localhost:8080/springMVC01/hello
      */
     @RequestMapping(value="hello")
     public String testHello() {
         System.out.println("success");
         return "success";
     }
     ```

   - __返回值，对应加上 `springMVC-servlet.xml` 配置的视图解析器，的前缀和后缀__

     1. `/WEB-INF/view/success.jsp` 跳转页面为此

   - 方法也可以设置参数，参数名，根据请求的参数值设置

     1. `localhost:8080/SpringMVC/hello?userName=admin&password=123`
     2. 参数可以设置 `public String testHello(String usernName, String password)`

   - 也可以设置参数类型为对象（`User`）

     1. 可以进行自动赋值

## @RequestMapping 

### @RequestMapping 介绍

1. 作用
   - 当请求路径与 `@RequestMapping(value="")` 的 value 一致时，则该注解所标注的方法即为处理请求的方法
2. 参数作用
   - 约束处理请求的规则
   - 属性越多，约束越严格
3. 标注位置
   - 可以放在 __方法__上
     1. 仅作用在此方法上
   - 可以方在 __类__ 上
     1. 作用类中全部方法
     2. 作用一般用于提取出类中方法 `@ResquestMapping()`的属性
     3. 一般在类上标注的只有 `value` 属性
   - 例如
     1. 此类中的方法全部是处理路径 `/test` 的请求，类中的方法每一个都写，比较麻烦，所以将此属性提取到类上 `@RequestMapping(value = "/test")` 
     2. __也可以作为命令空间，访问时在加一层__
4. __支持 Ant  路径风格__
   - Ant 3 种匹配分割
     1. `?` ：匹配文件名中的任意一个字符
     2. `*` ：匹配文件名中的任意一个或多个字符
     3. `*` ：匹配文件名中多层路径
   - 例如
     1. `user/*/a`
        - `user/b/a`
     2. `user/**/a`
        - `user/c/b/a` 
        - `user/a`

### @RequestMapping 属性

1. `method`
   - 处理请求的方式
     1. `GET` ：查询
     2. `POST` ：添加
     3. `DELETE` ：删除
     4. `PUT` ：修改
   - 类型（E）为枚举 `RequestMethod.` 就可以获取请求处理方式
   - `RESTFUL` 
     1. 请求路径相同，可以根据请求的处理方式不同，添加请求处理方法，这种写法，叫做 `RESTFUL`
     2. `@RequestMapping(value = "/test", method = RequestMethod.GET)` 处理 `GET`
     3. `@RequestMapping(value = "/test", method = RequestMethod.POST)` 处理 `POST`
2. `params`
   - 用来设置客户端传到服务器的数据，__支持表达式__
   - 例如
     1. `username` ：请求参数必须带有 `username`
     2. `!username` ：请求参数不能带有 `username`
     3. `username = admin` ：请求参数必须带有 `username ` 且必须等于 `admin`
     4. `username != admin` ：请求参数必须带有 `username ` 且必须不等于 `admin`
     5. `params = {"username", "age=12"}` ，请求参数必须带有 `username` 和 `age` ,且 `age` 等于 12
   - 类型为 `String[]` 
3. `headers` 
   - 将于请求头信息进行比对
   - 例如
     1. 设置 `heads = {"Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2"}` 
     2. 方法只会处理请求头包含此字符串的
     3. 如请求头的接受语言为 `Accept-Language: en;q=0.2,zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3` 方法将不会进行处理
   - 类型为 `String[]`

### 占位符

1. 获取 url 传递的参数

   - 以前传递参数 
     1. `localhost:8080/SpringMVC/test?id=1&username=root`
   - restful 传递参数
     1. `localhost:8080/SpringMVC/test/1001/root`

2. 如何获取 url 的参数（SpringMVC）

   - 以前

     ```java
     	@RequestMapping(value = "/test") 
     	public String test01(String id, String useranme) {return null;}
     ```

   - restful

     ```java
     @RequestMapping(value = "/test/{id}/{username}")
     public String test02(@PathVariable("id") Integer id, @PathVariable("username") String username) {return null;}
     ```

   - `@RequestMapping(value = "/test/{id}/{username}")` 获取 url 的参数值

   - `@PathVariable("id") Integer id` 获取 id 值，将其赋值给 `Integer id`

## RESTFUL(restful)

### 介绍

1. 参考地址
   - [参考地址](https://kb.cnblogs.com/page/186516/)
   - [参考地址](https://www.runoob.com/w3cnote/restful-architecture.html)
2. REST 是 `Representational State transfer` 的简称，意思是：资源表现层状态转换
3. 资源：网络实体，如照片、歌曲，使用 URI 表示出来
4. 表现层：把资源具体呈现出来的形式，可以使用 HTML、text 表现
5. 状态转换：基于 HTTP 的无状态，在客户端多次请求服务器时，服务器必须基于上次请求状态发生必要的状态转换
6. __在 HTPP 协议中，4 种操作方式 `GET/POST/PUST/DELETE` ，表示对资源的查询/创建/修改/删除__

### 类 HiddenHttpMethodFilte

1. 问题描述
   - `restful` 建议 4 种操作 `GET/POST/PUST/DELETE` 
   - 然而客户端（浏览器）只支持其中两个 `GET/POST` 
   - 倒是服务器 4 种操作全部支持
   
2. 解决问题思想
   - `GET` 继续使用 `GET`
   - 其他的操作方式全部改用 `POST` ，将 `POST` 方式操作设置参数，使用参数辨别其他 3 种操作
   - 相当于在客户端与浏览器增加一层过滤器，辨别操作方式
   
3. 实际操作
   - 使用过滤器，因为其执行顺序
     1. 监听器（项目启动时，执行）
     2. 过滤器（项目启动时，执行）
     3. Servlet（第一个人请求时，执行）
   - SpringMVC 提供了此类过滤器 `HiddenHttpMethodFilte`
   
4.  __`HiddenHttpMethodFilter` 实现过程__
   - 实际上就是一个过滤器（实现了 `Filter`）
   - 根据 请求方式是否为 `POST` 和参数 `_method` 是否有值，且值为什么，来做处理
     1. 不是 `POST` 不做处理，或者是 `POST` 但 参数 `_method` 长度为 0 也不做处理
     2. 封装 `HttpMethodRequestWrapper` 类，重写 `getMethod()` ，将其重写为获取 `_method` 参数值的方法
   
5. 在 `web.xml` 配置文件

   - 代码（过滤所有的请求 `/*`）

     ```xml
     <!-- 设置过滤器 -->
     <filter>
    <filter-name>HiddenHttpMethodFilter</filter-name>
         <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
     </filter>
     <filter-mapping>
         <filter-name>HiddenHttpMethodFilter</filter-name>
         <url-pattern>/*</url-pattern>
     </filter-mapping>
     ```
     
   
6. 在页面使用 `form` 表单的隐藏于提交 `<input type="hidden" name="_method" value="PUT">`

7. 控制层 `Servlet` 使用 `@Controller(method=RequestMethod.PUT)`

### 使用 Ajax 达到使用 `put`

## 处理请求数据

__说明：`inpput` 标签不写值，直接提交，处理器获取为空字符串；如果对应不上则为 `null`__

### 请求处理方法签名

1. SpringMVC 通过分析处理方法的签名（方法名 + 参数列表），将 HTTP 请求信息绑定在处理方法的响应形参上

2. SpringMVC 对处理器方法签名是宽松的，几乎可以按照喜欢的方式进行签名

3. 必要时可以对方法及方法的参数标注相应注解（`@PathVariable/@RequestParam/RequestHeader`）

4. SpringMVC 框架会将 HTTP 请求信息绑定在相应方法的形参上，并根据方法的返回值类型做出相应后续处理

5. `form` 表单实例（服务器如何获取数据）

   ```html
   <form action="param" method="post">
       username:<input type="text" name="username"><br>
       password:<input type="text" name="password"><br>
       age:     <input type="text" name="age"><br>
       province:<input type="text" name="province"><br>
       city:    <input type="text" name="city"><br>
       country: <input type="text" name="country"><br>
       <input type="submit" value="submit">
   </form>
   ```

### @RequestParam 注解

1. 普通方法（请求参数名与处理器方法形参名必须相同）

   - 参数数量可以不相同，但是参数名必须对象，__如果对应不上，则在形参在方法为 `null`__

   - 代码

     ```java
     @RequestMapping(value = "/param", method = RequestMethod.POST)
     public String param(String username, String password) {
         System.out.println("username:" + username + ", password:" + password);
         return "success";
     }
     ```

2. 使用 `@RequestParam()` 解决属性名和方法形参不同

   - 指定对应关系 
     1. `@RequestParam(value = "name") String username`
     2. 将属性 `name` 与形参 `username` 对应

3. 使用此注解属性和参数 __必须__ 匹配成功，否则报错

   - 修改为 `RequestParam(value = "name", required=false)` 不匹配成功也不报错
   - 默认为 `true`

4. 当匹配不成功时（形参为 `null`），设置默认值

   - `RequestParam(value = "name", required = false, defaultValue = "root")`
   - 分页时可以使用

### @RequestHeader 注解

1. 获取请求头信息

   - 和上一个注解用法相同

2. 获取请求头信息，对应方法形参

   - 代码

     ```java
     public String param(@RequestHeader(value = "Accept-Language") String acceptLanguage) {}
     ```

3. 必须匹配、默认值

4. 获取 Cookie 值

   - 代码

     ```java
     public String param(@RequestHeader(value = "Cookie")String cookie) {}
     ```

   - 获取的值为

     ```java
     JSESSIONID=61D08957906B53C23A4506B0C9B86D4B
     ```

5. __如何获取其中的 Cookie 中 JSESSIONID 的值__

   - 代码

     ```java
     public String param(@CookieValue(value = "JSESSIONID")String JSESSIONID) {}
     ```

   - 获取值

     ```java
     61D08957906B53C23A4506B0C9B86D4B
     ```

### 使用 POJO 作为参数对象

1. 使用 POJO 绑定请求参数值

2. SpringMVC 会按照请求参数名和 POJO 属性自动匹配，自动为该对象填充属性值，__支持级联属性，如 `dept.deptId`__

   - 属性名称必须一一对应

3. 级联赋值表单属性

   - 代码

     ```html
     <form action="param" method="post">
         username:<input type="text" name="username"><br>
         password:<input type="text" name="password"><br>
         age:     <input type="text" name="age"><br>
         province:<input type="text" name="address.province"><br>
         city:    <input type="text" name="address.city"><br>
         country: <input type="text" name="address.country"><br>
         <input type="submit" value="submit">
     </form>
     ```

4. 创建两个对象

   - User

     ```java
     private Integer id;
     private String username;
     private String password;
     private Integer age;
     private Address address;
     ```

   - Address

     ```java
     private String province;
     private String city;
     private String country;
     ```

5. 处理请求方法

   - 代码

     ```java
     public String param(User user) {}
     ```

### 使用 Servlet 原生 API 作为参数

1. MVC 的处理请求方法接受的 Servlet  API 哪些类型的参数

   - `HttpServletRequest`
   - `HttpServletResponse`
   - `HttpSession`
   - `java.security.Principal`
   - `locale` ：本地对象
   - `InputStream`
   - `OutputStream`
   - `Writer`
   - `Reader`

2. 将 Servlet API 的类型放置处理方法形参上，即可自动赋值

   - 代码

     ```java
     public String param(HttpServletRequest request) {
         request.getParameter("")
     }
     ```

### 设置作用域参数

__说明：SpringMVC 一共 3 种方法设置作用域（不包括 Servlet API）__

#### 原生 Servlet API

1. 使用 Servlet 原生的 APT 来设置作用域参数
   - `HttpServletRequest`
   - `HttpSession`

#### ModelAndView

1. SpringMVC 提供了 `ModelAndView` 类，用来装载传输数据和向哪里传输

   - 作用在 `request` 域中

   - 代码

     ```java
     @RequestMapping(value = "/param", method = RequestMethod.POST)
     public ModelAndView param() {
         ModelAndView mav = new ModelAndView();
         mav.addObject("username", "root");  // 向 request域中设置值
         mav.setViewName("success"); // 设置视图名称，实现页面跳转
         return mav; // 必须有返回值，以达到数据的传输
     }
     ```

2. 实现原理（源码）

   - 也是封装的 `request.getParameter()` 和转发

#### Map 同源的 ModelAndView

1. 写法虽然不同（更加简单），但是最后都会封装成 `ModelAndView` 对象

   - 代码

     ```java
     @RequestMapping(value = "/param", method = RequestMethod.POST)
     public String param(Map<String, Object> map) {
         map.put("username", "admin"); // 向 request 域中放值
         return "success"; // 转发
     }
     ```

#### Model 同源的 ModelAndView

1. 写法不同，但是最后都会封装成 `ModelAndView` 对象

   - 代码

     ```java
     @RequestMapping(value = "/param", method = RequestMethod.POST)
     public String param(Model model) {
         model.addAttribute("username", "admin"); // 向 request 域中放值
         return "success"; // 转发
     }
     ```

#### 总结

1. SpringMVC 设置域的参数，都是将 model 数据和 view 数据封装到 `ModelAndView` 中

## 视图解析

### SpringMVC 如何解析视图

1. 不论控制器返回的一个 `String/Model/view` 都会转换成 `ModelAndView` 对象，由视图解析器解析视图，然后进行页面__跳转__
2. 视图解析器：重要的两个接口
   - `View`
     1. 处理模型数据，实现页面跳转（转发、重定向）
3. View 视图子类类型
   - `InternalResourceView` ：转发视图
   - `JstlView` ：支持 JSTL 的视图，继承 `InternalResourceView`
   - `RedirectView` ：重定向视图
     1. 处理器返回 `success` 表示转发
     2. 处理器返回 `redirect:/index.jsp` 是重定向视图
4. 转发、重定向
   - 处理器返回 `redirect:/index.jsp`
     - 创建视图将此字符串分割、拼接得到 `项目名/index.jsp` 
     - 在执行 `response.sendRedirect()` 进行重定向
   - 处理器返回 `success` 
     - 结合配置文件中，定义的视图解析器的，前缀和后缀，进行拼接
     - 在进行转发（好像是，使用的 `request`）
     - __只有此种方法使用到了配置文件的视图解析器__
   - 处理器返回 `forward`
     - 待看

### 指定配置文件

1. 修改 SpringMVC 的默认配置文件路径以及名称

   - 之前
     1. 之前默认路径为 `/WEB-INF/` 
     2. 默认名称也是和 `<servlet-name>` 加 `-servlet.xml`

   - 修改

     1. 代码

        ```xml
        <servlet>
            <servlet-name>springMVC</servlet-name>
            <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
            <init-param>
                <!-- 此为 DispatcherServlet--> 的属性
                <param-name>contextConfigLocation</param-name>
                <!-- 默认路径为 src/conf/springMVC.xml-->
                <param-value>classpath:springMVC.xml</param-value>
            </init-param>
        </servlet>
        ```

     2. 根据 `<init-param>` 设置

     3. 资源配置文件编译后在类路径下 `src/springMVC.xml`

     4. 路径错误，则使用默认路径

2. 设置 `servlet` 加载时间

   - 默认为第一次访问时加载
   - 标签 `<load-on-startup>` 
     1. 正整数、（0、负整数）无效
     2. 整数有效，将 `servlet` 启动时间设为项目启动时
   - 多个 `servlet` 的值不能相同（值越小，优先级越高）

### 解决乱码问题

1. SpringMVC 的编码过滤器

2. 配置在 `web.xml` 第一个过滤器

   - 实例

     ```xml
     <!-- 编码过滤器-->
     <filter>
         <filter-name>CharacterEncodingFilter</filter-name>
         <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
         <init-param>
             <param-name>encoding</param-name>
             <param-value>UTF-8</param-value>
         </init-param>
     </filter>
     <filter-mapping>
         <filter-name>CharacterEncodingFilter</filter-name>
         <url-pattern>/*</url-pattern>
     </filter-mapping>
     ```

## 案例

### SpringMVC 的 `form` 标签

1. 引入标签库

   ```jsp
   <%@ taglib uri="http://www.springframework.org/tags/form" prefix="from" %>
   ```

2. 替代 `form`

   - 代码

     ```jsp
     <from:form action="" method=""></from:form>
     ```

3. 替换 `<input type="text" name="">`

   - 代码（`path` 就是 `name`）

     ```jsp
     <from:input path=""/>
     ```

4. 替换单选框 `<input type="radio" name="" value="">`

   - 代码

     ```jsp
     <from:radiobuttons path="" items="" itemValue="" itemLabel=""/>
     ```

   - `items`  是需要遍历的 `Map` 集合的，此 `Map` 集合是处理器放置在作用域中（例如 `request`）的。

     ```java
     Map<String, String> genders = new HashMap<>();
     genders.put("0", "男");
     genders.put("1", "女");
     map.put(genders); // 将其放在 request 域中
     ```

   - `itemValue` 是 `Map` 的 value 值，也是此单选框页面展示的内容

   - itemLabel` 是 `Map` 的 Key 值，是单选框在的 value

5. 替换下拉列表 

   - 原生

     ```jsp
     <select name="">
         <option value=""></option>
     </select>
     ```

   - 替换

     ```jsp
     <from:select path="" items="" itemValue="" itemLabel=""></from:select>
     ```

6. __SpringMVC 的 `form` 表单有自动回显功能（一出来就必须的有数据）__

   - 其中默认的回显数据从何而来呢？

     1. 回显对象的默认属性值为 `command`

     2. 从 `request` 中的 `command` 属性获得的，其属性值必须是一个对象

   - 所以需要在控制层的处理器，在 `request` 中放置以 `command` 属性，其值为对象（不能为 `null`）

     1. 使用使用 `map.put("command" , new Object());`

   - 自定义回显对象的属性名

     1. 设置的属性 ` modelAttribute`

        ```jsp
        <from:form action="" method="" modelAttribute="">
        ```

   - 当在添加、修改时，使用同一个页面显示数据时

     - 修改需要回显数据
       1. 需要使用回显对象（对象有值）
       2. 隐藏域需要有 `id` 回显，但页面不显示，使用 SpringMVC 的标签（可以回显）
       3. 隐藏域需要设置 `_method` ，使用原生的 `input` 没有回显功能
     - 添加不需要回显数据
       - 不需要回显数据，回显对象可以为空（对象的属性为 `null`）
       - 不要有 `id`
       - 不需要设置 `_method`
     - __综上判断页面是添加，还是修改跳转，使用回显对象的属性值是否为 `null`__

### 解决静态资源问题

1. 如页面引入 `css` 或者 js 的库时，springMVC 的核心处理器 `DispatcherServlet`也会将引入当成一次请求处理，因为无法找到满足此项处理的方法，所以，系统报错

2. 没有使用 springMVC 时，引入静态资源不会出现此类问题（当成一次请求），那是因为 Tomcat 有专门处理静态资源的处理器

3. 在 spingMVC 配置文件，配置 Tomcat 的 `DefaultServlet` 处理器。当开发人员配置的 `Servlet` 的 `url` 是 `/` 时，优先级大于 `DefaultServlet` ，因此先通过 `DispatcherServlet` 处理请求，如没有相应的处理器，则交给 `DefaultServlet` 处理

   - 样式

     ```xml
     <!-- Tomcat 默认处理器-->
     <mvc:default-servlet-handler/>
     <!-- MVC驱动 -->
     <mvc:annotation-driven></mvc:annotation-driven>
     ```

   - MVC 驱动功能强大：有待考察

   - 没有 `DefaultServlet` ，静态资源无法访问（因为 `DispatcherServlet` 无法处理）

   - 加上 `DefaultServlet` 后，客户端可以访问静态资源（但是其他页面无法访问），需要加上 MVC 驱动后，项目恢复正常

### springMVC 处理 JSON

1. 介绍

   - Json 有两种格式
     1. Json 对象 `{key:value, key:value}`
     2. Json 数组 `[value, value]`
   - 取值
     1. `对象.key`
     2. 循环遍历
   - Java 对象转换 Json
     1. bean 和 Map 转换 Json 对象
     2. List 转换 Json 数组

2. JavaScript 可以将 Json 自动住转成 Js 对象 ，Java 中可没有 Json 的数据形式，所以需要通过中间桥梁，可以转化 Json 对象的数据

   - 如，谷歌 `Gson` 、阿里 `fastJson` `Jackson`
   - __SpringMVC 可以将 Java 对象自动转成 Json，但是必须有 Jackson 的 JAR__

3. __`@ResponseBody`__

   - 返回处理器方法的返回值到页面上
   - 不会将处理器的返回值当作视图去解析

4. __处理 Json 步骤__

   - 开启 `MVC` 驱动

     1. SpringMVC 的配置文件

        ```xml
        <!-- MVC驱动 -->
        <mvc:annotation-driven></mvc:annotation-driven>
        ```

   - 导入 JAR 包

   - 在处理器方法上标注 `@ResponseBody` 注解

   - __与 Aajx 连用__

     1. 前端 Ajax 代码

        ```jsp
        <script type="text/javascript" src="${pageContext.servletContext.contextPath }/js/jquery-2.2.4.js"></script>
        <script type="text/javascript">
        	$(function() {
        		$("#button").click(function() {
        			$.ajax({
        				url:"test",
        				type:"post",
        				dataType:"json",
        				success:function(msg) {
        					for (var i in msg) {
        						alert(msg[i].id + ", " + msg[i].name);
        					}
        				}
        			});
        		});
        	});
        </script>
        
        <body>
            <input type="button" id="button" value="testAjax" >
        </body>
        ```

     2. 处理器处理

        ```java
        @RequestMapping(value = "/test")
        @ResponseBody
        public List<Student> test(String username) {
            System.out.println(username);
            List<Student> list = new ArrayList<>();
            list.add(new Student(1, "zhangsan", 19));
            list.add(new Student(2, "wangwu", 23));
            return list;
        }
        ```

### `HttpMessageConverter<T>`  接口

1. 作用（Spring 3.0 的新接口）
   - 负责将请求信息转换为一个对象（类型为 T）
   - 将对象（类型 T）输出为响应信息
2. 工作流程
   - 请求报文 -》 HttpInputMessage -》 HttpMessageConverter -》 Java 对象 -》 SpringMVC
   - SpringMVC -》 Java 对象 -》HttpMessageConverter -》HttpLOutputMessage -》 响应报文
3. 实现类
   - 一堆
4. __DispatcherServlet 默认装配 RequestMappingHandlerAdapter ，而 RequestMappingHandlerAdapter  默认装配了 HttpMessageConverter 的诸多实现类__

### 使用上述接口实现下载

1. 使用 `HttpMessageConverter<T>` 将请求信息转化并绑定到处理方法的形参上或获奖响应结构转化为对应的响应信息

2. Spring 提供了两种方法

   - `@RequestBody/@ResponseBody` 对处理方法进行标注
   - `HttpEntity<T>/ResponseEntity<T>` 作为方法的形参或返回值
   - `@RequestBody/@ResponseBody` 不需要成对出现

3. 当处理器方法使用 `@RequestBody/@ResponseBody` 或者 `HttpEntity<T>/ResponseEntity<T>` 时，Spring 首先根据响应头或者时请求头的 `Accept` 的属性选择匹配的 `HttpMessageConverter` ，进而根据参数类型和泛型过滤得到匹配的 `HttpMessageConverter`

4. 下载功能（返回一个实体类 `ResponseEntity<T>`）

   ```java
   @RequestMapping(value = "/down")
   public ResponseEntity<byte[]> down(HttpSession session) throws IOException {
   
       // 获取下载文件的路径 使用 session
       // 基于系统的路径
       String realPath = session.getServletContext().getRealPath("img");
       String filePath = realPath + "/" + "1.jpg";
   
       // 获取资源输入流
       InputStream is = new FileInputStream(filePath);
       // is.available() 获取资源流的大小
       byte[] b = new byte[is.available()];
   
       // 返回下载内容
       is.read(b);
       MultiValueMap<String, String> headers = new HttpHeaders();
       // 设置文件以附件下载
       headers.add("Content-Disposition", "attachment;filename=down.jpg");
       // 状态码 200
       HttpStatus statusCode = HttpStatus.OK;
   
       ResponseEntity<byte[]> entity = new ResponseEntity<byte[]>(b, headers, statusCode);
       return entity;
   }
   ```


### 上传文件

1. SpringMVC 将 `File` 进行再次封装成 `MultipartFile` 

   - Java 中只有 File 表示文件对象，进行再次封装必须使用 SpringMVC 定义的解析器，将 File 解析成 `MultipartFile`

2. 导包

   - `commond=fileupload`
   - `commons-io`

3. 配置解析器

   - SpringMVC 的配置文件

     ```xml
     <!-- File 解析器，id 固定不可以修改-->
     <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
         <!-- 设置编码格式，与页面保持一致-->
         <property name="defaultEncoding" value="UTF-8"></property>
         <!-- 设置最大上传大小，不支持表达式-->
         <property name="maxUploadSize" value="999999"></property>
     </bean>
     ```

   - 将 `File` 转换 `MultipartFile` ，此过程由 `multipartResolver` 解析器

4. `form` 的 `enctype="multipart/form-data"` 属性注意

5. 解决文件名重名问题

   - 使用 `UUID` 命名文件名

     1. 代码

        ```java
        finalFileName = UUID.randomUUID() + fileName.subString(fileName.lastIndex("."));
        ```

     2. `lastIndex(".")` 获取字符串最后一个 点`.` 的索引

     3. `subString(index)` ，从 `index` 开始截取（`.text/.jpg`）包括前面，不包括后面

6. 控制器处理方法

   - 代码

     ```java
     @RequestMapping(value="/up")
     public String upload(MultipartFile file, HttpSession session) throws IOException {
         // 获取表单元素的 name 属性
         //		String name = file.getName();
     
         // 获取上传文件的名称
         String filename = file.getOriginalFilename();
         // 获取上传文件的目录
         // File.separator 为系统文件分割副（/ 或者 \）
         String path = session.getServletContext().getRealPath("photo") + File.separator + filename;
     
         // 获取输入流
         InputStream inputStream = file.getInputStream();
         // 获取输出流
         OutputStream outputStream = new FileOutputStream(path);
     
         // 向目标文件写入数据
         int i = 0;
         while((i = inputStream.read()) != -1) {
             outputStream.write(i);
         }
     
         // 关闭流
         inputStream.close();
         outputStream.close();
         return "success";
     }
     ```

7. SpeingMVC 有更简单的写法

   - 代码

     ```java
     @RequestMapping(value="/up")
     	public String upload(MultipartFile file, HttpSession session) throws IOException {
     	
     		// 获取上传文件的名称
     		String filename = file.getOriginalFilename();
             
     		// 获取上传文件的目录
     		// File.separator 为系统文件分割副（/ 或者 \）
     		String path = session.getServletContext().getRealPath("photo") + File.separator + filename;
     		
     		// 创建 File 文件
     		File newFile = new File(path);
     		// 将 file 存储在 newFile 中
     		file.transferTo(newFile);
             
     		return "success";
     	}
     ```

8. 多个文件一起上传

   - 处理方法形参上使用 `MultipartFile[] file`
   - 方法体使用循环即可

## 拦截器

### 自定义拦截器

1. SpringMVC 可以使用拦截器对请求进行拦截处理，用户可以自定义拦截器来实现特定功能
   - 自定义拦截器可以实现 `HandlerInterceptor` 接口
     1. 3 个方法全部重写
   - 继承 `HandlerInterceptorAdapter`
     1. 可以随意实现 3 个方法中的任意一个
2. __过滤器与拦截器的不同__
   - __过滤器作用在请求和 `DispatcehrServlet` 之间__
   - __拦截器作用在 `DispatcherServlet` 调用处理器方法之间__
3. 方法功能
   - `preHandle()`
      	1. 这个方法在业务处理器请求之前调用，在该方法中对 `request` 进行处理
          - 已经进入方法中，只是处理器方法第一句还没有开始执行，`preHandler` 开始执行
         	2. 如果程序员决定该拦截器处理后还需要调用其他拦截器，或者是业务处理器进行处理，则返回 `true`
         	3. 如果不在调用其他组件处理请求，则返回 `false`
   - `postHandle`
     1. 这个方法在业务处理器请求之后调用，但在 `DispatcherServlet` 响应之前调用，在该方法中对 `request` 进行处理 
        - 已经完成了对视图的解析，和对 `ModelAndView` 的装配，就是没有向客户端发送
   - `afterCompletion`
     1. 这个方法在 `DispatcherServlet` 处理完请求之后调用
        - 相当于 AOP 的后置通知 `finally{}` 中执行，无论如何都会执行
     2. 该方法可以对资源清理的操作
   - __如果处理器的方法发生异常，只有 `postHandler` 不会执行__

### 拦截器实例

1. 继承 `HandlerInterceptorAdapter` 

2. 注解 `@Complement`

3. SpringMVC 定义了一个拦截器

4. 配置文件

   - 使用注解和不使用注解 __拦截所有请求__

     ```xml
     <mvc:interceptors>
         <!-- 默认拦截所有请求 -->
         <!-- 不是用注解 -->
         <bean class="model.crud.interceptor.FirstInterceptor"></bean>
         
         <!-- 使用注解 -->
         <ref bean="firstInterceptor"/>
     </mvc:interceptors>
     ```

   - 拦截部分请求（在大的部分，排除个别请求）

     ```xml
     <mvc:interceptors>
         <mvc:interceptor>
             <!-- 拦截器-->
             <bean></bean>
             <!-- 拦截什么请求-->
             <mvc:mapping path=""/>
             <!--在拦截请求中，排除什么请求-->
             <mvc:exclude-mapping path=""/>	
         </mvc:interceptor>
     </mvc:interceptors>
     ```

5. 多个拦截器的执行顺序

   - 拦截器定义，按上面的配置文件配置

     ```xml
     <!-- 第一个拦截器-->
     <bean class="first"></bean>
     <!-- 第二个拦截器-->
     <bean class="second"></bean>
     ```

   - 执行顺序__（重要）__

     ```tex
     first:preHandle
     second:preHandle
     second:postHandle
     first:postHandle
     second:afterHandle
     first:afterHandler
     ```

6. __多个拦截器的 `preHandle()`返回值不同是，执行的效果（`true/false`）__

   - 两个拦截器的 `preHandle()` 都返回 `false` 时
     1. 只会执行 First 的 `preHandle`
     2. 其他的两个方法都不执行
- First 的 `preHandle()` 返回 `true`，Second 为 `false`
       1. First 和 Second （全部）的 `proHandle()` 都会执行
      2. 俩个（全部）的 `postHandle()` 都不会执行
      3. 在 Second 拦截器前面执行的 First 拦截器，会执行 `afterHandle()` 方法
   - First 的 `preHandle()` 返回 `false`，Second 为 `true`
       1. 只有第一个的 `preHandle()` 会执行

## 异常处理

### 异常概述

1. 异常处理概述
   - SpringMVC 通过 `HandlerExecptionResolver` 程序处理异常，包括 `Handle` 映射、数据绑定以及目标方法执行时发生的异常
   - SpringMVC 提供的 `HandleExecptionReslover` 的实现类
2. SpringMVC 默认的异常处理类 `DefaultHandlerExecptionResolver`
   - 帮助处理一些异常（查看源码，就可以查看默认的异常处理可以帮助我们处理什么异常信息）

### 异常处理 `SimpleMappingExecptionResolver`

1. 如果希望对所有异常进行统一处理，可以使用 `SimpleMappingExecptionResolver` ，他将异常类名映射为视图名，即发生异常时使用对应的视图报告异常

2. 实例

   - SpringMVC 的配置文件

     ```xml
     <bean id="SimpleMappingExceptionResolver"
           class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
         <!-- Properties Map(key,value) -->
         <property name="exceptionMappings">
             <props>
                 <!-- 异常权限类名 -->
                 <prop key="java.lang.ArithmeticException">error</prop>
             </props>
         </property>
     </bean>
     ```

   - __`error` 时视图名称（视图解析器可以解析到的地方）__

3. 异常处理会将异常信息默认存放在 `request` 域中

   - `${ResquestScope.exception}` EL 表达式

## SpringMVC 的运行原理

### 运行流程

1. 运行流程图
   - 如图
2. 总结

## 整合 Spring 和 SpringMVC

### 不整合

1. SpringMVC 只是封装了 `Servlet` ，不适合管理数据源等……

### 整合

1. 在 Web 项目下，创建 Spring 的配置文件

   - 如何将其关联呢？将 Spring 的容器生效，必须在 `Servlet` 加载之前执行，因为 SpringMVC 是封装了 `Servlet` 的 `DispatcherServlet` 。

   - `Servlet` 依赖于 `Service` 层，所以在 `Servlet` 生效之前，`service` 必须生效

   - `Service` 是 Spring 管理的，所以创建容器即可，是其中的对象创建装载
   
     ```java
  // 创建容器 Application
     ```

2. 使用监听器加载 Spring 的配置文件，创建容器

   - __过滤器，每过滤一次请求，就是创建一次spring 的容器（创建一次容器中的对象），不满足__
   - __监听器是项目启动时执行，监听事件（某一个动作），满足要求__
     1. 如，可以用来监听 `Servlet context` 的生命周期，`Servlet Context` 是在项目启动时创建，满足要求
     2. `Servlet context` 创建时加载配置文件（创建 Spring 容器）

3. 模拟监听（Spring 已经自带了）

   - 创建监听器，用来监听 `ServletContext` 生命周期

     1. 在监听 `ServletContext` 初始化的方法中，创建 Spring 容器

        ```java
        /**
             * @see ServletContextListener#contextInitialized(ServletContextEvent)
             */
        public void contextInitialized(ServletContextEvent arg0)  { 
            // 创建容器
        }
        ```

4. 配置 `web.xml` 的监听配置文件

   - 自动配置

     ```xml
     <listener>
         <listener-class>model.crud.listener.ListenerServletContext</listener-class>
     </listener>
     ```

5. SpringMVC 和 Spring 扫描的组件，不要重复扫描（重复扫描会创建两次 bean）

   - 使用包含排除
   - SpringMVC 扫描 `Servlet` 包，SpringMVC 按注解扫描 `@Controller`
   - Spring 扫描，`Service` 和 `bean` 和 `dao` 层即可

6. 监听器，不止是只创建容器即可。有些对象不只是扫描组件就可以创建的

   - 如数据库事件
   - 数据库连接池（druid）

7. 正常的操作流程

   - 使用监听器创建 Spring 容器，但是如何获取容器中的对象呢？

   - 将容器放入 `ServletContext` 中

   - 在控制层（形参）通过 `HttpSession` 获取 `ServletContext` 

   - 然后通过 `ServletContext` 获取其中的容器，在做进一步处理

   - 代码

     ```java
     /**
          * @see ServletContextListener#contextInitialized(ServletContextEvent)
          */
     public void contextInitialized(ServletContextEvent arg0)  {
         ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml");
         ServletContext servletContext = arg0.getServletContext();
         // 将容器存放 ServletContext
         servletContext.setAttribute("applicationContext", ac);
     }
     ```

### 使用 SpringMVC 自带的监听器，加载 Spring 容器

__说明：在 `web.xml` 中，如下的创建，又快捷方式__

1. 在 `web.xml` 的配置文件，配置监听器

   - 代码

     ```xml
     <listener>
         <listener-class> org.springframework.web.context.ContextLoaderListener</listener-class>
     </listener>
     ```

   - 这样写，监听器回去寻找默认的 Spring 配置文件（`WEB-INF/applicationContext.xml` 配置文件）

   - 修改 Sping 的配置名称和位置

     ```xml
     <context-param>
         <param-name>contextConfigLocation</param-name>
         <param-value>classpath:spring.xml</param-value>
     </context-param>	
     ```

   - 修改为类路径下的 `spring.xml` 文件

2. `web.xml` 中标签的执行顺序

   - `<context-param>`
   - `<listener>`
   - `<filter>`
   - `<servlet>`

















 