# MAVEN 介绍

## MAVEN 作用

### JAR 包管理问题

1. 解决工作空间大量重复的文件（JAR 包）
   - 解决办法：创建本地仓库，将重复文件保存此仓库中，用时引用即可（坐标）
2. 解决 JAR 包之间的依赖问题
   - 可以根据依赖关系自动下载
3. 解决 JAR 包之间的冲突
   - 项目的依赖性，会导致 JAR 包的冲突
     1. A 项目依赖 B项目（B 依赖 `log4j.1.2.14.jar`），B项目依赖于 C 项目（C 依赖 `log4j.1.2.17.jar`），根据依赖的传递性，A 项目的结果是导入两个相同功能版本不同的 JAR 包
   - maven 可以根据规则引入 JAR 包
     - 最短路径优先
     - 先声明者优先
4. 如何获取 JAR 包
   - maven 自动下载所需 JAR 包
5. 利用 maven 可将项目拆分成多个项目
6. 可实现项目的分布式部署

## MAVEN 介绍

### 自动化构建工具

1. maven 是一款自动化构建工具，专注服务于 Java 平台的 __项目构建__ 和 __依赖管理__
2. 构建概念
   - 纯 Java 代码
     1. `.java` 扩展名源文件需要编译成 `.class` 扩展名名文字节码文件，才可以执行。此过程为 __编译__
   - Web 工程
     1. 将 Java 程序的 Web 工程编译结构，放入服务器指定目录下（Tomcat 服务），并需要启动服务器。此过程为 __部署__
     2. 手动部署
        - web 编译环境的 `bulid` 是类路径（`.class` 文件），需要拷贝到 tomcat 的 `webapps/XXX/WEB-INF`
3. 构建环节
   - 清理、编译
   - 测试、测试报告
   - 打包，Java 项目 JAR 包，Web 项目 war 包
   - 安装，将打包结果安装在 maven 仓库中
   - 部署
4. 自动化构建

### maven 核心概念

1. maven 之所以能够实现自动化构建，与其设计是紧密相关的。围绕 9 大核心

   - 如表

     | 核心           | 说明                                  |
     | -------------- | ------------------------------------- |
     | POM            | maven 依据 pom文件，解决包的关系      |
     | 约定的目录结构 | 根据目录结构解析项目                  |
     | 坐标           | 通过坐标（路径）加载第三方 JAR 包     |
     | 依赖管理       | 项目之间的依赖的传递等问题            |
     | 仓库管理       | 存放各种 JAR 包（第三方、自定义）插件 |
     | 生命周期       | maven 命令行                          |
     | 插件和目标     | 插件就是命令行                        |
     | 继承           |                                       |
     | 聚合           |                                       |

2. 搭建 maven 环境，配置文件 `apache-maven-3.3.9/conf/settings.xml`

   - 检查 jdk 的安装环境（路径）

     1. `echo $JAVA_HOME/echo %JAVA_HOME%`

   - 解压 maven 核心程序，到一个非中文路径中

   - 配置环境变量

     1. `M2_HOME` 指定 maven 的安装路径

   - 检查是否安装成功

     1. `mvn -version`

   - 配置本地仓库

     1. 默认的本地仓库为 `~/.m2/repository`
     2. 创建自定义的仓库

     3. 修改配置文件 `*/conf/settings.xml`

        ```xml
        <localRepository>
        	/path/
        </localRepository>
        ```

   - 修改云仓库的地址，不使用中央仓库，而是要使用中国大陆的镜像仓库

     1. 阿里镜像配置

        ```xml
        <mirrors>
            <mirror>
                <id>alimaven</id>
                <name>aliyun maven</name>
                <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
                <mirrorOf>central</mirrorOf>        
            </mirror>
        </mirrors>
        ```

   - 配置 JDK 的版本

     1. 引用 `JDK-1.8`

        ```xml
        <profile>  
            <id>jdk-1.8</id>  
            <activation>  
                <activeByDefault>true</activeByDefault>  
                <jdk>1.8</jdk>  
            </activation>  
            <properties>    
                <maven.compiler.source>1.8</maven.compiler.source>    
                <maven.compiler.target>1.8</maven.compiler.target>    
                <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>    
            </properties>  
        </profile>  
        ```

### 创建 maven 命令

