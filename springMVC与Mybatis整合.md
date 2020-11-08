# 依据众筹项目熟悉 Java 设计技术

## 模式介绍

1. 水滴筹、京东众筹，等众筹等项目模式
2. 众筹系统
   - 创新型项目
   - 风险投资 + 天使投资 + 投资者 + 吃瓜群众，投资创新型项目
   - 风险投资和天使投资需得到股权和债券
   - 吃瓜群众和投资者捐赠和奖励创新型项目

## 项目管理

1. 项目构成
   - 项目范围
   - 人员组成结构
2. 项目需求分析
   - 需求采集
   - 需求编制
   - 需求审核
   - 原型构建软件
     1. 快速构建网页模型，共客服参考以及美工设计样板
     2. 工具：Axure；Mockupcreator
3. 项目设计（三种设计方式）
   - 页面设计（美工）
   - 物理数据模型设计（PDM）
     1. 设计工具：PowerDesigner（设计完成后自动生成 SQL 语句）
     2. 抽取概念模型 -> 转换对象模型 -> 转换数据模型
     3. 使用 PowerDesigner 设计的数据库原型，MySQL 执行脚本创建数据库
   - 业务流程设计
     1. UML （统一建模语言）：是面向对象开发的一种通用的图像化建模语言，定义良好、已与表达、功能强大等特点
     2. 可实现工具：Rational_Rose

# 环境搭建

## 环境搭建工具

1. 工具
   - JDK 1.8+
   - STS 3.9.7
   - MySQL
   - Tomcat 8.5
   - Redis
   - 字符编码 UTF-8
2. 框架
   - Spring、SpringMVC、Mybatis
   - Maven
   - JQuery
   - Bootstrap
   - zTree
   - SpringSecurity

## 基于 Maven 工程管理

1. 项目拆分分类
   - 按照业务维度进行拆分（水平拆分）
     1. 用户系统、订单系统、支付系统、众筹项目系统
   - 按照代码层拆分（垂直拆分，层要做集群）
     1. 数据访问层、业务逻辑层、前端 web 层
     2. 模块：bean、dao、service、controller、webpage
2. __此项目采用垂直拆分__
   - 项目架构图
   
     1. parent 模块
   
        - 父工程：继承、聚合
   
     2. webui 模块
   
        - war 工程
        - 依赖 component
   
     3. component 模块
   
        - 此模块较为复杂（后续再补）
   
        - 依赖 entity、util 模块
   
     4. entity 模块
   
        - 实体类：逆向生成
   
     5. util 模块
   
        - 工具模块：不参与继承和聚合
   
     6. reverse 模块
   
        - 逆向工程模块：不参与继承和聚合，专门为逆向工程所建（逆向工程可能会影响项目结构，所以单独创建，再拷贝）
   
   - 工程计划
   
     1. 实例
   
        ```java
        atcrowdfunding01-admin-parent
            groupId:com.shentree.crowd
            artifactId:atcrowdfunding01-admin-parent
            package:pom
            
        atcrowdfunding02-admin-webui
            groupId:com.shentree.crowd
            artifactId:atcrowdfunding02-admin-webui
            package:war
            
        atcrowdfunding03-admin-component
            groupId:com.shentree.crowd
            artifactId:atcrowdfunding03-admin-component
            package:jar
            
        atcrowdfunding04-admin-entity
            groupId:com.shentree.crowd
            artifactId:atcrowdfunding04-admin-entity
            package:jar
           
        atcrowdfunding05-common-util
            groupId:com.shentree.crowd
            artifactId:atcrowdfunding05-common-util
            package:jar
            
        atcrowdfunding06-common-reverse
            groupId:com.shentree.crowd
            artifactId:atcrowdfunding06-common-reverse
            package:jar
        ```

## 基于 STS 创建工程（逆向工程）

1. 注意点

   - 创建 `war` maven 工程时，项目创建有缺陷，`src/main/webapp/WEB-INF/web.xml` 没有，项目报错，手动生成以下就可以。
   - 父工程使用 `maven project` 创建 ，子工程使用父工程 `maven module` 创建，父工程自动聚合

2. 父工程

   - 聚合子工程

     ```xml
     <!-- 聚合 -->
     <modules>
         <module>atcrowdfunding02-admin-webui</module>
         <module>atcrowdfunding03-admin-component</module>
         <module>atcrowdfunding04-admin-entity</module>
     </modules>
     ```

3. 完成上诉依赖关系

   - 使用 STS 工具自动完成
   
4. 使用 Mybatis 逆向工程生成 bean、XXXMapper 接口和映射文件

   - 命令 `mybatis-generator:generate`
   
   - 实体类中，没有有参构造器和 `toString()` 方法
   
   - 生成的内容
     1. 数据库对应的实体类（`XXXExample` QBC 查询，生成模式 `Mybatis3`）
  2. `XXXMapper` 接口和映射文件
     
