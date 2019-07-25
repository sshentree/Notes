---

---

# Python爬虫

## 爬虫介绍

### 前言

1. 互联网的组成：有无数个简单的网络通过路由器相互连接在一起，从而形成了互联两。因特网是世界上最大的互联网，连接在互联网上的计算机称作主机（服务器、个人pc）。为我们通过浏览器访问服务器，服务器会把HTML、CSS、JS代码返回给浏览器，这些代码经过浏览器的解析、渲染，将五彩缤纷的网页呈现在我们面前。[参考地址](https://www.cnblogs.com/sss4/p/7809821.html)

### 什么是爬虫

1. 如果我们将互联网当作一个蜘蛛网（当然仔细想一下还是满贴切的），数据存放在联入互联网的主机（服务器），每一个主机便是一个节点。
2. 浏览器向节点发送请求（请求有格式规范），等待服务器响应，服务器返回请求数据（HTML、CSS、JS源代码、图片、视频等），浏览器进行解析，再进一步处理。
3. 爬虫：实际上就是，要模仿浏览器向服务器发送请求，将服务器返回的请求数据，做进一步的分析，提取，保存。
4. 爬虫更具使用场景分为：通用爬虫、聚焦爬虫
   - 通用爬虫（搜索引擎）
     1. 目标：尽可能爬取互联网上所有网页，存入本地服务器中，在对网页数据再进一步处理（提取关键字、去掉广告），最后提供用户一个检索接口
     2. 遵循规则：Robots协议会规定通用爬虫可以爬取哪些网页（Robots.txt文件），只是协议，原则上的遵循
   - 聚焦爬虫（针对某种特定关键字）
     1. 目标：针对需求尽可能爬取相关的数据

### 爬虫程序怎么获取网页数据

1. 网页的三大特性

   - 每一个网页都有自己唯一的URL（统一资源定位符）来进行定位
   - 每一个网页都是杨HTML（超文本标记语言）来描述页面信息
   - 每一个网页都使用HTTP、HTTPS协议（超文本传输协议）来传输数据

2. 爬虫思路

   - 确定要爬取网页的URL地址

   - 通过HTPP、HTTPS协议来获取对应的页面的信息

   - 分析、处理页面数据

     1. 保存有用数据

     2. 如果网页有其他URL地址，继续执行第二步（通过HTTP、HTTPS获取数据）

## 解析浏览器请求

说明：以后再读，感觉还不错的文档（浏览器的工作原理）[参考地址](https://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/)

### HTTP、HTTPS协议

1. HTTP协议
   
   说明：[参考地址](https://www.cnblogs.com/wxisme/p/6212797.html)
   
   - HTTP协议（HtperText Transfer Protocol）是基于TCP的应用层协议，他不关系数据传输的细节，__主要是用来规定客户端和服务端的数据传输格式，默认端口是80__.
   - HTTP协议特点：
     1. HTTP协议是无状态的
        - 就是每次的HTTP请求都是独立的。任何两个求情之间没有什么必然联系。但是在实际应用中并不是完全独立的，因为引入了Cookie和Session机制来关联请求
     2. 基于TCP的持久性连接
        - 之前的HTTP协议版本一次请求，就是一次TCP请求连接，请求完成之后TCP连接关闭。TCP建立需要3次握手，断开需要4次挥手，而现在一个网页需要多次HHTTP请求，这样浪费资源。
        - 目前，HTTP协议版本默认不关闭TCP连接、也不用声明Connection:keep-alive字段，如果要关闭TCP连接可以指定Connection:close字段。创建管道机制，再一次TCP连接中，可以发送多次HTTP请求。增加了PUT PATCH OPTIONS DELDET HOST 丰富了客户端和服务器交互形式。
     3. 多次HTTP请求
        - 浏览器请求网页数据时大多数情况并不是一次请求就可以成功。服务器首先响应的HTML页面数据，浏览器接收到页面数据后发现其还引用了大量其他资源（图片、JS），所以浏览器还会自动的发送HTTP请求来获取这些数据。目前，HTTP协议支持管道机制，可以同时请求和响应多个请求，大大提升了效率。
   
2. HTTPS协议

   说明：[参考地址_1](https://kb.cnblogs.com/page/563885/) [参考地址_2](https://juejin.im/post/5a2fbe1b51882507ae25f991) [参考地址_3](https://blog.csdn.net/hherima/article/details/52469267)

   - HTTPS协议（Secure Hypertext Transfer Peotocol）安全超文本传输协议。HTTPS是基于HTTP添加__安全套接字层（SSL）__，HTTP协议采用明文传输信息，信息存在巨大风险。总结来说，HTTPS是HTTP的安全版本，它是用TLS/SSL加密的HTTP协议。HTTPS默认端口为443.

   - TLS/SSL （Transport Layer Security）称之为安全传输协议，介于TCP与HTTP之间的一层安全协议，不影响原来的TCP与HTTP协议的使用。

   - HTTPS设计思路

     说明：__对称加密__ __非对称加密__ __身份验证__

     1. 对称加密

        ![对称加密](git_picture/对称加密.png)

        对称加密就是__密钥s同时扮演加密和解密的角色__， 不能解决信息安全问题

     2. 非对称加密

        说明：非对称算法的特点是，密钥加密的密文，只要是公钥都可以解开，但是公钥加密的密文，只有私钥可以解开，私钥只有一个在本地，而公钥可以发给所有人。

        ![非对称加密](git_picture/非对称加密.png)

        服务器使用私钥加密密文，客户端使用公钥解密。客户端使用公钥加密密文，服务器使用私钥解密。客户端像向服务器发送数据是安全的。
        
     3. 身份验证（了解）
     
        说明：客户端如何安全的获取公钥
     
        - 数字证书
     
          加密服务器公钥，验证证书编号，是否一致（证书上一个编号、解密时会产生另一个）__大概是这么回事__
     
     4. HTTPS与HTTP关系
     
        - 如图解释
     
          > ![HTTPS区别HTTP](git_picture/HTTPS区别HTTP.png)
          >
          > 参考地址_2
     
     5. 总结
     
        > __HTTPS要使客户端与服务器端的通信过程得到安全保证，必须使用的对称加密算法，但是协商对称加密算法的过程，需要使用非对称加密算法来保证安全，然而直接使用非对称加密的过程本身也不安全，会有中间人篡改公钥的可能性，所以客户端与服务器不直接使用公钥，而是使用数字证书签发机构颁发的证书来保证非对称加密过程本身的安全。这样通过这些机制协商出一个对称加密算法，就此双方使用该算法进行加密解密。从而解决了客户端与服务器端之间的通信安全问题__。
        >
        > 参考地址_1

### URL理解

说明：[参考地址](https://blog.csdn.net/hhthwx/article/details/78567961)

1. URL定义

   - 在万维网中，HTML、CSS、JS、图片、视频等等，都有统一的且唯一的地址，该地址叫做 URL (Uniform Resource Locator) 统一资源定位符。它是万维网中统一资源定位标志，就是俗称的‘网址’

2. URL组成部分

   - URL是由三部分组成：资源类型、存放资源主机名、资源文件名

3. URL语句格式

   说明：[] 里的参数为可选

   - `protocal://hostname[:port]/path/[:parameterrs][?query]#fragment`

   - 对URL各个参数进行说明

     1. protocol

        说明：protocol为协议

        指定使用哪种传输协议，最常用的是HTTP协议、更安全的为HTTPS协议

        | 协议名称 | 作用                                                         | 使用格式           |
        | -------- | ------------------------------------------------------------ | ------------------ |
        | HTTP     | 通过HTTP协议访问网络资源                                     | http://            |
        | HTTPS    | 在HTTP上增加安全套接字层更安全的访问网络资源                 | https://           |
        | ftp      | ftp用于网络上控制文件双向传输协议                            | ftp://             |
        | file     | 访问本地计算机资源                                           | file:///(主机缺省) |
        | mailto   | 访问电子邮件                                                 | mailto:(邮箱地址)  |
        | gopher   | ？[参考地址](https://blog.chaitin.cn/gopher-attack-surfaces/) | ？                 |

     2. hostname

        说明：被访问主机名称

        是指存放资源的服务器域名主机名（DNS域名解析）、IP地址。有时。访问服务器是需要密码。其格式`username:password@hostname`.

     3. port

        说明：主机端口号,0-65535个端口号,2的16次方个，2的10次方个 0--1023为固定端口

        可选，缺省时，使用默认端口，传输协议都有默认的端口号，类如，HTTP的默认端口为80、HTTPS默认端口为443。如果输入时省略，则使用默认端口号。有时候出于安全或其他考虑，可以在服务器上对端口进行重定义，即采用非标准端口号，此时，URL中就不能省略端口号这一项

     4. path

        说明：访问的资源在服务器上的路径

        由0个或多个'/'符号隔开的字符串。例如 `https://www.numpy.org.cn/article/basics/understanding_numpy.html` URL地址中`/article/basics/understanding_numpy.html`就是一个文件地址

     5. parameter

        说明：参数

        用于指定特殊参数，可选

     6. query

        说明：查询

        可选，用于给动态网页（如使用CGI、ISAPI、PHP/JSP/ASP/ASP。NET等技术制作的网页）传递参数，可有多个参数，用“&”符号隔开，每个参数的名和值用“=”符号隔开

     7. fragment 

        说明：网页锚点（信息片段 ）

        字符串，用于指定网络资源中的片断。例如一个网页中有多个名词解释，可使用fragment直接定位到某一名词解释。

### HTTP的请求与响应

1. 浏览器发送HTTP请求的过程
   - 在浏览地址栏中输入URL http://www.baidu.com 并按回车，这是浏览器会向的服务器发送HTTP请求。HTTP请求主要包含两种分别为__Get 和 Post 两种方法__。
   - 这里的HTTP请求包含的不只是URL，还有headers信息，这些信息组合成一个Request请求发送给服务器去获取 http://www.baidu.com 的首页HTML文件，服务器把响应文件Response对象返回给浏览器。
   - 浏览器分析Response文件中的HTML信息，发现其中引用了很多其他文件。类如 Image、CSS文件等等，这时浏览器会自动再次发送Resquest请求去获取这些引用的文件。
   - 当浏览器将所有文件都下载完成后，浏览器会根据HTML语法结构，显示、渲染网格网页信息。

### 客户端HTTP请求格式

说明：URL只是标识资源的唯一位置，而HTTP请求是用来提交获取资源的信息，客户端发送一个HTTP请求到服务器端的的数据格式如下

​	__请求行__、__请求头__、__空行__、__请求数据__

- HTTP请求格式如图

  ![HTTP请求格式](git_picture/HTTP请求格式.png)

- HTTP请求示例（没有请求数据）

  1. GET请求
  
     ```tex
     GET https://www.baidu.com/ HTTP/1.1
     Host: www.baidu.com
     Connection: keep-alive
     Upgrade-Insecure-Requests: 1
     User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.99 Safari/537.36
     Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
     Referer: http://www.baidu.com/
     Accept-Encoding: gzip, deflate, sdch, br
     Accept-Language: zh-CN,zh;q=0.8,en;q=0.6
     Cookie: BAIDUID=04E4001F34EA74AD4601512DD3C41A7B:FG=1; (假)
     ```

  2. POST请求（cookie假）
  
     ```tex
     POST https://passport.baidu.com/v2/api/?login HTTP/1.1
     Host: passport.baidu.com
     Connection: keep-alive
     Content-Length: 3198
     Cache-Control: max-age=0
     Origin: https://www.baidu.com
     Upgrade-Insecure-Requests: 1
     Content-Type: application/x-www-form-urlencoded
     User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.142 Safari/537.36
     Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
     Referer: https://www.baidu.com/
     Accept-Encoding: gzip, deflate, br
     Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7
     Cookie: BAIDUID=EFFFF2453D442360DFBBAE2F16F8B7C6:FG=1; (假)
     ```

### 解析HTTP请求

1. 请求方式

   说明：`GET http://www.baidu.com/ HTTP/1.1`

   - 根据HTTP协议标准，HTTP请求可以使用多种请求方式。

     1. 现在普遍使用 HTTP 1.1 版本，有 8 种请求方式：__GET, POST, HEAD, OPTIONS, PUT, DELETE, TRACE, CONNECT方法__。

        | 编号 | 方法名  | 作用及描述                                                   |
        | ---- | ------- | ------------------------------------------------------------ |
        | 1    | GET     | 请求页面信息，并返回实体主体                                 |
        | 2    | HEAD    | 类似于GET请求，只不过返回的响应中没有具体内容，主要用于获取报头 |
        | 3    | POST    | 向指定资源提交数据并进行处理（例如：提交表单或上传文件），数据包含在请求体中。POS请求可能会导致新的资源建立或对已有的资源进行修改 |
        | 4    | PUT     | 从客户端向服务器传送的数据取代指定文档的内容                 |
        | 5    | DELETE  | 请求服务器删除指定的页面                                     |
        | 6    | CONNECT | HTTP/1.1 版本协议中预留给能够将连接改为管道方式代理的服务器  |
        | 7    | OPTIONS | 允许客户端查看服务器的性能                                   |
        | 8    | TRACE   | 回显服务器收到的请求，主要用于测试或诊断                     |

     2. 为普及的HTTP 2.0 版本（感觉已经在使用了，例如：网易）：请求、响应首部的定义基本没有改变，只是所有首部键全部小写，而且请求行要独立为 `:method、:scheme、:host、:path`

        实例：

        ```tex
        :authority: www.163.com
        :method: GET
        :path: /
        :scheme: https
        accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
        accept-encoding: gzip, deflate, br
        accept-language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7
        cache-control: max-age=0
        cookie: mail_psc_fingerprint=debe2f4076573db6bb93b3d4eff333c1; (假)
        ```

     3. HTTP协议主要两种请求辨析 `GET \ POST`

        - GET方法是从服务器获取数据，而POST是将服务器发送数据。

        - GET方法请求参数显示在浏览器的网址栏中，HTTP协议服务器根据该请求中 URL 所包含的参数来产生响应内容，即GET请求的参数是 URL 的一部分。例如：`https://www.google.com.hk/search?q=百度`。

        - POST方法的请求参数在请求体中，信息长度没有限制、以隐式的方式进行传递，通常用来向服务器提交__量大的数据__（例如：大量参数、文件上传、密码的上传），请求的参数包含在`Content-Type`消息中，指明该消息体的媒体类型和编码。

        - __禁止使用GET方式提交敏感数据，会导致安全问题__。

2. 常用请求报头

   说明：`Host: www.baidu.com`

   - HOST（主机和端口号）
     1. HOST;对应 URL 中的主机名和端口号，用于指定被请求资源的主机和端口号，通常式 URL 的一部分

3. Connection（连接类型）

   说明：Connection 表示客户端与服务器来连接的类型 

   `Connection: keep-alive`

   - Client 发送一个 `Connection:keep-alive`  的请求，HTTP/1.1 使用 `keep-alive` 为默认值（长连接，一般关闭浏览器断开）

   - 服务器收到请求后

     1. 如果服务器支持 Keep-alive ，响应一个包含 Connection:keep-alive 的内容，不关闭连。

     2. 如果服务器不支持 Keep-alive ，响应一个包含 Connection:close 的内容，关闭连接

     3. 如果客户端收到包含 Connection:keep-alive 响应，请求网页中的JS、图片、CSS可以直接发送请求，不必再次建立连接，直到一方（客户端、服务器）关闭连接。
     4. __keep-alive 作用式可以重用连接（TCP/IP），缩短响应时间，减少资源消耗，但是也分情况而定

4. Upgrade-Insecure——Request（升极为HTTPS协议）

   说明：HTTPS协议是以安全为目标的HTTP协议通道，所以HTTPS承载的页面不允许出现HTTP请求，一旦出现就会提示或报错 

   `Upgrade-Insecure-Requests: 1`
   
   - Upgrade_Insecure_Requset；升级不安全请求，在使用HTTP请求资源时会自动替换成HTTPS请求，让浏览器不再显示HTTPS页面中的HTTP请求报警.
   
5. Accept（传输文件类型）

   说明：指浏览器或客户端可以接受的 MIME（multipurpose Internet Mail Extensions）(多通途互联网邮件扩展) 文件类型，服务器可以根据它的判断并返回适当的文件格式 

   `Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8`

   - `Accept: */*`: 表示可以接受任何文件格式
- `Accept: image/gif`: 表示希望可以接受GIF格式的图片
   - `Accept: text/html`: 表示希望可以接受html格式文本
   - `Accept: text/html, application/xhtml+xml;q=0.9, image/*;q=0.8`: 表示浏览器支持的MIME类型分别为 html、xhtml、xml格式文本和所有图片格式资源
   - __q是权重系数，范围时 0 =< q <= 1，q值越大请求的资源格式越倾向谁。';' 前面的类型表示获取格式，如果没有指定 q 值，默认为 1。从左到右排序，如果 q = 0 则表示不接受次资源格式 __。
   
6. Referer（页面跳转）

   说明：Referer：表明产生页面的请求来自哪一个 URL，用户是从该 Referer 页面访问当前的页面。这个属性可以用来跟踪 Web 请求来自哪个页面，是从什么网站上来的。

   `Referer: https://www.baidu.com/`

   - 有时，遇到下载网站图片时，需要对应的 Referer ，否则无法下载。原理是根据 Referer 来判断下载图片的请求是否来自本网站的页面，如果是可以下载，否则拒绝。

7. Accept-Encoding(文件解压缩格式)

   说明：Accept-Encoding：指出浏览器可以接受的解压缩格式。解压缩方式不同于文件格式，它是为了压缩文件以加速文件传输速率的，浏览器在接受到服务器响应文件先进性解码，然后再检查文件格式，可以减少大量时间。

   `Accept-Encoding: gzip, deflate, br`

   - 如果多个 Encoding 同时匹配，按照 q 值大小排序，从左到右排序，如果没有标明文件编码格式则可以接受任何编码。

8. Accept-language（语言种类）

   说明：`Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7`

   - 浏览器可以接受的语言种类，如en或en-us指英语，zh或者zh-cn指中文，当服务器能够提供一种以上的语言版本时要用到，也会使用 q 权重系数来排序。

9. Accept-charset（字符编码）

   说明：指浏览器可以接受的字符编码格式

   `Accept-Charset:iso-8859-1,gb2312,utf-8`

   - 一般采用国际标准字符编码 Unicode 的 utf-8（可变长度的字符编码）
   - 如果此属性缺省，表示可以接受任何编码格式

10. Cookie（用户登录状态编码）

    说明：解决HTTP请求独立问题

    `Cookie: BAIDUID=EFFFF2453D442360DFBBAE2F16F8B7C6:FG=1; BIDUPSID=EFFFF2453D442360DFBBAE2F16F8B7C6; PSTM=1560908391;`

    - > HTTP请求是无状态的。也就是说即使第一次和服务器连接后并且登录成功后，第二次请求服务器依然不能知道当前请求是哪个用户。cookie的出现就是为了解决这个问题，第一次登录后服务器返回一些数据（cookie）给浏览器，然后浏览器保存在本地，当该用户发送第二次请求的时候，就会自动的把上次请求存储的cookie数据自动的携带给服务器，服务器通过浏览器携带的数据就能判断当前用户是哪个了。cookie存储的数据量有限，不同的浏览器有不同的存储大小，但一般不超过4KB。因此使用cookie只能存储一些小量的数据.
      >
      > [参考地址](https://www.cnblogs.com/xxtalhr/p/9053906.html)
      >
      > [参考地址](http://bubkoo.com/2014/04/21/http-cookies-explained/)
    
11. Cookie 和 Session

    说明：Cookie存储再客户端，Session存储在服务器端

    - 服务器和客户端的交互仅限于请求和响应，结束便断开，在下一次请求时，服务器会认是新的客户端（HTTP请求相互独立）。为了维护他们之间的链接，让服务器知道这是前一个用户发送的请求，必须在一个地方保存客户端的信息。

      **Cookie**：客户端 记录用户的身份。

      **Session**：服务器端 记录确定用户的身份。

### 解析HTTP响应报文

- HTTP响应是由四部分组成：__状态行、消息报头、空行、响应正文__

  ![HTTP响应格式](git_picture/HTTP响应格式.png)

- HTTP响应实例

  ```tex
  HTTP/1.1 200 OK
  Server: Tengine
  Connection: keep-alive
  Date: Wed, 30 Nov 2016 07:58:21 GMT
  Cache-Control: no-cache
  Content-Type: text/html;charset=UTF-8
  Keep-Alive: timeout=20
  Vary: Accept-Encoding
  Pragma: no-cache
  X-NWS-LOG-UUID: bd27210a-24e5-4740-8f6c-25dbafa9c395
  Content-Length: 180945
  
  <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" ....
  ```

- 状态行

  说明：状态行是由 3 部分组成 ，版本协议、状态码、状态码描述，之间有空格分割

  `HTTP/1.1 200 OK`

  1. 状态码解释

     | 状态码  | 解释用途                                                     |
     | :-----: | :----------------------------------------------------------- |
     | 100-199 | 表示 服务器成功接受部分请求，要求客户端继续提交余下请求才能完成整个处理过程 |
     | 200-299 | 表示 服务器成功接受请求并处理完成。（常用 200  ok）          |
     |   301   | 表示 永久重定向，搜索引擎将删除源地址，保存重定向地址        |
     |   302   | 表示 暂时重定向，重定向地址由响应头中的Location属性指定<br>（JSP中Forward和Redirect之间的区别）<br>由于搜索引擎的判定问题，较为复杂的URL容易被其它网站使用更为精简的URL及302重定向劫持 |
     |   304   | 表示 缓存文件未过期，可以继续使用，无需再次请求服务器获取数据 |
     |   400   | 表示 客户端请求有语法错误，不能被服务器识别                  |
     |   403   | 表示 服务器成功接受客户端请求，但拒绝提供服务（认证失败）    |
     |   404   | 表示 请求数据服务器无法提供（服务器找不到）                  |
     | 500-599 | 表示 服务器端出现错误（客户端不知道，也不敢问）              |

- 消息报头

  1. Server

     说明：`Server: Tengine`

     - 服务器名称和对应的版本。

  2. Cnnection

     说明：`Connection: keep-alive`

     - 作为回应客户端HTTP请求的 Connection：keep-alive ，通知客户端服务器的 tcp 连接也是一个长连接，客户端可以继续使用这个tcp连接发送HTTP请求。

  3. Date

     说明：`Date: Wed, 30 Nov 2016 07:58:21 GMT`

     - 服务器发送资源时的__服务器时间__（GMT是格林尼治所在地的标准时间）。HTTP请求中发送的时间都是GMT，主要是解决在互联网上，不同时区在相互请求资源的时候，时间混乱问题。

  4. Cache-Control（重要技术）

     说明：缓存的方式；强制缓存、对比缓存。数据缓存示意图 [缓存示意图](Notes\git_picture\HTTP-Response-Cache)

     `Cache-Control: no-cache`

     [参考地址](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Caching_FAQ)

     [参考地址](https://www.jianshu.com/p/dedb04225bc5) （建议读一下，想了解浏览器缓存的）

     - 浏览器缓存解析

       1. Cache-Control参数
  
          | 参数     | 作用                               |
          | -------- | ---------------------------------- |
          | private  | 默认值private，仅客户端可以缓存    |
          | public   | 客户端、代理服务器都可以缓存       |
          | no-cache | 使用对比缓存验证缓存数据是否可用   |
       | max-age  | max-age=XXX，缓存的数据将保存XXX秒 |
          | no-stroe | 所有数据都不缓存                   |

       2. 对比缓存：需要用来验证缓存数据是否可用的标识__（基于数据已缓存）__

          - 请求数据时，发送属性为 If-Modified-Since 获取数据的时间

            `If-Modified-Since: Tue, 27 Jun 2017 11:09:34 GMT`

            `Last-Modified: Tue, 27 Jun 2017 11:09:35 GMT`

          - 服务器响应响应请求时，发送属性为 Last-Modified 最后修改的时间

            `Last-Modified: Tue, 27 Jun 2017 11:09:35 GMT`

          - If-modifief-Snice 和 Last-Modified 的值大小

          - Etag 和 If-None-Match（优先级高于Last-Modified 和 If-Modified-Since）

            Etag：服务器响应请求时，告诉客户端当前资源再服务器的唯一标识

            `Etag: "ABfYctUWoo6IcnFWDuXnoFDYRhTh"`

            If-None-Match：再次请求服务器时，服务器收到带有 If-None-Match 属性的请求后，与被请求资源的唯一标识进行比对，不同，说明资源又被改动过，则响应整片资源内容，返回状态码 200；相同，说明资源无修改，则响应 HTTP 304，告知浏览器继续使用所保存的cache。

  5. Content-Type

     说明：`Content-Type: text/html;charset=UTF-8`
  
     - 通知客户端，响应资源文件的类型、字符编码，客户端通过规定解码方式对资源进行解码，然后对资源进行html解析。通常有些网站是乱码，往往就是服务器端没有返回正确的编码。

## 待续