1. 创建 maven 项目

   - 例如

     ```tex
     hello			    // 项目名
     ├── pom.xml
     └── src         
         ├── main
         │   ├── java      // 代码
         │   └── resources // 配置文件
         └── test          // 测试
             ├── java
             └── resources
     
     7 directories, 1 file
     ```

   - pom.xml 设置

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <project xmlns="http://maven.apache.org/POM/4.0.0"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                                  http://maven.apache.org/xsd/maven-4.0.0.xsd">
         <modelVersion>4.0.0</modelVersion>
     
         <!-- The Basics -->
         <groupId>model.maven</groupId>
         <artifactId>Hello</artifactId>
         <version>0.0.1-SNAPSHOT</version>
         <name>Hello</name>
         <dependencies>
             <dependency>
                 <groupId>junit</groupId>
                 <artifactId>junit</artifactId>
                 <version>4.9</version>
                 <scope>test</scope>
             </dependency>
         </dependencies>
     </project>
     ```

2. 创建实类与测试类

   - 实类 `hello/src/main/java/model/maven/Hello.java`

     ```java
     package model.maven;
     public class Hello {
         public String sayHello(String name) {
             return "hello " + name;
         }
     }
     ```

   - 测试类 `hello/src/test/java/model/maven/HelloTest.java`

     ```java
     package model.maven;
     import org.junit.Test;
     import static junit.framework.Assert.*;
     public class HelloTest {
     	@Test
     	public void testHello() {
     		Hello hello = new Hello();
     		String result = hello.sayHello("张三");
     		assertEquals("hello 张三", result);
     	}
     }
     ```

3. 执行命令

   - `mvn compile` 编译命令，编译 `main/*` （不编译测试类） ，产生 `hello/target/calss` 目录存储 `.class`
   - `mvn clean` 清除 `/hello/target`
   - `mvn clean compile` 清除+编译
   - `mvn test-compile` 编译测试类 `test/*` ，产生 `hello/target/test-class`
   - `mvn clean package` 打 JAR 包
   - `mvn source:jar` 源码包

### POM.xml 配置文件

1. `Project Object Model` ：项目对象模型，将 Java 项目相关信息封装为对象，以便于操作和管理模型

   - `pom.xml` 为工程核心配置文件

2. 对 `pom.xml` 配置文件的说明

   - 固定处

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <project xmlns="http://maven.apache.org/POM/4.0.0"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                                  http://maven.apache.org/xsd/maven-4.0.0.xsd">
         <modelVersion>4.0.0</modelVersion>
     ```

   - 标签说明

     1. 实例

        ```xml
        <!-- The Basics 坐标-->
        <!-- 一般为公司网址 + 项目名称 -->
        <groupId>model.maven</groupId> 
        <!-- 自定义：模块名 -->
        <artifactId>Hello</artifactId>
        <!-- 自定义：版本号 -->
        <version>0.0.1-SNAPSHOT</version>
        <!-- 项目名 -->
        <name>Hello</name>
        <!-- 依赖关系，会有很多 -->
        <dependencies>
            <dependency>
                <!-- 坐标 -->
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>4.9</version>
                <!-- 作用范围 -->
                <scope>test</scope>
    </dependency>
        </dependencies>
        </project>
        ```
        

### 坐标概念

1. maven 的坐标（定位 JAR 包），使用 3 个向量在 maven 仓库中定位唯一一个 maven 工程

   - `groupId` ：公司或组织的域名倒序 + 当前项目名称（包名）

   - `artifactId` ：当前项目的模块名称 （类名）

   - `version` ：当前模块的版本号

   - 实例

     ```xml
     <!-- 一般为公司网址 + 项目名称 -->
     <groupId>com.xxx.model.maven</groupId> 
     <!-- 自定义：模块名 -->
     <artifactId>Hello</artifactId>
     <!-- 自定义：版本号 -->
     <version>0.0.1-SNAPSHOT</version>
     ```

2. 如何通过坐标到仓库中查找 JAR 包

   - 将 gav 三个向量连接起来
     1. `com.xxx.model.maven + Hello + 0.0.1-SNAPSHOT`
   - 一连接起来的字符串作为目录结构到仓库中查找
     1. `com/xxx/model/maven/Hell/0.0.1-SNAPSHOT/Hello-0.0.1-SNAPSHOT`
   - 自定义的 Maven 工程需要执行安装操作才会进入 Maven 仓库 
     1. 安装命令 `mvn install`

3. 如何引用 JAR 包

   - 本地仓库有的，可以查看到 `gav` 信息，照写即可
   - 本地仓库没有（gav 当然也不了解），可以使用 [地址](https://mvnrepository.com/) 搜索坐标

## 依赖管理（项目依赖）

### 创建 maven 项目

1. STS 引用 maven

   - 创建服务器
   - 引用 maven + user setting （配置文件）

2. 创建 maven 项目

   - 创建简单的 maven 项目（跳过不必要的选择）
   - 根据包结构，命名 `gav`
   - 项目类型
     - `jar` Java 工程
     - `pom` 父工程
     - `war` web 工程

3. 配置 `pomx.xml` 文件

   - 只需要修改依赖的位置（都可以找到现成的，本地仓库，云仓库）

     ```xml
     <dependencies>
         <dependency>
             <groupId>junit</groupId>
             <artifactId>junit</artifactId>
             <version>4.9</version>
             <scope>test</scope>
         </dependency>
     
         <dependency>
             <groupId>model.maven</groupId>
             <artifactId>Hello</artifactId>
             <version>0.0.1-SNAPSHOT</version>
             <scope>compile</scope>
         </dependency>
     </dependencies>
     ```

4. 依赖关系

   - 直接依赖
   - 间接依赖

### 依赖范围

1. 当一个 maven 工程添加了对某个 JAR 包的依赖后，这个被依赖的 JAR 包可以对应下面几个可选的范围
2. compile（默认值）
   - main 目录下 java 代码可以访问这个范围
   - test 目录下 java 代码可以访问这个范围
   - 部署到 Tomcat 服务器上运行时，要放到 WEB-INF 的 lib 的目录下
   - 例如，对 Hello 的依赖，主程序、测试程序、服务器运行时都需访问
3. test
   - main 目录下 java 代码不可以访问这个范围
   - test 目录下 java 代码可以访问这个范围
   - 部署到 Tomcat 服务器上运行时，不要放到 WEB-INF 的 lib 的目录下
   - 例如，对 junit 的依赖，是测试程序
4. provided
   - main 目录下 java 代码可以访问这个范围
   - test 目录下 java 代码可以访问这个范围
   - 部署到 Tomcat 服务器上运行时，不要放到 WEB-INF 的 lib 的目录下
   - 例如，servlet-api，在服务运行时，Servlet 容器（Tomcat）会提供相关的 API，所以不需要部署
5. runtime
6. import\system

### 依赖的传递性

1. 当存在间接依赖的情况，主工程对间接依赖的 JAR 可以访问么？这要看间接依赖的依赖范围。只有依赖来范围 compile 时可以访问

   - 如表

     | Maven 工程 | 间接依赖范围 | 对 A 的可见性  |
     | ---------- | ------------ | -------------- |
     | A->B->C    | compile      | 可见（传递性） |
     |            | test         | 不可见         |
     |            | provided     | 不可见         |

### 依赖原则：解决 JAR 包冲突

1. 最短路径优先
   - A 依赖 B（B 依赖 `log4j.1.2.17`）
   - B 依赖 C（C 依赖 `log4j.1.2.14`）
   - 根据最短路径优先，A 项目则会有 B 依赖的 log4j
2. 路径相同则先声明者优先
   - A 依赖 B（B 依赖 log4j）
   - A 依赖 C（C 依赖 log4j）
   - 先声明者优先，看 `pom.xml` 配置文件中的顺序

### 依赖排除

1. 不需要依赖传递时，如何排除 JAR 包

   - 前提 ：A 依赖 B（B 依赖于 commons-logging）
   - A 不需要依赖于 commons-logging

2. 使用 `<exclusions>`

   - 示例（在 A 项目的 pom.xml）

     ```xml
     <dependency>
         <!-- 在依赖的项目中，排除依赖的 JAR -->
         <groupId>model.maven</groupId>
         <artifactId>Hello</artifactId>
         <version>0.0.1-SNAPSHOT</version>
         <scope>compile</scope>
         <!-- 排除 commons-logging -->
         <exclusions>
             <exclusion>
                 <groupId>commons-logging</groupId>
                 <artifactId>commons-logging</artifactId>
                 <!-- 都排除了，需要什么版本号 -->
             </exclusion>
         </exclusions>
     </dependency>
     ```

### 统一管理目标 JAR 包的版本

1. 以对 Spring 的 JAR 包的依赖为例，Spring 的每一个版本中都包含 spring-core、spring-context 等 jar 包。我们应该带入同一版本的 Spring 的 jar 包，而不是使用 spring-core 4.0.4 和 spring-context 4.0.1

2. 解决办法

   - 使用 `propreties` 标签（与 dependencies 同级）

   - 再使用自定义的子标签

   - 实例（设置全局变量）

     ```xml
     <properties>
     	<spring.version>4.0.0.RELEASE</spring-version>
     </properties>
     <dependencies>
     	<dependency>
             <!-- 使用表达式 -->
         	<version>${spring.version}</version>
         </dependency>
     </dependencies>
     ```

## 仓库

### 仓库分类

1. 本地仓库
   - 当前本地计算机上
2. 远程仓库
   - 私服
     1. 局域网中的服务器（公司自有）
   - 中央仓库
     1. 面对全世界
   - 中央仓库镜像
     - 分担中央仓的压力

### 仓库中的文件

1. maven 插件
2. 自定义的项目模块
3. 第三方 jar 包
4. __不管是什么 JAR 包，仓库都会根据坐标生成相应目录，所以可以使用统一的方式查询和依赖__

## 生命周期

### 什么是 maven 的生命周期

1. maven 生命周期定义了各个构建环节的执行顺序，有了这个清单，maven 就可以自动化执行构建命令

2. maven 有 3 套相互独立的生命周期

   - Clean Lifecycle
     1. 在构建之前，进行清理工作
   - Default Lifecycle
     1. 构建的核心部分，编译、测试、打包、安装、部署等
   - Site Lifecycle
     1. 生成项目报告，站点、发布站点
     2. __项目的说明文档__

3. clean Lifecycle 生命周期

4. Default Lifecycle 生命周期

   - 编译 `compile`
   - 编译测试源代码 `test-compile`
   - 测试 `test`
   - 打包 `package` 接受编译好的代码，打包 格式 JAR
   - 安装 `install`
   - 部署 `deploy`
   - __运行任何一个阶段，前面的阶段都会执行 __

5. Site Lifecycle 生命周期

   - 生成站点可能保存（原因：插件不匹配）

   - 版本需要对应（2.7-3.3/2.9-3.7.1）

   - 添加（项目的 pom.xml 文件）

     ```xml
     <build>
       <plugins>
         <plugin>
           <groupId>org.apache.maven.plugins</groupId>
           <artifactId>maven-project-info-reports-plugin</artifactId>
           <version>2.7</version>
         </plugin>
     
         <plugin>
           <groupId>org.apache.maven.plugins</groupId>
           <artifactId>maven-site-plugin</artifactId>
           <version>3.3</version>
           <configuration>
             <locales>zh_CN</locales>
           </configuration>
         </plugin>
       </plugins>
     </build>
     ```


### 插件和目标

### STS 整合 maven

## 继承机制

### 继承机制存在的必要性

1. 由于非 compile 范围的依赖是不能在依赖链中传递的，所以需要的工程只能单独配置
2. 创建父工程（pom），其他工程依赖父工程，即可引入相同的依赖
3. 一般父子工程的 `gav` 中的 `gv` 相同，所以子工程中的 `gv` 可以省略

### 实例

1. 创建父子工程

   - 创建父工程

     ```xml
     <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
         <modelVersion>4.0.0</modelVersion>
         <groupId>com.xxx.model</groupId>
         <artifactId>parent</artifactId>
         <version>0.0.1-SNAPSHOT</version>
         <!-- 项目类型：pom应用于声明父工程、jar声明Java工程（默认）、war声明Web工程 -->
         <packaging>pom</packaging>
     </project>
     ```

   - 子工程继承父工程（`parent` 与 `dependencies` 同级），子工程的配置文件 `pom.xml`

     ```xml
     <!-- 继承父工程 -->
     <parent>
         <groupId>com.xxx.model</groupId>
         <artifactId>parent</artifactId>
         <version>0.0.1-SNAPSHOT</version>
         <!-- 指定父工程配置文件 -->
         <relativePath>../parent/pom.xml</relativePath>
     </parent>
     ```

2. 父工程的依赖管理

   - 实例

     ```xml
     <!-- 依赖管理，父工程管理所有依赖，给子工程继承使用 -->
     <dependencyManagement>
         <dependencies>
             <dependency>
                 <groupId>junit</groupId>
                 <artifactId>junit</artifactId>
                 <version>4.9</version>
                 <scope>test</scope>
             </dependency>
         </dependencies>
     </dependencyManagement>
     ```

3. 子工程声明继承哪些父工程的依赖

   - __实例（只需要声明 `ga` 即可）__

     ```xml
     <dependencies>
         <dependency>
             <groupId>junit</groupId>
             <artifactId>junit</artifactId>
             <!-- 版本和使用范围不需要声明 -->
         </dependency>
     </dependencies>
     ```

## 聚合

### 解决的问题

1. 项目之间的依赖关系，如何理清，当需要进行安装时，谁又依赖与谁呢？

2. 在 __父工程中__ 使用聚合 maven 可以自动安装

3. 实例

   - 代码

     ```xml
     <modules>
         <module>../项目名A</module>
         <module>../项目名B</module>
         <module>../项目名C</module>
         <module>../项目名C</module>
     </modules>
     ```

4. 只需要安装父工程即可，所有项目都会自动安装

## 实战

### 创建 web 工程

1. 选择 `war` 创建 maven 工程
   - 没有 `src/main/webapp` 中没有 `WEB-INF` 和 `META-INF` 
   - 使用项目属性，先确定不是动态 web 项目，再确定是 web 动态项目，点击连接，创建缺少的目录
2. 创建 `index.jsp` 报错，缺少 Tomcat 提供的 JAR 包
   - 缺少 `servlet-api-jar` ，使用 maven 引入 JAR 包
3. `.jsp` 表达式无法提醒，缺少 Tomcat 的包 `jsp-api.jar/el-api.jar`
   - 使用 maven 引入 `jsp-api.jar` ，这样可以间接依赖 `el-api.jar` 包







