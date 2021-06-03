# Spring Boot 笔记

## Spring Boot 入门

### 原理介绍

1. Spring Boot 简化 Spring 应用（Spring、SpringMVC、Mybatis 整合）开发（约定大于配置）
2. Spring Boot 优点
   - 快速创建独立运行的 Spring 项目以及与主流的框架继承
   - 使用嵌入式的 Servlet 容器，应用无需打成 WAR 包
   - starters 自动依赖与版本控制
   - 大量自动配置，简化开发，也可以修改默认配置
   - 无需配置 XML，无代码生成，开箱即用
   - 准生产环境的运行时应用监控
   - 与云计算天然集成
3. 缺点
   - 入门简单，精通难
4. 微服务
   - 本身是一种架构风格
   - 一个应用应该是一组小型的服务，可以通过 HTTP 协议进行通信
   - 每一个功能元素，都可以独立替换和独立升级的软件单元
   - [参考文档](https://baijiahao.baidu.com/s?id=1600354904549354089&wfr=spider&for=pc)

### 开发工具以及环境

1. 工具名称以及版本

   - IDEA 2017

2. 环境

   - 如下表

     | 名称       | 版本  |
     | ---------- | ----- |
     | JDK        | 1.8   |
     | Maven      | 3.x   |
     | SpringBoot | 1.5.9 |

3. IDEA 设置成使用本地 maven，配置 `setting.xml` 配置文件

### 快速开始

1. 创建 maven 项目

   - 导入相关依赖

     ```xml
     <!--继承父工程-->
     <parent>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-parent</artifactId>
         <version>1.5.9.RELEASE</version>
     </parent>
     <!--导入依赖-->
     <dependencies>
         <dependency>
             <groupId>org.springframework.boot</groupId>
             <artifactId>spring-boot-starter-web</artifactId>
         </dependency>
     </dependencies>
     
     <!--此插件可以将应用打包成一个可执行JAR-->
     <build>
         <plugins>
             <plugin>
                 <groupId>org.springframework.boot</groupId>
                 <artifactId>spring-boot-maven-plugin</artifactId>
             </plugin>
         </plugins>
     </build>
     ```

2. 创建主程序类

   - 使用 `@SpringBootApplication` 注解，表示此类位主程序类

     1. 代码

        ```java
        /**
         * @SpringBootApplication 标识此类为主程序类(说明：这是一个SpringBoot 应用)
         */
        @SpringBootApplication
        public class HelloWorldApplication {
        
            public static void main(String[] args) {
        
                // SpringBoot 应用启动
                SpringApplication.run(HelloWorldApplication.class, args);
            }
        }
        ```

3. 创建控制层

   - 使用 `@Controller` 注解

     1. 代码

        ```java
        /**
         * 表示是控制层
         */
        @Controller
        public class HelloController {
        
            /**
             * 注解：使用 return 返回数据给浏览器；表示处理请求的类型
             * @return
             */
            @ResponseBody
            @RequestMapping(value = "/hello")
            public String hello() {
                return "hello world！";
            }
        }
        ```

4. 运行项目

   - 在主程序类中运行主函数即可
   
5. 打包

   - 安装插件
   - 将其打包成 JAR，直接可在系统上运行（内置 Tomcat 服务）

6. __总结__

   - 在完成上述的测试实例时，没有过多的配置其他的内容
   - 只需要完成逻辑的编写即可

### 解析 POM 文件

1. 父项目

   - 实例

     ```xml
     <!--继承父工程-->
     <parent>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-parent</artifactId>
         <version>1.5.9.RELEASE</version>
     </parent>
     
     <!--再继承父工程-->
     <!--此工程是真正管理项目的版本依赖的-->
     <parent>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-dependencies</artifactId>
         <version>1.5.9.RELEASE</version>
         <relativePath>../../spring-boot-dependencies</relativePath>
     </parent>
     ```

   - 作用

     1. “爷爷” 项目的版本管理中心已经管理的版本不需要在进行写版本号（没有 `dependencies` 的版本依然还需要写版本号）

2. 导入启动器

   - 实例

     ```xml
     <!--依赖导入-->
     <dependencies>
         <dependency>
             <groupId>org.springframework.boot</groupId>
             <artifactId>spring-boot-starter-web</artifactId>
             <!--没有版本号，是他的父项目进行了版本的管理-->
         </dependency>
     </dependencies>
     ```

   - `spring-boot-starter-web` 为 `web` 启动器导入 web 需要的模块

     1. `spring-boot-starter` 为 `sprinp-boot` 场景启动器
     2. `spring-boot` 有很多这样的启动器
     3. `spring-boot` 将根据功能使用的场景提取出许多启动器，后续更具项目的需要导入不同场景的启动器

### 主程序类

1. 注解 `@SpringBootApplication` 标识此类为 Spring Boot 应用的主程序类

   - SpringBoot 运行此类的 `main`· 方法来启动 SpringBoot 应用

2. `@SpringBootApplication` 为组合式注解

   - 源码

     ```java
     @Target({ElementType.TYPE})
     @Retention(RetentionPolicy.RUNTIME)
     @Documented
     @Inherited
     @SpringBootConfiguration
     @EnableAutoConfiguration
     @ComponentScan(
         excludeFilters = {@Filter(
         type = FilterType.CUSTOM,
         classes = {TypeExcludeFilter.class}
     ), @Filter(
         type = FilterType.CUSTOM,
         classes = {AutoConfigurationExcludeFilter.class}
     )}
     )
     public @interface SpringBootApplication {
     ```

   - 注解解析

     1. `@SpringBootConfiguration` 为 Spring Boot 的配置类（复合注解）

        - 标识该类为 Spring Boot 的配置类
        - 底层使用 `@Configuration` 配置类
        - 配置类等价于配置文件，配置类也是容器中的组件

     2. `@EnableAutoConfiguration` 开启自动配置

        - 通知 Spring Boot 开启自动配置功能

        - 进一步

          ```java
          @EnableAutoConfiguration
          	@AutoConfigurationPackage
          		@Import(Registrar.class)
          ```

          1. 底层注解 `@Import` ，向容器中导入一个组件，组件由 `Registrar.class` 所决定

          2. `@AUtoConfigurationpackage` 作用，是将主配置类所在包以及所有子包里面的所有组件扫描到 Spring 容器中

        - 进一步

          ```java
          @EnableAutoConfiguration
          	@Import({EnableAutoConfigurationImportSelector.class})
          ```

          1. `@Import` 向容器中导入组件
          2. 会添加哪些组件以及组件的作用：会向该容器中导入此场景需要的组件并自动配置完成
          3. 自动配置类会完成注入功能组件和配置的工作
          4. `SpringFactoriesLoader.loadFactoryNames(EnableAutoConfiguration.class, classLoader);` 作用
             - `AutoConfigurationImportSelector` 类的方法
             - `loadFactoryNames` 方法：从类路径下获取 `"META-INF/spring.factories"` 资源信息

3. 总结

   - Spring Boot 启动时从类路径下的 `MEAT-INF/spring.factories` 中获取 `EnableAutoConfiguration` 指定的值，将这些值作为自动配置类导入容器中，自动配置类就可以生效。
   - 之前的手动配置类，Spring Boot 帮忙做了


### 使用 Spring Initializer 快速创建 Spring Boot 项目

1. 使用 IDE 快速创建 Spring Boot 项目

   - 创建新项目使用 `Spring Assistant` 
   - 对 Maven 的设置（修改 maven 的 gav 和 包名的设置）
   - 删除不必要的文件
   - 对 `pom.xml` 文件进行比对
     1. 与手动项目的配置文件相比，对出测试的依赖
   - 多出一个文件夹 `src/main/javaresources` 

2. `resources` 文件夹

   - 内容

     ```java
     resources
     	|- static 				 // 保存所有的静态资源（js\css\images）
     	|- templates			 // 保存所有模板页面
     	|- application.properties //  spring boot 应用配置
     ```

   - `resources/templates` 

     1. 保存所有静态页面，spring boot 默认的Jar 包使用内嵌的 Tomcat，默认不支持 JSP 页面，所以使用模板引擎（freemarker\thymelelead）

   - `resources/application.properties` 

     1. 修改配置：修改端口 `server-port=8089`

## Spring Boot 配置

### 配置文件

1. Spring Boot 的全局配置文件（两个都可以）

   - `application.properties`
   - `application.yml` 
   - 位置 `resources/application.properties` 

2. 作用

   - 修改 Spring Boot 的配置的默认值（Spring Boot 底层将配置自动化）

3. YAML 类型的配置文件

   - 会以数据为中心，比 Json 和 Xml 更适合作为配置文件类型

   - 书写方式

     1. YAML(以数据中心)

        ```yaml
        server:
        	port:8080
        ```

     2. XML

        ```xml
        <server>
        	<port>8080</port>
        </server>
        ```

### YAML 语法

1. YAML 基本语法

   - 使用缩进表示层级关系
   - 缩进是不允许使用 `tab` 键，只允许使用空格键
   - 缩进空格数目不重要，只要相同层间的元素左对齐即可
   - 对大小写敏感

2. YAML 支持三种数据结构

   - 对象：键值对的集合
   - 数组：一组按次序排列的数组（List、Set）
   - 字面量：单个的、不可再分的值（数字、字符串、布尔）

3. 注意特点

   - `""` 不会转义特殊字符 ； `''` 会转移特殊字符

   - 值是对象的书写方式，`: ` 有空格

     1. 方式一

        ```yaml
        studnet:
            name: lisi
            age: 20
        ```

     2. 方式二（行内书写）

        ```yaml
        student: {name: lisi,age: 20}
        ```

   - 数组书写，`空格-空格cat`

     1. 方式一

        ```yaml
        pets:
         - cat
         - dog
         - pig
        ```

     2. 方式二

        ```yaml
        pets: [cat,dog,pig]
        ```

   - 多个文档使用 `---` 隔开

### 配置文件`@ConfigurationProperties`属性注入

1. 根据注释，将在配置文件 `application.yaml 或者 application.properties` 中的属性，按照格式匹配与类中的属性匹配。前提容器提供的此功能，则需要此组件（java 类） 需要加入容器中

   - 注释 `@ConfigurationProperties(prefix = "配置文件中的属性")`
   - `@Component` 

2. 导入配置文件处理器

   - 作用：编写配置文件时，会与提示

     ```xml
     <!--导入配置文件处理器，配置文件进行绑定会有提示-->
     <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-configuration-processor</artifactId>
         <optional>true</optional>
     </dependency>
     ```

     

3. 组件的属性

   - 代码

     ```java
     /**
      * 将配置文件中配置的属性值，映射到该组件中
      * @ConfigurationProperties 作用：将本类中的属性与配置文件中相关的配置值属性值进行绑定
      *      prefix = "preson" ：配置文件中属性下面的所有值与类属性一一映射
      *
      * 只有该组件是容器中的组件，容器才可以提供 @ConfigurationProperties
      */
     @Component
     @ConfigurationProperties(prefix = "person")
     public class Person {
     
         private String name;
         private Integer age;
         private Boolean boss;
         private Date brith;
     
         private Map<String, Object> map;
         private List<Object> list;
     
         private Dog dog;
         ...
         ...
     
     ```

4. 配置文件属性值

   - 代码

     ```yaml
     person:
         name: lisi
         age: 18
         brith: 2017/9/18
         boss: false
         map: {k1: v1,k2: 12}
         list:
           - lisi
           - zhaoliu
           - wangwu
         dog:
             name: tom
             age: 2
     ```

5. 测试（自动装配）

   - 代码

     ```java
     /**
      * SpringBoot 单元测试
      */
     @SpringBootTest
     class DemoApplicationTests {
         // 自动注入
         @Autowired
         Person person;
     
         @Test
         void contextLoads() {
             System.out.println(person);
         }
     }
     ```

6. 输出结果

   - 代码

     ```shell
     Person{name='lisi', age=18, boss=false, brith=Mon Sep 18 00:00:00 CST 2017, map={k1=v1, k2=12}, list=[lisi, zhaoliu, wangwu], dog=Dog{name='tom', age=2}}
     ```

7. 使用 `.properties` 编写配置文件

   - 代码

     ```properties
     # 配置 Person
     person.name=张三
     person.age=18
     person.boss=true
     person.map.k1=v1
     person.map.k2=12
     person.list=1,2,3
     person.dog.name=jack
     person.dog.age=3
     ```

   - 效果与 `.yaml` 相同，不需要修改热河注释等……

     1. Linux 上中文乱码

### 使用 Spring 底层注解`@Value`属性注入

1. 与上诉方式相同都需要将组件加入容器中，容器才可以向其提供属性注入的方法

2. 在 `.xml` 文件中使用 `<bean><property></property></bean>` 加入容器中，和对属性进行赋值

   - 代码

     ```xml
     <bean>
     	<property name="name" value="zhagsan"></property>
     </bean>
     ```

   - 其中 `value="字面量["${从环境、配置文件中获取值}", "#{表达式}"]"`

   - 使用 `@Vaue()` 注解代替 `@ConfigurationProperties`

3. Java 类

   - `@Component` 将组件加入容器中，根据 `@Value()` 获取的值，对属性进行赋值
   - 使用 `application.properties` 配置文件，__没有中文乱码，你说怪不怪！！！（使用的 `application.properties` 配置文件）__

   - 代码

     ```java
     @Component
     //@ConfigurationProperties(prefix = "person")
     public class Person {
     
         @Value("${person.name}")
         private String name;
         @Value("#{2 * 2}")
         private Integer age;
         @Value("true")
         private Boolean boss;
         private Date brith;
     
         private Map<String, Object> map;
         private List<Object> list;
     
         private Dog dog;
     ```

4. 输出结果

   - 代码

     ```shell
     Person{name='张三', age=4, boss=true, brith=null, map=null, list=null, dog=null}
     ```

### `@Value` 与 `@ConfigurationProperties` 对比

1. 都可以使用 `.yaml 或者 。properties` 获取值

2. 功能对比

   - 如表

     | 功能                    | `@ConfigurationProperties` | `@Value`             |
     | ----------------------- | -------------------------- | -------------------- |
     | 注入个数                | 批量注入配置文件的属性     | 一个个指定注入的属性 |
     | 松散绑定                | 支持                       | 不支持               |
     | SpEL(表达式)            | 不支持                     | 支持                 |
     | JSR303                  | 支持                       | 不支持               |
     | __复杂类型数据（Map）__ | 支持                       | 不支持               |

3. JSR303 是对数据的校验

   - 如在属性加入注解

     ```java
     // 校验是否是合法的邮箱数据
     @Email
     private Sting email;
     ```

   - 不是合法的邮箱，则会直接报错

4. __`@Value` 使用场景__

   - 小范围的使用，建议使用 `@Value`

### 加载指定的资源文件 `@PropertySource`

1. 作用：加载指定的资源文件，而不是加载默认的配置文件 `application.yaml 或者 application.properties`

2. 理由：当所有的配置信息都写在默认的配置文件中时，配置文件的内容过于庞大不利于维护

3. 用法

   - 新建配置文件 `person.peoperties`

   - 代码

     ```java
     @PropertySource(value = {"classpath":person.properties}) // 读取指定的配置文件
     @Component // 加入容器中
     @ConfigurationProperties(prefix = "person") // 读取person之下的属性
     public class Person {
         private ...;
         ...
     }
     ```

### 导入 Spring 配置文件 `@ImportResource`

1. 作用：将 Spring 的配置文件生效

   - __自定义的配置文件，不能生效__

2. 将 Spring 的配置文件生效

   - 使用 `@ImportResource` 注解

   - 将其标注在配置类上

   - 如下

     ```java
     @ImportResource(locations = {"classpath:beans.xml"})
     @SpringBootApplication
     public class DemoApplication {
     
         public static void main(String[] args) {
             SpringApplication.run(DemoApplication.class, args);
         }
     
     }
     ```

3. 可将 `beans.xml` 配置的组件加载进容器中

   - 如下

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
     
         <bean id="helloService" class="com.sshen.springboot.service.HelloService"></bean>
     
     </beans>
     ```

#### SpringBoot 推荐方式

1. 使用配置类代替，Spring 的配置文件

   - __推荐全注解__

2. 创建配置类

   - 使用 `@Configuration`（`@SpringBootApplication` 是个复合注解，包含配置类注解）

   - 正常创建类，使用配置类注解即可

     ```java
     @Configuration
     public class AppConfig {
     }
     ```

3. 使用 `@Bean` 添加组件

   - 示例

     ```java
     /**
      * 表明当前类为配置类，代替配置文件
      */
     @Configuration
     public class AppConfig {
     
         /**
          * 将方法的返回值添加到容器中，组件 ID 默认为方法名
          * @return
          */
         @Bean
         public HelloService helloService() {
             return new HelloService();
         }
     }
     ```

   - __`@Bean` 标记在方法上，将方法的返回值添加到容器中，组件 ID 默认为方法名__

### 配置文件占位符

1. 配置文件占位符，`.yaml 或者 .properties` 都可以使用

2. 配置文件中可以使用随机数

   - 如下
     1. `${random.value}`
     2. `${random.int}`
     3. `${random.long}`
     4. `${random.int(10)}`
     5. `${random.int[1024,65535]}`

3. 属性配置占位符

   - 如下

     1. 实例

        ```properties
        app.name=Myapp
        app.description=${app.name} is a SpringBoot application
        ```

     2. 可以使用默认值，当属性值没有找到时

        ```properties
        ${app.name:Whoapp} # 默认值
        ```

     3. __如果没有找到相应的值（没有默认值）程序默认为 `null`__ 或者我i本身的值。

        - `Person` 类属性中没有 `parent` ，但是在配置文件中却使用 `${person.parent}` 结果为 `person.parent`  
        - `Person` 类属性中有 `name` ，在配置文件中没有于其配置，使用 `person.name` j结果为 `null`

### Profile 功能

1. 作用：

   - Pofile 是 Spring 对不同环境提供的不同配置功能的支持，可以通过激活指定的参数等快捷方式迅速切换环境

2. 用法

   - 多 Profile 文件
   - yaml 支持的多文档快方式
   - 激活指定 Profile

3. __多 Profile 文件__

   - 配置文件名需带有 profile 标识的（`.properties\.yaml` 都支持）`application-{profile}.yaml`
     1. `application-dev.yaml` 开发环境下的配置
     2. `application-prod.yaml` 生产环境下
   - 程序默认读取 `application.properties` 配置文件
   - 激活指定的配置文件，在著配置文件中
     1. `spring.profile.active=dev` 激活开发配置文件

4. 文档块（yaml 使用）

   - 使用 `---` 对 yaml 配置文件进行文档切分，不需要创建多个配置文件

     1. `Document1\Document2`

   - 使用实例

     ```yaml
     # 激活哪一个文档块
     spring:
         profiles:
             active: dev # 激活开发环境下
     ---
     # 开发环境下
     server:
         port: 8082
     spring:
         profiles: dev
     ---
     # 生产环境下
     server:
         port: 8083
     spring:
         profiles: prod
     ```

   - 还可以使用命令行模式

     1. `--spring.profiles.active=dev` 激活开发环境
     2. 命令含行优先级大于配置文件中的配置

   - 虚拟机参数

     1. `-Dspring.profiles.active=dev`

### 配置文件加载位置

1. SpringBoot 启动时，会扫描以下位置的 `application.properties` 或者 `application.yaml` 文件作为 Spring Boot de 默认配置文件
2. 默认配置文件位置
   - `file:./config/` ：`项目名/config/配置文件` ，文件夹 `config` 与 `pom.xml` 同级
   - `file:./` ：`项目名/配置文件` `配置文件` 与 `pom.xml` 同级
   - `classpath:/config/` ：`resource/config/配置文件`
   - `calsspath:/` ：`resource/配置文件`
3. 优先级
   - 上述是按照 __优先级从高到低__ 的顺序，所有位置的配置文件都会被加载，优先级高的将覆盖优先级低的内容
4. __SpringBoot 会从四个位置加载配置文件，优先级高的会覆盖优先级低的（相同配置），相反高优先级的没有的配置会从低的优先级读取使用__
5. __可以通过配置 `spring.config.location` 来改变默认配置__
   - 通过命令行的方式，优先级最高，与其他配置文件共同起作用，形成互补形式。
6. __项目的访问路径__
   - `server.context-path=/boot` 
   - `localhost:8080/boot/hello` 这样

### 外部配置加载顺序

1. SpringBoot支持外部配置文件
   - [参考地址](https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#boot-features-external-config)
   - 官方文档提供 17 种方式
2. 举例（优先级从高到低）
   - 命令行参数
     1. 打包成 JAR，执行 `java -jar 0.0.1.jar --server.port=8087`
   - JAR 包中的配置文件
     1. JAR 包外部的 `application-{profile}.properties 或者 application.yaml` ，带 `spring.profile` 配置文件
        - 与 JAR 包同级目录即可
     2. JAR 包内部的 `application-{profile}.properties 或者 application.yaml` ，带 `spring.profile` 配置文件
     3. JAR 包外部的 `application-{profile}.properties 或者 application.yaml` ，不带 `spring.profile` 配置文件
        - 与 JAR 包同级目录即可
     4. JAR 包内部的 `application-{profile}.properties 或者 application.yaml` ，不带 `spring.profile` 配置文件
     5. __总体思想__
        - 由 JAR 包外向内进行寻找
        - 优先加载带 `profile` ，再加载不带 `profile`

### 自定配置原理

1. [官方参考文档](https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#appendix)
   - SpringBoot 是 2.xxx 版本
   - 与以前版本对比，2 版本对函数的功能进行了进一步的细化，将以前的一个方法拆分为几个方法，所以有所不同。但是类的功能没有改变。
   
2. 自定配置过程
   - SpringBoot 启动时加载主配置类 `@SpringBootAppication` 注解标注的类，开启自动配功能 `@EnableAutoConfiguration` 注解
   - `@EnableAutoConfiguration` 注解，开启自动配置功能
     1. `@Import(EnableAutoConfigurationImportSelector.class)` ,`@Import()` 注解向容器导入组件，导入哪些组件由 `EnableAutoConfigurationImportSelector.class)` 决定
     2. 查看 `EnableAutoConfigurationImportSelector.class)` 的父类 `AutoConfigurationImportSelector`
     3. `AutoConfigurationImportSelector` 类方法 `List<String> getCandidateConfigurations()` 中的 `SpringFactoriesLoader.loadFactoryName()` 方法，参数为 `EnableAutoConfiguration.class`，此方法调用下列方法
     4. 查看 `SpringFactoriesLoader` 类的方法 `loadSpringFactories()` 方法， `getSpringFactoriesLoaderFactoryClass()` 为
        1. 此方法获取 `META-INF/spring.factories` 文件，将其转换成 `.properties` 配置文件
        2. 读取以 `org.springframework.boot.autoconfigure.EnableAutoConfiguration=` 的值，该值就是自动配置类
        3. 查看 `spring.factories` 文件可以了解，自动装配了哪些自动配置类
   
3. 配置原理

   - 以 `org.springframework.boot.autoconfigure.web.servlet.HttpEncodingAutoConfiguration,\ 为例解释自动配置原理

   - `HttpEncodingAutoConfiguration` 类的代码

     1. 全部注解

        ```java
        @Configuration(proxyBeanMethods = false)
        @EnableConfigurationProperties(ServerProperties.class)
        @ConditionalOnWebApplication(type = ConditionalOnWebApplication.Type.SERVLET)
        @ConditionalOnClass(CharacterEncodingFilter.class)
        @ConditionalOnProperty(prefix = "server.servlet.encoding", value = "enabled", matchIfMissing = true)
        ```

     2. 解释

        - 表示一个配置类，相当于配置文件，也可以给容器添加组件
        - 启用 `ConfigurationProperties` 功能（启用指定类的此功能），将配置文件中的值和 `@ServerProperties` 绑定起来。__（将 `ServerProperties.class` 类属性与配置文件的配置相互绑定，并添加到容器中）__
          1. `@ConfigurationProperties` 从配置文件中获取指定的值和 bean （类创建的对象）进行绑定（配置文件 `application.yaml\properties`）
          2. 所有在配置文件中配置的属性都可以在 `XxxProperties` 类中封装着，配置文件可以配置什么属性可以参考某个对应的这个属性类。
        - 根据条件判断，整个配置类是否要生效，与 Spring 底层 `@Conditional` 注解相同
          1. `@ConditionalOnWebApplication` 判断是否为 Web 应用
        - 判断是否包含某一个类
          1. `@ConditionalOnClass` JAR 包中是否包含 `CharacterEncodingFilter` 类
          2. `CharacterEncodingFileter` SpringMVC 中解决乱码的问题
        - 判断配置文案金中是否含有某一项配置
          1. `matchIfMissing=true` 如果不存在，也当其存在。

     3. 综合以上判断结果，决定配置类是否生效，如果生效则向容器中添加组件

        - 生效则向容器中添加组件

          ```java
          @Bean
          @ConditionalOnMissingBean
          public CharacterEncodingFilter characterEncodingFilter() {
              CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
              filter.setEncoding(this.properties.getCharset().name());
              filter.setForceRequestEncoding(this.properties.shouldForce(Encoding.Type.REQUEST));
              filter.setForceResponseEncoding(this.properties.shouldForce(Encoding.Type.RESPONSE));
              return filter;
          }
          ```

        - 像容器添加 `CharacterEncodingFilter` 对象，编码格式来自 `this.properties` 配置文件中

          1. `this.properties` 是和 `appication.yaml\properties` 已经产生映射，其映射步骤