- 将生成的内容拷贝到应有的模块中
  
     1. XXXMapper.xml 和配置文件都放入 `webui` 子模块
     2. `mybatis/mapper/XXXMapper.xml` 
     
   - 逆向工程 `pom.xml` 配置文件
   
     ```xml
     <!-- 依赖MyBatis 核心包 -->
     <dependencies>
         <dependency>
             <groupId>org.mybatis</groupId>
             <artifactId>mybatis</artifactId>
             <version>3.2.8</version>
         </dependency>
     </dependencies>
     
     <!-- 控制Maven 在构建过程中相关配置 -->
     <build>
         <!-- 构建过程中用到的插件 -->
         <plugins>
             <!-- 具体插件，逆向工程的操作是以构建过程中插件形式出现的 -->
             <plugin>
                 <groupId>org.mybatis.generator</groupId>
                 <artifactId>mybatis-generator-maven-plugin</artifactId>
                 <version>1.3.0</version>
                 <!-- 插件的依赖 -->
                 <dependencies>
                     <!-- 逆向工程的核心依赖 -->
                     <dependency>
                         <groupId>org.mybatis.generator</groupId>
                         <artifactId>mybatis-generator-core</artifactId>
                         <version>1.3.2</version>
                     </dependency>
                     <!-- 数据库连接池 -->
                     <dependency>
                         <groupId>com.mchange</groupId>
                         <artifactId>c3p0</artifactId>
                         <version>0.9.2</version>
                     </dependency>
                     <!-- MySQL 驱动 -->
                     <dependency>
                         <groupId>mysql</groupId>
                         <artifactId>mysql-connector-java</artifactId>
                         <version>5.1.8</version>
                  </dependency>
                 </dependencies>
          </plugin>
         </plugins>
     </build>
     ```
   
   - `generatorConfig.xml` 配置文件
   
     ```xml
     <!DOCTYPE generatorConfiguration PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN" "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
     <generatorConfiguration>
     	<!-- mybatis-generator:generate -->
     	<context id="DB2Tables" targetRuntime="MyBatis3">
     		<commentGenerator>
     			<!-- 是否去除自动生成的注释 true:是;false:否 -->
     			<property name="suppressAllComments" value="true" />
     		</commentGenerator>
     		<!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
     		<jdbcConnection driverClass="com.mysql.jdbc.Driver"
     			connectionURL="jdbc:mysql://localhost:3306/crowd"
     			userId="root"
     			password="123">
     		</jdbcConnection>
     		<!-- 默认 false，把 JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，为 true 时把 JDBC DECIMAL 
     			和 NUMERIC 类型解析为 java.math.BigDecimal -->
     		<javaTypeResolver>
     			<property name="forceBigDecimals" value="false" />
     		</javaTypeResolver>
     		<!-- targetProject:生成 Entity 类的路径 -->
     		<javaModelGenerator targetProject="./src/main/java"
     			targetPackage="com.shentree.crowd.entity">
     			<!-- enableSubPackages:是否让 schema 作为包的后缀 -->
     			<property name="enableSubPackages" value="false" />
     			<!-- 从数据库返回的值被清理前后的空格 -->
     			<property name="trimStrings" value="true" />
     		</javaModelGenerator>
     		<!-- targetProject:XxxMapper.xml 映射文件生成的路径 -->
     		<sqlMapGenerator targetProject="./src/main/java"
     			targetPackage="com.shentree.crowd.mapper">
     			<!-- enableSubPackages:是否让 schema 作为包的后缀 -->
     			<property name="enableSubPackages" value="false" />
     		</sqlMapGenerator>
     		<!-- targetPackage：Mapper 接口生成的位置 -->
     		<javaClientGenerator type="XMLMAPPER"
     			targetProject="./src/main/java"
     			targetPackage="com.shentree.crowd.mapper">
     			<!-- enableSubPackages:是否让 schema 作为包的后缀 -->
     			<property name="enableSubPackages" value="false" />
     		</javaClientGenerator>
     		<!-- 数据库表名字和 entity 类对应的映射指定 -->
     		<table tableName="t_admin" domainObjectName="Admin" />
     	</context>
     </generatorConfiguration>
     ```

## 父工程依赖管理

1. 父工程管理 JAR 的版本以及各个模块公共的 JAR 包

2. 更具下面的配置文件说明以下问题

   - spring 的包只是导入了 3 个，但是根据 maven 的特性，其他的 spring 包，会根据这 3 个包的依赖传递性，maven 将自动导入

3. 父工程配置文件 `pom.xml`

   - 版本声明（spring 和 spring.security 和 jackson 版本应该对应）

     ```xml
     <!-- 版本管理 -->
     <properties>
         <!-- 声明属性，对 Spring 的版本进行统一管理 -->
         <spring.version>4.3.20.RELEASE</spring.version>
         <!-- 声明属性，对 SpringSecurity 的版本进行统一管理 -->
         <spring.security.version>4.2.10.RELEASE</spring.security.version>
     </properties>
     ```

   - 依赖管理

     ```xml
     <!-- 依赖管理 -->
     <dependencyManagement>
         <dependencies>
             <!-- Spring 依赖 --> 
             <!-- https://mvnrepository.com/artifact/org.springframework/spring-orm -->
             <dependency>
                 <groupId>org.springframework</groupId>
                 <artifactId>spring-orm</artifactId>
                 <version>${spring.version}</version>
             </dependency>
             <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
             <dependency>
                 <groupId>org.springframework</groupId>
                 <artifactId>spring-webmvc</artifactId>
                 <version>${spring.version}</version>
             </dependency>
             <dependency>
                 <groupId>org.springframework</groupId>
                 <artifactId>spring-test</artifactId>
                 <version>${spring.version}</version>
             </dependency>
             <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
             <dependency>
                 <groupId>org.aspectj</groupId>
                 <artifactId>aspectjweaver</artifactId>
                 <version>1.9.2</version>
             </dependency>
             <!-- https://mvnrepository.com/artifact/cglib/cglib -->
             <dependency>
                 <groupId>cglib</groupId>
                 <artifactId>cglib</artifactId>
                 <version>2.2</version>
             </dependency>
             <!-- 数据库依赖 -->
             <!-- MySQL 驱动 -->
             <dependency>
                 <groupId>mysql</groupId>
                 <artifactId>mysql-connector-java</artifactId>
                 <version>5.1.3</version>
             </dependency>
             <!-- 数据源 -->
             <dependency>
                 <groupId>com.alibaba</groupId>
                 <artifactId>druid</artifactId>
                 <version>1.0.31</version>
             </dependency>
             <!-- MyBatis -->
             <dependency>
                 <groupId>org.mybatis</groupId>
                 <artifactId>mybatis</artifactId>
                 <version>3.2.8</version>
             </dependency>
             <!-- MyBatis 与 Spring 整合 -->
             <dependency>
                 <groupId>org.mybatis</groupId>
                 <artifactId>mybatis-spring</artifactId>
                 <version>1.2.2</version>
             </dependency>
             <!-- MyBatis 分页插件 -->
             <dependency>
                 <groupId>com.github.pagehelper</groupId>
                 <artifactId>pagehelper</artifactId>
                 <version>4.0.0</version>
             </dependency>
             <!-- 日志 -->
             <dependency>
                 <groupId>org.slf4j</groupId>
                 <artifactId>slf4j-api</artifactId>
                 <version>1.7.7</version>
             </dependency>
             <dependency>
                 <groupId>ch.qos.logback</groupId>
                 <artifactId>logback-classic</artifactId>
                 <version>1.2.3</version>
             </dependency>
             <!-- 其他日志框架的中间转换包 -->
             <dependency>
                 <groupId>org.slf4j</groupId>
                 <artifactId>jcl-over-slf4j</artifactId>
                 <version>1.7.25</version>
             </dependency>
             <dependency>
                 <groupId>org.slf4j</groupId>
                 <artifactId>jul-to-slf4j</artifactId>
                 <version>1.7.25</version>
             </dependency>
             <!-- Spring 进行 JSON 数据转换依赖 -->
             <dependency>
                 <groupId>com.fasterxml.jackson.core</groupId>
                 <artifactId>jackson-core</artifactId>
                 <version>2.9.8</version>
             </dependency>
             <dependency>
                 <groupId>com.fasterxml.jackson.core</groupId>
                 <artifactId>jackson-databind</artifactId>
                 <version>2.9.8</version>
             </dependency>
             <!-- JSTL 标签库 -->
             <dependency>
                 <groupId>jstl</groupId>
                 <artifactId>jstl</artifactId>
                 <version>1.2</version>
             </dependency>
             <!-- junit 测试 -->
             <dependency>
                 <groupId>junit</groupId>
                 <artifactId>junit</artifactId>
                 <version>4.12</version>
                 <scope>test</scope>
             </dependency>
             <!-- 引入 Servlet 容器中相关依赖 -->
             <dependency>
                 <groupId>javax.servlet</groupId>
                 <artifactId>servlet-api</artifactId>
                 <version>2.5</version>
                 <scope>provided</scope>
             </dependency>
             <!-- JSP 页面使用的依赖 -->
             <dependency>
                 <groupId>javax.servlet.jsp</groupId>
                 <artifactId>jsp-api</artifactId>
                 <version>2.1.3-b06</version>
                 <scope>provided</scope>
             </dependency>
             <!-- https://mvnrepository.com/artifact/com.google.code.gson/gson -->
             <dependency>
                 <groupId>com.google.code.gson</groupId>
                 <artifactId>gson</artifactId>
                 <version>2.8.5</version>
             </dependency>
             <!-- SpringSecurity 对Web 应用进行权限管理 -->
             <dependency>
                 <groupId>org.springframework.security</groupId>
                 <artifactId>spring-security-web</artifactId>
                 <version>${spring.security.version}</version>
             </dependency>
             <!-- SpringSecurity 配置 -->
             <dependency>
                 <groupId>org.springframework.security</groupId>
                 <artifactId>spring-security-config</artifactId>
                 <version>${spring.security.version}</version>
             </dependency>
             <!-- SpringSecurity 标签库 -->
             <dependency>
                 <groupId>org.springframework.security</groupId>
                 <artifactId>spring-security-taglibs</artifactId>
                 <version>${spring.security.version}</version>
             </dependency>
         </dependencies>
     </dependencyManagement>
     ```

