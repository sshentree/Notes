# Mybatis 笔记

__说明：[参考地址](https://mybatis.org/mybatis-3/zh/getting-started.html)__

## Mybatis 介绍

### 和其他持久层技术对比

1.   JDBC
     -   SQL 夹杂在 java 代码中耦合读高，导致硬编码内伤
     -   维护不易且实际开发中 SQL 有变化，频繁修改的情况多见
     -   代码冗余，开发效率低
2.   Hibernate 和 JPA
     -   操作简单，开发效率高
     -   程序中长难的 SQL 需要绕过框架
     -   框架自动生成的 SQL ，不容易做特殊优化
     -   基于全映射的自动框架，大量字段的 POJO （Plain Old Java Object，简单的对象）进行部分映射时比较困难
     -   反射操作太多，导致数据库性能下降
3.   Mybatis
     -   轻量级，性能出色
     -   SQL 和 java 编码分开，功能边界清晰。java专注业务，SQL 专注数据
     -   开发效率略低于 Hibrenate，但是完全可以接受

## 环境搭建

### 导入 `jar` 包

1. `Mybatis.jar` ：持久层框架
2. `log4j.jar`  ：日志操作包（log for java）
   - 配置文件 `log4j.xml` （上网百度一个就可以）
   - 支持 `.properties`
3. `JDBC` ：链接数据库工具（mysql-connector-java-8.0.20.jar）

### 创建 Mybatis 全局配置（核心）文件

1. 主要内容
   - 如何连接数据库，不包含 SQL 语句
   - 配置文件通用名称 `mybatis-config.xml`
   
2. 如何创建配置文件

   - 创建 `mybatis-config.xml` 文件，并复制 mybatis 的说明文档的配置实例
   - 说明文档正文第 3 页

3. 解析配置文件

   - 配置文件说明 __（标签有顺序）__

     ```xml
     <?xml version="1.0" encoding="UTF-8" ?>
     <!-- 规定 xml 文件中，可以书写那种标签，与命名空间相同-->
     <!DOCTYPE configuration
     PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
     "http://mybatis.org/dtd/mybatis-3-config.dtd">
     
     <!-- configuration 是根标签，与 DOCTYPE configuration 保持相同-->
     <configuration>
         
         <!-- 设置或引用 jdbc.properties 文件，连接数据的配置信息-->
         <!-- resource 引用类路径下，url 引用磁盘或磁盘路径下访问-->
         <properties resource="jdbc.properties"/>
         <!-- 或者配置变量-->
         <!--
     	<properties>
         	<property name="jdbc.driver" value="com.mysql.jdbc.Driver"></property>
         </properties>
     	-->
         <!-- 部分引用jdbc.properties，部分使用配置变量 -->
         <!--
         <properties resource="jdbc.properties">
             <property name="username" value="xxx"/>
             <property name="password" value="xxx"/>
         </properties>
         -->
         
         <!-- 用来设置连接数据库的环境，属性 default 设置默认使用的数据库。标签 envitonment 可以有多个，id 就是 environments 的属性 default 的值-->
         <environments default="development">
             
             <!-- 某一个具体的数据库环境，属性 id 为唯一标识-->
             <environment id="development">
                 
                 <!-- type=(JDBC|MANAGED)-->
                 <!--事务管理器，type 生命是谁管理：JDBC 是使用最原始的事务管理方式管理手动提交（ autocommit();）MANANGED 是能管理谁管理，提交回滚自己完成-->
                 <transactionManager type="JDBC" />
                 
                 <!-- type=(POOLED|UNPOOLED|JNDI)-->
                 <!-- 使用数据库连接池，不使用数据库连接池，使用上下文中的数据源-->
                 <dataSource type="POOLED">
                     
                     <!-- 使用上述引用 .properties 文件的值，或者使用变量 -->
                     <!-- 使用的是 .properties 文件的值 -->
                     <property name="driver" value="${jdbc.driver}" />
                     <property name="url" value="${jdb.url}" />
                     <property name="username" value="${jdbc.username}" />
                     <property name="password" value="${jdbc.password}" />
                     
                     <!-- 使用配置变量 -->
                     <!--
                     <property name="username" value="${username}" />
                     <property name="password" value="${password}" />
     				-->
                     
                 </dataSource>
             </environment>
         </environments>
         <mappers>
             <!-- 引入配置文件，根据以后的 XXXMapper 对应书写-->
             <mapper resource="org/mybatis/example/BlogMapper.xml" />
         </mappers>
     </configuration>
     ```
   
- 此配置文件如何有提示信息(标签提示)
  
     1. 在 `mybatis.jar -> bulider -> XML` 复制 `mybatis-3-config.dtd` ，这是此配置文件的标签库（`/org/apache/ibatis/builder/xml/mybatis-3-mapper.dtd` 是 映射文件的标签库）
     2. 在 window 中设置查询 `XML Catalog` 配置标签库(加在 user 标签库中)
  3. 一种加入 `public id` `-//mybatis.org//DTD Config 3.0//EN` ；一种加入 `URI` `http://mybatis.org/dtd/mybatis-3-config.dtd` 。
  
- mybatis 配置文件中的配置的顺序性
  
     -   ```xml
         <!--
          properties?,settings?,typeAliases?,typeHandlers?,objectFactory?,
          objectWrapperFactory?,reflectorFactory?,plugins?,environments?,
          databaseIdProvider?,mappers?)".
          -->
      ```
  
- 配置数据库连接池
  
     ```xml
     <dataSource type="POOLED">
         <property name="driver" value="com.mysql.cj.jdbc.Driver" />
         <property name="url" value="jdbc:mysql://localhost:3306/ssm" />
         <property name="username" value="root" />
         <property name="password" value="123" />
     </dataSource>
  ```

- `setting` 设置

     ```xml
     <configuration>
         <settings>
             
             <!-- 将下划线的命名，转换为驼峰命名，不怎么需要使用-->
             <!-- 解决属性与字段不匹配问题-->
             <setting name="mapUnderscoreToCamelCase" value="true"></setting>
             
             <!-- 优先使用 log4j 日志-->
             <setting name="logImpl" value="LOG4J"/>
         </settings>
     ```

- `tyepAliases` 为 Java 类型起别名，用别名代替全类名（不建议使用）

     ```xml
     <typeAliases>
         <!-- type 为 Java 类型-->
         <!-- 这样写就可以，默认别名是（user\User），不区分大小写-->
         <!-- 可以在映射文件中（UserMapper.xml），使用别名-->
         <typeAlias type="model.bean.User" />
         
         <!-- 指定别名（指定的是什么加） -->
         <!--
         <typeAlias type="model.bean.User" alias="User"/>
     	-->
         
         <!-- 包下所有的类都有别名-->
         <package name="model.bean"/>
     </typeAliases>
     ```


### 创建映射文件，并配置

1. 主要内容
   - 如何操作数据库，包含 SQL 语句
   
   - 映射文件名称 `XXXMapper.xml` (`XXX` 表示 Java 的对象)
   
   - 复制说明文档的正文 4 页 的 2.1.5（也可以配置标签提示）
   
     1. 实例
   
        ```xml
        <?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
        <mapper namespace="org.mybatis.example.BlogMapper">
            <select id="selectBlog" resultType="Blog">
                select * from Blog where id = #{id}
            </select>
        </mapper>
        ```
   
2. 创建映射文件的步骤

   - 创建数据库表

     1. SQL 代码（实体的各个属性）

        ```sql
        CREATE TABLE `user` (
          `uid` int(11) NOT NULL AUTO_INCREMENT,
          `user_name` varchar(255) NOT NULL,
          `password` varchar(50) NOT NULL,
          `age` int(11) NOT NULL,
          `sex` varchar(20) NOT NULL,
          PRIMARY KEY (`uid`)
        ) ENGINE=InnoDB DEFAULT CHARSET=utf
        ```

   - 创建 JAVA 的实体类（bean 包）

     1. 属性代码

        ```java
        private Integer uid;
        private String userName;
        private String password;
        private Integer age;
        private String sex;
        ```

     2. __数据库表中字段，与实体类对应不上，可以使用别名__

   - 创建mybatis 的映射文件 `UserMapper.xml` ，__先复制上述的内容__

     1. SQL 语句（更具 id 查找 User 的实体类对象）

        ```xml
        <!-- 命名空间-->
        <mapper namespace="org.mybatis.example.BlogMapper">
            <select id="selectBlog" resultType="Blog">
                <!-- 修改查询的数据库，及字段名称-->
                select * from user where uid = #{id}
            </select>
        </mapper>
        ```

     2. 获取形参时 `#{id}` ，底层使用 `?` 通配符

     3. 获取参数时 `${value}` ，底层使用字符串拼接

     4. 模糊查询，批量删除必须使用 __字符串拼接__

### 创建 Mapper 接口，实现两个绑定

1. 创建接口 `UserMapper` （mapper 包）

   - 代码

     ```java
     import model.bean.User;
     
     public interface UserMapper {
         public abstract User getUserByUid(String uid);
     }
     ```

2. 实现两个绑定之一

   - 绑定映射文件与接口之间的绑定

   - 将接口的全限名，绑定在映射文件的命名空间中，复制到 `<mapper namespace="">`

     ```xml
     <mapper namespace="model.mapper.UserMapper">
     </mapper>
     ```

3. 绑定之二

   - 将接口的方法名，与 `<select id="">` 进行绑定

     ```xml
     <select id="getUserByUid">
     </select>
     ```

   - 接口的方法名为 `getUserByUid()`

   - 将方法的返回值类型，与 `<select id="" resultType=""> ` 绑定

     ```xml
     <select id="getUserByUid" resultType="model.bean.User">
     	
     </select>
     ```

4. __修改核心配置文件的映射对象__

   - 实例（以单个映射文件引入）

     ```xml
     <mappers>
         <!--映射文件全路径 src/main/resources/mapper/UserMapper.xml-->
         <mapper resource="mapper/UserMapper.xml" />
     </mappers>
     ```
     
   - 实例（以包为单位引入映射文件）
   
     ```xml
     <mappers>
         <!--
     		以包为单位引入映射文件，要求：
     			1.mapper接口所在的包要和映射文件所在的包一致
     			2.mapper接口要和映射文件的名字一致
     		映射文件的两种存放方式：
     			1.将mapper映射文件放在mapper包下
     			2.将mapper映射文件放入resource中，再起一个与mapper接口包路径一致的文件加
     	-->
         <package name="com.sshen.mapper"/>
     </mappers>
     ```
   
     

### 获取 Mybatis 操作数据库的会话对象 `Sqlsession` 并测试

1. 创建 Java 的测试单元

   - 代码（回抛出 `IO` 异常，获取文件流）

     ```java
     // 根据核心配置文件获取资源的输入流
     InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
     // 创建 SqlSessionFactory（接口），根据 SqlSessionFactoryBuilder.build() 和资源输入流
     SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
     // 根据 SqlSessionFactory 创建 SqlSession
     SqlSession sqlSession = sqlSessionFactory.openSession();
     
     // 根据 XXXMapper 接口，创建 XXXMapper 对象
     UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
     // 调用接口的方法
     User user = userMapper.getUserByUid("1");
     
     System.out.println(user);
     ```

2. __`getMapper(Class)` 核心方法 ，为何一个接口，可以创建一个实体类对象呢？，接口无法创建实体类__

   - 根据动态代理，动态生成 `UserMapper` 的代理实现类

## 实例讲解

### 实现增删改查的实例

1. 步骤一：引入 JAR 包

   - `mybatis.jar`
     1. 配置文件 `mybatis-config.xml`
     2. 使用 `<properties>` 标签引入 `jdbc.properties` 数据库连接信息
     3. 使用 `<settings>` 优先使用 log4j 日志工具
   - `log4j.jar`
     1. 配置文件 `log4j.xml`
   - `jdbc.jar`
     1. 配置文件 `jdbc.properties`

2. 步骤二：创建数据库表（emp 表）

   - 定义字段名称

3. 步骤三：创建 Java 对象（Emp 类）

   - 根据数据库表中字段，创建属性值

     1. 代码

        ```java
        private Integer eid;
        
        private String ename;
        
        private Integer age;
        
        private String sex;
        ```

4. 步骤四：创建 `XXXMapper.java` 接口

   - 根据实际需要编写对应的数据库查询语句

     1. 代码

        ```java
        // 根据 eid 查询员工信息
        public abstract Emp getEmpByEid(String eid);
        
        // 获取所有员工信息
        public abstract List<Emp> getAllEmp();
        
        // 添加员工信息
        public abstract void addEmp(Emp emp);
        
        // 修改员工信息
        public abstract void updateEmp(Emp emp);
        
        // 删除员工信息
        public abstract void deleteEmp(String eid);
        ```

5. 步骤五：创建 `XXXMapper.xml` 配置文件

   - 向 `mybatis-config.xml` 注册 `<XXXMapper.xml>`

     1. 代码

        ```xml
        <mappers>
            <mapper resource="EmpMapper.xml"/>
            <mapper resource="DeptMapper.xml"/>
        </mappers>
        ```

   - 根据 `XXXMapper.java` 接口，完成 `XXXMapper.xml` 的编写 __（绑定类，绑定方法）__

     1. 代码

        ```xml
        <mapper namespace="model.mapper.EmpMapper">
            <!-- 查询，返回单个实体-->
            <select id="getEmpByEid" resultType="model.bean.Emp">
                select eid, ename, age, sex from emp where eid = #{eid}
            </select>
        	<!-- 查询，返回集合-->
            <select id="getAllEmp" resultType="model.bean.Emp">
                select eid, ename, age, sex from emp
            </select>
        
            <insert id="addEmp" >
                insert into emp values(null, #{ename}, #{age}, #{sex})
            </insert>
        
            <update id="updateEmp">
                update emp set ename = #{ename}, age = #{age}, sex = #{sex} where eid = #{eid}
            </update>
        
            <delete id="deleteEmp">
                delete from emp where eid = #{eid}
            </delete>
        </mapper>
        ```

   - __方法返回值说明__

     1. 查询，返回单个实体对象时，使用类名接受
     2. 查询，返回实体集合时，也使用类名接受
     3. 更新（增加、删除、修改），底层的方法返回 `true\false` 表示操作成功；还有返回受影响的条数（`int`），这样固定的返回值，__可以不写返回值类型。__

### 测试实例

1. 测试代码

   - 更新数据库，必须自己手动提交（因为 `mybatis-config.xml` 设定的事务管理为 `JDBC` ，原生的事务管理），**如果没有提交事物，并且主键是自增，那么没有提交的数据也会占用主键，下次的数据会跳过已被占用的主键向后顺延**

     1. 提交代码

        ```java
        sqlSession.commit(); // 提交事务
        ```

     2. 创建 `SqlSession` 时，开启自动处理事务

        ```java
        SqlSession sqlSession = sqlSessionFactory.openSession(true);
        ```

2. 整体测试

   - 代码

     ```java
     InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
     
     SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
     // 设置为 true 自动提交事务
     SqlSession sqlSession = sqlSessionFactory.openSession(true);
     
     EmpMapper empMapper = sqlSession.getMapper(EmpMapper.class);
     
     // 根据 eid 查询
     //		Emp empByEid = empMapper.getEmpByEid("1");
     //		System.out.println(empByEid);
     
     // 获取所有员工
     //		List<Emp> list = empMapper.getAllEmp();
     //		System.out.println(list);
     
     // 添加员工
     //		empMapper.addEmp(new Emp(null, "tom", 201, "男"));
     //		sqlSession.commit();
     
     // 修改员工信息
     //		empMapper.updateEmp(new Emp(2, "王尔马", 33, "女"));
     
     // 删除数据
     empMapper.deleteEmp("8");
     ```

### 设置更新数据库的返回值

1. 增删改的返回值

   - `boolean` 操作是否成功
   - `Integer` 受影响的行数

2. 只需要将 `XXXMapper.java` 的数据库操作方法，响应的设置返回值 `Integer/boolean` ，mybatis 会对应的返回值

   - `XXXMapper.java` 设置返回值

     ```java
     // 删除员工信息
     public abstract Integer deleteEmp(String eid);
     ```

   - `XXXMapper.xml` 不需要修改，mybatis 会帮你完成

     ```xml
     <delete id="deleteEmp">
         delete from emp where eid = #{eid}
     </delete>
     ```

### 使用包管理，引入 `XXXMapper.xml`

1. `mybatis-config.xml` 会引入 `XXXMapper.xml` 文件，如果文件过多，会比较麻烦，所以将 `XXXMapper.xml` 放入包中，当然包是在 `conf` 中建立的
2. 此种写法前提条件 
   - __`XXMapper.java` 和 `XXXMapper.xml` 必须在同一包下__
   - 但是不建议这样写
   - __解决办法：在 `conf` 建立与 `XXXMapper.java` 相同的包__，在项目中，是分开的，但实际上，一个配置文件和一个接口还是在同一包下

### 使用 Map 集合，作为查询接受类型

1. 查询单个实体对象时，可以使用 `java.util.HashMap` 作为接受类型

   - 原因是，mybatis 产生映射关系，与 Map 集合特性相当，`XXXMapper.xml`

     ```xml
     <!-- 查询，返回单个实体-->
     <select id="getEmpByEid" resultType="java.util.HashMap">
         select eid, ename, age, sex from emp where eid = #{eid}
     </select>
     ```

   - 如查询的映射关系 `{eid:11,ename:tom, age:12, sex:男}` ，`XXXMapper.java`

     ```java
     public abstract Map<String, Object> getEmpByEid(String eid);
     ```

2. 查询返回多个实体对象时，也可以使用 `java.util.HashMap` 作为接受类型

   - 但是这样实际上是，每一个实体属性为一个 `value` ，但是如何设置 `key` ，`XXXMapper.xml`

     ```xml
     <!-- 查询，返回集合-->
     <select id="getAllEmp" resultType="java.util.HashlMap">
         select eid, ename, age, sex from emp
     </select>
     ```

   - 使用注解 `@MapKey("eid")` ，映射结构 `{eid1:Emp1,eid1:Emp2}`

     ```java
     @MapKey("eid")
     public abstract Map<String, Object> getAllEmp();
     ```

### parameterType=""

1. 在 `XXXMapper.xml` ，传递的参数类型

   - 实例

     ```xml
     <delete id="deleteEmp" parameterType="model.bean.Emp">
         delete from emp where eid = #{eid}
     </delete>
     ```

2. 声明传递的参数类型，但是不建议写，mybatis 有自动推断机制

### mybatis 获取参数值的两种方式

1. 方式（`XXXMapper.xml`）

   -   `${}`
       1. 当传递参数为单个 String 或字面量和包装类，名称必须相同，如果不同则可以使用 `${value/_parameter}`
       2. 本质：字符串拼接 （Statement）
   - `#{}` 
     1. 当传递参数为单个 String 或 字面量和包装类时，通配符的名称与参数的名称不同没有关系
     1. 本质：占位符赋值（PreparedStatement）
   - 当传递参数为 `bean` 时，`#{}/${}` 都可以根据属性名直接获取值
   - 当传递的参数为多个形参时，如 （String， String）
     1. 可以使用 `#{0}` `#{1}` 获取两个参数，还可以使用 `#{param1/param2}`
     2. 可以使用 `${param1/param2}` 获取参数，单引号问题
   - __当传入参数为多个时，mybatis 会将这些值放在 `Map` 中__
     1. key 为 `0, 1, 2, 3` value 为参数值
     2. key 为 `param1, param2` value 为参数中
   - 当传递参数为 `Map<String, Object>` 时
     1. 这样就可以根据自己设置的 key 的值，获取 value 参数值 `#{key1}` `#{key2}`
     2. `#{}/${}` 都可以获取参数值，注意单引号问题
     3. __使用 `@Param("key")` 不需要手动的添加 `Map` 集合中，`XXXMapper.java` 中，自动添加 Map 集合中__
   - 当传递参数为 `List/Array` 时，也会将其放入 `Map` 中
     1. 键值 `key` 为 `list/array`
   
1. 举例说明（5 种方式）

   -   mapper 接口方法的参数为单个的字面量类型
   
       1.   可以通过 `${}` 或者 `#{}` 以任意名称获取数值（建议：见名释义），但是需要注意 `${}` 的单引号问题
   
       2.   `${}` 如下，字符串拼接
   
            ```xml
            <!-- TUser getTUserByName(String userName); -->
            <select id="getTUserByName" resultType="com.sshen.bean.TUser">
                <!-- userName也可以换成aa -->
                select * from t_user where username = '${userName}'
            </select>
            
            <!-- 控制台日志 -->
            debug(BaseJdbcLogger.java:135) - ==>  Preparing: select * from t_user where username = 'admin'
            debug(BaseJdbcLogger.java:135) - ==> Parameters: 
            debug(BaseJdbcLogger.java:135) - <==      Total: 1
            TUser{id=26, userName='admin', password='123456', age=23, sex='man', email='admin@163.com'}
            ```
   
       3.   `#{}` 如下，占位符
   
            ```xml
            <!-- TUser getTUserByName(String userName); -->
            <select id="getTUserByName" resultType="com.sshen.bean.TUser">
                <!-- userName也可以换成aa -->
                select * from t_user where username = #{userName}
            </select>
            
            <!-- 控制台日志 -->
            debug(BaseJdbcLogger.java:135) - ==>  Preparing: select * from t_user where username = ?
            debug(BaseJdbcLogger.java:135) - ==> Parameters: admin(String)
            debug(BaseJdbcLogger.java:135) - <==      Total: 1
            TUser{id=26, userName='admin', password='123456', age=23, sex='man', email='admin@163.com'}
            ```
   
   -   **mapper 接口方式的参数为多个，此时 Mybatis 会将这些参数放在一个 map 中（以两种方式进行存储）<br> `{arg0 : value1, arg1:vaule2, param1 : value1, param2 : value2}` ，因此只需要通过 `${}` 或者 `#{}` 以键的方式访问即可，但是需要注意 `${}` 的单引号问题**
   
       1.   `${}` 如下，字符串拼接
   
            ```xml
            <!-- TUser checkLogin(String userName, String password); -->
            <select id="checkLogin" resultType="com.sshen.bean.TUser">
                <!-- wrong -->
                <!-- select * from t_user where username = `${userName}` and password = `${password}` -->
                <!-- right -->
                select * from t_user where username = '${arg0}' and password = '${arg1}'
            </select>
            
            <!-- 控制台 -->
            debug(BaseJdbcLogger.java:135) - ==>  Preparing: select * from t_user where username = 'admin' and password = '123456'
            debug(BaseJdbcLogger.java:135) - ==> Parameters: 
            debug(BaseJdbcLogger.java:135) - <==      Total: 1
            TUser{id=26, userName='admin', password='123456', age=23, sex='man', email='admin@163.com'}
            ```
   
       2.   `#{}` 如下，占位符
   
            ```xml
            <!-- TUser checkLogin(String userName, String password); -->
            <select id="checkLogin" resultType="com.sshen.bean.TUser">
                <!-- wrong -->
                <!-- select * from t_user where username = #{userName} and password = #{password} -->
            
                <!-- right -->
                <!-- select * from t_user where username = #{param1} and password = #{[param2} -->
                <!-- select * from t_user where username = #{arg0} and password = #{arg1} -->
            </select>
            
            <!--控制台 -->
            debug(BaseJdbcLogger.java:135) - ==>  Preparing: select * from t_user where username = ? and password = ?
            debug(BaseJdbcLogger.java:135) - ==> Parameters: admin(String), 123456(String)
            debug(BaseJdbcLogger.java:135) - <==      Total: 1
            TUser{id=26, userName='admin', password='123456', age=23, sex='man', email='admin@163.com'}
            ```
   
   -   接上回知识点，“mapper 接口方式的参数为多个，Mybatis 将参数设置成 `map` “ 其中 key 为 Mybatis 自动设置。我们也可以手动设置 key，将参数手动封装成 map，通过`#{}` 或者 `${}` 以键的方式访问。
   
       1.   都参数时，手动封装成map
   
            ```xml
            <!-- TUser checkLoginByMap(Map<String, Object> map); -->
            <select id="checkLoginByMap" resultType="com.sshen.bean.TUser">
                select * from t_user where username = #{userName} and password = #{password}
            </select>
            
            <!--
            		// 获取Session
                    SqlSession sqlSession = SqlSessionUtil.getSqlSession();
            		// mapper接口
                    ParameterMapper parameterMapper = sqlSession.getMapper(ParameterMapper.class);
            		// 定义参数，封装成map
                    Map<String, Object> map = new HashedMap<>();
                    map.put("userName", "admin");
                    map.put("password", "123456");
            		// 接口调用
                    TUser tUser = parameterMapper.checkLoginByMap(map);
                    System.out.println(tUser);
            -->
            ```
   
   -   mapper 接口方式的参数是**实体类**类型的参数，只需要通过 `#{}` 或者 `${}`  以属性名称访问属性即可
   
       1.   实体类类型参数，通过属性名访问属性值（屏蔽属性的 `set/get` 方法后，不受影响）
   
            ```xml
            <!-- int insertTUser(TUser tUser); -->
            <insert id="insertTUser">
                insert into t_user values(null, #{userName}, #{password}, #{age}, #{sex}, #{email})
            </insert>
            
            <!--
            	参数为实体类对象，使用#{属性名}
            -->
            ```
   
   -   使用 `@Param` 注解命名参数
   
       1.   **mapper 接口定义接口时，使用 `@Param` 注解，Mybatis 会以 `@Param` 注解变量 `value` 的值当 map 的 key，参数 `userName` 的值作为 value，注意：还是会以 `param1` 作为 key**
   
            ```java
            TUser checkLoginByParam(@Param(value = "userName") String userName, @Param(value = "password") String password);
            ```
   
       2.   mapper 的 xml 文件
   
            ```xml
            <!-- TUser checkLoginByParam(@Param(value = "userName") String userName, @Param(value = "password") String password); -->
            <select id="checkLoginByParam" resultType="com.sshen.bean.TUser">
                select * from t_user where username = #{userName} and password = #{password}
            </select>
            ```
   
2. `#{}` 可以使用通配符设置 SQL 语句的参数，所以此方式获取 SQL 语句的是 `PreparedStatement`

3. `${}` 使用 `Statement` 

   - __只支持拼接 SQL，注意引号的使用__
   - 模糊查询、批量删除必须使用此方法

4. 在 mybatis 中表现形式

   - `#{}` 使用的是通配符，不需要管单引号

     ```xml
     <update id="updateEmp">
         update emp set ename = #{ename}, age = #{age}, sex = #{sex} where eid = #{eid}
     </update>
     ```

   - `${}` 需要手动拼接，注意单引号的使用，`Statement` 会将双引号去掉，字符串需要手动添加单引号

     ```xml
     <update id="updateEmp">
         update emp set ename = '${ename}', age = ${age}, sex = '${sex}' where eid = ${eid}
     </update>
     ```


### 主键生成方式，获取主键值

1. 在添加操作时，获取主键的 `id` 

   - 在多对一时，如何获取多的id（员工表、部门，需要获取部门的 id，来对应员工的部门的 `id`）

   - JDBC 原生的写法

     ```java
     class.forName("");
     Connection conn = DriverManager.getConnection("", "", "");
     PreparedStatement ps = conn.preparedStatement("inert into emp values()", 1);
     ps.setString(1, "root");
     ps.setString(2, "23");
     ps.setString(3, "男");
     ps.executeUpdate(); // 执行
     
     ResultSet rs = ps.getGenerateKeys();
     rs.next();
     int id = rs.getInt(1); // 获取插入主键 id
     ```

2. mybatis 写法

   - 代码

     ```xml
     <insert id="addEmp" useGeneratedKeys="true" keyProperty="eid">
         insert into emp values(null, #{ename}, #{age}, #{sex})
     </insert>
     ```

   - 属性 `useGeneratedKeys=""` ，获得自动生成的主键

   - 属性 `keyProperty=""` ，获取的主键赋值给谁，形参传递 `emp` 对象，将主键获取给 `emp.eid`

   - 实现代码

     ```java
     // 添加员工
     Emp emp = new Emp(null, "jack", 201, "男");
     empMapper.addEmp(emp);
     System.out.println(emp.getEname());		
     ```

## 表关系

### 表的建立

1. 两个表不需要主外键关联

### 解决查询结果映射问题

1. 问题描述

   - 当表关联时，不需要建立主外键关系
   - emp 表中，有 dept 表的 did，java 实体类中的属性为 `Dept`
   - 现查询员工信息，和所在部门的名称
   - 查询结果返回值中，如果使用 emp 类接受，则部门的名称不能匹配
   - 所以待解决问题

2. 解决方案

   - 自定义定义映射关系，使用 `<resultMap>`  标签 ，处理复杂的表关系

     1. 代码

        ```xml
        <resultMap type="model.bean.Emp" id="empMap">
            <!-- 主要表的主键-->
            <id column="eid" property="eid"/>
            <result column="ename" property="ename"/>
            <result column="age" property="age"/>
            <result column="sex" property="sex"/>
            <!-- dept 表的映射 dept 为java bean属性-->
            <result column="did" property="dept.did"/>
            <result column="dname" property="dept.dname"/>
        </resultMap>
        
        <select id="getAllEmp" resultMap="empMap">
            select e.eid, e.ename, e.age, e.sex, d.did d.dname from emp e left join dept d on e.did = d.did 
        </select>
        ```
        
     2. 方案二
     
        ```xml
        <resultMap type="model.bean.Emp" id="empMap">
            <!-- 主要表的主键-->
            <id column="eid" property="eid"/>
            <result column="ename" property="ename"/>
            <result column="age" property="age"/>
            <result column="sex" property="sex"/>
            <!-- dept 表的映射-->
        	<association property="dept" javaType="model.bean.Dept">
                <id column="did" property="did"/>
                <result column="dname" property="did"/>
            </association>
        </resultMap>
        
        <select id="getAllEmp" resultMap="empMap">
            select e.eid, e.ename, e.age, e.sex, d.did d.dname from emp e left join dept d on e.did = d.did 
        </select>
        ```
   

### 多对一的分步查找

1. 先查询员工信息，按照员工 `did` 查询部门信息

   - 先在 `DeptMapper.java` 写函数

   - `DeptMapper.xml` 写相应的 SQL 语句

   - 在配置 `EmpMapper.java/EmpMapper.xml`

   - 配置文件
   
     ```xml
     <resultMap type="model.bean.Emp" id="empMap">
         <!-- 主要表的主键-->
         <id column="eid" property="eid"/>
         <result column="ename" property="ename"/>
         <result column="age" property="age"/>
         <result column="sex" property="sex"/>
         <!-- select 接口的权限命名 + 方法名；column 的分步查询的条件-->
     	<association property="dept" select="model.mapper.DeptMapper.方法名" column="did">
             <id column="did" property="did"/>
             <result column="dname" property="dname"/>
         </association>
     </resultMap>
     
     <!-- 查询必须有 did-->
     <select id="getAllEmp" resultMap="empMap">
         select eid, ename, age, sex, did from emp
     </select>
      <!-- 返回结果 [Emp [eid=1, ename=张三, age=20, sex=男, dept=Dept [did=1, dname=人事部]]}-->
     ```
   

### 分步查询延迟加载

1. 延迟加载，只对分布查询有效
   
   - 懒加载不可用，默认为 `false`
   
   - 代码
   
     ```xml
     <settings>
         <!-- 将 user_name 映射为 userName 下划线映射为驼峰 -->
         <setting name="mapUnderscoreToCamelCase" value="true"></setting>
         <setting name="logImpl" value="LOG4J" />
         <!-- 开启延迟加载 默认为 false -->
         <setting name="lazyLoadingEnable" value="true"/>
         <!-- 当开启懒加载时，是否还查询所有的信息，默认为 true -->
         <setting name="aggressiveLazyLoading" value="false"/>
     </settings>
     ```
   
   - 其中 `lazyLoadingEnable` 和 `aggressiveLazyLoading` 必须是相反值
   
2. 作用是，当使用分步查询时，查询出结果，没有使用其他表的信息时，将不会执行分步查询的另一个 SQL 语句

   - 当结果只是用了 `ename` 属性时

     ```java
     EmpMapper empMapper = sqlSession.getMapper(EmpMapper.class);
     List<Emp> allEmp = empMapper.getAllEmp();
     System.out.println(allEmp.get(0).getEname()); // 只是用了 ename 属性
     ```

   - 执行的 SQL 语句

     ```xml
     DEBUG 08-12 14:11:26, 882 ==>  Preparing: select eid, ename, age, sex, did from emp (BaseJdbcLogger.java:137)
     DEBUG 08-12 14:11:26, 993 ==> Parameters:  (BaseJdbcLogger.java:137)
     DEBUG 08-12 14:11:27, 135 <==      Total: 5 (BaseJdbcLogger.java:137)
     张三
     ```

3. 当使用了其他表的信息时

   - 当结果使用了 `dept`

     ```java
     EmpMapper empMapper = sqlSession.getMapper(EmpMapper.class);
     List<Emp> allEmp = empMapper.getAllEmp();
     System.out.println(allEmp.get(0).getDept());
     ```

   - 执行了语句

     ```xml
     DEBUG 08-12 14:14:39, 404 ==>  Preparing: select eid, ename, age, sex, did from emp (BaseJdbcLogger.java:137)
     DEBUG 08-12 14:14:39, 539 ==> Parameters:  (BaseJdbcLogger.java:137)
     DEBUG 08-12 14:14:39, 745 <==      Total: 5 (BaseJdbcLogger.java:137)
     DEBUG 08-12 14:14:39, 746 ==>  Preparing: select did, dname from dept where did = ? (BaseJdbcLogger.java:137)
     DEBUG 08-12 14:14:39, 748 ==> Parameters: 1(Integer) (BaseJdbcLogger.java:137)
     DEBUG 08-12 14:14:39, 758 <==      Total: 1 (BaseJdbcLogger.java:137)
     Dept [did=1, dname=人事部]
     ```

### 一对多

1. Java 的 bean 部门对应员工信息

   - 将 Java bean 中的部门类，添加属性 `List<Emp>` 添加 `set\get` 方法
   - 使用 `<ccollection>` 标签，处理一对多，和多对多的问题

2. `DempMapper.java` 接口的方法

   - 代码

     ```java
     // 多对以查询
     public abstract Dept getDeptEmpsById(String did);
     ```

3. `DeptMapper.xml` 的写法

   - 代码

     ```xml
     <resultMap type="model.bean.Dept" id="DeptEmps">
         <id column="did" property="did"/>
         <result column="dname" property="dname"/>
         
         <!-- 处理一对多 ofType 集合存储的类型 -->
         <collection property="emps" ofType="model.bean.Emp">
             <id column="eid" property="eid"/>
             <result column="ename" property="ename"/>
             <result column="age" property="age"/>
             <result column="sex" property="sex"/>
         </collection>
         
     </resultMap>
     
     <select id="getDeptEmpsById" resultMap="DeptEmps">
         select d.did, d.dname,e.eid, e.ename, e.age, e.sex from dept d left join emp e on e.eid = d.did 
         where d.did = #{did}
     </select>
     ```

### 一对多分步查询

1. 思想：

   - 先查询部门信息
   - 再通过部门信息查询员工的集合（一个部门对应多个员工）

2. `DeptMapper.java` 接口的写法

   - 和以上多对一写法相同，只是使用 `<collection>` 标签加 `select` 属性
   - 延迟也相同，不用重复设置
   - __第二个SQL 语句 的 `column` 的属性，当是多个值时，可以使用 Map 集合 `{id=did}`__
     1. `did` 时上一个 SQL 语句查询出来的值与 `property` 相同
     2. id 键值，是第二个 SQL 获取的值，`#{id}` ，这两个值，必须相同

3. __针对某一个分步查询设置延迟加载，或者不设置延迟加载__

   - 上面的 `Setting` 是全局的设置
   - 可以在 `<collection>` 或者 `<ass……>` 设置属性 `fetchType="lazy"` 延迟，不延迟 `fetchType="eager"`

   

## 动态 SQL 语句

### 介绍

1. 极大简化拼装 SQL 语句的操作
2. 方便 SQL 的多条件的查询

### if

1. 当查询条件为多个时，可以使用 bean 进行封装。但是到实际上是，条件的多是从客户端传递过来的，所以无法判断

   - 如果客户端没有传值，而 SQL 语句 进行了拼接（以条件查询）

   - 客户端

     ```java
     EmpMapper empMapper = sqlSession.getMapper(EmpMapper.class);
     
     Emp emp = new Emp();
     emp.setEid(1);
     //		emp.setEname("张三");
     emp.setAge(20);
     emp.setSex("男");
     
     List<Emp> empsByMore = empMapper.getEmpsByMore(emp);
     ```

   - SQL 语句

     ```xml
     <!-- public abstract List<Emp> getEmpsByMore() -->
     <select id="getEmpsByMore" resultType="model.bean.Emp">
         select eid, ename, age, sex, did from emp
         where
         eid = #{eid} and
         ename = #{ename} and
         age = #{age} and
         sex = #{sex}
     </select>
     ```

   - 后果：实体属性是有默认值的，ename 没有赋值默认 `null` ，SQL 语句会议 `ename = null` 查询，严重影响查询结果

2. 所以使用 `if` 在 SQL 语句拼写时发挥作用，bean 有值，则进行拼接

   - 代码

     ```xml
     <!-- public abstract List<Emp> getEmpsByMore() -->
     <select id="getEmpsByMore" resultType="model.bean.Emp">
         select eid, ename, age, sex, did from emp
         where
         <if test="eid != null">
             eid = #{eid}
         </if>
         <if test="ename != null and ename != ''">
            and  ename = #{ename}
         </if>
         <if test="age != null">
             and age = #{age}
         </if>
         <if test="sex == '男' or sex == '女'">
            and sex = #{sex}
         </if>
     </select>
     ```

3. 解释

   - 使用 `<if test="">` 来判断值是否符合情况

     1. String 类型需要判断 `null` 或者是空字符串 `""`
     2. Integer 类型判断 `null`

   - 上面有错误地方，使用 `if` 判断 `==` 只能判断数值（字符串是数字 `sex` 只能是字符串数字，其他的不可以）

     - `sex` 的设置成 `String` 类型，数据库存储 `0\1` 来表示男女

     - 正确写法

       ```xml
       <if test="sex == 1 or sex == 0">
           sex = #{sex}
       </if>
       ```

   - 如果第一个条件不成立，SQL 语句会多出一个 `and`

     1. 解决办法，加一个恒成立的条件

        `where 1 = 1` 即可

     2. 也可以使用 `<where></where>` 标签，增加 `where` 关键字和去掉 `where`  后多余 `and` 如 `where and` 去掉 `and` 但是 `where 1=1 and` 则不会去掉

        ```xml
        <where>
            <if test="sex == 1 or sex == 0">
                sex = #{sex}
            </if>
        </where>
        ```

   - 使用 `<trim>` 标签去掉其包含的 SQL 片段的指定前缀和后缀

     1. `<trim prefix="" suffix="" prefixOverrides="" suffixOverrides="">`

        - 前缀、后缀、重现前缀、重写后缀

     2. 代码

        ```xml
        <trim prefix="where" suffixOverrides="and|or">
        
        </trim>
        ```

     3. 在截取的 SQL 语句片段中，前增加 `where` ，后去掉 `and` 或者 `or`

   - `<set>` 解决方法

### choose（when 、otherwise）

1. 相当于 `if() / else if() / else if() / else` 只有一个条件执行

   - 代码（根据 eid、ename、age 三个条件的其中以个查询）

     ```xml
     where
     <choose>
     	<when test="eid != null">
             eid = #{eid}
         </when>
         <when test="ename != null and ename != ''">
         	ename = #{ename}
         </when>
         <otherwise>
         	age = #{age}
         </otherwise>
     </choose>
     ```

### foreach（批量删除）

1. 批量删除 `where eid in ()`

   - 使用 `eid` 拼接的字符串

   - 删除语句

     ```sql
     delete from emp where eid in (#{eids})
     ```

   - eids 为传入的 eid 拼接的字符串

     1. 在测试方法中传入 `"1, 2, 3"` ，使用 `#{}` 通配符赋值，默认给字符串加单引号，所以这样做，只能删除 `eid = 1` 的实体

   - 必须使用 ${value} 来获取 `eids` 

     1. 第一保证可以获取到参数值
     2. 保证字符串拼接正确

2. 使用 `foreach` 实现批量删除

   - 通过 `List` 集合批量删除 `List<Integer> eids` 

   - 当参数为 List 集合或 Array 数组时，默认将其放入 Map 中，通过键值 `#{键值}` 

     1. List 集合键值为 list
     2. Array 数组为 array 

   - 删除语句

     ```xml
     delete from emp where eid in (
     <foreach collection="list" item="eid" close="" open="" separator="," index="">
     	#{eid}
     </foreach>
     )
     ```

   - 属性说明

     1. collection 遍历的集合或数组
     2. item 设置别名
     3. close 设置循环体的结束符
     4. open 开始符
     5. separator 分隔符，每次循环的分隔符
     6. index 遍历 list 为索引，遍历 map 时表示键

   - 没有用到的可以删除

   - 使用开始，结束符，可以添加 `()` 

3. 删除

   - 删除语句

     ```sql
     delete from emp where eid = 1 or eid = 2 or eid = 3
     ```

   - 使用 `foreach` 拼接

### 批量操作总结

1. 批量删除

   - 代码

     ```sql
     delete from emp where eid in ();
     delete from emp where eid = 1 or eid = 2 or eid = 3;
     ```

2. 批量查询

   - `select` 与批量删除相同使用 `where` 条件判断

     ```sql
     select * from emp where eid in ();
     select * from emp where eid = 1 or eid = 2 or eid = 3;
     ```

3. 批量修改

   - 把每条数据修改为相同内容

     ```sql
     update emp set ... where eid in();
     update emp set ... where eid = 1 or eid = 2
     ```

   - 把每条数据修改为相对应内容

     ```sql
     update emp set ... where eid = 1;
     update emp set ... where eid = 2;
     update emp set ... where eid = 3;
     update emp set ... where eid = 4;
     ```

   - `XXXMapper.java` 接口方法

     ```java
     public abstract void updatetMoreByArray(@Param("emps")Emp[] emps);
     ```

   - `XXXMapper.xml`

     ```xml
     <update id="updateMoreByArray">
     	<foreach collection="emps" item="emp">
         	update emp set ename = #{emp.eanme}, age = #{emp.age}, sex = #{emp.sex} where eid = #{emp.eid};
         </foreach>
     </update>
     ```

   - 预编译对象 `PreparedState` 只能预编译一条 SQL 语句，不能有一次预编译多条 SQL 语句

   - 解决办法

     1. 在 `jdbc.properties` 文件中修改 `url`，允许预编译多条 SQL 语句

        ```prop
        jdbc.url=jdbc:mysql://localhost:3306/ssm?allowMultiQueries=true
        ```

4. 批量添加

   - 代码

     ```sql
     insert into emp values(),()();
     ```

   - `XXXMapper.java` 接口方法定义（使用 `@Param` 自定义键值）

     ```java
     public abstract void insertMoreByArray(@Param("emps")Emp[] emps);
     ```

   - `XXXMapper.xml` 

     ```xml
     <insert id="insertMoreByAray">
         insert into emp values
     	<foreach collection="emps" item="emp" separator=",">
         	(null, #{emp.ename}, #{emp.age}, #{emp.sex}, 1)
         </foreach>
     </insert>
     ```

5. __设置 SQL 语句的公共片段，可以被当前的映射文件所有 SQL语句访问__

   - 代码

     ```xml
     <mapper>
         <!-- 公共 SQL 片段 -->
         <sql id="empColumn">
             select eid, ename, age, sex, did from emp
         </sql>
         <!-- 引用 -->
         <select id="getEmpsByMore" resultType="model.bean.Emp">
             <!-- 引用 -->
             <include refid="empColumn"></include>
             <trim prefix="where" suffixOverride="and|or">
                 <if test="eid != null">
                     eid = #{eid} and
                 </if>
                 <if test="ename != null and ename != ''">
                     ename = #{ename}
                 </if>
             </trim>
         </select> 
     </mapper>
     ```

## 缓存技术

### 缓存机制

1. 介绍
   - 系统默认定义了两级缓存
   - 默认情况，只有一级缓存（SqlSession 级别的缓存，也称为本地缓存）开启
   - 二级缓存需要手动配置，基于 `namespace` 级别的缓存
   - 为了提高扩展性，Mybatis 定义了缓存接口，可以通过实现 `Cache` 接口来定义二级缓存

### 一级缓存设置

1. 一级缓存默认开启（SqlSession 级别）

   - 相同的 `SqlSession` 会使用相同的缓存内容
   - 对同一个 `SqlSession` 执行相同的 SQL 语句，会直接使用缓存

2. 一级缓存失效情况

   - 不同的 `SqlSession` 对应的不同的缓存

   - 同一个 `SqlSession` 单查询条件不同

   - 同一个 `SqlSession` 两次查询期间执行了任意一次增删改操作

     1. 不管增删改操作是否成功，都会清空缓存

   - 同一个 `SqlSession` 两次查询期间手动清空缓存

     1. 代码

        ```java
        sqlSession.clearCache(); // 手动清空缓存
        ```

### 二级缓存

1. 默认不开启（需要手动开启）

2. Mybatis 提供二级缓存的接口一级实现，缓存实现要求 POJO 实现 `Serializable` 接口

3. 二级缓存是在 `SqlSession` 关闭后提交后生效。__二级缓存是映射文件级别的，不管创建几个 `SqlSession` 只要访问 `XXXMapper.xml` 相同的 SQL 语句，就会使用二级缓存__

4. 二级缓存使用步骤：

   - 全局配置文件中，开启二级缓存（全局配置文件）

     ```xml
     <setting name="cacheEnabled" value="true" />
     ```

   - 需要使用二级缓存映射文件（`XXXMapper.xml`）

     ```xml
     <cache />
     ```

   - 被查询的实体类需要实现 `Serializable` 接口

5. 二级缓存相关属性

   - `eviction=FIFO`  缓存回收策略
     1. `LRU` 最近最少使用原则，移除最长时间不被使用的对象
     2. `FIFO` 先进先出，按对象进入缓存的顺序移除
     3. `SOFT` 软引用，移除基于垃圾回收器状态和软引用规则的对象
     4. `WEAK` 弱引用，更积极移除基于垃圾回收器状态和弱引用规则的对象
   - `flushInterval` 刷新间隔，单位毫秒（刷新内存）
     1. 默认情况不设置，也就是没有刷新间隔，缓存仅仅是调用语句时刷新
   - `size` 引用数目，正整数
     1. 代表缓存可以存储多少个对象，太大容易内存溢出
   - `readOnly` 只读 `true/false`
     1. `true` 只读缓存，会给所有调用者返回相同的实例对象，因此这些对象不能被修改（加入修改了实体对象，内存的没有被修改，造成数据脏读），提供了重要的性能优势
     2. `false` 读写缓存，返回对象的拷贝。这回慢一些，但是安全，默认值
   - `type` 设置第三方缓存

6. 以上二级缓存配置对映射文件中所有查询语句有效

   - 在二级缓存开启的情况下，对于个别不想开启二级缓存的使用 `useCache` 默认为 `true`

     ```xml
     <select id="" useCache="false"></select>
     ```

7. __缓存设置__

   - 全局 setting 的 cacheEnable
     1. 配置二级缓存的开关，一级缓存一直开着
   - select 标签的 useCache 属性
     1. 配置二级缓存是否使用，一级缓存一直使用
   - sql （select insert update detele ）标签 flushCache 属性
     1. 增删改默认为 true，sql 执行完成，同时清空一级二级缓存
     2. 查询为 false
   - SqlSession.clearCache() 只用来清空一级缓存

### 整合第三方缓存

1. 为了提供扩展性，Mybatis 定义了缓存接口 `Cache` ，我们可以通过实现 `Cache` 接口来自定义二级缓存

2. `EhCache` 是一个纯 Java 的进程内缓存框架，具有快速精干等特点，是 Hibernate 中默认的 `CacheProvider`

3. 整合 `EhCache` 的缓存步骤

   - 导入 `EhCache`  包，以及整合包，日志包

     1. `ehcache-core-2.6.8.jar` 核心包
     2. `mybatis-ehcache-1.0.3.jar` 整合包（Mybatis 开发的）
     3. `slf4j-api-1.6.1.jar` 日志包
     4. `sl4j-log4j12-1.6.2.jar` 日志包

   - 编写 `ehcache.xml` 配置文件

     1. 文件配置

        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                 xsi:noNamespaceSchemaLocation="../config/ehcache.xsd"> 
            <!-- 磁盘保存路径 -->
            <diskStore path="D:\atguigu\ehcache" />
            <defaultCache
                          maxElementsInMemory="1000"
                          maxElementsOnDisk="10000000"
                          eternal="false"
                          overflowToDisk="true"
                          timeToIdleSeconds="120"
                          timeToLiveSeconds="120"
                          diskExpiryThreadIntervalSeconds="120"
                          memoryStoreEvictionPolicy="LRU">
            </defaultCache>
        </ehcache>
        ```

     2. 属性说明

        ```xml
        <!--
        属性说明： 
        diskStore：指定数据在磁盘中的存储位置。
         
        defaultCache：当借助CacheManager.add("demoCache")创建Cache时，EhCache 便会采用<defalutCache>指定的的管理策略
        
          
        以下属性是必须的： 
        1: maxElementsInMemory - 在内存中缓存的element的最大数目
        
        2: maxElementsOnDisk - 在磁盘上缓存的element的最大数目，若是0表示无穷大
        
        3: eternal -设定缓存的elements是否永远不过期。如果为true，则缓存的数据始终有效，如果为false那么还要根据timeToIdleSeconds，timeToLiveSeconds判断 
        
        4: overflowToDisk- 设定当内存缓存溢出的时候是否将过期的element缓存到磁盘上
        
        
        以下属性是可选的： 
        5: timeToIdleSeconds -当缓存在EhCache中的数据前后两次访问的时间超过 timeToIdleSeconds的属性取值时，这些数据便会删除，默认值是0,也就是可闲置时间无穷大 
        
        6: timeToLiveSeconds - 缓存element的有效生命期，默认是0.,也就是element存活 时间无穷大
        diskSpoolBufferSizeMB 这个参数设置DiskStore(磁盘缓存)的缓存区大小.默认是30MB.每个Cache都应该有自己的一个缓冲区. 
        
        7: diskPersistent -在VM重启的时候是否启用磁盘保存EhCache中的数据，默认是 false。 
        
        8: diskExpiryThreadIntervalSeconds- 磁盘缓存的清理线程运行间隔，默认是120 秒。每个120s，相应的线程会进行一次EhCache中数据的清理工作 
        
        9: memoryStoreEvictionPolicy - 当内存缓存达到最大，有新的element加入的时候，移除缓存中element的策略。默认是LRU（最近最少使用），可选的有LFU （最不常使用）和FIFO（先进先出）
        -->
        ```

4. 使用 `type` 属性，指定 `Cache` 实现类即可（映射文件 `.XXXMapper.xml`）

   - `<cache type="org.mybatis.caches.ehcache.EhcacheCache"/>`
   - mybatis 的二级缓存交由 Ehcache 管理

5. __注意__

   - 当内存放的条数超过规定条目，就会向磁盘中写入数据
   - 其他的不需要添加，正常设置即可

### 逆向工程

1. 逆向工程配置文件 [参考地址](http://mybatis.org/generator/configreference/xmlconfig.html)

   - 实例（`generatorConfig.xml`）

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
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
                             connectionURL="jdbc:mysql://localhost:3306/数据库"
                             userId="root" password="...">
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
             <table tableName="table" domainObjectName="Class" />
         </context>
     </generatorConfiguration>
     ```

2. 生成实体类、Mapper接口、映射文件 [参考地址](http://mybatis.org/generator/running/runningWithJava.html)

   - 执行代码

     ```java
     List<String> warnings = new ArrayList<String>();
     boolean overwrite = true;
     File configFile = new File("generatorConfig.xml");
     ConfigurationParser cp = new ConfigurationParser(warnings);
     Configuration config = cp.parseConfiguration(configFile);
     DefaultShellCallback callback = new DefaultShellCallback(overwrite);
     MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
     myBatisGenerator.generate(null);
     ```

3. 生成方式不同

   - `Mybatis3` ：有 QBC 查询
   - `Mybatis3Simple` 简单 SQL 语句

4. QBC 查询

   - 代码（and 连接）

     ```java
     XXXExample example = new XXXExample(); // XXXExample 是生成的，XXX 是类名称
     Criteria c1 = example.createCriteria();
     c1.andEnameLike("%a%"); // 条件名字中有 a 的
     c1.andSexEqualTo(1); //性别为 1
     mapper.selectByExample(example); // 查询名字有 a、and 性别是1 的实体
     
     mapper.selectByExample(null); // 什么也不写查询所有
     ```

   - 代码（or 连接）

     ```java
     XXXExample example = new XXXExample(); // XXXExample 是生成的，XXX 是类名称
     
     Criteria c1 = example.createCriteria(); // 条件一
     c1.andEnameLike("%a%"); // 条件名字中有 a 的
     
     Criteria c2 = example.createCriteria(); // 条件二
     c1.andSexEqualTo(1); //性别为 1
     
     example.or(c2)
     mapper.selectByExample(example); // 查询名字有 a、or 性别是1 的实体
     ```

## 实例

### 测试分页

1. 导入架包

2. 使用 `plugins` 标签

   - 代码

     ```xml
     <plugins>
     	<plugin>全限命名 PageInterceptor</plugin>
     </plugins>
     ```

   - 此类实现 mybatis 的 `Interceptor` 接口

3. 实现

   - 代码

     ```java
     PageHelper.startPage(2, 2);
     List<Emp> list = mapper.getAllEmps();
     PageInfo<Emp> pageInfo = new PageInfo<>(list,5); // 页码
     
     pageInfo.
     ```

## SSM 整合

### 注意事项

1. 整合注意事项
   - 查看不同的 Mybatis 版本整合 Spring 时需要使用的适配包
     1. `Mybatis-Spring`
   - 下载整合适配包
     1. [下载地址](https://github.com/mybatis/spring/releases)
2. 整合步骤
   - 导入 JAR 包
     1. spring.jar
     2. springMVC.jar
     3. mybatis.jar
     4. 第三方 JAR 包
        - log4j.jar
        - pageHelper.jar
        - AspectJ.jar
        - jackson.jar
        - jstl.jar
   - 搭建 SpringMVC
     1. web.xml
        - DiaspatcherServlet（核心 Servlet）
        - HiddenHttpMethodFilter（除了 get/post 的请求方式）
        - CharacterEncodingFilter（编码过滤器）
     2. springMVC.xml
        - 扫描控制层
        - 视图解析器
        - DefaultServlet（处理请求静态资源的）
        - MVC 驱动
        - MultipartResolver，拦截器
   - 整合 SpringMVC 和 Spring
     1. web.xml
        - ContextLoaderListener ，指定参数 context-param
     2. spring.xml
        - 扫描组件（排除控制层）
        - 事务管理器
   - 搭建 mybatis
     1. 核心配置文件
     2. 创建 mapper 接口以及映射文件
   - spring 整合 mybatis
     1. spring.xml
        - 引入 properties 资源文件
        - DataSource 数据源的配置
        - 事务管理器，开启事务驱动
        - mybatis-spring 适配器提供了 `SqlSessionFactoryBean` 类，此类可以获取 `SqlSession` 对象
        - 还提供了 MapperScannerConfigurer
   - REST CRUD
     1. 查询 + 分页
     2. 修改（form）

### 创建 web 项目

1. 导入 JAR 包（27 个 JAR 包）

   - Spring 包

     | JAR 包                                  | 说明信息      |
     | --------------------------------------- | ------------- |
     | com.springsource.net.sf.cgib.jar        | 转换代理      |
     | com.springsource.org.aopalliance.jar    | aop 依赖包    |
     | com.springsource.org.aspectj.weaver.jar | aspect 核心包 |
     | commons.loging.jar                      | 日志          |
     | spring-aop.jar                          |               |
     | spring-aspectj.jar                      |               |
     | spring-beans.jar                        |               |
     | spring-context.jar                      | 命名空间      |
     | spring-core.jar                         |               |
     | spring-expression.jar                   |               |
     | spring-jdbc.jar                         |               |
     | spring-orm.jar                          |               |
     | spring-tx.jar                           | 事务管理器    |
     | spring-web.jar                          |               |
     | spring-webmvc.jar                       |               |
     | taglibs-standard-impl.jar               | jstl 包       |
     | tagibs-standard-spec.jar                | jstl 包       |

   - 其他 JAR 包

     | JAR 包                   | 说明                   |
     | ------------------------ | ---------------------- |
     | log4j.jar（+配置文件）   |                        |
     | mybatis-spring.jar       | mybatis，spring 整合包 |
     | mysql-connector-java.jar |                        |
     | mybatis.jar              |                        |
     | jsqlparser.jar           | 分页功能               |
     | pagehelper.jar           | 分页功能               |
     | druid.jar                | 数据库连接池           |
     | jackson 包（3 个 JAR）   | 处理 json 数据         |

2. 搭建 SpringMVC（web.xml）

   - 配置 spring 过滤器

     1. Spring 编码过滤器
        - 定义编码格式 UTF-8
     2. 处理请求方式过滤器
        - 扩展 get/post 提交方式

   - 配置核心控制器

     1. DispatcherServlet 类
     2. 将 Servlet 启动时间，设置到项目启动时

   - 代码

     ```xml
     <!-- 配置字符编码过滤器 -->
     <filter>
         <filter-name>CharacterEncodingFilter</filter-name>
         <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
         <!-- 属性值设置 -->
         <init-param>
             <param-name>encoding</param-name>
             <param-value>UTF-8</param-value>
         </init-param>
     </filter>
     <filter-mapping>
         <filter-name>CharacterEncodingFilter</filter-name>
         <url-pattern>/*</url-pattern>
     </filter-mapping>
     
     <!-- REST 请求方式处理 -->
     <filter>
         <filter-name>HiddenHttpMethodFilter</filter-name>
         <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
     </filter>
     <filter-mapping>
         <filter-name>HiddenHttpMethodFilter</filter-name>
         <url-pattern>/*</url-pattern>
     </filter-mapping>
     
     <!-- 配置核心控制器 -->
     <servlet>
         <servlet-name>DispatcherServlet</servlet-name>
         <servlet-class>springframework.web.servlet.DispatcherServlet</servlet-class>
         <!-- 配置文件加载位置，属性值 -->
         <init-param>
             <param-name>contextConfigLocation</param-name>
             <param-value>classpath:springMVC.xml</param-value>
         </init-param>
         <!-- 将 Servlet 启动时间设置为项目启动时 -->
         <load-on-startup>1</load-on-startup>
     </servlet>
     <servlet-mapping>
         <servlet-name>DispatcherServlet</servlet-name>
         <url-pattern>/</url-pattern>
     </servlet-mapping>
     ```

3. 设置 SpringMVC 的配置文件（`conf/springMVC.xml`）

   - 导入命名空间

   - 扫描控制层

   - 视图解析器

   - 默认 Servlet

   - MVC 驱动

   - 实例

     ```xml
     <!-- 扫描控制层 -->
     <context:component-scan base-package="model.controller"></context:component-scan>
     
     <!-- 配置视图解析器 -->
     <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
         <!-- 设置前缀、后缀 -->
         <property name="prefix" value="/WEB-INF/view/"></property>
         <property name="suffix" value=".jsp"></property>
     
         <!-- 视图解析器有多个,可以设置优先级 -->
         <!-- <property name="order"></property> -->
     </bean>
     
     <!-- 开启默认驱动，解析静态资源 -->
     <mvc:default-servlet-handler/>
     
     <!-- MVC 驱动 -->
     <mvc:annotation-driven />
     ```

4. 整合 Spring 和 SpringMVC（web.xml）

   - 配置监听器

   - 默认配置文件为 `/WEB-INF/applicaionContext.xml`

   - 代码

     ```xml
     <!-- 整合 spring 设置监听器 -->
     <listener>
         <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
     </listener>
     <!-- 位置 -->
     <context-param>
         <param-name>contextConfigLocation</param-name>
         <param-value>classpath:spring.xml</param-value>
     </context-param>
     ```

5. spring 配置文件（`conf/spring.xml`）

   - 扫描组件

     1. 排除 `servlet` （controller 层） 使用注解排除

   - 代码

     ```xml
     <!-- 扫描除控制层组件 -->
     <context:component-scan base-package="model">
         <!-- 排除控制层组件 -->
         <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
     </context:component-scan>
     ```

6. 配置 mybatis 配置文件（mybatis-config.xml）

   - 代码

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <!DOCTYPE configuration
     PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
     "http://mybatis.org/dtd/mybatis-3-config.dtd">
     <configuration>
         <properties resource="jdbc.properties"></properties>
         <settings>
             <!-- 将 user_name 映射为 userName 下划线映射为驼峰 -->
             <setting name="mapUnderscoreToCamelCase" value="true"></setting>
             <setting name="logImpl" value="LOG4J" />
             <!-- 开启延迟加载 默认为 false -->
             <setting name="lazyLoadingEnabled" value="true"/>
             <!-- 当开启懒加载时，是否还查询所有的信息，默认为 true -->
             <setting name="aggressiveLazyLoading" value="false"/>
             <!-- 开启二级缓存 -->
             <setting name="cacheEnabled" value="ture"/>
         </settings>
     
         <!-- 别名-->
         <!--
          <typeAliases>
           <typeAlias type="model.bean.User"/>
           <package name="model.bean"/>
          </typeAliases>
      	-->
     
         <!-- 组件 分页查询 -->
         <plugins>
             <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
         </plugins>
     
         <environments default="development">
             <environment id="development">
                 <transactionManager type="JDBC" />
                 <dataSource type="POOLED">
                     <property name="driver" value="${jdbc.driver}" />
                     <property name="url" value="${jdbc.url}" />
                     <property name="username" value="${jdbc.username}" />
                     <property name="password" value="${jdbc.password}" />
                 </dataSource>
             </environment>
         </environments>
     
         <!-- 批量引入 Mapper 映射文件 -->
         <mappers>
             <package name="model.mapper"/>
         </mappers>
     </configuration>
     ```

   - 创建 XXXMapper.java 和 XXXMapper.xml 

7. 整合 Spring 和 mybatis 

   - 管理 mybatis 的数据源（spring.xml）

   - 事务管理器

   - 管理 `SqlSession` 对象，核心配置文件、别名

   - 管理映射文件，自动创建对应的动态代理实现类，不需要使用 `sqlSesion.getMapper(Class)` 来获取动态代理实现类

   - 代码

     ```xml
     <!-- 引入 数据库的数据源文件 -->
     <context:property-placeholder location="classpath:jdbc.properties"/>
     <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
         <property name="driverClassName" value="${jdbc.driver}"></property>
         <property name="url" value="${jdbc.url}"></property>
         <property name="username" value="${jdbc.username}"></property>
         <property name="password" value="${jdbc.password}"></property>
     </bean>
     
     <!-- 声明事物管理器 -->
     <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
         <property name="dataSource" ref="dataSource"></property>
     </bean>
     
     <!-- 开启事物注解驱动 -->
     <tx:annotation-driven transaction-manager="transactionManager"/>
     
     <!-- 管理 mybatis 数据库会话对象 -->
     <bean id="" class="org.mybatis.spring.SqlSessionFactoryBean">
         <!-- 设置 mybatis 的核心配置文件 -->
         <property name="configLocation" value="classpath:mybatis-config.xml"></property>
         <!-- 设置数据源 -->
         <property name="dataSource" ref="dataSource"></property>
         <!-- 批量设置别名 -->
         <!-- <property name="typeAliasesPackage" value=""></property> -->
         <!-- 引入映射文件 -->
         <property name="mapperLocations" value="classpath:model/mapper/*.xml"></property>
     </bean>
     
     <!-- 自动实现 mapper 动态代理实现类 -->
     <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
         <!-- mapper 包 -->
         <property name="basePackage" value="model.mapper"></property>
     </bean>
     ```
     
   - 可以在 mybatis-config.xml 删除对应内容
   
     ```xml
     <properties resource="jdbc.properties"></properties>
     <!-- 别名-->
     <!--
      <typeAliases>
       <typeAlias type="model.bean.User"/>
       <package name="model.bean"/>
      </typeAliases>
      -->
     <environments default="development">
         <environment id="development">
             <transactionManager type="JDBC" />
             <dataSource type="POOLED">
                 <property name="driver" value="${jdbc.driver}" />
                 <property name="url" value="${jdbc.url}" />
                 <property name="username" value="${jdbc.username}" />
                 <property name="password" value="${jdbc.password}" />
             </dataSource>
         </environment>
     </environments>
     
     <!-- 引入 Mapper 映射文件 -->
     <mappers>
         <package name="model.mapper"/>
     </mappers>
     ```
   
8. 逻辑设置

   - controller 层中自动装配 service 层

   - service 层中自动装配 mapper 层

     1. mapper 层动态代理实类，由此标签产生

        ```xml
        <!-- 自动实现 mapper 动态代理实现类 -->
        <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
            <!-- mapper 包 -->
            <property name="basePackage" value="model.mapper"></property>
        </bean>
        ```

        