4. __SpringBoot 核心__

   - SpringBoot 启动会加载当量的自动配置类
   - 根据功能需要查看 SpringBoot 默认的自动配置类
   - 了解自动配置类，都配置了哪些组件（根据需求在进行配置）
   - 根据自动配置类向容器中添加组件，会根据 `XxxProperties` 类中的属性，从配置文件中获取这些属性值

5. __自动配置过程__

   - `XxxAutoConfiguration` 自动配置类，会向容器中添加组件
   - `XxxPropeties` 类，封装配置文件中的属性值
     1. 配置文件的内容需根据 `XxxProperties` 来写
     2. `XxxProperties` 的属性值从配置文件中获取

### @Conditional 派生条件判断

1. 作用

   - `@Conditional` ，根据其指定的条件，判断是否需要进一步操作

2. 派生的 `@Conditional` 注解的种类

   - 如表

     | @Conditional 扩展注解            | 判断是否满足当前的指定条件                           |
     | -------------------------------- | ---------------------------------------------------- |
     | @ConditionalOnJava               | 系统的 Java 版本是否符合要求                         |
     | @ConditionalOnBean               | 容器中存在指定 Bean                                  |
     | @ConditionalOnMissingBean        | 容器中不存在指定 Bean                                |
     | @ConditionalOnExpression         | 满足 SpEL 表达式                                     |
     | @ConditionalOnClass              | 系统中指定的类                                       |
     | @ConditionalOnSingleCandidate    | 容器中只有一个指定的 Bean，或者这个 Bean 是首选 Bean |
     | @ConditionalOnProperty           | 系统中指定的属性是否有指定的值                       |
     | @ConditionalOnResource           | 类路径下是否存在指定的资源文件                       |
     | @ConditionalOnWebApplication     | 当前是 Web 环境                                      |
     | @ConditionalOnNotWwebApplication | 不是 Web 环境                                        |
     | @ConditionalOnOnJndi             | JNDI 存在指定项                                      |