## spring 与 Mybatis 整合

### 目标以及整合到哪个子模块

1. 目标

   - 将 XXXMapper 通过 IOC 容器装配当前组件中， 就可以直接调用 XXXMapper 的方法

     1. 使用框架整合，就不需要再创建 `SqlSession` 和 `Adminmapper` 接口的动态代理对象

   - 实例

     ```java
     @Service
     public class AdminServiceImpl implements AdminService {
         
         @Autowired
         private AdminMapper adminMapper;
         
         public Admin getAdmin(Integer id) {
             return adminMapper.selectByPrimaryKey(id);
         }
     }
     ```

2. 确定使用哪一个模块来完成此项目

   - 使用 `component` 子工程，完成整合

   - 依赖的解决（父工程的继承）

     1. pom.xml

        ```xml
        <!-- 父模块继承 -->
        <!-- Spring 依赖 -->
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-orm -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
        </dependency>
        <!-- https://mvnrepository.com/artifact/cglib/cglib -->
        <dependency>
            <groupId>cglib</groupId>
            <artifactId>cglib</artifactId>
        </dependency>
        <!-- MySQL 驱动 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <!-- 数据源 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
        </dependency>
        <!-- MyBatis -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
        </dependency>
        <!-- MyBatis 与 Spring 整合 -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
        </dependency>
        <!-- MyBatis 分页插件 -->
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper</artifactId>
        </dependency> 
        <!-- 日志 -->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.7</version>
        </dependency>
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.2.3</version>
        </dependency>
        <!-- 其他日志框架的中间转换包 -->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>jcl-over-slf4j</artifactId>
            <version>1.7.25</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>jul-to-slf4j</artifactId>
            <version>1.7.25</version>
        </dependency>
        <!-- Spring 进行 JSON 数据转换依赖 -->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-core</artifactId>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
        </dependency>
        <!-- JSTL 标签库 -->
        <dependency>
            <groupId>jstl</groupId>
            <artifactId>jstl</artifactId>
        </dependency> 
        <!-- https://mvnrepository.com/artifact/com.google.code.gson/gson -->
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
        </dependency>
        ```


### 配置文件设置

