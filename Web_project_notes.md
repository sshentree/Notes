# 项目第一部分

## WEB 环境搭建

### Web 服务器介绍

1. Web 服务器
- Web 服务器主要用来接收客户端发送的请求和响应客户端请求
   - 常见 JavaWeb 服务器
     1. Tomcat（Apache）：当前应用最广泛的 JavaWeb 服务器
     2. JBoss（Redhat 红帽）：支持 JavaEE
     3. Resin（Caucho）：支持 JavaEE，应用越来越广泛

### Tomcat 介绍

1. Tomcat 介绍

   - Tomcat 安装 [8.5.55下载地址](http://us.mirrors.quenda.co/apache/tomcat/tomcat-8/v8.5.56/bin/apache-tomcat-8.5.56.tar.gz)

   - 目录介绍 [参考地址](https://blog.csdn.net/jdjdndhj/article/details/52694202)

     1. 如表

        | 目录    | 作用                                                 |
        | ------- | ---------------------------------------------------- |
        | bin     | 脚本文件（startup.sh/shutdown.sh 启动、关闭脚本）    |
        | conf    | 配置文件                                             |
        | lib     | 类库（存储 jar 包）                                  |
        | logs    | 日志目录                                             |
        | temp    | 临时目录                                             |
        | webapps | Web 项目目录（一个目录对应一个项目，自带了几个文件） |
        | work    | 工作空间（运行项目时，生成的文件）                   |

   - 启动 Tomcat

     1. 启动 `./startup.sh`
     2. 关闭 `./shutdown.sh`
     3. 使用浏览器访问 Tomcat 默认项目的默认页面 `http:localhost:8080`
        - 默认项目的默认页面 `Webapp/ROOT/index.jsp`

   - 修改服务器端口号

     1. 文件位置 `conf/server.xml`

     2. 修改位置

        ```xml
        <!-- A "Connector" represents an endpoint by which requests are received
                 and responses are returned. Documentation at :
                 Java HTTP Connector: /docs/config/http.html
                 Java AJP  Connector: /docs/config/ajp.html
                 APR (HTTP/AJP) Connector: /docs/apr.html
                 Define a non-SSL/TLS HTTP/1.1 Connector on port 8080
            -->
        <Connector port="8080" protocol="HTTP/1.1"
                   connectionTimeout="20000"
                   redirectPort="8443" />
        ```

   - 在原 Tomcat 目录中部署项目

     1. 项目目录位置 `Webapp`
     2. 创建目录 `TomcatDemo` ，此文件夹就是项目名称
     3. 访问项目中的 `index.html`页面方式 `http://localhost:8080/TomcatDemo/index.html`


### 在开发环境中部署开发 Web 项目

#### 在 vscode 运行 Java Web 项目 

__说明：[参考地址](https://blog.csdn.net/asty9000/article/details/90205328)__

1. 安装 __Tomcat for Java__ 插件（需要 Apache tomcat 和 Debugger for Java 插件）

2. 创建 Web 项目（vscode 打开文件夹，此文件夹为 Web 项目目录）

3. 在 vscode 命令模式（`ctrl+p` 调出），输入 `>tomcat` 执行 `Generate War Package from Current Folder`
   
- 生成 `war` 包，命令规则为 `项目名.war`
  
4. 添加 Tomcat 服务（再安装完成 Tomcat for Java 插件时，在视图窗口会出现 __TOMCAT SERVERS__）
   
- 选择 tomcat 的目录
  
5. 执行 `war` 包
   
   - `Run on Tomcat Server`
6. 设置 Tomcat Workspace 目录
   - 存储整个项目内容（代码）
   - 包括 Tomcat 服务，但是此处 Tomcat 服务缺少 `bin` 和 `lib` 所以还是需要依赖本地的 Tomcat 服务

7. 使用浏览器打开项目

   - 项目默认打开首页是 `http://localhost:8080/TomcatTest/index.html` ，但显示是 `http://localhost:8080/TomcatTest/`

   - 在 Tomcat Workspace 目录中的 `tomcat/conf/web.xml` 文件中

     1. 在末尾处显示（从上到下，优先级由高到低）

        ```xml
        <web-app xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" version="3.1">
            <welcome-file-list>
                <welcome-file>index.html</welcome-file>
                <welcome-file>index.htm</welcome-file>
                <welcome-file>index.jsp</welcome-file>
            </welcome-file-list>
        </web-app>
        ```

#### 在 STS 中开发部署 Web 项目

1. [参考地址](https://www.jianshu.com/p/453d23f514fb)

## HTTP 介绍

1. HTTP 协议介绍
   - HTTP 超文本传输协议
   - 详细的规定了浏览器与万维网服务器之间的通信规则
   - 客户端与服务端传输的内容称之为报文（请求报文、响应报文）
   
2. HTTP 协议会话方式
   - 建立连接
   - 发送请求
   - 会送响应
   - 关闭连接
   
3. HTTP 1.0 与 HTTP 1.1 区别
   - 1.0 短连接（网页图片数据，需要再次建立连接）
   - 1.1 长连接（网页图片数据，不需要再次建立连接，直接发送求情）
   - 对应浏览器中的 __请求体中的 `Connection:keep-alive` 长连接__ 
   
4. 报文
   - 报文格式
     1. 报文首部（服务器端和客户端需要处理请求与响应的内容及属性）
     2. 空行（回车符：16进制 `0x0d`；或者是换行符：16进制 `0x0a`）
     3. 报文主体（应被发送的数据）
   - 请求报文
     1. 报文格式
        - 请求首行（请求行）
        - 请求头信息（请求头）
        - 空行
        - 请求体
     2. __GET 请求__
        - 没有请求体
        - 数据在请求行中的 URL 中
     3. __POST 请求__
        - 具有请求体
   - 响应报文
     1. 报文格式
        - 响应首行（响应行）
        - 响应头信息（响应头）
        - 空行
        - 响应体
   
5. 响应码

   - 响应码对浏览器很重要，它告诉浏览器的响应结果（响应码是服务器发送给浏览器的，在响应报文中的相应行中）

   - 常用的响应码

     1. 如表

        | 响应码 | 作用                                         |
        | ------ | -------------------------------------------- |
        | 200    | 请求成功                                     |
        | 404    | 请求资源没有找到（客户端请求了不存在的资源） |
        | 500    | 请求资源找到了，但是服务器内部出现了错误     |
        | 302    | 重定向                                       |

## Servlet 

### Servlet 介绍

1. Servlet 介绍 [参考地址](https://www.runoob.com/servlet/servlet-intro.html)

   - 作用

     1. 在 Java 与 HTML 之间构建一座桥梁
     2. Servlet （server applet）服务端小程序

   - 特点

     1. Servlet 是 Java 编写的（`java.servlet.Servlet` 接口），用于 HTML 的特点
     2. Servlet 对象由 Servlet 容器创建

   - 使用 Servlet（`Servlet.jar` 在 Tomcat 的 lib 中）

     1. 使用传统方式
        - 创建类实现接口
        - new 实现类对象
        - 调用类的方法
     2. 使用 Servlet 接口
        - 创建类实现 Servlet 接口
        - 在 web.xml 中 __注册__ 该实现类
        - Tomcat （Servlet 容器）会创建实现类对象

   - 验证是否可以正常执行

     1. 创建类实现 `Servlet` 接口

        - 实现抽象方法

        - 其中 `service()` 方法即是处理请求的行为方法

          ```java
          @Override
          public void service(ServletRequest arg0, ServletResponse arg1) throws ServletException, IOException {
              /**
               * 处理请求的
               */
              System.out.println("hello world");
          }
          ```

     2. 在 `web.xml` 中注册该实现类

        - 注册内容

          1. 加载类的路径
          2. URL（提交请求的地址，即 form 标签的 action 属性）

        - 样式

          ```xml
            <!-- 注册类,标明 url 地址 -->
            <servlet>
     	<servlet-name>demo</servlet-name>
            	<servlet-class>ServletDemo.Demo</servlet-class>
            </servlet>
            <servlet-mapping>
            	<servlet-name>demo</servlet-name>
            	<url-pattern>/Demo</url-pattern>
            </servlet-mapping>
          ```
          


### Servlet 生命周期

1. 工作流程
   - Web 请求 -》 web.xml 中 servlet-mapping 中的 url-pattern -》 servelet-name -》 servlet 中的 servlet-name -》servlet-class
   - 通过 servlet-calss 加载 Demo 类，执行 `service()` 方法处理请求
2. 方法作用
   - 方法 `destroy()` ，Servlet 消亡时执行
   - 方法 `init()` ，创建对象后执行，初始化 Servlet 对象的一些参数值
   - 方法 `service()` ，处理请求
3. 创建 Servlet 过程
   - 构造器 -》 `init()` -》 `service()` -》 `destroy()`
   - 构造器
     1. 执行次数：在整个 Servlet 的生命周期中执行一次
     2. 执行时机：在第一次接受 Web 请求时执行
   - init()
     1. 执行次数：在整个 Servlet 的生命周期中执行一次
     2. 执行时机：在第一次接受 Web 请求时执行
   - service()
     1. 执行次数：在整个 Servlet 的生命周期中执行 __多__ 次
     2. 执行时机：在 __每次__ 接受 Web 请求时执行
   - destroy()
     1. 执行次数：在整个 Servlet 的生命周期中执行一次
     2. 执行时机：关闭服务器时执行
4. __总结__
   - 构造器只执行一次，所以 Servlet 是单例，整个服务器中只有一个 Servlet
   - init() 也值初始化一次 Servlet 的参数

### ServletConfig 与 ServletContext

1. ServletConfig 与 ServletContext

   - ServletConfig：表示 servlet 的配置信息 __对象__

   - ServletContext：表示 servlet 的上下文信息 __对象__

2. ServletConfig 作用：

   - 获取 Servlet 初始化参数

     1. `string initParameter = config.getInitParameter("参数名")`

     2. 初始化参数设定

     3. 在 web.xml 中的 servlet 标签中嵌套

        ```xml
        <init-param>
            <param-name>encode</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        ```

   - 获取 servletContext 对象

     1. `ServletContext context = config.getServletContext();`

   - 获取 servlet 名称

     1. `string servletName = config.getServletName()`

3. ServletContext 作用

   - 获取上下文（全局）初始化参数

     1. `String param = context.getInitParameter()`

   - 参数设置（该参数与上面参数作用域不同）

   - 在 web.app 标签中

     ```xml
     <context-param>
         <param-name>age</param-name>
         <param-value>18</param-value>
     </context-param>
     ```

   - 获取项目真是路径

     1. `String path = context.getRealPath("项目")` ，从系统的根目录获取 `/home/shen/...`

   - 域对象（四个）

### 创建 Servlet  最终的方式

1. 继承 `HttpServlet` 类

   - `HttpServlet` 继承 `GenericServlet` 
   - `GenericServlet`  继承 `Servlet`

2. __直接创建 `servlet`__ 而不是创建 `calss` 。此时创建的 `EndServlet` 自动继承 `HttpServlet` 。只有两个方法 `doGet` 和 `doPost` 。`web.xml` 自动配置

   - 效果

     ```java
     import java.io.IOException;
     public class EndServlet extends HttpServlet {
     	private static final long serialVersionUID = 1L;
            
         public EndServlet() {
             super();
         }
     
     	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
     		response.getWriter().append("Served at: ").append(request.getContextPath());
     	}
     
     	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
     		doGet(request, response);
     	}
     ```

   - `doGet()` 处理 `get` 请求、`doPost()` 处理 `post` 请求

3. `GenericServlet` 作用

   - `getServletConfig()` 提供 `ServletConfig` 类
   - `getServletContext()` 提供 `ServletContext` 类
   - 将 `service()` 抽象，子类必须实现

4. `HttpServlet` 作用

   - 重载一个 `service()` ，参数为 `HttpServletRequest` 和 `HttpServletResponse`
   - 实现 `GenericServlet` 中的 `service()` 抽象方法，将 `ServletRequset` 和 `ServletRespose` 强转为 `HttpServletRequest` 和 `HttpServletResponse` ，并调用重载方法 `service(HttpServletRequest, HttpServletResponse)`
   - 重载的 `service()` 判断提交方式是什么（post、get）对应的调用 `doGet()` 或者 `doPost()` ，子类覆盖父类（HttpServlet）的方法，创建 `doGet()` 和 `doPost`
   
5. 最终建立 Servlet 的类

   - 重载 `doGet()` 方法和 `doPost()` 方法，因为现在大部分请求方法是 POST 和 GET 方式
   - 继承的 `HttpServlet` 和 `GenericServlet` 类的方法


### 请求与响应

1. 请求：客户端向服务器发送的报文
   - 类型：`HttpServletRequest`
   - 创建：该对象由服务器（Tomcat）创建，发送给（`doGet()/doPost()`）方法，做处理
     1. 实际上 `HttpServletRequest` 是在 `HttpServlet` 类中的 `service()` （实现 `GenericServlet` 类的 `service()`）方法中，将 `ServletRequest` 强转为 `HttpServletRequest` 
   - `HttpServletRequest` 作用：
     1. 获取请求参数
     2. 获取项目虚拟路径，`/` + 项目名称（`ServletContext` 可以获取项目真实路径）
     3. 转发（页面的跳转，或路径跳转）
     4. 域对象
2. 响应：根据客户端的请求，回送响应
   - 类型：`HttpServletReponse`
   - 创建：该对象由服务器（Tomcat）创建，发送给（`doGet()/doPost()`）方法，做处理
     1. 实际上 `HttpServletResponse` 是在 `HttpServlet` 类中的 `service()` （实现 `GenericServlet` 类的 `service()`）方法中，将 `ServletResponse` 强转为 `HttpServletResponse` 
   - `HttpServletResponse` 作用：
     1. 响应文本或 HTML（页面）
     2. 重定向（跳转页面）
     3. 转发（页面的跳转，或路径跳转）
3. 请求 `HttpServletRequest`
   - 获取页面提交的参数（通过属性 `name`）
     1. 获取单个参数 `String uname = request.getParameter(name)`
     2. 获取复选框参数（多个） `String[] parameters = request.getParameterValues(name)`
   - 获取项目的虚拟路径
     1. `String path = request.getContextPath();`
        - 结果 `/TomcatTest1` ，即 `/` + 项目名
   - 转发
     1. 获取转发器 `RequestDispatcher requestDispatcher = request.getRequestDispatcher("loginSuccess.html")`
     2. 执行转发 `requestDispatcher.forward(request, response)`
4. 响应 `HttpServletResponse`
   - 响应
     1. 获取响应流 `PrintWriter writer = response.getWriter()`
     2. 响应内容（hello） `writer.write("hello")`
     3. 也可以响应 HTML `writer.write("<h1>hello</h1>")`
   - 重定向
     1. `response.sendRedirect("loginSuccess.html")`
     2. 直接重定向（不需要获取转发器）

### 转发和重定向

1. 转发与重定向代码对比
   - 转发由 `HttpServletRequest` 对象操作
     1. 获取转发器 -》执行转发
     2. 浏览器地址栏不变化 `http://localhost:8080/TomcatTest1/EndServlet?username=hello`
   - 重定向由 `HttpServletResponse` 对象操作
     1. 直接使用 `.sendRedirect(path)` 完成
     2. 浏览器地址栏，__重定向的页面__ `http://localhost:8080/TomcatTest1/loginSuccess.html`
2. 转发
   - __客户端发送一次请求__：服务器操作的转发，然后响应给客户端
3. 重定向
   - __客户端发送两次请求__：第一次请求页面，服务器发现重定向，响应客户端重定向页面地址；第二次请求，客户端向服务器发送重定向地址。服务器响应页面
4. 区别
   - __转发可以访问 `WEB-INF` 下资源，重定向不可以__
     1. `WEB-INF` 是 Web 项目的私有目录，客户端无法访问
     2. 原因：转发是服务器操作，而重定向是客户端操作的
   - 转发可以携带 `request` 对象

### Web 应用路径问题

1. 注意 __Servlet 默认路径是在当前项目下的__
2. 在 Web 应用中，使用转发跳转路径时，地址栏不变。此时使用相对路径时，会出现安全隐患，所以要 __使用绝对路径__
3. 绝对路径
   - 使用 `/` 开头的路径，称之为绝对路径
4. 解析方式
   - 服务器解析，表示当前项目路径 `http://localhost:8080/TomcatTest1`
     1. `web.xml` 文件的配置信息 URL 由服务器解析
     2. __转发__ 由服务器解析
   - 浏览器解析，表示当前服务器路径 `http://local:8080/`
     1. __重定向__ 由浏览器解析
     2. HTML 中的路径（属性 `src` `href` `action`）由浏览器解析
5. HTML 基于某一路径继续查找
   - 在 `<head></head>` 标签中设置 `<base href="/TomcatTest/"/>`
   - 在 HTML 标签中只要不使用 `/` 开头的路径，其余路径都基于标签的路径继续查找

### Web 乱码问题

1. 默认客户端与服务器编码解码问题

   - 服务器编码解码为 `IOS-8859-1` ，不支持中文编码解码
   - 浏览器编码解码
     1. 浏览器默认编码为 ：`UTF-8` （HTML5 的有定义），向服务器发送请求
     2. 浏览器默认解码为 `GBK` （中国），接受服务器的响应

2. 出现乱码的原因
   - 乱码：编码与解码不一致（码表不同）
   - 请求乱码：客户端编码，服务器解码
   - 响应乱码：服务器编码，客户端解码

3. 解决乱码问题

   - 请求乱码（客户端编码为 UTF-8，所以设置服务器的解码为 UTF-8）

     1. POST 请求（数据在报文中）

        - 在 `doPost()` 方法中第一行设置解码方式
          1. `request.setCharacterEncoding("UTF-8")` 

     2. Get 请求（数据在 URL 中）

        - 修改服务器的配置文件 `server.xml` 中的 端口号的标签中，添加属性 `URIEncoding="UTF-8"`

          ```xml
          <Connector connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443" URIEncoding="UTF-8"/>
          ```

   - 响应乱码（服务器编码 ISO-8859-1，客户端解码为 UTF-8）

     1. 将服务器的编码设置为 GBK（文本内容时为 GBK ，HTML 是 UTF-8）
        - 在 `doPost()` 与 `doGet()` 方法第一行设置编码方式
          1. `response.setCharacterEncoding("GBK")` 
     2. 将服务器与浏览器的 __编解码__ 都设置为 UTF-8
        - 设置响应的响应头的 `content-type: text/html; charset=UTF-8` ，这样服务器的响应以 UTF-8 编码，也通知了浏览器以 UTF-8 解码
          1. `response.setContentType("text/html;charset=UTF-8")`

4. __总结__

   - 将服务器的处理 POST（报文中） 和 GET （URL 中）请求的编码方式设置为 UTF-8，再进行下一步处理
   - 将服务器的响应设置为 UTF-8 编码 ，并通知浏览器以 UTF-8 解码（响应头中的 `content-type` 属性）

# 项目第二部分

## 与项目目录相关事项

### 工具介绍

1. 使用 VSCode 作为数据库可视化工具
   - 使用插件
     1. MySQL Client for vscode
     2. MySQL Syntax

### 创建数据库

1. 依据项目要求创建数据库
2. 创建数据库的 table

### 依据数据库创建 JAVA 

1. 创建 Bean 包
   - 包含数据库是实体对象
2. 创建 Util 包
   - 工具包
     1. 建立数据库连接，通过连接池获取连接
3. 创建 Dao 包
   - 数据库操作行为
4. 创建 Servlet 包
   - HTML 与 JAVA 的桥梁

### 修改 HTML 中路径问题

1. 设置根目录标签 `<base href="/项目名/">`
   - 在 Servlet 中说明

### 导入资源（jar 包）

#### 导入 `WebContent/lib`  下

1. 数据库连接池
2. JDBC  和基于JDBC 的 dbutils 操作模块

#### 创建 Source folder 目录

1. 存放配置文件
2. 数据库连接池配置文件

### 创建 Servlet

1. 构建 HTML 与 Java 的桥梁
2. 工作流程
   - HTML 提交数据（提交地址 `action=""`）
   - 通过 `web.xml` 找到对应的 `Servlet` 
   - `Servlet` 调用的对应的 `dao` 实现类 （`UserDao userDao = new UserDaoImpl()`），验证数据是否正确
   - 数据正确不正确做响应处理

### 软件中的三层架构

#### 表示层

1. 网页页面
   - 与客户交互，用于接受数据
2. Servlet 
   - HTML 与 Java 之间的桥梁

#### 业务逻辑层

1. 创建 service 包
   - 设计模式，接口 + 是实现类

#### 数据访问层

1. dao 层
   - Java 与 数据库交互

#### 整体说明

1. __表示层__ 调用 __业务逻辑层__ 调用 __数据访问层__

## JSP

### JSP 介绍

1. 全称：`Java Server Pages` （java 服务器页面）

   - 是 JavaWeb 中的动态页面，其本质是一个 Servlet
   - Jsp 只能运行在服务其中（HTML 运行在 浏览器）

2. 理解

   - 可以在类 HTML 中，编写 Java 代码（`<% Java 代码 %>`）

3. 实例

   - 输出 0-100 的偶数（`<%=i %>` 显示在页面上）

     ```jsp
     <body>
     	<%
     		for (int i = 0; i <= 100; i++) {
     		if (i % 2 == 0) {
     	%>
     			<%=i%>
     	<%
     		}
     	}
     	%>
     </body>
     ```

### JSP 工作原理

1. __Jsp 本质是 Servlet__
2. 当运行 `.jsp` 文件时，服务器（Tomcat）将其翻译成 `.java` 文件。Java 编译器将其编译成 `.class` 文件
   - Jsp 本质是 Servlet 那么必然会有 `service()` 等方法，或者继承 `Servlet` 等
   - 查看 Jsp 翻译成 Java 的代码，位置 `STS-Workspace/.metadata/.plugins/org.eclipse.wst.server.core/tmp0/work` 中
   - 如 `helloworld.jsp` 翻译成 `helloworld_jsp.java`
   - 代码解释
     1. `_jspService()` Jsp 的 Java 代码都会翻译在此函数中
     2. `org.apache.jasper.runtime.HttpJspBase` 继承 `HttpServlet` 
     3. __上面有说 Servlet 作用__
3. 服务器在第一次访问 Jsp 文件时，会翻译，以后文件在没有修改的情况下，不会再次翻译

### JSP 基本法

1. 总览
   - 指令 `<%@ %>`
   - 脚本片段 `<% %>` ，编写 Java ，翻译在 `_jspService()` 中，不能定义方法（方法不能嵌套）
   - 表达式 `<%= %>` ，显示页面的数据
   - 模板代码（HTML）
   - 声明 `<%! %>` ，编写 Java 代码，翻译在 类 中，可以定义方法。__与上面的作用域不同__
   - 注释
     1. HTML 注释 `<!-- 注释 -->`
     2. Java 注释
        - 单行 `//`
        - 多行 `/**/` ，不支持文档注释 `/***/` 
     3.  Jsp 注释 `<%--注释--%>`

### JSP 指令

1. 语法

   - `<%%@ 指令名 属性=属性值>`

2. 常用指令

   - page 指令

     1. 被动使用

        ```jsp
        <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
        ```
   
2. 属性
   
   - language：支持语言，默认 Java
        - contentType：与 `response.setContentType()` 作用一样
        - pageEncoding：jsp 页面编码格式
        - import：导包（Java 包）
        - errorPage：当前页面错误显示此页面
        - iserrorPage：设置当前页面是否为错误页面
          1. 设置为 `true` 是，`_jspService()` 会创建 exception 来处理异常
   
- include 指令
  
  - `<%@ include file="test.jsp"%>`
     - 包含 `test.jsp` 页面，紧接着指令上面的页面
     - 特点：静态包含，（第一次被翻译编译后）被包含的页面文件 __不会__ 被翻译和编译（包含页面会被翻译、编译）。 __当执行包含页面时（jsp），包含页面会被翻译、编译，被包含页面不会随着再次翻译、编译。包含页面翻译成 Servlet 时，被包含页面内容引入包含文件。__
  
- taglib 指令

### JSP 动作标签

1. 语法

   - `<jsp: 标签名 属性=属性值></jsp: 标签>`

2. 常用标签

   - 转发

     1. `<jsp:forward page="test.jsp"></jsp:forward>` ，__不带参数标签之间不能有空格__
     2. 运行当前页面，实际看到的页面效果为 `test.jsp` 页面

     3. __转发可以携带参数__

        - 实例

          ```jsp
          <jsp:forward page="test.jsp">
              <jsp:param value="lisi" name="username">
          </jsp:forward>
          ```

        - 参数可以在 `test.jsp` 中使用

        - 获取参数值

          ```jsp
          <%=request.getParameter("username")%>
          ```

   - 动态包含

     1. `<jsp:include page="test.jsp"></jsp:include>` 
     2. 特点：被包含的页面文件 __会__ 被翻译和编译（并且在包含页面被翻译、编译之前）。__执行包含文件时，会翻译、编译，被包含页面也会随着翻译、编译。与静态不同，被包含页面内容不会写入包含页面的 Servlet 的文件中，只是在页面被请求时，插入内容。__

### JSP 9 大隐含对象

1. 定义
   - 可以在 JSP 中直接使用的对象（不需要创建对象，服务器为其创建）
   - 在 `jspService()` 形参（2 个）加上方法中创建的对象（6 个），其中 `isErrorPage="true"` 设置当前页面为错误页面，才会创建第 9 个对象
2. application（类型为 `ServletContext`）
   - 作用：域对象（在方法体中创建）
   - Servlet 中获取方式：`this.getServletContext()`
3. session（类型 `HttpSession`）
   - 作用：域对象
   - Servlet 中获取方式：`request.getSession()`
4. request（类型 `HttpServletRequest`）
   - 作用：域对象（4个作用）
   - Servlet 中获取方式：
5. pageContext（类型 PageContext）
   - 作用：域对象；”JSP 老大“ 可以通过 pageContext 获取其他 8 个对象
   - Servlet 中获取方式：无
6. response（类型 `HttpServletResponse`）
   - 作用：与 request 作用一样
   - Servlet 中获取：参数传递
7. page （类型 Object）
   - 作用：`page = this` ，当前类的对象
   - Servlet 中获取：`this`
8. out（类型 JspWriter）
   - 作用：与 Servlet 中 PrintWriter  作用类似（兄弟关系，都继承 `java.io.writer`）
   - Servlet 中获取：无
9. config（类型 ServletConfig）
   - 作用：与 Servlet 中 ServletConfig 作用一致
   - Servlet 中获取：`this.getServletConfig()` ，`this` 继承 `Servlet`
10. exception（类型 `Throwable`）
    - 作用：接受处理异常信息

### JSP 四大域对象

1. 程序区域（Web 应用不同资源的数据相互传递）
   - 数据有区域限制
2. 域对象共有方法
   - `getAttribute()`
   - `setAttribute()`
   - `removeAttribute()`
3. 详情
   - application
     1. 当前项目中有效
   - session
     1. 当前会话中有效
   - request
     1. 当前请求中有效
     2. 转发可以获取，重定向获取不到
   - pageContext
     1. 当前页面中有效

## 将 HTML 页面改成 JSP 页面

### 页面代码的简化

1. 将 page 指令，添加到第一行，将 HTML 文件变为 JSP 文件

   - page 指令

     ```jsp
     <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
     ```

   - 修改文件后缀名 `.jsp`

   - 将 jsp 文件中的 html 引用改为 jsp

2. 获取项目名称

   - Java 代码 `request.getContextPath()`

   - __`<base href="<%=request.getContextPath()%>/">`__

   - request 可以直接使用，因为是 `_jspService()` 形参

3. 将网页重复的代码，提取出来，使用包含（动态包含、静态包含）

   - 重复代码

     1. `<base href="<%=request.getContextPath()%>/">`
     2. `<link type="text/css" rel="stylesheet" href="path">`
     3. `<srcript tyep="text/javascript" src=""></script>`

   - 将提取出的代码放入 `WebContent/WEB-INF` ，因为此路径客户端无法直接访问

   - 使用静态包含（页面是 HTML，不需要编译）

     1. 代码

        ```jsp
        <%@ page language="java" contentType="text/html; charset=UTF-8"
            pageEncoding="UTF-8"%>
        <base href="<%=request.getContextPath()%>/">
        <link type="text/css" rel="stylesheet" href="static/css/style.css">
        <script type="text/javascript" src="static/script/jquery-1.7.2.js"></script>
        ```

   - 引用 jsp 代码

     1. `<%@ include file="/WEB-INF/include/base.jsp" %>` 
     2. 此路径由服务器解析，根路径为项目名
   
4. 获取 `http:localhost:8080`

   - 使用 `request` 对象
     1. 获取协议 `request.getScheme()` 
     2. 获取服务主机 `request.getServerName()`
     3. 获取端口号 `request.getServerPort()`
   - 代码
     1. `<base href="<%=request.getScheme() %>://<%=request.getServerName() %>:<%=request.getServerPort() %><%=request.getContextPath()%>/">`

5. 修改 Servlet 中的重定向和转发的路径

   - 将 `.html` 修改为 `.jsp`

### 动态获取提示信息

1. 当登录失败后，页面转发到登录页面

   - 都是登录页面没有什么差别，所以根据 Servlet 中是否登录成功，动态获取提示信息

2. 使用 __域对象（4 个域对象）__

   - 使用原则：能使用小域，就不使用大域
   - 使用 `request`
     1. `request.setAttribute("mag", "输入用户名或密码有误，请重新输入！！！")`

3. 注意事项

   - 使用中文时，注意编码格式

4. 代码演示

   - Servlet 代码

     ```java
     // 登陆失败，转发（使用服务器解析路径）
     request.setAttribute("msg", "用户名或密码输入有误，请重新输入！！！"); // 设置登录失败错误标记
     ```

   - JSP 代码

     ```jsp
     <span class="errorMsg"><%=request.getAttribute("msg") == null?"请输入用户名和密码":request.getAttribute("msg") %></span>
     ```

   - __默认显示 `请输入用户名和密码` ，当你输入错误时，`request` 设置了属性，所以再次转发时，提示错误信息__

5. 注册改善

   - 当用户名已存在时，提示错误信息

### 信息回显

1. 当用户名已存在时，回显用户名和邮箱

   - 回显时，Servlet 使用转发，因为只有一个 `request` 可以直接使用 `request.getParameter()`
   - 重定向，则不可以使用，因为涉及两个 `request`

2. JSP 页面代码

   - 直接只用 `request.getParameter("参数")`

     ```jsp
     value="<%=request.getParameter("username") == null?"":request.getParameter("username") %>"
     value="<%=request.getParameter("email") == null?"":request.getParameter("email") %>"
     ```

3. 说明

   - 输入框的默认值 `placeholder` 属性，当输入框为空（`null`）或者空字符串（`""`） 显示默认值
   - 用户名、密码、邮箱，都有输入验证，但此输入验证的监听事件为 __提交按钮__ ，__所以回显信息没有经过验证，但重复提交是，一样进行验证（Js）__

### 优化 Servlet

1. 将 `LoginServlet` 和 `RegistServlet` 合并成 `UserServlet` ，因为登录、注册都是使用的 bean 中的 User

   - 就是将 `login` 和 `regist` 提交的数据，都调用 `UserServlet` 进行处理

2. 使用标记，用来区分是哪一个 `request` 提交的数据

   - 标记是需要和提交数据一起提交的，所以使用 `<input>` 标签
   - 但是此标签，只是提交用来区分谁提交的数据，而不需要供用户使用，所以使用 `<input type="hidden" name="method" value="login/regist">`
   - 使用 `request.getParameter("method")`  获取
   - __使用 `<input type="hidden">` 不会影响页面布局__

3. 使用 `<form action="UserServlet?method=regist">` ，提交数据

   - 必须使用 `POST` 提交方式，如果使用 `GET` 则会覆盖掉标签 form 的参数

4. 将登录、注册提取为方法

   - __将大量的 `if else` 转换成动态获取__

   - 使用反射通过方法名，动态获取方法对象

     1. 代码

        ```java
        // 登陆 或是 注册
        String method = request.getParameter("method");
        // 使用反射通过方法名动态获取方法对象
        try {
            Method declaredMethod = this.getClass().getDeclaredMethod(method, HttpServletRequest.class, HttpServletResponse.class);
            declaredMethod.invoke(this, request, response);
        } catch (Exception e) {
            e.printStackTrace();
        }
        ```

5. 将 `Servlet` 中的 `doGet/doPost` 提取出来，封装成 `BaseServlet` ，只要继承就可以

   - 好处，这样就可以专心写逻辑，调用的已经做完了

     1. `form` 的 `action` 提交参数（post 提交），获取参数（此参数就是方法名）
     2. 使用反射通过方法名，动态获取方法对象，调用

   - `BaseServlet` 实例

     1. 代码

        ```java
        protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
            // 设置编码 UTF-8
            request.setCharacterEncoding("UTF-8");
            response.setContentType("text/html;charset=UTF-8");
        
            // 登陆 或是 注册
            String method = request.getParameter("method");
            // 使用反射通过方法名动态获取方法对象
            try {
                Method declaredMethod = this.getClass().getDeclaredMethod(method, HttpServletRequest.class, HttpServletResponse.class);
                declaredMethod.invoke(this, request, response);
            } catch (Exception e) {
                e.printStackTrace();
            }
            //		if ("login".equals(method)) {
            //			login(request, response);
            //		} else if ("regist".equals(method)) {
            //			regist(request, response);
            //		}
        }
        
        protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
            doGet(request, response);
        }
        ```

## EL

### 基本介绍

1. EL 介绍
   - EL 表达式（Expression Language）
   - EL 是 JSP 内置的表达式语言，用以访问页面的上下文及不同作用域中的对象，取得对象的属性值，或执行简单的运算或判断操作 __（访问的数据必须在上下文中或作用域中）。__
     1. __如果直接写 `${i}` 表示从最小域找__
   
2. EL 作用
   - EL 在得到某个数据类型时，会自动进行数据类型的转换
   - __EL 表达式用于代替 JSP 表达式 `<%= %>` 在页面中做输出操作__
   
3. EL 特点
   - EL 表达式仅仅用来读取数据，而不能对数据进行修改
   - EL 表达式输出数据时，有则输出，`null` 时什么也不输出
   
4. EL 与 JSP 的区别
   - 如果数据为空（`null`） ，JSP 输出 `null` 
   - EL 显示的数据，必须在上下文中或者域中
   
5. EL 中域对象

   - EL 与 JSP 域对象，存在对象关系

     1. 如表格

        | 域的称呼       | JSP 域对象  | EL 域对象        |
        | -------------- | ----------- | ---------------- |
        | application 域 | application | applicationScope |
        | session 域     | session     | sessionScope     |
        | request 域     | request     | requestScope     |
        | page 域        | pageContext | pageScope        |

   - 演示

     1. 代码

        ```jsp
        <body>
        	<%
        		int i = 10;
        		pageContext.setAttribute("i", i);
        		request.setAttribute("i", i);
        		session.setAttribute("i", i);
        		application.setAttribute("i", i);
        	%>
        	EL:${pageScope.i }
        </body>
        ```

     2. __当域对象的属性设置相同时，查找范围从小到大。如果 4 个域都没有找到，什么也不显示。__

### 基本语法

1. EL 表达式总是放在 `{}` 中，前边 `$` 为前缀（JQuery `$()`）

   - `${}` 

2. 获取域中的对象可以直接使用对象名，如获取域中的 `user` 对象

   - `${user}`

   - 获取对象的属性值 `对象.属性`（属性要有 `set/get` 方法）

     1. `${user.name}`

   - 实例

     1. 创建 `Student` 类，对比 JSP 与 EL 的书写

     2. 代码

        ```jsp
        <body>
        	<%
        		Student student = new Student("lisi", 20);
        		request.setAttribute("stu", student);
        	%>
        	<h1>EL</h1>
        	${stu.name }
        	<h1>JSP</h1>
        	<%=((Student)request.getAttribute("stu")).getName() %>
        </body>
        ```

### EL 的 11 个隐含对象

1. pageContext：（与 JSP 中的 pageContext 作用一样）
   - 域对象
   - 可以取到 JSP 的 8 个对象（EL 中也是一样，可以取到 JSP 的 8 个对象）
2. 请求域的四个对象
   - applicationScope
   - sessionScope
   - requestScope
   - pageScope
3. 请求参数
   - 参数对象主要获取 `get` 和 `post` 请求中的参数
     1. `param` ：获取指定的请求参数（JSP `request.getParameter()`）
        - 如 `${param.username}`
     2. `paramValues` ：获取请求参数数组，复选框（JSP `request.getParamterValues()`）
        - 如 `${paramValues.hobby}` 返回一个 String 类型数组
4. 其他
   - header：获取报文的头信息（请求，响应）
   - headerValues
   - initParam：获取初始化参数（`web.xml` 配置的参数，Servlet 中有讲）
   - cookie

### 运算符

1. EL 运算符
   - 一般运算符
     1. 算数运算符
     2. 逻辑运算符
     3. 比较运算符
     4. 位运算符三元运算符
2. __非空判断 (`empty` 是否为空)__
   - `${empty ""}` 空返回 `true``
   - ``${not empty ""}` 或 `${!empty ""}`
   - 3 种空判断
     1. 判断 `""` 空字符串、`null` 、集合/数组长度位 0
   - __判断数据时，放不放入域中，都一样比较__

## JSTL

### JSTL 介绍

1. JSP Standard Tag Library（JSP 标准标签库）
2. JSP 虽然提供了 EL 表达式来代替 JSP 的表达式，但是由于 EL 表达式仅仅具有输出功能，而不替代页面中的 JSP 脚本。为了解决这个问题，JSP 为我们提供了自定义标签库（Tag Library）的功能，替代 JSP 的脚本代码（`<% %>`）。
3. 所谓的自定义标签，就是指可以在 JSP 页面中使用类似于 HTML 标签的形式调用 Java 的方法
4. JSTL 由 5 个不同的标签库组成

### 使用 JSTL

1. 使用 JSTL标签必须导入两个库

   - `taglibs-standard-impl.jar`
   - `taglibs-standard-spec.jar`

2. 在 JSP 页面通过 `taglibs` 指令引入标签库

   - `<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`
   - `prefix` 用来指定前缀名，通过该名来使用 JSTL
   - `uri` 相当于库的唯一标识，因为 JSTL 由多个不同的库组成，使用该属性指定要导入哪个库

3. 修改对象中的属性值

   - uesr 属性 name 和 age，__user 在（request）域中__
   - 代码
     1. `<c:set target="${requestScope.user}" property="name" value="lisi">`

4. 实例

   - 输出

     1. 代码

        ```jsp
        <body>
        	<%
        		int i = 0;
        		request.setAttribute("i", i);
        	%>
        	jsp:<%=i %>
            
        	<c:set var="j" value="100" scope="request"></c:set>
        	jstl:<c:out value="${requestScope.j }"></c:out>
        </body>
        ```

   - 如果没有指定域 `scpoe="request"` ，则默认方法最小域 `scpose="pageScpoe"`

   - `<c:remove var="i">` 如果没有指定域，则删除所有域中的 `i`

### 选择结构

1. `if `实例

   - 代码（判断 `user` 对象是否为空，将判断值赋值给 `isUserEmpty` (true/false)），并将 `isUserEmpty` 设置在 `request` 域中

     ```jsp
     <%
     boolean isUserEmpty;
     if (isUserEmpty = (null == "user")) {
         request.setAttribute("isUserEmpty", isUserEmpty);
     }
     %>
     
     <c:if test="empty user" var="isUserEmpty" scope="request">
     	user is empty
     </c:if>
     ```

2. `if / else`

   - 代码（判断 请求参数 age是否 大于 18）

     ```jsp
     <c:choose>
         <c:when test="${param.age>=18 }">成年</c:when>
         <c:otherwise>未成年</c:otherwise>
     </c:choose>
     ```

   - 多条 `if` 时，添加 `<c:when>` 即可

### 循环结构

1. `<c:forEach>`

   - 实例（items 循环对象，接受的变量 `var`）

     ```jsp
     <c:forEach items="${list }" var="user" begin="0" end="4" step="1" varStatus="vs">
         ${vs.index } -- ${user.name }
     </c:forEach>
     ```

2. 普通 `for` 循环

   - 代码（i 从 0 循环的 100，步长位 2），__没有指定域，默认最小的域__

     ```jsp
     <c:forEach var="i" begin="0" end="100" step="2">
         ${i }
     </c:forEach>
     ```

3. 增强 `for` 循环

   - 代码（列表，接受变量 student）

     ```jsp
     <c:forEach items="${sessionScope.list }" var="student">
         name:${student.name }
     </c:forEach>
     ```


## 使用 EL + JSTL 替换 JSP

### 准备工作

1. 引入指令（JSTL 库）
   - `<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`
   - 将将其指令放入 `WEB-INF/include/base.jsp` 中，这样所有 `.jsp` 文件都将包含此命令。

### 替换 login.jsp 文件

1. 替换一

   - 目标

     ```jsp
     <span class="errorMsg"><%=request.getAttribute("msg") == null?"请输入用户名和密码":request.getAttribute("msg") %></span>
     ```

   - 替换

     ```jsp
     <c:if test="${empty requestScope.msg }">
         <span class="errorMSg">请输入用户名和密码</span>
     </c:if>
     <c:if test="${not empty requestScope.msg }">
         <span class="errorMsg">requestScope.msg</span>
     </c:if>
     ```

### 替换 regist.jsp 文件

1. 替换一

   - 目标

     ```jsp
     <span class="errorMsg"><%=request.getAttribute("msg") == null?"":request.getAttribute("msg") %></span>
     ```

   - 替换 __（EL 等式为空，什么也不显示）__

     ```jsp
     <span class="errorMsg">${requestScope.msg }</span>
     ```

2. 替换二

   - 目标

     ```jsp
     <input value="<%=request.getParameter("username") == null?"":request.getParameter("username") %>" class="itxt" type="text"
     								placeholder="请输入用户名" autocomplete="off" tabindex="1"
     								name="username" id="username" />
     
     <input value="<%=request.getParameter("email") == null?"":request.getParameter("email") %>" class="itxt" type="text" placeholder="请输入邮箱地址"
     								autocomplete="off" tabindex="1" name="email" id="email" />
     ```

   - 替换

     ```jsp
     value="${param.username}"
     value="${param.email}"
     ```

### 修改登录后的提示信息

1. 需要修改登录后的页面有 5 个

   - login_success.jsp

   - regist_success.jsp

   - cart.jsp

   - checkout.jsp

   - order.jsp

   - 登陆后的页面显示

     ```jsp
     <div>
         <span>欢迎<span class="um_span">韩总</span>光临尚硅谷书城</span>
         <a href="pages//order/order.jsp">我的订单</a>
         <a href="index.jsp">注销</a>&nbsp;&nbsp;
         <a href="index.jsp">返回</a>
     </div>
     ```

2. 需要修改为登录的页面

   - index.jsp

   - 未登录的页面显示

     ```jsp
     <div>
         <a href="pages/user/login.jsp">登录</a> | 
         <a href="pages/user/regist.jsp">注册</a> &nbsp;&nbsp; 
         <a href="pages/cart/cart.jsp">购物车</a> 
         <a href="pages/manager/manager.jsp">后台管理</a>
     </div>
     ```

3. 登录和未登录的提示信息

   - 现在的页面（购物车、登录成功等……）都可以随意访问（不管登陆与否），所以修改此处 bug。
   - 登录显示登录的提示信息；未登录提示未登录信息

4. 提取公共的代码（登录、未登录）

   - 新建 `include/welcome.jp`

   - 代码

     ```jsp
     <%@ page language="java" contentType="text/html; charset=UTF-8"
         pageEncoding="UTF-8"%>
     <!--<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>-->
     <div>
         <span>欢迎<span class="um_span">韩总</span>光临尚硅谷书城</span>
         <a href="pages//order/order.jsp">我的订单</a>
         <a href="index.jsp">注销</a>&nbsp;&nbsp;
         <a href="index.jsp">返回</a>
     </div>
     
     <div>
         <a href="pages/user/login.jsp">登录</a> | 
         <a href="pages/user/regist.jsp">注册</a> &nbsp;&nbsp; 
         <a href="pages/cart/cart.jsp">购物车</a> 
         <a href="pages/manager/manager.jsp">后台管理</a>
     </div>
     ```

5. 如何判断是否登录

   - 使用 `session` 域，可以取到 `username` 则为登录
   - 不需要再次导入 `JSTL` ，因为引入的页面已经导入。

# 项目第三部分

## 完善图书管理部分

### 创建图书的数据库表

### 在 Bean 中创建图书类

1. 根据数据库表的字段，设置 Java 的属性 `Book.java`

### 在 Dao 层创建与数据库操作有关

1. 创建 `BookDao.java`  接口
2. 在 Impl 包中实现其接口 `BookDaoImpl.java` ，此类继承 `BaseDao.java` (连接数据库等操作)，实现 `BookDao.java` 接口。
   - 其中实现 `BookDao.java` 的接口分成两步
     1. 创建 SQL 语句
     2. 调用 `BaseDao.java` 的基本方法
3. 在 Test 中测试以上的接口是否正常执行

### 在 Service 创建 service 类

1. 创建接口 `BookService.java` 接口
   - 现在和 `BookDao.java` 接口方法名一样，目的只是多一层逻辑
2. 在 Impl 包中，实现接口
   - 实例化 `BookDaoImpl` 类，调用方法

### 创建 Servlet

1. 创建 `BookServlet.java` 时，继承 `BaseSerlet.java` （已经做了提取动作）
   - 只需要完成你需要的动作即可（`doGet/doPost` 不需要你管理，已经通过反射可以动态获取）

### 修改请求的连接

1. 此处请求的连接，已经被提取到 `header.jsp` 中，
2. 修改请求连接，并携带参数（方法名）

### 修改 book_manager.jsp

1. 修改样式

   - 代码

     ```jsp
     <c:forEach items="${requestScope.allBooks }" var="book">
         <tr>
             <td>${book.title }</td>
             <td>${book.price }</td>
             <td>${book.author }</td>
             <td>${book.sales }</td>
             <td>${book.stock }</td>
             <td><a href="pages/manager/book_edit.jsp">修改</a></td>
             <td><a href="#">删除</a></td>
         </tr>	
     </c:forEach>	
     ```



## 分页设置

### 目的及样式

1. 提高用户体验度
2. 提升程序性能（SQL 语句的执行）
3. `首页 上一页 1 2 3... 下一页 末页 共10页 40条记录 到第[2] 确认`

### 入手角度

1. 从数据库查寻语句入手

   - `select * from books limit 0, 4;` 开始位置，查询条数

2. 语句分页规律分析

   - __用户传入的数据为页面__

   - 表格（`limit` 的参数设置）

     | 页面 | 开始下标 | 条数 |
     | ---- | -------- | ---- |
     | 1    | 0        | 5    |
     | 2    | 5        | 5    |
     | 3    | 10       | 5    |

   - 规律 `(页面-1)*条数`

### 创建业务 Bean

1. 创建业务 Bean 的目的
   - 因为在分页的页面，需要的参数为 `当前页、总条数、总页数` 等
   - 单独传入数据复杂，所以将其封装成对象
2. 创建分页对象注意事项
   - 使用泛型（可以保证其他对象也可以使用）
   - 成员变量设计
     1. `private List<T> list;` 存放查询结果
     2. `private int pageNo;` 当前页
     3. `private int totalPageNo;` 总页数
     4. `priavte int totalRecord;` 总条数
     5. `private static final int PAGE_SIZE=2;` 页面显示条数

### 在 Dao 层实现接口

1. 在 `bookDao.java` 接口中添加查询接口

### 修改前端入口

1. 开始的查询是查询所有，即点击 `图书馆里` 触发查询功能。
2. 现在实现分页查询，所以点击 `图书馆里` 触发分页查询
3. 修改 `/WEB-INF/include/header.jsp` ，提交的目的
4. 按要求修改 `book_manager.jsp`

### 实现分页第1种

1. 将 `index.jsp` 的分页 `div` 复制到 `book_manager.jsp` 中

2. 按照域的属性，取出页码信息

3. 设置首页、末页，不显示上一页和下一页信息

   - 代码

     ```jsp
     <c:if test="${requestScope.page.pageNo > 1}">
         <a href="BookServlet?method=getBooksByPage&pageNo=${requestScope.page.pageNo - 1 }">上一页</a> 
     </c:if>
     <c:if test="${requestScope.page.pageNo < requestScope.page.totalPageNo}">
         <a href="BookServlet?method=getBooksByPage&pageNo=${requestScope.page.pageNo + 1 }">下一页</a>
     </c:if> 
     ```

4. 指定页码查询

   - 页面代码

     ```jsp
     <input value="${requestScope.page.pageNo }" name="pn" id="pn_input" />页 
     <input id="sub_page" type="button" value="确定">
     ```

   - 使用 Js 代码提交请求 Servlet

     ```javascript
     <script type="text/javascript">
         $(function(){
         $("#sub_page").click(function(){
             // 取值
             var pageNo = $("#pn_input").val();
             // 请求 BookServlet, location 相当于设置地址栏的 URL
             location.href = "BookServlet?method=getBooksByPage&pageNo=" + pageNo; 
         });
     });
     </script>
     ```

   - 如果页码非法，怎么进行判断，在 Bean 中的 get 方法进行判断

     ```java
     public int getPageNo() {
         if (pageNo < 1) {
             return 1;
         }
         if (pageNo > this.getTotalPageNo()) {
             return this.getTotalPageNo();
         }
         return pageNo;
     }
     ```

### 实现分页第2种

1. 将 `index.jsp` 提取到 `pages/client/book_client.jsp` 中，创建 `bookClientServlet.jsp` （在实现转发到 `book_client.jsp`） 中
2. `index.jsp` 和 `book_client.jsp` 是一个东西（index.jsp 中有转发到 `bookClientServlet.jsp` 中）
3. 修改 `book_client.jsp`

### 价格区间查询

1. `book_client.jsp` 中
2. 和分页查询相同，只是 SQL 语句添加两个参数

## Cookie 介绍

### 存在必要性

1. HTTP 协议为无状态协议，所以服务器无法区分用户。cookie 为服务器创建的，发送给客户端（客户端保存），客户端再次放松请求时，携带 cookie 值，来识别用户身份

### 创建、获取 Cookie

1. 在 `doGet/doPost` 中，创建和获取操作

   - 创建 Cookie 

     ```java
     Cookie cookie1 = new Cookie("stuName", "zhansan"); // 创建 cookie
     response.addCookie(cookie1); // 将创建的 cookie 添加进入（cookie 有一个默认值，所以只是添加）
     // 浏览器显示 cookie 用 ; 分割
     ```

   - 获取 Cookie

     ```java
     Cookie[] cookies = resquest.getCookies(); // 获取 cookie 值
     for (Cookie cookie : cookies) {
         cookie.getName;
         cookie.getValue();
     }
     ```

2. Cookie 键值问题

   - Name 不可以为中文
   - Value：可以为中文（设置编码集）

### 修改 Cookie 

1. 覆盖修改

   - 创建同名的 Cookie ，将其 older 值，覆盖掉

     ```java
     Cookie cookie = new Cookie("name", "value");
     response.addCookie(cookie); // 覆盖掉 cookie 值
     ```

2. 遍历修改

   - 循环遍历，找到要修改的 Cookie 值，使用 `setValue()`

     ```java
     Cookie[] cookies = resquest.getCookies();
     
     for (Cookie cookie : cookies) {
         if ("name".equal(cookie.getName())) {
             cookie.setValue("value");
             response.addCookie(cookie); // 添加进客户端
         }
     }
     ```

### Cookie 有效性问题

1. 默认情况

   - Cookie 为会话级别，与浏览器有关（关闭浏览器，转换浏览器）

2. 持久化 Cookie

   - 代码

     ```java
     // 创建 Cookie 
     Cookie cookie = new Cookie("name", "value");
     // 持久化 Cookie
     cookie.setMaxAge(seconds); // 存活多少秒（与关闭浏览器无关），时间已过就无效（取值范围）
     // 添加 Cookie 在 Response 中
     ```

   - seconds 取值注意

     1. `>0`
     2. `<0` 即恢复到默认情况（无时间戳）
     3. `=0` 立即失效（response也会发送给客户端，并带有时间戳）

### Cookie 有效路径

1. 默认的有效路径

   - 当前项目路径，当前项目的路径下 Cookie 都有效。就是进入设置的有效路径后，浏览器的 request 才有该 cookie

   - 但是设置 cookie 的 Servlet 是有触发机制的，触发服务器就会设置 cookie 值，（response）发送给客户端，但是此 cookie 只有到了有效路径后，才会显示有效。

   - 设置有效路径（基于当前项目下设置路径）

     ```java
     cookie.setPath(request.getContextPath() + "/path");
     ```

### Cookie 应用

1. Cookie 记住密码（7 天免登录）

   - 在 Servlet 中书写代码

     ```java
     // 取出密码，用户名
     String username = request.getParameter("username");
     String password = request.getOarameter("password");
     // 获取复选框，复选框不选中为 null，选中返回 value
     String rp = resquest.getParameter("rp");
     if (rp != null) {
         // 创建 Cookie
         Cookie cookieName = new Cookie("username", username);
         Cookie cookiePwd= new Cookie("password", password);
     
         // 持久化 Cookie
         cookieName.setMaxAge(60 * 60 * 24 * 7);
         cookiePwd.setMaxAge(60 * 60 * 24 * 7);
     
         // 将 cookie 值响应给浏览器
         response.addCookie(cookieName);
         response.addCookie(cookiePwd);
     }
     // 跳转
     ```

   - 在 jsp 页面中，如何回显用户名、密码。使用 EL，有隐藏对象 `cookie`

     ```jsp
     <input value="${cookie.username.value }" type="text">
     <input value="${cookie.password.value }" type="password">
     ```

   - __`cookie.username` 获取的是 cookie 对象，对象名是 key值，在其对象中有（key，value）__

### Cookie 缺陷

1. Cookie 的 value 为 String 类型
2. Cookie 是存放在浏览器中，Cookie 的值过多浪费资源

## Session 介绍

### 介绍

1. 介绍
   - 类型 `HttpSession` ，服务器创建
   - Session 表示当前会话，浏览器不关闭，会话即未结束
2. Session 工作原理
   - 客户端请求
   - 服务器创建 Session ，同时创建 __特殊的 Cookie __ 
     1. 特殊的 Cookie ，其中 `key` 为 `JSESSION` ，`value` 为 `session` 对象的唯一 ID
     2. 将 Cookie 相应客户端
     3. 客户端再次请求时，根据客户端发送的 Cookie 查询 session 对象

### Session 获取

1. HTML 中没有隐藏对象
   - 获取请求 Servlrt `request.getSession()`
2. JSP 存在隐藏对象
   - 直接获取

### Session 有效性

1. 默认有效期，为当前会话

   - 因为特殊的 Cookie 也是会话中的，Cookie 没有了，Session 便无法查找
   - 如果找不到 Session 便会创建Session 和 Cookie

2. 持久化 Session

   - 只需要持久化 __特殊的 Cookie__ 即可（么？）

   - 查询特殊 Cookie （key 为 JSESSION）

     ```java
     Cookie[] cookies = resquest.getCookies();
     
     for (Cookie cookie : cookies) {
         if ("JSESSION".equal(cookie.getName())) {
             cookie.setMaxAge(30); // 持久化
             response.addCookie(cookie); // 添加进客户端
             break;
         }
     }
     ```

   - 实际上 Session 默认也是有存活时间的（30 分钟）

     1. 默认在 servers 目录中的 `web.xml` （与项目同级的目录）

        ```xml
          <!-- ==================== Default Session Configuration ================= -->
          <!-- You can set the default session timeout (in minutes) for all newly   -->
          <!-- created sessions by modifying the value below.                       -->
        
            <session-config>
                <session-timeout>30</session-timeout>
            </session-config>
        ```

   - 将其复制到项目 `WEB-INF/web.xml` 中，在进行设置

     1. 设置最小时间为 __分钟__

3. 设置 Session 非活动时间

   - 为什么这么叫？

     1. Session 是服务器创建（Tomcat），使用不使用都会创建，所以是记录 Session 调用结束，和下一次调用的间隔时间
     2. Cookie 是编写项目的程序员确定什么时候创建（创建便是用户使用时，便开始计时）

   - 代码

     ```java
     HttpSession session = request.getSession();
     session.MaxInactiveInterval(30); // 非空时间为 30 秒
     ```

   - 时间参数

     1. `>0` ：多少秒失效
     2. `<=0` ：永不失效

4. Session 立即失效（Session 时是客户端请求时创建）

   ```java
   session.invalidate(); // 立即失效
   ```

### Session 的钝化与活化

1. 钝化
   - 将 Session 对象及对象中的数据，一同从内存序列化到硬盘的过程，称之为 __钝化__
   - 时机：服务器关闭触发
   - 表现：服务器重启后，服务器的 Session 对象不变，客户端继续正常访问
2. 活化
   - 将 Session 对象及对象中的数据，一同从硬盘反序列化到内存的过程，称之为 __活化__
   - 时机：服务器重启
3. 序列化文件
   - `/.metadata/.plugins/org.eclipse.wst.server.core/tmp0/work/Catalina/localhost/BookStore01/SESSIONS.ser`
   - 关机序列化，创建文件 `SESSIONS.ser`
   - 开机反序列化，删除文件
4. Session 可以使用 `setAttribute(String, Object)`，可以比 Cookie 更加灵活。Session 序列化对象时，对象必须允许序列化（实现 `Serialization` 接口）

### 表达重复提交

1. 重复提交原因

   - 转发，F5（刷新）
   - 连续点击提交按钮
   - 提交后，点击回退按钮，继续提交

2. 解决重复提交

   - 提交 - servlet - 响应

   - 在 Servlet 只处理第一次提交的表单
   - 在 Servlet 设置标记（是否是第一次提交的表单），放入 __域__ 中
   - 标记不怎么好，所以使用 UUID 作为 Token，解决表单重复提交

3. UUID（32 为 16进制随机数）

   - 使用 UUID 作为 Token 存放在 Session 和隐藏域中

   - 判断是否多次提交

     1. 隐藏域  和 Session 做对比，提交一次，便移除 Session 的 UUID

   - 获取 UUID

     ```java
     String uuid = UUID.randomUUID.toString().replace("-", "");
     ```

   - JSP 页面

     1. 生成 UUID，并将其放入 Session 和 隐藏域中

        ```jsp
          <%
          	String uuid = UUID.randomUUID.toString().replace("-", "");
          	session.setAttribute("uuid", uuuid); // 放入 session
          %>
          
          <form action="" method="">
              <input type="hidden" name="uuid", value="<%=uuid %>"> <!--放入隐藏域中-->
          </form>
        ```

   - Servlet 代码

     1. 取出两个域的 uuid ，对比是否相等，相等移除 Session 的 UUID，做进一步处理；不等
     
        ```java
        HttpSession session = request.getSession();
        
        Object uuidSession = session.getAttribute("uuid");
        String uuidHidden = request.getParamter("uuid");
        
        if (uuidSesion != null && uuidSession.toString().equal(uuidHidden)) {
            // 提交
            session.removeAttribute("uuuid");
        }
        ```
     
     

# 项目第四部分

## 注销

### 简单操作

1. 分析
   
   - 注销是用户行为，所以确定 `UserServlet.java` 。页面注销的按键，已经放入 `WEB-INF/include/welcome.jsp` 中
   
2. 修改 `welcome.jsp`

   - 代码

     ```jsp
     <a href="UserServlet?method=logout">注销</a>
     ```

3. 编写 `UserServlet.jsp` ，创建 `logout` 函数，做注销操作

## 验证码

### 基本了解

1. 使用 Google 提供的 jar 包
   - 导入 `kaptcha-2-3-2.jar` 包
   - 在 `web.xml` 注册 Servlet
   - [参考地址](https://www.jianshu.com/p/a3525990cd82)
   - [下载地址](https://code.google.com/archive/p/kaptcha/downloads)
2. 源解读
   - kaptcha 有一个 Servlet （`KaptchaServlet`），只有一个 `doGet` 方法，因为不可能出现 `doPost` 提交的数据
   - `doPost` 只有表单 `<form method="post">` 时，才会使用 POST 提交表达，其他都是 GET
   - 超链接 `<href>` 资源链接 `src` 都是 GET 请求
   - 过程
     1. 生成文本信息，将文本信息存储在 Session 域中（key， value）
     2. 按照文本生成图片
     3. 响应图片
3. 配置信息
   - 在 session 中如何取出文本信息，就是 `.getAttribute("key")` ，其中 key 是何物？
     1. 默认 `KAPTCHA_SESSION_KEY`

### 使用情况

1. 注册 Servlet

   - `web.xml` 注册 Servlet

     ```xml
       <servlet>
       	<servlet-name>KaptchaServlet</servlet-name>
       	<servlet-class>com.google.code.kaptcha.servlet.KaptchaServlet</servlet-class>
       </servlet>
       <servlet-mapping>
       	<servlet-name>KaptchaServlet</servlet-name>
       	<url-pattern>/KaptchaServlet</url-pattern>
       </servlet-mapping>
     ```

   - 验证是否注册成功

     1. 浏览器地址栏 `http://localhost:8080/BookStore01/KaptchaServlet` ，是否返回验证码图片
   
2. JSP 引用 `Servlet` 即可

   - JSP 代码

     ```jsp
     <img alt="验证码" src="KaptchaServlet.jpg"
          style="float: right; margin-right: 40px; width:70px; height:30px;"> 
     ```

### 刷新验证码

1. 使用 JS 风格

   - 代码（有的浏览器，如果赋值一样，就不会赋值，所以后面加一个随机数）

     ```javascript
     // 刷新验证码
     $("#codeImg").click(function() {
         $(this).attr("src", "KaptchaServlet.jpg?random=" + Math.random());
     });
     ```

### 修改注册 Servlet 的验证码识别

1. 前端，只判断了，验证码是否为空
2. 获取前的验证码，获取 Session 的验证码，两者比较
   - 前端 `request.getParameter("code");`
   - Session `session.getSttribute("KAPTCHA_SESSION_KEY")`

# 项目第五部分

## 购物车

### 实现方式

1. 基于数据库的表，将购物信息存放在表中
2. 基于 Session 的，将购物信息存放在 Session 中，服务器保存
3. 基于 Cookie 的，客户端保存

### 创建业务 Bean

1. 创建购物车商品类

   - 商品名称，Book 类中
   - 数量
   - 单价，Book 类中
   - 金额 = 数量 * 单价

2. 创建购物车类

   - __商品类集合__（使用 LinkedHashMap` ，满足有序唯一的要求）
     1. key 为 `Book.id` ，虽然 id 为 Integer 但是，此处设为 `String` ，这样可以和 `Servlet` 更好接触
     2. value 为 `CartItem`
     3. `Map<String, CartItem> map = new LinkedHashMap<>();`
   - 购物车的商品总数
   - 总金额

3. 不需要使用 Dao 和 Service，因为使用 Session 存储购物车信息，所以在 Bean 中完成所i有操作动作

   - 将 Map 的 CartItem 遍历出来

   - 加入购物车
   - 删除购物项
   - 清空购物车
   - 修改购物数量

### 关联前端

1. 在 Servlet 调用 Bean 中方法

2. 修改 `index.jsp` 

   - 增加点击事件

   - 如何取到 `book.id`  （将按钮的 id 设为 book 的 id）

     ```jsp
     <button id="${book.id }">加入购物车</button>
     ```

   - 单击事件

     ```javascript
     // 添加购物车
     $("#book_add button").click(function(){
     // 取值 book.id
     var bookId = $(this).attr("id");
     // 调用 CartServlet
     location.href = "CartServlet?method=addBookToCart&bookId=" + bookId;
     });
     ```

### Servlet 的书写

1. 代码

   ```java
   HttpSession session = request.getSession();
   // 取 bookId 的值
   String bookId = request.getParameter("bookId");
   
   // 通过 id 获取 book 信息（BookService）
   Book book = bookService.getBookById(bookId);
   // 调用 Cart 的 addBookToCart(),Cart 是从 Session 中取出
   // 保证一直是一个 Cart
   Cart cart = (Cart)session.getAttribute("cart");
   // 第一次，没有 Cart，创建
   if (cart == null) {
       cart = new Cart();
       // 存放域中
       session.setAttribute("cart", cart);
   }
   
   // 添加购物车
   cart.addBookToCart(book);
   // 前段提示
   session.setAttribute("title", book.getTitle());
   
   // 跳转
   response.sendRedirect(request.getContextPath() + "/index.jsp");
   ```

### 购物车显示

1. 直接在 Session 中取出数据即可

2. 精度问题

   - `0.33 * 0.33` 会出现精度问题

   - 使用 `BigDecimal` 解决

     ```java
     BigDecimal bd1 = new BigDecimal("0.33");
     BigDecimal bd2 = new BigDecimal("0.33");
     
     BigDecimal rs = bd1.multiply(bd2);
     double rsDouble = rs.doubleValue();
     ```


### 跳转回到跳转之前的页面

1. 使用 `request` 的 `Referer` 属性

2. 实现代码

   - 获取 `referer`

     ```java
     String url = request.getHeader("Referer");
     // 跳转
response.sendRedirect(url);
     ```
     

### 修改购物s商品的数量

1. 通过 Map 的 key 值，也就是 book 的 id，就在 `cart.jsp` 的 book 数量上允许手动修改，提交。

2. 涉及两个参数，`book.id` 和 修改的数量

3. 修改 `cart.jsp` 

   - 代码（添加鼠标监听；事件）

     ```javascript
     $(".cartItemCount").change(function(){
     // 调用 CartServlet
     var bookId = $(this).attr("name");
     var count = $(this).val();
     location = "CartServlet?method=updateCartItemCount&bookId=" + bookId + "&count=" + count;
     });
     
     <td><input type="text" class="cartItemCount" name="${cartItem.book.id }" value="${cartItem.count }" size="4"></td>
     ```

### 校验库存，域购买数量是否合法

1. 校验输入数量（`cart.jsp` 页面），通过 JS 判断

   - 使用正则表达式匹配 __正整数__

   - 代码

     ```javascript
     // 获取默认值
     var defValue = this.defaultValue
     countReg = /^\+?[1-9][0-9]*$/;
     if (!countReg.test(count)) {
         alert("输入不合法")
         $(this).val(defvalue); // 默认值赋值回去
         return false;
     } else {
         location = "CartServlet?method=updateCartItemCount&bookId=" + bookId + "&count=" + count;
     }
     ```

2. 校验 `book_client.jsp` 添加购物车的商品数量，是否合法。__使用 Servlet 层验证商品的库存和购物车的总数量__

   - (当 `cart.jsp` 页面的数量达到最大值时，在 `book_client.jsp` 添加购物车还是成功的，所以在此处修改)
- Servlet 验证
   - `book_client.jsp` 修改

## Filter 过滤器

### 介绍

1. 实现 `Filter` 接口，过滤 request 和 response 的

2. 注册（与`Servlet` 几乎相同）

   - `web.xml` 代码

     ```xml
     <filter>
     	<filter-name>TestFilter</filter-name>
         <filter-class>TestFilter</filter-class>
     </filter>
     <filter-mapping>
     	<filter-name>TestFilter</filter-name>
         <url-pattern>/UsreServlet</url-pattern>
     </filter-mapping>
     ```

3. 大概实现过滤过程

   - 必备内容 Servlet 和 Filter
   - Servlet 注册完成，再注册 Filter，但是 Filter 的 `<url-pattern>` 为 Servlet 的，这样才可以过滤 Servlet
   - 前端请求 `UserServlet` 先走 filter ，再走 Servlet
   - 但是过滤，有放行或者不放行
     1. 放行 `chain.doFilter(request, response)`

### 生命周期

1. 构造器
   - 在整个生命周期执行的次数：执行一次
   - 执行时机：启动服务器时执行
2. init() 方法
   - 在整个生命周期执行的次数：执行一次
   - 执行时机：启动服务器时执行
3. doFilter()
   - 在整个生命周期执行的次数：客户端请求一次执行一次
   - 执行时机：过滤请求时执行
4. destroy()
   - 在整个生命周期执行的次数：执行一次
   - 执行时机：关闭服务器时执行

### 工作原理

1. 工作流程
   - 客户端请求
   - 执行过滤器 `request`（是 `chain.doFilter(request, response)` 前的代码）
   - 执行 `chain.doFilter()` 放行，执行 `servlet`
   - 执行过滤 `response` 是 `chain.doFilter(request, response)` 后的代码）
2. __多个 Filter 的过滤顺序__
   - 有 3 个 Filter （1， 2， 3）
   - 放行前 1，2，3；Servlet 响应；放行后 3，2，1
   - 有图更形象
3. 多个 Filter 执行的先后顺序和注册的先后顺序有关 `web.xml` 
   - `<url-pattern>` 在前，先执行

### URL 的配置规则

1. 精准配置（都和 `Servlet` 有关）
   - 配置 Servlet 的  `<url-pattern>/UserServlet</url-pattern>` 
   - 配置 Servlet 的 `<servlet-name>UserServlet</servlet-name>`
2. 模糊配置（包含 `*` 的路径）
   - 前置模糊 ：`<url-pattern>*.jsp</url-pattern>` ，请求 `.jsp` 的过滤器
   - 后置模糊 ：`<url-pattern>/pages/user/*</url-pattern>`  user 下的页面都过滤
   - __没有中间模糊（相对于 `*` 的位置）__ ：`<url-pattern>/*.jsp</url-pattern>`  没有中间模糊

### HttpFilter

1. Servlet 的使用规则
   - 创建 `SelfServlet` ，继承 `HttpServlet` 
   - `HttpServlet` 继承 `GenericServlet` （抽象了 service() 方法）
   - `GenericServlet` 继承 `Servlet` 
   - 以上目的：是为了简化，书写流程
2. Filter 的使用规则
   - 创建 `SelfServlet` ，继承 `HttpFilter`
   - `HttpFilter` ，继承 `Filter`
   - __注意：在 `web.xml` 注册，服务器就会自动创建实类（抽象类，无法实类化）__
3. 自定义创建 `HttpFilter`

### 创建字符集过滤器

1. 创建 Filter 类文件，类名 `CharSetFilter` 继承 `HttpFilter`

2. 修改注册器的 `<url-pattern>` 过滤字符集，所有请求响应都应该过滤

   1. `<url-pattern>/*</url-pattern>` 表示当前项目下的所有内容

3. __可以创建初始化参数，在 `web.xml` 中__

   - 设置字符编码

     ```xml
       <filter>
         <init-param>
         	<param-name>coding</param-name>
         	<param-value>UTF-8</param-value>
         </init-param>
       </filter>
     ```

   - 如何获取

     1. 使用 `this.getFilterConfig()` 获取 `FilterCinfig` 对象（相当于 `ServletConfig`）
     2. 在使用  `FilterCinfig` 对象，调用 `getInitParameter("coding")`

## Listener 监听器

###  介绍

1. 用于监听 JavaWeb 中的事件
   - 例如 `ServletContext` `HttpSession` `ServletRequest` 的创建、修改和删除
2. 监听的对象
3. 监听的事件
4. 监听的需求
5. __监听器的的执行在 Filter 之前__
   - 服务器启动时，创建，并执行
   - 服务器关闭时，销毁

### 监听对象的创建与销毁

### 监听对象的属性变化

### 监听 Session 内的对象

### 创建监听器

1. 实现接口，注册

2. 实现接口

   - 代码

     ```java
     public class ListenerDemo implements ServletContextListener {
     
     	@Override
     	public void contextDestroyed(ServletContextEvent arg0) {
     		// 销毁
     	}
     
     	@Override
     	public void contextInitialized(ServletContextEvent arg0) {
     		// 创建
     	}
     }
     ```

3. 注册

   - 代码（只需要 `class` 即可）

     ```xml
     <listener>
         <listener-class>listener.ListenerDemo</listener-class>
     </listener>
     ```

# 项目第六部分

## 结账

### 分析

1. 结账时，要提示登录（登录才能结账）
2. 生成订单
3. 订单需要持久化（形成数据库的表格）

### 创建订单表和订单详情表

1. 订单表是根据用户信息创建的，所以需要与 Users 表进行管理
   - 一个用户对应多条订单信息（在订单表中加入用户 ID）
2. 订单详情与订单相关联
   - 一个订单对象多条订单详情（在详情中加入订单 ID）
3. 结账的行为结构
   - __将 Session 保存的购物车（Cart）信息，写入订单表中，持久化__
   - __将 Session 保存的购物车的购物项（CartItem），写入订单详情表中，持久化__
   - __更改图书的库存和销量__
   - 清空购物车信息

### 建表

1. 依据项目要求，创建表格，以及表的外键

### 创建 Bean

1. 创建 Order

   - 属性

     ```java
     private String id; // 订单号
     private Date orderTime; // 订单日期
     private int totalCount; // 商品数量
     private double totalAmount; // 商品金额
     private int state; // 商品状态 0未发货；1发货；2订单完成
     private int userId; // 订单所属用户
     ```

2. 创建 OrderItem

   - 属性

     ```java
     private Integer id; // 订单详情id
     private int count; // 同类书，买了几本
     private double amount; // 总金额
     private String title; // 书名
     private String author; // 作者
     private double price; // 单价
     private String imgPath; // 图书封面
     private String orderId; // 订单 id
     ```

### 创建 Dao

1. 创建 `OrderDao`
2. 创建 `OrderItemDao`
3. 在 `BookDao` 增加修改库存和销量的方法
   - 重载 `updateBook(int stock, int sales, int bookId)`

### 前端

1. 创建 `OrderServlet`
   - 这里在 `session` 中获取`cart` 和 `user` ，所以必须登录才会有 `user`

### 解决未登录去结账，报 `null` 错

1. 原因
   - 没有登录时，`Session` 没有用户信息
2. 解决办法
   - 判断是否登录，在去结账之前（在 Servlet 之前 Filter）

### 批处理优化结账

1. 在创建订单时，会对购物车的项目，进行遍历，加入 `order_item` 中，这样就会多次调用接口，使用 __批处理__ 解决

2. 在 `BaseDao` 定义批处理方法

   - 代码

     ```java
         /**
          * 批处理 更新数据库操作
          */
         public int[] batchUpdate(String sql, Object[][] args) {
         	int[] count = null;
             try {
     			count = qr.batch(JDBCUtils.getConnection(), sql, args);
     		} catch (SQLException e) {
     			e.printStackTrace();
     		}
             return count;
         }
     ```

   - 正常方法为 `public int update(String sql, Object... args)` ，参数只是一维数组（多参数，实际就是数组）

3. 参数多出一维的作用

   - 第一个维度，表示执行次数
   - 第二个维度，执行的参数

4. 在 `OrderDao/OrderItem` 重载方法

   - `public abstract void insertOrder(Object[][] args);`
   - `public abstract void insertOrderItem(Object[][] args);`

5. 使用（修改 `OrderService`）

   - 代码

     ```java
     List<CartItem> cartItems = cart.getCartItems(); // 获取购物项
     
     Object[][] orderItemParams = new Object[cartItems.size()][]; // 创建批处理参数的二维数组
     Object[][] bookParams = new Object[cartItems.size()][]; // 创建批处理参数的二维数组
     
     // 有多少购物项，就有多少详情
     for (int i = 0; i < cartItems.size(); i++) {
         CartItem cartItem = cartItems.get(i);
     	// 获取各种属性 double amount = cartItem.getAmount();
         orderItemParams[i] = new Object[] {count, amount, title, author, price, imgPath, orderId};
     
         // 修改库存，计算库存和销量
         bookParams[i] = new Object[] {sales, stock, bookId};
     }
     
     // 批处理数据插入操作
     orderItemDao.insertOrderItem(orderItemParams);
     bookDao.updateBook(bookParams);
     ```

### 事务优化结账

1. 三个 SQL 的操作。因为一个事务，同时成功，或同时失败。
   - 创建订单
   - 创建订单详情
   - 修改库存以及销量
2. Mysql 对事物的设置（开启事务）
   - Mysql 对事务自动提交（执行一条提交一条）
   - 将其设置非禁止自动提交
     1. `connection.setAutoCommit(false)` ，默认是 `true`
   - 提交事务 
     1. `connection.commit()`
     2. `connection.rollback()`
   - __事务是对 `connection` 设置的，所以要保证数据库操作时，`connection` 是同一个__
3. __解析__
   - 操作共用一个 `Connection`
     1. 使用 `ThreadLocal` 管理 `connection` ，这样保证同一个线程使用的是同一个 `Connection`
     2. 数据库操作 `BaseDao`（更新、添加后）不关闭 `Connection`
   - 统一异常处理，使用 Filter 处理异常
     1. 抛出 `BaseDao` 和 `BaseServlet` 异常，Filter 统一处理
     2. 统一开启事务，提交或者回滚
   - __注意__
     1. Filter 捕获的异常不能小于那两个抛出的异常
4. 新建 Filter 管理 Connection

### 结账重复提交问题

1. 按照事务优化结账功能后，当你结账完成时，在浏览器回退到购物车页面，再次提交时（结算时），还是可以产生订单号。

   - 在数据库中可以查看 `order` 的订单信息
   - 在数据库 `order_item` 却没有订单详情

2. 按理说，事务优化已经完成，不可能出现事务不同步现象。

   - 事务同时执行，也同时撤销

3. 分析

   - 当结算购物车时，清空购物车时删除 `map<String Book.id, CartItem cartitem>` 集合，但 `cart` 还在 `session` 域中

   - 结算-创建订单表，取出以下信息

     ```java
     orderDao.insertOrder(new Order(orderId, new Date(), cart.getTotalCount(), cart.getTotalAmount(), 0, user.getId()));
     ```

   - 总金额、总数量都为 0 （看源码）

   - 创建订单详情时使用 __批处理技术__ ，但是在第一次结算时，购物车的购物项（`Map`）被清空了，无法经行循环赋值

     1. `cartItems.size()` 为 0
     2. 批处理，执行了 0 次

4. 原因

   - 第一次结算，只清除购物项（`Cart` 的 `Map` 集合）没有删除 `cart`，再次结算时，创建订单的信息来自 `session` 域中的 `cart` 所以可以创建（但是结算被设计成事务），详情、修改库存时，购物项无法遍历（`cartItem.size() = 0`），所以批处理了 0 次，这就是说，事务正确执行，只是事务执行了 0 次。
   - 表象：数据库有订单信息，但没有详情

5. 解决

   - 在 `Servlet` 第一结算完成时，在 `session` 中删除 `cart` 信息

     ```java
     // 调用
     String orderId = orderService.createOrder(cart, user); // 返回订单号
     session.setAttribute("orderId", orderId);
     session.removeAttribute("cart"); // 删除 session 的 cart
     // 跳转
     response.sendRedirect(request.getContextPath() + "/pages/cart/checkout.jsp");
     ```

   - 但在次结算时，在 `Servlet` 中，获取 `cart` 为空，会报 `Caused by: java.lang.NullPointerException` ，在 Filter 捕获，再次转发。

## Ajax 技术

__说明：[参考地址](https://www.runoob.com/manual/jquery/)__

### 介绍

1. 全称： `Asynchronous JavaScript and XML` ，为异步的 JS 的 XML
2. 作用：不发生页面跳转，异步载入内容并改写页面的技术
   - 也可以简单理解为通过 `JavaScript` 向服务器发送 __异步请求__
   - 异步：不发生页面的跳转（地址栏：不刷新），但载入内容
3. 回调函数
   - 实现原理为回调函数
   - 
4. 异步请求和同步请求
   - 同步请求缺陷
     1. 当只需要局部刷新时，而需要整个页面刷新。
     2. 两次请求，只能等待第一次响应返回
   - 异步优点
     1. Ajax 请求不会影响其他请求
     2. 可以实现局部刷新

### 简单使用（Jquery）

1. API 参考实类

   - 代码

     ```javascript
     $.ajax({
        type: "POST", // 请求方式 GET
        url: "some.php", // 请求地址
        data: "name=John&location=Boston", // 携带的数据
        success: function(msg){
          alert( "Data Saved: " + msg );
        }
     });
     ```

2. 说明

   - 按照上面写法，页面加载时，就执行了 Jquery 中的 Ajax 请求

   - `data` ：为 `request` 携带，获取 `getParameter("param")`
   - `success` ：即为回调函数，如果想处理 `$.ajax` 得到的数据，则需要使用 __回调函数__
     1. `beforeSend`
     2. `dataFilter`
     3. `success`
     4. `complete`

3. 回调函数的作用

   - `beforeSend`
     1. 在发送请求之前调用，并且传入一个XMLHttpRequest作为参数。
   - `error`
     1. 在请求出错时调用。传入XMLHttpRequest对象，描述错误类型的字符串以及一个异常对象（如果有的话）
   - `dataFilter `
     1. 在请求成功之后调用。传入返回的数据以及"dataType"参数的值。并且必须返回新的数据（可能是处理过的）传递给success回调函数。
   - `success`
     1. 当请求之后调用。传入返回后的数据，以及包含成功代码的字符串。
   - `complete`
     1. 当请求完成之后调用这个函数，无论成功或失败。传入XMLHttpRequest对象，以及一个包含成功或错误代码的字符串。

4. 属性

   - 除了上面的 5 个属性，还有一个 `dataType`
   - `dataType` ：服务器预期返回类型
     1. `json` ：__常用__
     2. `text` ：默认格式
     3. `javaScript`
     4. `html`

5. `success` 函数的型参数

   - 设置参数数值

   - 代码

     ```java
     PrintWriter writer = response.getWriter();
     writer.write("hello world");
     ```

### 服务器返回类型为 Json

1. json 文件是以键值对为规范

   - 实例

     ```json
     {"name": "zhangsan", "age": 19}
     ```

2. 生命返回类型为 Json

   - 使用属性 `dataType: "json"`

3. 在 Ajax 中如何取 json 数据

   - 回调函数的参数 `msg` ，直接 `msg.name` ；`msg.age`

   - 实例

     ```javascript
     success: function(msg){
         alert( "Data Saved: " + msg.msg );
     },
     ```

4. Servlet 书写 json

   - 代码

     ```java
     PrintWriter writer = response.getWriter();
     writer.write("{\"msg\": \"hello world\"}");
     ```

   - `\` 表示转义，Java 中字符使用 `''` 

### Ajax 简写

1. get 方式

   - `$.get(url, [data], [callback], [dataType])`

   - __url__ ：待载入页面的URL地址

     __data__ ：待发送 Key/value 参数。

     __callback__ ：载入成功时回调函数。

     __dataType__ ：返回内容格式，xml, html, script, json, text, _default

   - [参考地址](https://www.runoob.com/manual/jquery/)

2. post 方式

3. getJSON

4. 总结

   - __以上回调函数都是 `success` ，但是如果出错，怎什么也不做__

### Json

1. 介绍
   - 全名 `JavaScript Object Notation` ，是 JS 提供的数据交换格式
2. 原理
   - Json 实际就是 JS 的对象（只要满足格式就转成 JS 对象）
   - 不同语言中的数据传递
3. 解析 Json 工具类
   - Google 的 `Gson` 。
   - Gson 提供了 `fromJson()`  和 `toJson()`  两个直接用于解析和生成的方法，前者实现反序列化，后者实现了序列化。同时每个方法都提供了重载方法，我常用的总共有5个。

### Gson 介绍

1. 主要使用 `toJson()` 和 `fromJson()` 方法，生成和解析

2. 对象

   - 代码

     ```java
     Gson gson = new Gson(); // 创建 Gson 对象
     
     Student stu = new Student("zhangsan", 18); // 创建序列化对象
     
     String jsonStu = gson.toJson(stu); // 转化
     
     Student fromJson = gson.fromJson(jsonStu, Student.class); // 转化回对象
     ```

3.  集合类的

   - 代码

     ```java
     Gson gson = new Gson(); // 创建 Gson 对象
     List<Student> list = new ArrayList<>(); // 创建集合
     
     list.add(new Student("zhangsan", 21));
     list.add(new Student("lisi", 25)); // 添加对象
     
     String jsonToList = gson.toJson(list);
     
     List<Student> fromJson = gson.fromJson(jsonToList, new TypeToken<List<Student>>() {}.getType()); // 使用这种方法，才能将集合中的对象转化回来
     ```

   - 如果按照上面的参数 `List<Student>` ，这样只是转换了 List ，List 的 Student 对象没有转化，还是 Jsons

     ```java
     List fromJson = gson.fromJson(jsonToList, List.class); // ctrl + 1 自动提示的，集合无泛型，佐证了 List 转换了，但是 List 泛型（内容）没有转换
     ```


### 使用 Ajax 完善注册页面

1. 问题
   - 注册会提示，登录名是否重复（Dao 底层数据库验证），其他是否合法是 JavaScript 验证的
2. 使用 Ajax 完成异步刷新
   - 输入框的 `change` 事件，提交用户名（验证是否重复），然后局部刷新
3. 修改 `regist.jsp` 输入框
   - 绑定 `change` 事件
4. 在 `UserServlet` 创建验证用户名方法

### 修改购物车

1. 问题
   - 购物车，商品数量是可以修改的，但是每次修改都会重新刷新一次页面
2. 使用 Ajax 完成异步刷新
   - 输入数量时，完成总价钱，总数量的刷新
3. 修改 `cart.jsp` 页面
   - 绑定 `change` 事件
4. 修改 `CartServlet` 方法
   - 将跳转，转换为携带 Json 数据，传给回调函数
5. 总结
   - __Ajax 传回的值，不需要考虑太多，直接使用 JS 赋值即可，之前的 EL / JSTL 不需要管__

## 文件上传和下载

### 上传下载原理

1. 客户端、服务器和应用程序

2. 上传

   - 应用程序在客户端读取数据
   - 应用程序向服务器写入

3. 下载

   - 是相反操作

4. 按照以前的做法

   - 前端 `.jsp`  文件，

     ```html
     uploadFile: <input type="file" name="upfile">
     ```

   - Servlet 获取参数

     ```java
     String file = request.getParameter("upfile");
     ```

   - 只能获取到文件名称（String 类型）

### 实际上传文件要求

1. 表单 `form` 要求
   - 表单提交方式为 `POST`
   - 表单的 enctype 属性必须是 `multipart/form-data`
   - 上传文件的控件是 `input` type 是 `file`
2. 理由
   - `GET` 会丢失数据，超出要求字符长度，会自动丢弃。`POST` 数据在报文中
   - 属性 `enctype` ，改变了传输数据的编码格式，需要文件格式（不能使用默认的形式文本）
3. 使用组件要求
   - `commons-fileupload` 是 `Apache` 开发专门处理上传工具，它的作用就是可以从 `request` 对象解析出，用户请求的参数和上传的文件流
   - `commons-fileupload` 依赖于 `commons-io` ，两个包同时导入

### commons-fileupload

1. 工厂类
   - 工厂类`DiskFileItemFactory` ，用于创建 `ServletFileupload` 、设置缓存等
   - 该类使用构造器创建 `new`
     1. `DiskFileItemFactory dfif = new DiskFileItemFactory();`
   - 设置缓存文件大小
     1. `dfif.setSizeThreshold(int sizeThreshold);` 默认值为 `10kb`
   - 设置文件缓存位置
     1. `dfif.setRepository(File repository);` 默认系统缓存目录
2. 解析类
   - `ServletFileupload` 类，用于解析 `request` 对象从而获取用户发送的请求参数（普通参数和文件）
   - 该类需要调用有参构造器创建实例，构造器需要 `DiskFileItemFactory` 作为参数
     1. `ServletFileUpload sfl = new ServletFileUpload(dfif);`
   - 解析 `request` 对象，获取请求参数，返回 `List<FileItem>` ，保存 `FileItem` 对象，一个对象代表一个请求参数
     1. `List<FileItem> parseRequest = sfl.parseRequest(request);` 此方法会抛出异常
     2. 如果上传文件、总内容超出限制，会报 `FileSizeLimitExceedeExecption`
   - 设置单个文件缓存大小限制
     1. `sfl.setFileSizeMax(long fileSizeMax);` ，单位 `B`
   - 设置请求内容的总大小
     1. `sfl.setSizeMax(long sizeMax);` 单位 `B`
3. 封装请求参数类
   - 该类用于封装用户发送的参数和文件，将用户发送的请求信息封装成 `FileItem` 对象中
   - 该类不需手动创建，`ServletFileItem` 解析 `request` 返回
   - 获取表单项名称（input 的 name 属性）
     1. `String getFieldName()`
   - 获取上传文件名，普通请求参数为 `null`
     1. `String getName()` 
   - 获取内容
     1. `String getString()` ，参数 `encoding` 指定字符集
     2. 文件，将文件流转换为字符串
     3. 请求参数，则获取 `value`
   - 判断当前 `FileItem` 封装的是普通请求参数，还是文件
     1. `boolean isFormField()`
     2. 普通参数返回 `true`
   - 获取文件的 `MIME` 类型
     1. `String getContentType()`
   - 获取内容大小
     1. `long getSize()`
   - 将文件流写入服务器
     1. `write(File file)`
     2. 获取真实路径，在 `Servlet` 中 `String realPath = this.getServletContext().getRealPath("/upload");` ，获取上线文。
   - 清除文件缓存
     1. `delete()`

### 实现文件上传

1. 对应 `form` 表单的设置（3 个注意事项）

2. Servlet 实现

   - 代码

     ```java
     DiskFileItemFactory dfif = new DiskFileItemFactory(); // 工厂类，创建解析器
     
     ServletFileUpload dfu = new ServletFileUpload(dfif); // 创建解析器
     
     String realPath = this.getServletContext().getRealPath("/upload"); // 获取真是路径
     
     try {
         List<FileItem> fileItems = dfu.parseRequest(request); // 解析 request
     
         // 迭代集合
         for (FileItem fileItem : fileItems) {
             if (!fileItem.isFormField()) {
                 // 是文件 返回 false
                 try {
                     // 获取文件名 fileItem.getName() 有后缀名
                     fileItem.write(new File(realPath + "/" + fileItem.getName())); // 写入服务器
                 } catch (Exception e) {
                     e.printStackTrace();
                 }
             }
         }
     } catch (FileUploadException e) {
         e.printStackTrace();
     }
     ```

### 优化

1. 同名文件

   - 问题：同名文件覆盖

   - 使用 UUID 优化同名文件覆盖问题（本质：文件名不同即可）

     1. `UUID.randomUUID().toString.replace("-", "");`

   - 优化代码

     ```java
     try {
         // 以后使用 - 分割 uuid 和文件名
         String uuid = UUID.randomUUID().toString().replace("-", "") + "-";
         fileItem.write(new File(realPath + "/" + uuid + fileItem.getName()));
     } catch (Exception e) {
         // TODO Auto-generated catch block
         e.printStackTrace();
     }
     ```

2. 上传文件大小

   - 设置缓存文件大小（略）
   - 设置单个文件上传大小（如超出大小限制，前端一样显示成功，但在 Servlet 中报错）
     1. `sfl.setFileSizeMax(long fileSizeMax);` `ServletFileUpload df`
   - `Servlet` 捕获异常，跳转

### 下载文件

1. 前端页面直接使用服务器资源路径
   - `<a href="${pageContext.request.contextPath}/download/xxx.mp3"></a>`
     1. 不使用 `requestScope` 是因不是获取域属性，而是获取其他的方法
   - 对应服务器目录下的资源即可
   - 问题
     1. 资源保密性没有
     2. 中文文件，有可能乱码（文件名也在 URL 地址栏中，Ubuntu 没有这个问题）
        - 实际上浏览器直接显示文件内容了（中文乱码，设置响应通知浏览器 `html/text;charset=UTF-8` 应该可以）
     3. 资源放在 `WEB-INF/` 下这种方法无法请求
2. 使用 `Servlet` 完成文件下载
   - `<a href="${pageContext.request.contextPath}/FileDownLoadServlet?fileName=xxx.pm3"></a>`
   - 请求 `FileDownLoadServlet` 的 `Servlet` ，参数是 `fileName`
   - 创建 `Servlet` （设置请求与响应的编码）
     
     1. 代码
     
        ```java
        // 设置编码（请求、响应）
        request.setCharacterEncoding("UTF-8");
        // response.setContentType("text/html;charset=UTF-8"); // 下载文件不再是此浏览器响应类型
        
        // 获取参数（文件名）
        String fileName = request.getParameter("fileName");
        // 获取下载目录地址
        String realPath = this.getServletContext().getRealPath("/download");
        // 拼接下载文件绝对路径
        String filePath = realPath + "/" + fileName;
        
        // 设置浏览器响应类型（参数文件名）
        String mimeType = request.getServletContext().getMimeType(fileName); // 获取文件类型
        System.out.println(mimeType);
        response.setContentType(mimeType); // 设置浏览器响应类型（浏览器按照文件格式打开，如 mp3 则放歌等）
        
        // 设置浏览器响应体内容格式为 附件（不要打开）
        response.setHeader("Content-Disposition", "attachment; filename=" + fileName);
        
        // 解决中文名乱码问题
        String agent = request.getHeader("User-Agent"); // 获取客户端 user-agent
        if (agent != null && agent.contains("Firefox")) {
            fileName = "=utf-8?B?" + new BASE64Encoder().encode(fileName.getBytes("utf-8")) + "?=";
        } else {
            fileName = URLEncoder.encode(fileName, "UTF-8");
        }
        
        // 创建输入输出流
        FileInputStream fis = new FileInputStream(filePath);
        ServletOutputStream outputStream = response.getOutputStream();
        
        byte[] b = new byte[1024];
        while (fis.read(b) != -1) {
            outputStream.write(b);
        }
        
        // 关闭流
        fis.close();
        outputStream.close();
        ```
     
   - __有很多问题没有解决__