3. 判断项目下使用了什么自动配置类

   - 开启 debug 模式
   - 在配置文件中 `debug=true` ，来控制太打印自动配置报告

## Spring Boot 与日志

### 日志的介绍

1. 日志分为接口与之实现

   - 如下

     | 日志接口                                                     | 日志实现                                         |
     | ------------------------------------------------------------ | ------------------------------------------------ |
     | ~~JCL（Jakarta Commons Logging）~~、SLF4j（Simple Logging Facade for Java）、~~jboss-logging~~ | Log4j、Log4j2、JUL（java.util.logging）、Logback |

   - 常用日志门面 `SLF4J`

   - 日志实现 `Logback`

2. SpringBoot 的实现方

   - 底层 Spring 底层使用 `JCL` （Commons logging 包）
   - __SpringBoot 使用 `SLF4j` 和 `logback`__

### SLF4j 使用

1. 开发时，日志记录方法调用。不应该直接调用日志实现类，而是调用日志接口层的方法

   - [参考地址](http://www.slf4j.org/manual.html)

2. 导入 `slf4j` JAR 包，和 `logback` JAR 包

   - 使用时类

     ```java
     import org.slf4j.Logger;
     import org.slf4j.LoggerFactory;
     
     public class HelloWorld {
         public static void main(String[] args) {
             Logger logger = LoggerFactory.getLogger(HelloWorld.class);
             logger.info("Hello World");
         }
     }
     ```

3. 日志配置文件

   - 虽然使用接口编程 `slf4j` ，但是日志的配置文件还是需要根据具体使用哪种日志实现类来进行配置
   - 例如使用 `logback` 日志，则配置文件就需要使用 `logback` 配置的格式进行配置

### 遗留问题

1. 什么是遗留问题
   - 系统使用 `slf4j + logback`
   - 但是一些框架使用其他的日志 ，例如 Spring 使用`Commons logging`，等等
2. 如何统一使用 `slf4j + logback`  呢
   - 以 Spring 为例，修改日志实现包 `Commons logging` 
   - 首先排除对 `Commons loging` 包的依赖，但是排除后，Spring 底层调还是会调用 `Commons logging`，所以会报错。
   - 如何解决保存，使用 `jcl-over-slf4j.jar` 代替 `Commons logging` ，其与 `Commons logging` 里面的类方法相同，作用是将 `Commons logging` 体替换
   - 调用 `jcl-over-slf4j.jar` 后，此 JAR 包，再调用 `slf4j` 包，`slf4j` 是接口所以就会调用 `logback`，这样就可以完成 __偷天换日__
3. [参考地址](http://www.slf4j.org/legacy.html)

### SpringBoot 日志关系

1. SpringBoot 的依赖

   - 哪个项目模块都需要依赖的

     ```xml
     <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter</artifactId>
         <version>2.3.4.RELEASE</version>
         <scope>compile</scope>
     </dependency>
     ```

   - 日志依赖的

     ```xml
     <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-logging</artifactId>
         <version>2.3.4.RELEASE</version>
         <scope>compile</scope>
     </dependency>
     ```

   - SpringBoot 的启动器为 `spring-boot-starter-logging`

2. SpringBoot 使用 `slf4j` 作为接口，`logback` 作为实现类

   - `logback` 的两个 JAR 包
     1. `logback-classic`
     2. `logback-core`
   - SpringBoot 将其他的日志包替换，使用以下的包替换（中间替换包）
     - `jul-to-slf4j`
     - `log4j-over-slf4j`
     - `jcl-over-slf4j`

3. 中间包

   - 中间包，使用包名，类名，都和要替换的日志包相同
   - __如果引用其他框架，一定将这个框架日志包移除掉__

4. 查看是否移除了 Spring 的日志包 `Commons-logging`

   - 实例

     ```xml
     这个版本的SpringBoot 没有看到排除日志的依赖
     ```

5. __总结__

   - __SpringBoot 能自动适配所有的日志，而且底层使用 `slf4j + logback`  的方式记录日志，引入其他框架是偶，只需要将这个日志框架排除掉。__

### SpringBoot 默认配置

1. SpringBoot 已经默认配置好了日志功能

2. 日志使用

   - 实例

     ```java
     // 记录器
     Logger logger = LoggerFactory.getLogger(类)；
     
     // 日志级别，由低到高
     logger.trace("xxx"); // 跟踪
     logger.debug("xxx"); // debug 信息
     logger.info("xxx"); // info 信息
     logger.warn("xxx"); // 警告信息
     logger.error("xxx"); // 错误信息
     ```

3. __SpringBoot 默认的日志级别是 `info` 级别__

4. 调整日志级别

   - 调整日志级别，书写配置文件 `application.properties`

     ```properties
     //logging.level.包名
     logging.level.com.sshen=trace
     ```

   - 将日志输出到指定的文件中（自动创建日志记录文件，默认与 `pom.xml` 同级，也可以指定完整的路径 `/home/shen/springboot.log`）

     ```properties
     logging.file=springboot.log
     ```

   - 解释 `logging.path` ，指定目录，输出到指定目录的 `spring.log`

     1. 同时设置了 `loging.path` 和 `logging.file` 时，`logging.file` 起作用
     2. `logging.path` 会自动创建文件夹，和 `spring.log` 文件
     3. 指定目录 `/spring/log` ，日志文件默认为 `spring.log` ，默认是在该磁盘的根你目录下

   - 日志输出格式

     1. `logging.pattern.file` 文件输出格式

     2. `logging.pattern.console` 控制台输出格式

     3. 日志格式

        ```xml
        日志输出格式
        	%d 表示日期格式
        	%thread 表示线程名
        	%-5level 级别从左显示5个字符宽度
        	%logger{50} 表示 logger 名字最长 50 个字符，否则按照点分割
        	%msg 日志消息
        	%n 换行符
        实例
        	%d{yyyy-MM-dd HH:mm.SSS} [%thread] %-5level %logger{50} - %msg%n
        ```

5. SpringBoot 日志默认配置文件 `logback`

   - 配置文件位置 `org/springframework/boot/logging/logback/base.xml`
   - 其文件依赖 `org/springframework/boot/logging/logback/default.xml` 文件

### 自定义 SpringBoot 日志配置文件

1. [参考地址](https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#boot-features-logging-format)

2. [自定义配置文件](https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#boot-features-custom-log-configuration)

   - 例如使用 `logback` 日志框架，如果使用自定义配置文件，只需要在类路路径下 `logback.xml` 或者 `logback-spring.xml` 即可加载自定义配置文件

   - 推荐使用 `logback-spring.xml`  配置文件，是由 SpringBoot 加载和解析，可以使用高级特性；如果直接使用 `logback.xml` 配置文件，直接会被日志框架识别

     1. 实例

        ```xml
        <springProfile name="staging">
            <!-- configuration to be enabled when the "staging" profile is active -->
            <!-- 特定条件下激活这段日志配置 -->
        </springProfile>
        ```

     2. `dev` 表示开发环境，`!dev` 非开发环境

### 切换日志框架

1. SpringBoot 默认是 `slf4j + logback` ，如果使用 `slf4j + log4j` 该如何使用呢？
2. 步骤（主要看步骤，实际上没有什么意义）
   - 将 `logback` 的包排除掉
     1. `logback-classic` 和 `logback-core`
   - 然后将 `log4j-over-slf4j` 排除掉（其本身是要替换 `log4j` 的）
     1. 其他的替换包不要排除
   - 加入 `log4j` 适配层的包 `slf4j-logj12`
     1. 适配层的包依赖 `log4j` 包
     2. `pom.xml` 依赖 `log4j` 包

## Spring Boot 与 Web 开发

### 介绍

1. 使用步骤
   - 创建 SpringBoot 的应用，选择需要的模块
   - SpringBoot 默认将这些场景默认配置完成，只需要在配置文件中少量配置信息
2. 自动配置原理
3. 创建 Web 工程
   - 正常创建即可

### SpringBoot 对静态资源的映射规则

1. SpringBoot 对 SpringMVC 的配置都在 `WebMvcAutoConfiguration` 类中

   - 查看资源映射规则（此函数定义规则）

     ```java
     @Override
     public void addResourceHandlers(ResourceHandlerRegistry registry)
     ```

   - 此类定义了静态资源的配置信息

     ```java
     @ConfigurationProperties(prefix = "spring.resources", ignoreUnknownFields = false)
     public class ResourceProperties {
         
     }
     ```

   - 所有的 `/webjars/**` ，都会去 `/META-INF/resources/webjars/` 寻找资源

     1.  `webjars` ：是以 jar 包的方式引入静态资源的
     2. [参考地址](https://www.webjars.org/)
     3. 静态资源：例如 `jquery` 或者是 `Bootstrap`

   - 添加静态资源 `jquery`

     - pom.xml 创建依赖

       ```xml
       <!--引入静态资源 jquery-->
       <dependency>
           <groupId>org.webjars</groupId>
           <artifactId>jquery</artifactId>
           <version>3.3.1</version>
       </dependency>
       ```

2. 访问路径

   - `localhost/8080/webjars/abc` 请求，会去 `/META-INF/resources/webjars/`  寻找 `abc`
   - 如果访问 `jquery` ，则 `localhost:8080/webjars/jquery/3.3.1/jquery.js` 访问

3. __`/**` 访问当前项目的任何资源时，在没有处理器处理的话，默认会去这几个文件夹下，寻找资源__

   - 寻找的位置

     ```java
     "classpath:/META-INF/resources/",
     "classpath:/resources/",  完整的路径是 resources/resources，与 static 目录同级
     "classpath:/static/", 
     "classpath:/public/",
     "/" 当前项目的根路径
     ```

   - 注意：创建项目时的目录如 `java` 和 `resources` 代表的根路径

   - 例如：将 `asserts` 目录的资源存放到 `resources/static` 目录下，访问路径为

     1. `localhost:8080/asserts/js/Chart.min.js` 就可以访问

4. 欢迎页的映射

   - 当然也在 `WebMvcAutoConfiguration` 中，方法名如下：

     ```java
     @Bean
     public WelcomePageHandlerMapping welcomePageHandlerMapping()
     ```

   - 欢迎页：静态资源文件夹下所有 `index.html` 页面，被 `/**` 映射，__（静态资源文件夹，就是上面说的五个路径）__ 

     1. 在 `resources/public/index.html` 

   - `localhost:8080/` 就会找 `index.html` 页

5. 设置图标（默认启动）

   - `**/favicon.ioc` 都是在静态资源文件下寻找

6. 配置自定义静态资源文件夹

   - `application.properties`D

     ```properties
     spring.resources.static-location=classpath:/hello/,classpath:/sshen/
     ```

### 导入 `thymeleaf` 模板引擎

1. 需要理由：

   - SpringBoot 使用 JAR 包的方式，使用嵌入式的 `Tomcat` ，所以无法使用 `Jsp` 
   - 所以需要使用模板引擎
     - `JSP`
     - `Velocity`
     - `Freemarker`
     - `Thymeleaf`

2. 模板引擎的作用

   - Template 模板（里面嵌入表达式）
   - Data 数据
   - TemplateEngine 模板引擎，根据模板表达式将数据填充到模板中，输出 `output`

3. SpringBoot 推荐的模板引擎 Thymelead

   - 引入模板引擎 [参考地址](https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#using-boot-starter)

   - SpringBoot 有模板引擎的场景

     1. `spring-boot-starter-thymeleaf`

     2. 引入依赖

        ```xml
        <!--引入thymeleaf依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        ```

4. 提高 Thymeleaf 版本

   - 注意 Thymeleaf 3 ，适配 `layout` 2 以上版本

   - 替换版本

     ```xml
     <properties>
         <thymeleaf.version>3.0.2.RELEASE</thymeleaf.version>
         <thymeleaf-layout-dialect.version>2.1.1</thymeleaf-layout-dialect.version>
     </properties>
     ```

### 使用 Thymeleaf

1. 根据自动配置原理，查看配置信息

   - `spring-boot-autoconfigur` 下的 `org` 下的 `thymeleaf` 中
   - `ThymeleafAutoConfiguration` 自动配置的组件
   - `ThymeleafProperties`  自动配置信息

2. 渲染

   - 将 `.html` 页面，放入 `claspath:templates/` Thymeleaf  就会自动渲染页面

   - 源码（前缀，后缀）

     ```java
     @ConfigurationProperties(prefix = "spring.thymeleaf")
     public class ThymeleafProperties {
     
     	private static final Charset DEFAULT_ENCODING = StandardCharsets.UTF_8;
     
     	public static final String DEFAULT_PREFIX = "classpath:/templates/";
     
     	public static final String DEFAULT_SUFFIX = ".html";
     ```


### Thymeleaf 语法

__说明：[参考地址](https://www.thymeleaf.org/)__

1. 创建 `success.html` 

   - 默认位置 `classpath:/templates/success.html`

2. 创建 `controller`

   - 代码

     ```java
         @RequestMapping(value = "/success")
         public String success() {
             // 会对字符串添加前后缀
             return "success";
         }
     ```

   - 根据规则拼接前后缀，返回客户端页面地址

     ```java
     classpath:/templates/success.html
     ```

3. 导入名称空间

   - 在 `.html` 中导入（有语法提示）

     ```html
     <!DOCTYPE html>
     <html lang="en" xmlns:th="http://www.thymeleaf.org">
     <head>
         <meta charset="UTF-8">
         <title>Title</title>
     </head>
     ```

4. 简单使用

   - 书写控制层的处理器 __（将数据放入 `request` 请求域中）__

     ```java
     @RequestMapping(value = "/success")
     public String success(Map<String, Object> map) {
     
         map.put("hello", "你好！");
         // 会对字符串添加前后缀
         return "success";
     }
     ```

   - 在 `.html` 中使用 `Thymeleaf` 模板引擎处理 `.html`

     - 代码

       ```html
       <!--将div中的内容替换为指定的值-->
       <div th:text="${hello}">
           这是显示文本内容区域
       </div>
       ```

     - 使用模板引擎处理的，`${hello}` 数据，将会代替 `这是显示文本内容区域` 的数据

     - 如果不使用模板引擎则会显示 `这是文本内容的区域` 数据

5. 语法规则

   - `th:text`  改变当前元素的文本内容

     1. 可以使用 `th:htm任何属性` 作用是改掉 `html属性` 的值
     2. `th:id="${}"` 改掉元素 id
     3. `th:class="${}"` 改掉类

   - 表达式语法

     1. `${}` 获取变量值（OGNL 用法）

        - 获取对象属性
        - 调用方法
        - 可以使用内置的基本对象（内置对象）和内置的工具类

     2. `*{}` 变量选择表达式，与 `${}` 功能相同

        - 特殊功能，`*{}` 可以表示上一个的对象，配合`th:object=${}` 使用

          ```html
          <div th:object="${session.user}"> 
              <p>Name: <span th:text="*{firstName}">Sebastian</span>.</p> 
              <p>Surname: <span th:text="*{lastName}">Pepper</span>.</p> 
              <p>Nationality: <span th:text="*{nationality}">Saturn</span>.</p>
          </div>

          <!--相同-->          
          
          <div> 
              <p>Name: <span th:text="${session.user.firstName}">Sebastian</span>.</p> 
              <p>Surname: <span th:text="${session.user.lastName}">Pepper</span>.</p> 
              <p>Nationality: <span th:text="${session.user.nationality}">Saturn</span>.</p>
</div>
          ```
          
        
     3. `#{}` 获取国际化内容
     
     4. `@{}` 定义 URL
     
        - 实例
     
          ```html
          <a href="details.html" th:href="@{http://localhost:8080/gtvg/order/details(orderId=${o.id})}">view</a>
          ```
     
        - 替换 URL ，然后还可以`(${})` 添加参数
     
        - 传多个变量，使用都好分割即可
     
        - 也可以直接省略主机名和项目名使用 `/` 代替
     
     5. `~{}` 片段引用
   
6. 简单使用

   - 请求域中放入 `<h1>你好<h1>` 和数组`users:{1， 2， 3}`

   - 获取请求域中数据

     ```html
     <!--不会转义，直接显示 <h1>-->
     <div>
         th:text="${hello}"
     </div>
     
     <!--会将<h1>处理，加大字号-->
     <div>
         th:utext="${hello}"
     </div>
     
     <!--每遍历一次，就会有一个 <h4> 标签-->
     <h4>
         th:text="${user} th:each="user:${users}"
     </h4>
     
     <!--[[]] [()] 是否转义，在标签中显示数据-->
     <h4>
         <span th:each="user:${users}">[[${user}]]</span>
     </h4>
     <h4>
         <span th:each="user:${users}">[(${user})]</span>
     </h4>
     ```

### SpringBoot 对 SpringMVC 配置原理

1. [SpringMVC 自动配置 参考地址](https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#boot-features-developing-web-applications) ，官方说明文档
2. 以下是 SpringBoot 对 SprringMVC 的默认配置
   - Inclusion of `ContentNegotiatingViewResolver` and `BeanNameViewResolver` beans.
     1. 自动配置了 `ViewResolver` （视图解析器，根据方式法的返回值，得到视图对象，视图对象决定如何渲染（转发？还是重定向））
     2. 组合所有视图解析器
     3. 如何定制视图解析器
        - 向容器中添加一个自定义视图解析器，便会自动组合挤奶
   - Support for serving static resources, including support for WebJars (see below).
     1. 静态资源文件夹路径 `webjars`
   - Automatic registration of `Converter`, `GenericConverter`, `Formatter` beans.
     1. 自动注册转换器 `Converter` 类型转换（字符串 18 转换为 Integer）
     2. 格式化器 `Formatter` ·2020-2-19· 转换成 `Date`
     3. 自动配置转换器，添加进容器中
   - Support for `HttpMessageConverters` (see below).
     1. SpringMVC 用来转换 Http 请求和响应的 `User--Json`
     2. 从容器中获取，也可以添加自定义的组件
   - Automatic registration of `MessageCodesResolver` (see below).
     1. 错误代码生成规则
   - Static `index.html` support.
     1. 静态首页访问
   - Custom `Favicon` support (see below).
     1. 访问的图标
   - Automatic use of a `ConfigurableWebBindingInitializer` bean (see below).
     1. 可以替换

### 扩展 SpringMVC

1. 扩展内容

   - 以前使用 SpringMVC 的配置文件，进行配置

   - 如下

     ```xml
     <!--请求映射，到指定视图-->
     <mvc:view-controller path="/hello" view-name="success" />
     <!--拦截指定请求-->
     <mvc:interceptors>
         <mvc:interceptor>
             <mvc:mapping path="/hello"/>
             <bean></bean>
         </mvc:interceptor>
     </mvc:interceptors>
     ```

2. 使用 SpringBoot 也达到以上功能

   - 编写配置类 `@Configuration` ，是 `WebMvcConfigurerAdapter` 类型，不能标注 `@EnableWebMvc`

   - `WebMvcConfigurerAdapter` 可以扩展 SpringMVC 的功能

   - 请求页面（不需要使用 controller 处理，只需要做一个简单的映射即可）

     ```java
     // 使用 WebMvcConfigurerAdapter 可以扩展 SpringMVC 的功能
     @Configuration
     public class MyMvcConfig extends WebMvcConfigurerAdapter {
     
         @Override
         public void addViewControllers(ViewControllerRegistry registry) {
             // 请求页面，不需要使用方法来处理这个映射
             registry.addViewController("/sshen").setViewName("success");
         }
     }
     ```

3. 扩展起作用

   - 方法会遍历容器中的所有 `WebMvcConfigurer` ，SpringBoot 默认的以及自定义的都会被使用
   - `@EnableWebMvc` 注解的意思，全面接管 SpringBoot 的 SpringMVC，默认的不起作用

## CRUD 实验

### 项目首页设置

1. 将准备好的资源规则访问指定的文件夹下

2. 确定项目的首页面（三种方法），如果 `public` 中 `index.html` 则会先访问（优先级高）

   - 在控制层，添加控制函数

     ```java
     @RequestMapping(value = {"/", "index"})
     public String index() {
         return "index";
     }
     ```

   - 使用 SpringMVC 扩展

     1. 将扩展组件添加进容器中 `@Bean`

     2. 使用匿名类 `WebMvcConfigurerAdapter`

        ```java
        @Bean
        public WebMvcConfigurerAdapter webMvcConfigurerAdapter() {
            WebMvcConfigurerAdapter adapter = new WebMvcConfigurerAdapter() {
                @Override
                public void addViewControllers(ViewControllerRegistry registry) {
                    // 请求页面，不需要使用方法来处理这个映射
                    registry.addViewController("/").setViewName("index");
                    registry.addViewController("/index.html").setViewName("index");
                    
                }
            }
        }
        ```

   - 配置文件

### 静态资源以及项目名

1. 修改连接请求（静态资源请求）

   - 自定义 `.css` 、 `bootstrap.css` 和 图片，改为 `th:`

   - 实例

     ```html
     <!-- Bootstrap core CSS -->
     <link href="asserts/css/bootstrap.min.css" th:href="@{/webjars/bootstrap/4.0.0/css/bootstrap.css}" rel="stylesheet">
     
     <!-- Custom styles for this template -->
     <link href="asserts/css/signin.css" th:href="@{/asserts/css/signin.css}" rel="stylesheet">
     
     <!--图片静态资源-->
     <img class="mb-4" src="asserts/img/bootstrap-solid.svg" th:src="@{/asserts/img/bootstrap-solid.svg}" alt="" width="72" height="72">
     ```

2. 设置项目名

   - `application.properties` 文件

     ```properties
     server.servlet.context-path=/crud
     ```

### 国际化

1. __国际化（根据浏览器的语言设置，返回页面的返回语言）__

   - SpringMVC 做法

     1. 编写国际化配置文件
     2. 使用 `ResourceBoundleMessageSource` 管理国际化资源文件
     3. 在页面使用 `fmt:message` 取出国际化内容

   - __SpringBoot 做法（自动配置好了管理国际化资源文件的组件）__

     1. 编写国际化配置文件

        - 配置文件可以直接放在类路径下 `message.properties`

        - 也可以配置在某个文件夹下 `resources/i18n`

          ```properties
          spring.messages.basename=i18n.index
          ```

   - 创建过程

     1. 创建文件夹，存放国际化配置文件

        - `resources/i18n` 文件夹

     2. 创建配置文件，国际化`index.html`

        - 默认的配置文件 `index.properties`
        - 中文国际化 `index_zh_CN.properties`
        - 美国 `index_en_US.properties`

     3. 国际化配置文件

        - 默认

          ```txt
          index.btn=登陆~
          index.password=密码~
          index.remember=记住我~
          index.tip=请登陆~
          index.username=用户名~
          ```

        - 中文

          ```txt
          index.btn=登陆
          index.password=密码
          index.remember=记住我
          index.tip=请登陆
          index.username=用户名
          ```

        - 英文

          ```txt
          index.btn=Sign in
          index.password=Password
          index.remember=Remember me
          index.tip=Please sign in
          index.username=Username
          ```

   - 页面获取国际化信息（Thymeleaf 模板引擎使用 `#{}` 获取国际化）

     1. 实例如下

        ```html
        <h1 class="h3 mb-3 font-weight-normal" th:text="#{index.tip}">Please sign in</h1>
        
        <label>
            <input type="checkbox" value="remember-me"/> [[#{index.remember}]]
        </label>
        ```

   - 使用按钮选择显示中英文

     - 原理：国际化 `Locale` （区域信息对象）：`LocaleResolver`（获取区域信息对象）

     - 默认会根据请求 `request` 获取 `getLocal()` 区域信息

     - 自定义 `Local` 国际化对象（`component.MyLocaleResolver`）

       ```java
       import javax.servlet.http.HttpServletRequest;
       import javax.servlet.http.HttpServletResponse;
       import java.util.Locale;
       
       public class MyLocalResolver implements LocaleResolver {
           @Override
           public Locale resolveLocale(HttpServletRequest request) {
               String lang = request.getParameter("lang");
               Locale locale = Locale.getDefault();
               if (StringUtils.isEmpty(lang)) {
                   String[] split = lang.split("_");
                   locale = new Locale(split[0], split[1]);
               }
               return locale;
           }
       
           @Override
           public void setLocale(HttpServletRequest request, HttpServletResponse response, Locale locale) {
       
           }
       }
       ```

     - 在配置类中声明（`MyMVCConfig`）

       ```java
           @Bean
           public LocaleResolver localeResolver() {
               return new MyLocalResolver();
           }
       ```


### 登陆

1. 介绍

   - 不连接数据库，只验证用户名以及密码

   - 修改页面实时生效（模板引擎）

     1. 禁用模板引擎（`application.properties`）

        ```properties
        spring.thymeleaf.cache=false
        ```

     2. 修改页面完成后，使用 `ctrl + F9` ，重新编译

2. 对 `form` 表达的提交地址，进行设置 

   - `th:action="@{/use/login}"`

3. 创建 `LoginController`

   - 进判断用户名与密码是否正确
     1. 如果不正确，将返回错误信息

   - 实例

     ```java
     @Controller
     public class LoginController {
     
         //    @RequestMapping(value = "/user/login", method = RequestMethod.POST)
         //    @GetMapping
         //    @PutMapping
     
         @PostMapping
         @RequestMapping(value = "user/login")
         public String login(@RequestParam("username") String username,
                             @RequestParam("password") String password,
                             Map<String, Object> map) {
     
             // Do not connect to database,only determine
             // whether the user name and password exist,
             // and the password is equal to 123456
             if (!StringUtils.isEmpty(username) && password.equals("123456")) {
                 // login successful
                 return "dashboard";
             } else {
                 // login failed
                 map.put("msg", "username or passowrd is incorrect"); // error message
                 return "index";
             }
         }
     }
     ```

4. 页面的修改

   - 对 `form` 表单中提交的数据设置 `name` 属性

   - 将 `form` 表单的提交方式改为 `method=post`

   - 提示错误信息（如果 `msg` 为空，则不会显示 `<p>` 标签）

     ```html
     <!--determin-->
     <p style="color: red" th:text="${msg}" th:if="${#strings.isEmpty(msg)}}"></p>
     ```

5. 解决表单重复提交

   - 使用重定向（两次 `request`）

   - 增加视图映射（`MyMvcConfig`）

     ```java
     registry.addViewController("main.html").setViewName("dashboard");
     ```

   - 控制器的重定向（当前项目下的 `main.html`）

     ```java
     if (!StringUtils.isEmpty(username) && password.equals("123456")) {
         // login successful
         return "redirect:/main.html";
     ```

### 拦截器登陆检查

1. 作用

   - 转发是服务器的行为，是需要对用户名以及密码进行验证，成功后转发到 `dashboard.html`
     - 浏览器的地址栏 `localhost:8080/crud/user/login` ，表单可以重复提交
     - 表单重复提交，浏览器就会有提示
   - 为了防止表单重复提交，将转发变成重定向
     1. 重定向是客户但行为再次发送 `request` 请求 浏览器地址为 `localhost:8080/crud/main.html` （重定向的，实际访问的 `dashboard.html`）
     2. 但是问题来了，重定向是客户端的行为，则可以不通过用户名以及密码的验证，直接浏览器中输入 `localhost:8080/crud/main.html` ，这样用户登陆便失效了。
   - __解决办法：使用拦截器对登陆进行检查__

2. 创建拦截器

   - 实现接口 `HandlerInterceptor` 

     ```java
     /**
      * 拦截器的作用
      *      登陆的检查
      */
     public class LoginHandlerInterceptor implements HandlerInterceptor {
     
         // 目标方法执行之前
         @Override
         public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
             Object user = request.getSession().getAttribute("loginUser");
             if (user == null) {
                 // 未登录，返回登录页面
                 // 转发
                 request.setAttribute("msg", "没有权限请先登录");
                 request.getRequestDispatcher("/index.html").forward(request, response);
                 return false;
             } else {
                 // 以登录
                 return true;
             }
     
         }
     
         @Override
         public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
     
         }
     
         @Override
         public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
     
         }
     }
     ```

   - 注册到容器中 `MyMvcConfig`

     ```java
     
     // 注册拦截器
     @Override
     public void addInterceptors(InterceptorRegistry registry) {
         // springBoot 不需要关心静态页面
         registry.addInterceptor(new LoginHandlerInterceptor()).addPathPatterns("/**")
             .excludePathPatterns("/index.html", "/", "/user/login");
     	}
     };
     return adapter;
     ```

3. 在控制层 `login()` 控制器，加入 `HttpSession session`

   - 将用户名添加进 `session` 中

     ```java
     if (!StringUtils.isEmpty(username) && password.equals("123456")) {
         // login successful
         session.setAttribute("loginUser", username);
         return "redirect:/main.html";
     } else {
         // login failed
         map.put("msg", "用户名密码错误"); // error message
         return "index";
     }
     ```

     





## SpringBoot 配置心得

### 模式

1. SpringBoot 自动配置组件时，一般会有两种选择
   - 会先查看容器中的组件（有没有用户自定义的组件 `@Bean\@Component`） ，有则使用用户自定义的
   - 有些组件可以多个同时使用时，SpringBoot 会将自定义的组件和默认的组件一起使用
2. 对 SpringBoot 进行扩展配置（SpringBoot 只做了基础的配置）
   - `XxxConfigurer` 类是帮助进行扩展配置的

## Spring Boot 与 Docker

## Spring Boot 与数据访问

## Spring Boot 启动配置原理

## Spring Boot 自定义 starters

## Sping Boot 与缓存



​    