1. 配置文件（保证可以连接数据库）

   - 创建 `jdbc.properties` 配置文件

     1. 实例

        ```properties
        jdbc.driver=com.mysql.jdbc.Driver
        jdbc.url=jdbc:mysql://localhost:3306/crowd?allowMultiQueries=true
        jdbc.username=root
        jdbc.password=123
        ```

   - 创建 `mybatis-config.xml` 配置文件（mMybatis 的文件）

     1. 此配置文件，一般不起什么作用

     2. 实例

        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
        <configuration>
        		
        </configuration>
        ```

   - 创建 `spring-persist-mybatis.xml` （spring 与 mybatis 整合配置文件）

     1. 加载外部属性文件
     2. 创建数据源（DruidDataSource）
        
     - 数据库的属性（驱动、url、用户名、密码）
       
     1. 实例

        ```xml
        <!-- 加载外部属性文件 -->
        <context:property-placeholder location="classpath:jdbc.properties"/>
        
        <!-- 配置数据源 -->
        <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
            <property name="driverClassName" value="${jdbc.driver}"></property>
            <property name="url" value="${jdbc.url}"></property>
            <property name="username" value="jdbc.username"></property>
            <property name="password" value="${jdbc.password}"></property>
        </bean>
        ```

   - 创建测试类（webui 模块下，因为配置文件都在旗下）

     1. 依赖文件

        - `webui` 的 `pom.xml` 文件，解决依赖

        - junit 测试和 spring-test，这两个作用范围都是 `<scope>test<scope>` 不能传递

          ```xml
          <!-- 创建测试类依赖 -->
          <!-- spring-test 测试 -->
          <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-test</artifactId>
          </dependency>
          <!-- junit 测试 -->
          <dependency>
              <groupId>junit</groupId>
              <artifactId>junit</artifactId>
              <scope>test</scope>
          </dependency>
          ```
        
     2. 创建测试类（wenui 模块）

        - 测试主要内容是否可以与数据库相连
     
        - `TestCrowd.java`
     
          ```java
          // spring 与 Junit 整合，Junit 可以使用 spring 的内容
          @RunWith(SpringJUnit4ClassRunner.class)
          // 解决测试类中使用自动装配
          @ContextConfiguration(locations = {"classpath:spring-persist-mybatis.xml"})
          public class CrowdTest {
          	
          	@Autowired
          	private DataSource dataSource;
          	
          	@Test
          	public void testConnection() throws SQLException {
          		Connection connection = dataSource.getConnection();
          		System.out.println(connection);
          	}
          
          }
          ```
        
     3. 配置文件（使用 `SqlSessionFactoryBean`）
     
        - `spring-persist-mybatis.xml`
     
        - 实现步骤
     
          1. XXXMapper.xml 的配置文件路径
          2. Mybatis 核心配置文件（mybatis-config.xml）路径
          3. 数据源（`DruidDataSource`）
          4. XXXMapper 接口
     
        - 配置文件 `spring-persist-mybatis.xml`
     
          ```xml
          <!-- 配置 SqlSessionFactoryBean -->
          <bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
              <!-- mybatis 全局配置文件 -->
              <property name="configLocation" value="classpath:mybatis/mybatis-config.xml"></property>
              <!-- 指定 XXXMapper.xml 文件 -->
              <property name="mapperLocations" value="classpath:mybatis/mapper/*Mapper.xml"></property>
              <!-- 指定数据源 -->
              <property name="dataSource" ref="dataSource"></property>
          </bean>
          
          <!-- 配置 MapperScannerConfigurer 扫描 Mapper 接口所在的包 -->
          <bean id="" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
              <property name="basePackage" value="com.shentree.crowd.mapper"></property>
          </bean>
          ```
     
        - 测试代码
     
          ```java
          // spring 与 Junit 整合，Junit 可以使用 spring 的内容
          @RunWith(SpringJUnit4ClassRunner.class)
          // 解决测试类中使用自动装配
          @ContextConfiguration(locations = {"classpath:spring-persist-mybatis.xml"})
          public class CrowdTest {
          	
          	@Autowired
          	private DataSource dataSource;
          	
          	@Autowired
          	private AdminMapper adminMapper;
          	
          	@Test
          	public void testConnection() throws SQLException {
          		Connection connection = dataSource.getConnection();
          		System.out.println(connection);
          	}
          	
          	@Test
          	public void testInsertAdmin() {
          		int insert = adminMapper.insert(new Admin(null, "shentree", "123", "shenDZ", "123@163.com", null));
          		System.out.println(insert);
          	}
          
          }
          ```
     
   

## 日志系统

### 作用

1. 系统运行出现任何问题，就需要通过日志来进行排查

2. 一般日志实现

   - 接口编程，使用 `slf4j-api.jar` 作为接口
   - 具体实现类为 
     1. `logback-classic.jar/logback-core.jar` ，可以作为接口的直接实现类
     2. `slf4j-simple.jar` ，可以作为接口的直接实现类
     3. `log4j.jar` 则需要使用适配器 `slf4j-log412.jar`
     4. `JVM runtime` 则需要使用 `slf4j-jdk14.jar` 适配器

3. spring 默认的日志中间件为 `commons-logging.jar` （jcl）

   - 如何对默认的中间件日志进行转换呢？

   - 将 `commons-loging.jar`  换成 `jcl-over-slf4j.jar` ，目的：是让 spring 感觉不到日志工具已经换掉；将日志风格转换成 `slf4j-api.jar` ，实现类 `logback`

   - 将各个子项目的 `commons-log.jar` 全部排除出去

   - 使用上述的 3 个 JAR 包，完成 spring 日志向 `logback` 转换

     ```xml
     <!-- 日志 -->
     <!-- 接口 -->
     <dependency>
         <groupId>org.slf4j</groupId>
         <artifactId>slf4j-api</artifactId>
     </dependency>
     <!-- 具体实现 -->
     <dependency>
         <groupId>ch.qos.logback</groupId>
         <artifactId>logback-classic</artifactId>
     </dependency>
     <!-- 其他日志框架的中间转换包 -->
     <!-- 对 commons-logging 的转换 -->
     <dependency>
         <groupId>org.slf4j</groupId>
         <artifactId>jcl-over-slf4j</artifactId>
     </dependency>
     <!-- 对 java unti 日志的转换 -->
     <dependency>
         <groupId>org.slf4j</groupId>
         <artifactId>jul-to-slf4j</artifactId>
     </dependency>
     ```

     

4. 将 `sysout` 换成日志输出

   - 代码

     ```java
     @Test
     public void testLog() {
         Logger logger = LoggerFactory.getLogger(CrowdTest.class);
     
         logger.debug("debug");
         logger.info("info");
         logger.warn("warn");
         logger.error("error");
     }
     ```

5. 如果将 `commons-logging.jar` 去除，但是也没有使用 `jcl-over-slf4j.jar` 代替（转换成 `logback`）

   - 报错

     ```tex
     java.lang.Exception: No tests found matching [{ExactMatcher:fDisplayName=testInsertAdmin], {ExactMatcher:fDisplayName=testInsertAdmin(com.shentree.crowd.test.CrowdTest)], {LeadingIdentifierMatcher:fClassName=com.shentree.crowd.test.CrowdTest,fLeadingIdentifier=testInsertAdmin]] from org.junit.internal.requests.ClassRequest@21b8d17c
     at org.junit.internal.requests.FilterRequest.getRunner(FilterRequest.java:40)
     at org.eclipse.jdt.internal.junit4.runner.JUnit4TestLoader.createFilteredTest(JUnit4TestLoader.java:83)
     at org.eclipse.jdt.internal.junit4.runner.JUnit4TestLoader.createTest(JUnit4TestLoader.java:74)
     at org.eclipse.jdt.internal.junit4.runner.JUnit4TestLoader.loadTests(JUnit4TestLoader.java:49)
     at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.runTests(RemoteTestRunner.java:526)
     at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.runTests(RemoteTestRunner.java:770)
     at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.run(RemoteTestRunner.java:464)
     at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.main(RemoteTestRunner.java:210)
     ```

### 配置文件（logback.xml）

1. 可以控制日志的输出格式，以及日志的输出级别（debug、info、warn、error）

2. 建立配置文件

   - `logback.xml` （名称固定）

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <configuration debug="true"> 
         <!-- 指定日志输出的位置 -->
         <appender name="STDOUT"
                   class="ch.qos.logback.core.ConsoleAppender">
             <encoder> 
                 <!-- 日志输出的格式 --> 
                 <!-- 按照顺序分别是：时间、日志级别、线程名称、打印日志的类、日志主体 内容、换行 -->
                 <pattern>[%d{HH:mm:ss.SSS}][%-5level] [%thread] [%logger][%msg]%n</pattern>
             </encoder>
         </appender>
         <!-- 设置全局日志级别。日志级别按顺序分别是：DEBUG、INFO、WARN、ERROR --> 
         <!-- 指定任何一个日志级别都只打印当前级别和后面级别的日志。 -->
         <root level="INFO"> 
             <!-- 指定打印日志的 appender，这里通过“STDOUT”引用了前面配置的 appender -->
             <appender-ref ref="STDOUT" />
         </root>
         <!-- 根据特殊需求指定局部日志级别 -->
         <logger name="com.shentree.crowd.mapper" level="DEBUG" />
     </configuration>
     ```

## 声明式事务

### 编程式事务

1. 编程代码

   - 使用 `try/catch/finally`

     ```java
     try{
         // 开启事务，对应 AOP 的前置通知
         connection.setAutoCommit(false);
         // 核心操作
         adminService.insertAdmin(admin);
         // 提交事务，对应返回通知
         connection.commit();
         
     } catch {
         // 回滚事务，异常通知
         connection.rollback();
     } finally {
         // 释放数据库连接，对应后置通知
         connection.close();
     }
     ```

### 声明式事务

1. 在框架下通过配置，由 spring 来管理事务操作

2. 使用注解事务（SSM）中，可以查看 `@Transcational` ，定位到 `service` 的方法

3. 使用 XML 声明事务，（使用切面表达式定位方法）

4. 配置事务

   - 配置事务管理器
   - 配置 AOP
   - 配置事务属性
   - 创建文件（`spring-persist-tx.xml`） ，做到单一职责原则

5. 步骤

   - 需要命令空间（aop、tx、context）

   - 创建 `service`

     - `com.shentree.crowd.service`
     - `com.shentree.crowd.service.api`
     - `com.shentree.crowd.service.impl`

   - 自动扫描包

     ```xml
     <!-- 自动扫描 service 包 -->
     <context:component-scan base-package="com.shentree.crowd.service"></context:component-scan>
     ```

   - 配置事务管理器

     1. 需要数据源

   - 事务通知

     1. 引用：事务管理器
     2. 配置事务属性

   - 事务切面

     - 切面表达式
     - 事务通知与切面表达式关联

   - 实例

     ```xml
     <!-- 自动扫描 service 包 -->
     <context:component-scan base-package="com.shentree.crowd.service"></context:component-scan>
     
     <!-- 配置事物管理器 -->
     <bean id="dataSourceTransactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
         <!-- 装配数据源 -->
         <!-- 指向 spring-persist-mybatis 的数据源，在IOC 容器中可以找到 -->
         <property name="dataSource" ref="dataSource"></property>
     </bean>
     
     <!-- 配置事物通知 -->
     <!-- 事物管理器 -->
     <tx:advice id="txAdvice" transaction-manager="transactionManager">
         <!-- 配置事物属性 -->
         <tx:attributes>
             <!-- 查询方法：配置只读属性时，数据库知道为只读操作，会进行优化 -->
             <tx:method name="get*" read-only="true"/>
             <tx:method name="find*" read-only="true"/>
             <tx:method name="count*" read-only="true"/>
             <tx:method name="query*" read-only="true"/>
     
             <!-- 增删改：配置事物传播行为，回滚异常 -->
             <!-- 
                 propagation 属性
                 默认值：REQUIRED，表示必须在事物中工作，如果当前线程没有开启事物，则自己开启事物；
                     如果已经开启事物，则使用已有事物。
                     使用其他事物，可能受其他事物回滚影响
                 建议：  REQUIRES_NEW：只会在自己开启事物中执行
             -->
             <!--  
                  rollback-for 属性
                   默认：运行时异常回滚
                   建议：java.lang.Exception 编译和运行时异常都会滚
              -->
             <tx:method name="save*" propagation="REQUIRES_NEW" rollback-for="java.lang.Exception"/>
             <tx:method name="update*" propagation="REQUIRES_NEW" rollback-for="java.lang.Exception"/>
             <tx:method name="remove*" propagation="REQUIRES_NEW" rollback-for="java.lang.Exception"/>
             <tx:method name="batch*" propagation="REQUIRES_NEW" rollback-for="java.lang.Exception"/>
         </tx:attributes>
     </tx:advice>
     
     <!-- 配置事物切面 -->
     <aop:config>
         <!-- 扫描定位到 Service 的实现类 -->
         <aop:pointcut expression="execution(* *..*ServiceImpl.*(..))" id="txPointcut"/>
         <!-- 将事物通知和切入点表达式关联 -->
         <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointcut"/>
     </aop:config>
     ```


### 测试

1. 创建 `AdminService` 接口，再创建 `AdminServiceImpl` 实现类

   - 实现类

     ```java
     @Service
     public class AdminServiceImpl implements AdminService {
     	
     	@Autowired
     	private AdminMapper adminMapper;
     
     	@Override
     	public void saveAdmin(Admin admin) {
     		
     		adminMapper.insert(admin);
     		
     	}
     }
     ```

2. 测试类（还是在 `webui` 中）

   - 代码

     ```java
     // spring 与 Junit 整合，Junit 可以使用 spring 的内容
     @RunWith(SpringJUnit4ClassRunner.class)
     // 解决测试类中使用自动装配
     @ContextConfiguration(locations = {"classpath:spring-persist-mybatis.xml", "classpath:spring-persist-tx.xml"})
     public class CrowdTest {
     
         @Autowired
         private DataSource dataSource;
     
         @Autowired
         private AdminMapper adminMapper;
     
         @Autowired
         private AdminService adminService;
     
         @Test
         public void testTx() {
             Admin admin = new Admin(null, "ssong", "123", "songMJ", "123@163.com", null);
             adminService.saveAdmin(admin);
         }
     
         @Test
         public void testConnection() throws SQLException {
             Connection connection = dataSource.getConnection();
             System.out.println(connection);
         }
     
         @Test
         public void testInsertAdmin() {
             int insert = adminMapper.insert(new Admin(null, "shentree", "123", "shenDZ", "123@163.com", null));
             System.out.println(insert);
         }
     
         @Test
         public void testLog() {
             Logger logger = LoggerFactory.getLogger(CrowdTest.class);
     
             logger.debug("debug");
             logger.info("info");
             logger.warn("warn");
             logger.error("error");
         }
     
     }
     ```

## 表述层配置

### web.xml 、 spring 与 springMVC 的配置文件关系

1. 用什么连接 web.xml 与 spring 的关系
   - `Listener` 监听器
     1. `ContextLoaderListener` ：作用加载 `spring` 配置文件（两个配置文件），创建 IOC 容器
     2. IOC 容器包含：service（事务）、mapper
     3. service 装配 mapper 对象（动态代理机制）
   - `Filter` 过滤器
     1. `CharacterEncodingFilter` ：字符编码过滤器
     2. `HiddenHttpMethodFilter` ：rest
   - `Servlet` 
     1. `DispatcherServlet` ：作用加载 springMVC 配置文件，创建 springMVC 的 IOC 容器
     2. `spring-web.mvc.xml`
     3. IOC 容器包含：Controller、Interceptor（拦截器）、异常映射机制
     4. Controller 装配 service 对象
2. 目标
   - Controller 装配 Service
   - 页面可以访问 Controller
     1. 通过 @ReuqestMapping` 

### 配置 web.xml

1. 配置 `ContextLoaderListener` 监听器

2. 配置 `CharacterEncodingFilter` 过滤器

3. 配置 `servlet` 

4. 实例

   ```xml
   <!-- 监听器，初始化 spring 的 IOC 容器 -->
   <!-- needed for ContextLoaderListener -->
   <context-param>
       <param-name>contextConfigLocation</param-name>
       <param-value>classpath:spring-persist-*.xml</param-value>
   </context-param>
   
   <!-- Bootstraps the root web application context before servlet initialization -->
   <listener>
       <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
   </listener>
   
   <!-- 此过滤器必须在其他过滤器之前 -->
   <!-- 字符编码过滤器 -->
   <filter>
       <filter-name>characterEncodingFilter</filter-name>
       <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
       <!-- 指定字符集 UTF-8 -->
       <init-param>
           <param-name>encoding</param-name>
           <param-value>UTF-8</param-value>
       </init-param>
       <init-param>
           <param-name>forceRequestEncoding</param-name>
           <param-value>true</param-value>
       </init-param>
       <init-param>
           <param-name>forceResponseEncoding</param-name>
           <param-value>true</param-value>
       </init-param>
   </filter>
   <filter-mapping>
       <filter-name>characterEncodingFilter</filter-name>
       <url-pattern>/*</url-pattern>
   </filter-mapping>
   
   <!-- 加载 springMVC 配置文件 -->
   <!-- The front controller of this Spring Web application, responsible for handling all application requests -->
   <servlet>
       <servlet-name>springDispatcherServlet</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       <init-param>
           <param-name>contextConfigLocation</param-name>
           <param-value>classpath:spring-web-mvc.xml</param-value>
       </init-param>
       <!-- 将 Servlet 的生命周期初始化，提前到 web 应用启动时创建 -->
       <!-- 原因是 DispatcherServlet 初始化创建对象后有大量框架初始化，如在第一次请求时初始化，则有点不合适 -->
       <load-on-startup>1</load-on-startup>
   </servlet>
   
   <!-- Map all requests to the DispatcherServlet for handling -->
   <servlet-mapping>
       <servlet-name>springDispatcherServlet</servlet-name>
       <!-- <url-pattern>/</url-pattern> -->
       <url-pattern>*.html</url-pattern>
       <url-pattern>*.json</url-pattern>
   </servlet-mapping>
   ```

### 配置 springMVC 配置文件

1. 配合实例

   - 代码

     ```xml
     <!-- 配置自动扫描 springMVC 的 IOC 容器 -->
     <context:component-scan base-package="com.shentree.crowd.mvc"></context:component-scan>
     
     <!-- 配置 springMVC 注解驱动 -->
     <mvc:annotation-driven/>
     
     <!-- 配置视图解析器 -->
     <bean id="" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
         <property name="prefix" value="/WEB-INF/"></property>
         <property name="suffix" value=".jsp"></property>
     </bean>
     ```

2. 通过扫描的包可以看出，springMVC 的 IOC 容器不止只有 `controller`

   - `controller`
   - `interceptor`
   - `config`

### 测试通过页面访问

1. 解决报错问题

   - 原因缺少 `Servlet-api` ，但是时机部署时 `Tomcat` 自带

   - 使用 `<scope>provided</scope>`

   - 实例（父类引用依赖）`webui/pom.xml`

     ```xml
     <!-- 引入 Servlet 容器中相关依赖 -->
     <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>servlet-api</artifactId>
         <scope>provided</scope>
     </dependency>
     <!-- JSP 页面使用的依赖 -->
     <dependency>
         <groupId>javax.servlet.jsp</groupId>
         <artifactId>jsp-api</artifactId>
         <scope>provided</scope>
     </dependency>
     ```

   - 因为作用范围无法传递，所以 `componment` 也得依赖 `servlet-api` 需要使用 `HttpSession`

2. Jsp 页面

   - 代码

     ```jsp
     <%@ page language="java" contentType="text/html; charset=UTF-8"
         pageEncoding="UTF-8"%>
     <!DOCTYPE html>
     <html>
     <head>
     <meta charset="UTF-8">
     <title>Insert title here</title>
     </head>
     <body>
     	<a href="${pageContext.request.contextPath}/test/ssm.html">测试 SSM 整合</a>
     </body>
     </html>
     ```

3. Controller 层

   - 实例代码

     ```java
     @Controller
     public class TestController {
     
         @Autowired
         private AdminService adminService;
     
         @RequestMapping(value = "test/ssm.html")
         public String testSsm(ModelMap modelMap) {
             List<Admin> adminList = adminService.getAllAdmins();
             modelMap.addAttribute("adminList",adminList);
             return "target";
         }
     }
     ```
     
   - __转发到 `/WEB-INF/target.jsp` 中__

     1. 注意是转发，浏览器地址栏不发生变化

4. Service 层

   - 接口

   - 实现类

     ```java
     @Service
     public class AdminServiceImpl implements AdminService {
     
         @Autowired
         private AdminMapper adminMapper;
     
         @Override
         public void saveAdmin(Admin admin) {
             adminMapper.insert(admin);
         }
     
         @Override
         public List<Admin> getAllAdmins() {
             return adminMapper.selectByExample(new AdminExample());
         }
     }
     ```
     
   
5. 注意点

   - 使用 `base` 标签

     ```jsp
     <!-- http://localhost:8080/atcrowdfunding02-admin-webui/test/ssm.html -->
     <base href="http://${pageContext.request.serverName }:${pageContext.request.serverPort }${pageContext.request.contextPath }/" />
     </head>
     <body>
     	<a href="test/ssm.html">测试 SSM 整合</a>
     ```

   - 使用 `base` 标签后，其他标签不可以使用 `/` ，使用的话 `base` 标签就没有用了

## Ajax 请求

### 介绍

1. 请求介绍
   - 普通请求：`controller` 处理，返回页面
   - Ajax：`controller` 处理，返回 `json`
2. Ajax 请求常用注解
   - `Requestbody` 将请求体转换成 java 类型
   - `ResponseBody` 将响应体转换成 `json` （java 转成 json）
   - 注解需要 `jackson` 包
   - 同时配置 `mvn:annotation-driven`

### Requestbody 注解的使用

1. 导入 `JQuery` 包

2. 发送数组到服务端

   - 发送数据到服务端

     ```javascript
     $(function(){
         $("#but1").click(function() {
             $.ajax({
                 "url":"send/array.html", // 请求资源地址
                 "type":"post",			 // 请求方式
                 "data":{
                     "array":[1, 3, 5]    // 请求数据
                 },		 
                 "dataType":"text",	     // 如何对待服务器返回的数据类型
                 "success":function(response) {  // 服务器处理成功后调用的会调函数，response 响应体
     
                 },
                 "error":function(response) {    // 服务器处理失败后调用的会调函数
     
                 }
             });
         });
     });
     ```

   - 接受 `json` 数据的格式 `"dataType:"application/json;charset=UTF-8"`

   - 服务端（参数数组 `array[]`有变化的）

     ```java
     @ResponseBody
     @RequestMapping(value = "/send/array.html")
     public String testReceiveArrayOne(@RequestParam("array[]") List<Integer> array) {
     
         for (Integer number : array) {
             System.out.println(number);
         }
         return "success";
     }
     ```

3. 使用 `RequestBody`

   - JQuery 代码

     ```javascript
     $("#but1").click(function() {
         var array = [1, 2, 3];
         // 将 json对象转换成 json字符串
         var requestBody = JSON.stringify(array);
         $.ajax({
             "url":"send/one.html",   // 请求资源地址
             "type":"post",			 // 请求方式
             "data":requestBody,      // 请求数据 
             "contentType":"application/json;charset=UTF-8", // 请求体数据类型
             "dataType":"text",       // 如何对待服务器返回的数据类型
             "success":function(response) {  // 服务器处理成功后调用的会调函数，response 响应体
                 alert(response);
             },
             "error":function(response) {    // 服务器处理失败后调用的会调函数
     
             }
         });
     });
     ```

   - controller 代码

     ```java
     @ResponseBody
     @RequestMapping(value = "/send/one.html")
     public String testReceiveArrayOne(@RequestBody List<Integer> array) {
     
         for (Integer number : array) {
             System.out.println(number);
         }
         return "success";
     }
     ```

### 规范 Ajax 请求的返回格式

1. 创建一个类，封装返回数据

   - 代码（属性）

     ```java
     public static final String SUCCESS = "SUCCESS";
     public static final String FAILED = "FAILED";
     
     // 当前请求处理状态（成功、失败）
     private String result;
     
     // 请求失败原因
     private String message;
     
     // 要返回的数据
     private T data;
     ```

## 异常映射

### 介绍

1. 使用异常映射机制对整个项目的异常和错误提示信息进行统一的管理

   - 补充（`spring-web-mvc.xml`）

     ```xml
     <!-- 当controller 只发生跳转页面时，可以简单替换 -->
     <!--
     	@RequestMapping(value = "xxx/xxx.html")
     	public String xxx() {
     		return "target";
     	}
     -->
     
     <!-- 替换 -->
     <mvc:view-controller path="xxx/xxx.html" view-name="target" />
     ```

2. 异常映射机制

   - 普通请求时，发生异常，则向上抛出，捕获异常返回特定页面
   - Ajax 请求时，发生异常，则向上抛出，捕获异常返回 jaon 数据（提示错误）

3. 异常捕获机制，则需要区分普通异常和 Ajax 异常

4. 异常处理机制分两种

   - 基于 XML 的异常映射
     - 配置文件发生错误
   - 基于注解的异常映射
     - 注解发生错误
   - 两种都需要使用

### 基于 XML 的异常映射

1. 在 `spring-web-mvc.xml` 中配置

2. 实例

   - 代码演示

     ```xml
     <!-- 基于 XML 的异常映射 -->
     <bean id="simpleMappingExceptionResolver" class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
         <!-- 配置异常类型与视图的对应关系 -->
         <property name="exceptionMappings">
             <props>
                 <!-- 视图：会拼接前后缀 -->
                 <prop key="java.lang.Exception">system-error</prop>
             </props>
         </property>
     </bean>
     ```

3. 获取异常信息

   - 在页面中

     ```jsp
     <!-- 打印异常信息 -->
     ${requestScope.exception.message }
     ```

### 基于注解的异常映射

1. 定义在工具类中

   - `CrowdUtil`
   - 会使用 `servlet-api` 依赖

2. 判断是否为普通请求还是 Ajax 请求

   - 判断请求头是否含有  `Accept:application/json` 或者 `X-Requested-With:XMLHttpRequest`

     ```java
     /**
     	 * 判断是否为普通请求或者是 Ajax 请求
     	 * @param request
     	 * @return
     	 * 	true 为 Ajax 请求
     	 * 	false 为 普通请求
     	 */
     public static boolean judgeRequestType(HttpServletRequest request) {
     
         String acceptHeader = request.getHeader("Accept");
         String xRequestHeader = request.getHeader("X-Requested-With");
     
         return (
             (acceptHeader != null && acceptHeader.contains("application/json"))
             || 
             (xRequestHeader != null && xRequestHeader.contains("XMLHttpRequest"))
         );
     }
     ```

   - Controller 层测试（`HttpServletRequest`）

     ```java
     boolean judgeRequestType = CrowdUtil.judgeRequestType(request);
     System.out.println(judgeRequestType);
     ```

3. 在 `/atcrowdfunding03-admin-component/src/main/java/com/shentree/crowd/mvc/config/CrowdExceptionResolver.java` 创建异常映射类

   - 简单示例

     ```java
     // @ControllerAdvice 表示基于注解的异常处理器的类
     @ControllerAdvice
     public class CrowdExceptionResolver {
     
     	// @ExceptionHandler 将具体异常与方法相关联
     	/**
     	 * 
     	 * @param exception 实际捕获异常类型
     	 * @param request   当前请求对象
     	 * @return
     	 * @throws IOException 
     	 */
     	@ExceptionHandler(value = NullPointerException.class)
     	public ModelAndView resolverNullPointerException(NullPointerException exception, 					HttpServletRequest request,
     			HttpServletResponse response) throws IOException {
     
     		// 判断当前请求是什么类型（普通请求、Ajax 请求）
     		boolean requestType = CrowdUtil.judgeRequestType(request);
     
     		// Ajax 请求
     		if (requestType) {
     			// 创建 ResultEntity 对象
     			// 信息为异常信息
     			ResultEntity<Object> resultEntity = ResultEntity.failed(exception.getMessage());
     			// 创建 Json 对象
     			Gson gson = new Gson();
     			// 将 ResultEntity 转换成 Json 对象
     			String json = gson.toJson(resultEntity);
     			// 将 Json 字符串作为响应体返回给客户端（HttpServletResponse）
     			response.getWriter().write(json);
     			// 以通过 Response 返回了 Json 字符串，不需要返回 ModeAndView
     			return null;
     		}
     		
     		// 普通请求
     		ModelAndView modelAndView = new ModelAndView();
     		// 将异常放入 ModelAndView
     		modelAndView.addObject("exception", exception);
     		// 设置对应的视图
     		modelAndView.setViewName("system-error");
     		// 返回 ModelAndView
     		return modelAndView;
     	}
     }
     ```

4. 对上诉的异常映射类进行优化

   - 优化代码

     ```java
     // @ControllerAdvice 表示基于注解的异常处理器的类
     @ControllerAdvice
     public class CrowdExceptionResolver {
     
     	// @ExceptionHandler 将具体异常与方法相关联
     	/**
     	 * 
     	 * @param exception 实际捕获异常类型
     	 * @param request   当前请求对象
     	 * @return
     	 * @throws IOException 
     	 */
     	@ExceptionHandler(value = NullPointerException.class)
     	public ModelAndView resolverNullPointerException(NullPointerException exception, 					HttpServletRequest request,
     			HttpServletResponse response) throws IOException {
             // 异常处理跳转页面
     		String viewName = "system-error";
     		return commonrResolverException(viewName, exception, request, response);
     	}
     	
     	/**
     	 * 异常映射公共处理方法
     	 * @param viewName
     	 * @param exception
     	 * @param request
     	 * @param response
     	 * @return
     	 * @throws IOException
     	 */
     	private ModelAndView commonrResolverException(String viewName, Exception exception, 				HttpServletRequest request,
     			HttpServletResponse response) throws IOException {
     
     		// 判断当前请求是什么类型（普通请求、Ajax 请求）
     		boolean requestType = CrowdUtil.judgeRequestType(request);
     
     		// Ajax 请求
     		if (requestType) {
     			// 创建 ResultEntity 对象
     			// 信息为异常信息
     			ResultEntity<Object> resultEntity = ResultEntity.failed(exception.getMessage());
     			// 创建 Json 对象
     			Gson gson = new Gson();
     			// 将 ResultEntity 转换成 Json 对象
     			String json = gson.toJson(resultEntity);
     			// 将 Json 字符串作为响应体返回给客户端（HttpServletResponse）
     			response.getWriter().write(json);
     			// 以通过 Response 返回了 Json 字符串，不需要返回 ModeAndView
     			return null;
     		}
     		
     		// 普通请求
     		ModelAndView modelAndView = new ModelAndView();
     		// 将异常放入 ModelAndView
     		modelAndView.addObject("exception", exception);
     		// 设置对应的视图
     		modelAndView.setViewName(viewName);
     		// 返回 ModelAndView
     		return modelAndView;
     	}
     }
     ```

## 创建常量类

### 作用

1. 创建该类用于存放常量
   - 对项目属性，进行统一管理
2. 位置
   - `/atcrowdfunding05-common-util/src/main/java/com/shentree/crowd/util/CrowdConstant.java`

### 例

1. 将上面的 `eception` 字符串放入常量类中

2. 例如

   - 代码

     ```java
     package com.shentree.crowd.util;
     
     public class CrowdConstant {
     
         public static final String ATTR_NAME_EXCEPTION = "exception";
     }
     ```

   - 异常映射类，则可以使用常量来表示 `exception`

     ```java
     /*
     		// 将异常放入 ModelAndView
     		modelAndView.addObject("exception", exception);
     */
     
     // 将异常放入 ModelAndView
     modelAndView.addObject(CrowdConstant.ATTR_NAME_EXCEPTION, exception);
     ```

3. 实例

   - 代码

     ```java
     package com.shentree.crowd.util;
     
     public class CrowdConstant {
     
         public static final String ATTR_NAME_EXCEPTION = "exception";
     
         public static final String MESSAGE_LOGIN_FAILED = "抱歉！账号密码错误！清重新输入！";
     
         public static final String MESSAGE_LOGIN_ACCT_ALREADY_IN_USER = "账号已被使用！";
     
         public static final String MESSAGE_ACCESS_FORBIDEN = "登陆后访问！";
     }
     ```

## 管理员登陆界面

### 搭建前端开发环境

1. 引入前端的页面
   - 将静态资源拷贝到 `webapp` 下

### 创建后台管理员登陆页面

1. 创建 `admin-login.jsp` 页面，放入`WE-INF` 下

2. 将对应的 `html` 文件拷贝到 `admin-login.jsp`
   - 修改页面的字符编码 `UTF-8`
   - 创建 `<base>` 标签
   
3. 设置表单的 属性

   - `action`

   - `method`

   - 输入表格设置 `name` 属性

     1. `name="loginAcct"`
     2. `name="userPswd"`

   - 将 `a` 标签换成 `button` 标签

     ```html
     <button type="submit" class="btn btn-lg btn-success btn-block">登陆</button>
     ```

### 如何跳转到这个页面

1. 跳转到此页面不需要带任何的参数

   - 没有携带数据，则不需要使用 `Controller` 处理器

2. 使用配置文件

   - 在 `spring-web-mvc.xml` 配置文件中配置 `view controller`

   - 直接将请求地址将视图名称关联起来

   - 示例

     ```xml
     <!-- 配置 View Controller -->
     <!-- 
       @RequestMapping(value = "/admin/to/page.html")
       -->
     <mvc:view-controller path="/admin/to/login/page.html" view-name="admin-login"/>
     ```

### 引入 layer

1. 有工具包，直接解压就可以

   - layer 会使用 JQuery 的技术，所以引入 layer 需要在引入 JQuery 之后

   - 引入

     ```html
     <script type="text/javascript" src="jquery/jquery-2.1.1.min.js"></script>
     <script type="text/javascript" src="layer/layer.js"></script>
     ```

2. 使用

   - 按钮事件代码

     ```javascript
     $("#but3").click(function() {
     layer.msg("layer提示弹框");
     });
     ```

   - 按钮代码

     ```html
     <button id="but3">layer</button>
     ```

### 修改 `system-error.jsp` 页面

1. 对异常页面进行优化（使用管理员登陆页面样式）
2. 优化一
   
   - 返回上一步
   
   - HTML 代码
   
     ```html
     <button id="btn01">返回上一步</button>
     ```
   
   - Js 代码
   
     ```javascript
     $(function() {
         $("#btn01").click(function() {
             // 相当于浏览器的后退
             window.history.back();
         });
     });
     ```

### 对密码进行加密

1. 使用 `MD5` 对密码进行加密

2. 创建工具方法，对密码进行加密

3. 创建工具类的位置

   - /atcrowdfunding05-common-util/src/main/java/com/shentree/crowd/util/CrowdUtil.java`

   - `MD5` 加密算法

     ```java
     /**
     	 * MD5 加密算法
     	 * @param source
     	 * 	待加密字符串
     	 * @return
     	 * 	返回字符串
     	 */
     public static String md5(String source) {
         // 判断是否为有效字符串
         if (source == null || source.length() == 0) {
             throw new RuntimeException(CrowdConstant.MESSAGE_STRING_INVAILDATE);
         }
     
         try {
             // 获取 MessageDigest 对象
             String algorithm = "md5";
             MessageDigest instance = MessageDigest.getInstance(algorithm);
     
             // 获取 source 字节数据
             byte[] sourceBytesInput = source.getBytes();
             // 加密
             byte[] sourceBytesOutput = instance.digest(sourceBytesInput);
     
             // 创建 BigInteger
             int signum = 1;
             BigInteger bigInteger = new BigInteger(signum , sourceBytesOutput);
     
             // 将 bigInteger 按照16进制转换为字符串
             int radix = 16;
             String encoded = bigInteger.toString(radix);
     
             return encoded;
         } catch (NoSuchAlgorithmException e) {
             e.printStackTrace();
         }
         return null;
     }
     ```



### 创建自定义异常

1. 创建位置

   - `/atcrowdfunding05-common-util/src/main/java/com/shentree/crowd/util/LoginFailedException.java`
   - 继承 `RuntimeException` 异常

2. 主要使用父类的构造器

   - 实例

     ```java
     public LoginFailedException(String message) {
         super(message);
     }
     ```

3. 在 `com.shentree.crowd.mvc.config.CrowdExceptionResolver` 中的异常处理中修改

   - 定义登陆失败的异常处理机制

   - 实例

     ```java
     /**
     	 * 
     	 * @param exception
     	 * 	登陆异常
     	 * @param request
     	 * @param response
     	 * @return
     	 * @throws IOException
     	 */
     @ExceptionHandler(value = LoginFailedException.class)
     public ModelAndView resolverLoginException(LoginFailedException exception, HttpServletRequest request,
                                                HttpServletResponse response) throws IOException {
         // 登陆失败回到登陆页面
         String viewName = "admin-login";
         return commonrResolverException(viewName, exception, request, response);
     }
     ```

4. 在登陆页面显示异常消息

