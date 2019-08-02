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

## TCP/IP协议族理解

### TCP 3 次握手，4 次挥手

1. 关键名词

   SYN：请求握手包

   FIN：请求挥手包

   ACK：确认包

   SYN+ACK：请求握手 + 确认包

   FIN+ACK：请求挥手 + 确认包

2. TCP 3 次握手

   - 第一次握手，客户端向服务器发送 SYN，请求服务器建立连接。
   - 第二次握手。服务器向客户端发送 SYN+ACK，服务器建立连接完成，请求客户端建立连接，并确认。
   - 第三次握手，客户端向服务器发送 ACK，确认客户端也建立完成，可以通信。
   - __注：每次发送请求包、确认包中都包含两个参数 SEQUENCE_NUM 和 ACK_NUM 用来检测请求是否成功__

3. 数据发送形式

   - 在建立连接完成之后，再发送数据
   - 等待对方收到数据，再次发送确认包
   - 如一段时间没有收到确认包，即再次发送数据
   - TCP 比 UDP 稳定

4. TCP 4 次挥手

   说明：假设客户端先请求关闭连接

   - 第一次挥手，客户端发送 FIN 通知服务器我要关闭连接。
   - 第二次挥手，服务器发送 ACK 确认我收到。
   - 第三次挥手，服务器发送 FIN 通知我也要关闭连接。
   - 第四次挥手，客户端发送 ACK 确认收到。

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
     Cookie: BAIDUID=04E4001F34EA74AD4601512DD3C41A7B:FG=1; BIDUPSID=04E4001F34EA74AD4601512DD3C41A7B; PSTM=1470329258;            MCITY=-343%3A340%3A; BDUSS=nF0MVFiMTVLcUh-Q2MxQ0M3STZGQUZ4N2hBa1FFRkIzUDI3QlBCZjg5cFdOd1pZQVFBQUFBJCQAAAAAAAAAAAEAAADpLvgG0KGyvLrcyfrG-AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAFaq3ldWqt5XN; H_PS_PSSID=1447_18240_21105_21386_21454_21409_21554; BD_UPN=12314753; sug=3; sugstore=0; ORIGIN=0; bdime=0; H_PS_645EC=7e2ad3QHl181NSPbFbd7PRUCE1LlufzxrcFmwYin0E6b%2BW8bbTMKHZbDP0g; BDSVRTM=0
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
        | 3    | POST    | 向指定资源提交数据并进行处理（例如：提交表单或上传文件），数据包含在请求体中。POST请求可能会导致新的资源建立或对已有的资源进行修改 |
        | 4    | PUT     | 从客户端向服务器传送的数据取代指定文档的内容                 |
        | 5    | DELETE  | 请求服务器删除指定的页面                                     |
        | 6    | CONNECT | HTTP/1.1 版本协议中预留给能够将连接改为管道方式代理的服务器  |
        | 7    | OPTIONS | 允许客户端查看服务器的性能                                   |
        | 8    | TRACE   | 回显服务器收到的请求，主要用于测试或诊断                     |

     2. 为普及的HTTP 2.0 版本（感觉已经在使用了）：请求、响应首部的定义基本没有改变，只是所有首部键全部小写，而且请求行要独立为 `:method、:scheme、:host、:path`

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
   
5. Uesr-Agent（浏览器名）

   说明：客户端浏览器名称，在互联网中浏览器是一个合法身份去访问其他网站的身份。

   [参考地址](https://www.jianshu.com/p/c5cf6a1967d1)

   `User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.99 Safari/537.36`

   - 解释 Uesr-Agent

     Mozilla/5.0 (平台) 引擎版本 浏览器版本号

6. Accept（传输文件类型）

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

## urllib.request、urllib.parse使用

说明：Python2 中使用 urllib2，而到了 Python3 时，使用 urllib.request，其他方法调用方式一样。

[urllib.request官方地址](https://docs.python.org/3.6/library/urllib.request.html?highlight=urllib#module-urllib.request) [urllib.parse官方地址](https://docs.python.org/3.6/library/urllib.parse.html?highlight=urllib#module-urllib.parse)

- urllib.requset模块作用

  > The [`urllib.request`](https://docs.python.org/3.6/library/urllib.request.html?highlight=urllib#module-urllib.request) module defines functions and classes which help in opening URLs (mostly HTTP) in a complex world — basic and digest authentication, redirections, cookies and more.

- urllib.parse模块作用

  > This module defines a standard interface to break Uniform Resource Locator (URL) strings up in components (addressing scheme, network location, path etc.), to combine the components back into a URL string, and to convert a “relative URL” to an absolute URL given a “base URL.

### urllib.request.urlopen()使用

1. 代码实列

   ```python
   # urllib.request用于HTTP/1.1,并且请求中属性Connection: close(请求完成及关闭连接)
   import urllib.request
   
   
   # 定义一个url地址
   request = 'http://www.baidu.com'
   
   # 使用urlopen()打开url地址，但是参数url可以是url地址字符串，也可以时Request对象
   # urlopen()返回值为，服务器响应的类文件对象
   response = urllib.request.urlopen(url=request)
   
   # 打印服务器响应类文件对象
   print('*' * 39)
   print(response)
   
   # 打印，验证是否为响应类文件对象
   print('*' * 39)
   print(type(response))
   
   # 类文件对象，支持文件对象的操作方法，如read()方法读取文件全部内容，返回字节（b）
   html = response.read()
   
   # 打印验证。是否为字节
   print('*' * 39)
   print(type(html))
   
   # 将字节解码为字符串，默认格式为utf-8
   html = html.decode('utf-8')
   
   print('*' * 39)
   print(html)
   
   运行结果
   <http.client.HTTPResponse object at 0x0000023E3A1C8C18>
   ***************************************
   <class 'http.client.HTTPResponse'>
   ***************************************
   <class 'bytes'>
   ***************************************
   <!DOCTYPE html>
   <!--STATUS OK-->
   空很多行....
   <html>
   <head>
   
       <meta http-equiv="content-type" content="text/html;charset=utf-8">
       <meta http-equiv="X-UA-Compatible" content="IE=Edge">
           <meta content="always" name="referrer">
       <meta name="theme-color" content="#2932e1">
       <link rel="shortcut icon" href="/favicon.ico" type="image/x-icon" />
       <link rel="search" type="application/opensearchdescription+xml" href="/content-search.xml" title="百度搜索" />
       <link rel="icon" sizes="any" mask href="//www.baidu.com/img/baidu_85beaf5496f291521eb75ba38eacbd87.svg">
   
   
           <link rel="dns-prefetch" href="//s1.bdstatic.com"/>
           <link rel="dns-prefetch" href="//t1.baidu.com"/>
           <link rel="dns-prefetch" href="//t2.baidu.com"/>
           <link rel="dns-prefetch" href="//t3.baidu.com"/>
           <link rel="dns-prefetch" href="//t10.baidu.com"/>
           <link rel="dns-prefetch" href="//t11.baidu.com"/>
           <link rel="dns-prefetch" href="//t12.baidu.com"/>
           <link rel="dns-prefetch" href="//b1.bdstatic.com"/>
   
       <title>百度一下，你就知道</title>
   ```

2. 解释：验证爬下代码是否和浏览器的相同，在浏览器中查看源代码。

### urllib.request.Request()使用

1. 在使用 urllib.request.urlopen() 中 url 参数为 url地址（字符串），如果我们需要添加 HTTP 报头时，就需要使用 urllib.request.Request() 实例来作为 urllib.requset.urlopen()  的 url 参数的实参。

2. 验证使用 urllib.request.Request() 是否和上一个效果一样,代码实列

   ```python
   import urllib.request
   
   
   url = 'http://www.baidu.com'
   
   # 初始化 Request 对象
   request = urllib.request.Request(url=url)
   
   # 将 Request 对象作为 url 形参的实参
   response = urllib.request.urlopen(url=request)
   
   html = response.read()
   print(type(html))
   
   # 默认的解码方式为 UTF-8
   html = html.decode()
   print(type(html))
   print(html)
   ```

3. 解释：运行结果对比一样

4. 添加 HTTP 报头

   - 浏览器就是互联网公认合法的身份，如果我们希望我们的爬虫程序逼真的模仿一个真实用户。第一步，就是需要伪装成一个被公认的浏览器。用不同的浏览器在发送请求的时候，会有不同的User-Agent属性。 urllib2默认的User-Agent头为：`urllib"Python-urllib/x.y`  x和y是Python主版本和次版本号,例如 Python-urllib/3.6）。

   - __解析HTTP请求__也已经说明 headers 里面有很多属性，可以通过 `urllib.request.Request()` 的对象的方法 `Request.add_header(key='', val='')`来添加属性值，还可以通过 `Resquest.get_header(header_name='')` 来获取已有的属性

   - 添加 User-Agent 代码实例（最常用的属性之一）
   
     ```python
     import urllib.request
     
     
     # url.request.Resquest() 中的 headers形参 是 HTTP 请求报头，以字典的形式添加请求的属性
     # Uesr-Agent的信息
     headers = {'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.100 Safari/537.36'}
     url = 'http://www.baidu.com' 
     
     request = urllib.request.Request(url= url, headers=headers)
     
     # 向指定url地址发送请求，并返回服务器响应的类文件对象
     response = urllib.request.urlopen(request)
     
     html = response.read().decode()
     print(type(html))
     print(html)
     
     运行结果
     <class 'str'>
     <!DOCTYPE html> 
     <!--STATUS OK-->
     空行...
    html文件
     ```
   
   - 添加其他属性（Connection）
   
     ```python
     import urllib.request
     
     
     # url.request.Resquest() 中的 headers形参 是 HTTP 请求报头，以字典的形式添加请求的属性
     # Uesr-Agent的信息
     headers = {'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.100 Safari/537.36'}
     url = 'http://www.baidu.com' 
     
     request = urllib.request.Request(url= url, headers=headers)
     
     # 添加一个 Connection 属性
     request.add_header(key='Connection', val='Keep-alive')
     
     # 获取属性已有属性值
     # 在获取 User-agent 时，注意大小写
     print('*' * 39)
     print(request.get_header(header_name='User-agent'))
     print(request.get_header(header_name='Connection'))
     print('*' * 39)
     
     # 获取已有属性，返回值为列表
     # Request.header_items() -> List[Tuple[str, str]]
     list_headers = request.header_items()
     print(type(list_headers))
     print('*' * 39)
     
     for val in list_headers:
         print(val)
     
     print('*' * 39)
     # 向指定url地址发送请求，并返回服务器响应的类文件对象
     # response = urllib.request.urlopen(request)
     
     # html = response.read().decode()
     # print(type(html))
     # print(html)
     
     运行结果
     ***************************************
     Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.100 Safari/537.36
     Keep-alive
     ***************************************
     <class 'list'>
     ***************************************
     ('User-agent', 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.100 Safari/537.36')
     ('Connection', 'Keep-alive')
     ***************************************
     ```
   
   - 随机选取 User-Agent 属性
   
     ```python
     import urllib.request
     import random
     
     
     url = 'http://www.baidu.com/'
     
     # 构造一个浏览器代理列表
     list_ua = [
         "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.6; rv2.0.1) Gecko/20100101 Firefox/4.0.1",
         "Mozilla/5.0 (Windows NT 6.1; rv2.0.1) Gecko/20100101 Firefox/4.0.1",
         "Opera/9.80 (Macintosh; Intel Mac OS X 10.6.8; U; en) Presto/2.8.131 Version/11.11",
         "Opera/9.80 (Windows NT 6.1; U; en) Presto/2.8.131 Version/11.11",
         "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_0) AppleWebKit/535.11 (KHTML, like Gecko)      Chrome/17.0.963.56 Safari/535.11"
     ]
     
     # 在list_ua列表中随机选取一个代理
     user_agent = random.choice(list_ua)
     
     # 构造一个请求
     request = urllib.request.Request(url=url)
     
     # 使用 add_header() 方法 添加一个HTTP报头
     request.add_header('User-Agent', user_agent)
     
     # get_header()获取 HTTP 的报头的 User-Agent 属性
     print('随机选取的User-Agent')
     print(request.get_header('User-agent'))
     
     运行结果
     随机选取的User-Agent
     Opera/9.80 (Macintosh; Intel Mac OS X 10.6.8; U; en) Presto/2.8.131 Version/11.11
     ```
   
5. 获取服务器响应的内容（状态码、实际的响应地址、报头信息等）

   ```python
   import urllib.request
   
   
   # Uesr-Agent的信息
   header_ua = {'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.100 Safari/537.36'}
   
   request = urllib.request.Request(url='http://www.baidu.com', headers=header_ua)
   
   response = urllib.request.urlopen(request)
   
   # 服务器响应返回的类文件对象支持Python文件对象的操作方法
   # read()方法读取全部内容，返回字符串
   html = response.read()
   print(type(html))
   # 打印状态码
   # HTTP响应码 200成功、5服务器出错
   print(response.getcode())
   
   # 实际响应的url，防止重定向
   print(response.geturl())
   
   print('+' * 20)
   
   # 返回服务器响应的HTTP报头
   print(response.info())
   
   
   运行结果
   <class 'bytes'>
   200
   https://www.baidu.com/
   ++++++++++++++++++++
   Bdpagetype: 1
   Bdqid: 0xe3027768001ffcd7
   Cache-Control: private
   Content-Type: text/html
   Cxy_all: baidu+22a54232e49cbb8e239b5ee93abd1b24
   Date: Sun, 28 Jul 2019 07:02:18 GMT
   Expires: Sun, 28 Jul 2019 07:01:22 GMT
   P3p: CP=" OTI DSP COR IVA OUR IND COM "
   Server: BWS/1.1
   Set-Cookie: BAIDUID=417C4FA013EB3CF60C3661BC828D8B77:FG=1; expires=Thu, 31-Dec-37 23:55:55 GMT; max-age=2147483647; path=/; domain=.baidu.com
   Set-Cookie: BIDUPSID=417C4FA013EB3CF60C3661BC828D8B77; expires=Thu, 31-Dec-37 23:55:55 GMT; max-age=2147483647; path=/; domain=.baidu.com
   Set-Cookie: PSTM=1564297338; expires=Thu, 31-Dec-37 23:55:55 GMT; max-age=2147483647; path=/; domain=.baidu.com
   Set-Cookie: delPer=0; path=/; domain=.baidu.com
   Set-Cookie: BDSVRTM=0; path=/
   Set-Cookie: BD_HOME=0; path=/
   Set-Cookie: H_PS_PSSID=1469_21092_18560_29520_28518_29098_29567_28833_29221_26350; path=/; domain=.baidu.com
   Strict-Transport-Security: max-age=172800
   Vary: Accept-Encoding
   X-Ua-Compatible: IE=Edge,chrome=1
   Connection: close
   Transfer-Encoding: chunked
   ```

### urllib.parse.urlencode()使用

1. URL中 `protocal://hostname[:port]/path/[:parameterrs][?query]#fragment` 有查询的参数、锚点参数，一般 HTTP 的 GET 请求数据时（带有参数），需要将__参数__编码成 URL 格式， 然后做成 URL 的一部分。

2. HTTP 的 POST 请求数据时（带有参数），但与 GET 方法不同，POST的参数在 HTTP 请求体中，就是__请求数据__（最后一行，一般抓包工具在 form_body 中可以查看 POST 的提交的数据具体包含什么信息）。

3. 代码演示

   ```python
   import urllib.request
   import urllib.parse
   
   word = {'wd': '沈阳'}
   
   # 进行编码
   encode_word = urllib.parse.urlencode(word)
   
   # 进行解码
   decode_word = urllib.parse.unquote(encode_word)
   
   print(word)
   print(encode_word)
   print(decode_word)
   
   运行结果
   {'wd': '沈阳'}
   wd=%E6%B2%88%E9%98%B3
   wd=沈阳
   ```

### GET请求

1. GET 方式请求一般注意： __请求可能被缓存、请求保存在浏览器历史记录中、请求可能被收藏为书签、请求不应在处理敏感数据使用、请求有长度限制（GET方法向 URL 添加数据，而 URL 最大长度为 2048 个字节）、用于向服务器获取数据__。

   - 类如：用百度搜索 _沈阳_  `[https://www.baidu.com/s?wd=%E6%B2%88%E9%98%B3](https://www.baidu.com/s?wd=沈阳)` 浏览器地址栏显示  `https://www.baidu.com/s?wd=沈阳` 但实际发送的数据是 `https://www.baidu.com/s?wd=%E6%B2%88%E9%98%B3` ,可以看出实际上是先进性编码，然后在发送的。

   - 带有查询参数的代码演示

     ```python
     import urllib.request
     import urllib.parse
     
     
     word = {'wd': '沈阳'}
     
     # 进行编码
     encode_word = urllib.parse.urlencode(word)
     
     url = 'http://www.baidu.com/s'
     # 拼接url地址
     url = url + '?' + encode_word # 使用 ？ 号来标识查询参数
     header = {
         'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36'
         }
     
     request = urllib.request.Request(url=url, headers=header)
     
     resopnse = urllib.request.urlopen(request)
     
     # 实际使用 read() 读出的是字节码，需要进行解码
     html = resopnse.read().decode() # 默认解码方式为 UTF-8
     print(type(html))
     print(html)
     
     运行结果
     <class 'str'>
     <!DOCTYPE html> 
     <!--STATUS OK-->
     空行...
     内容
     ```

2. 批量爬取贴吧页面数据

   说明：URL 参数使用 `?` 开始，使用 `&` 隔开

   - 分析贴吧的 URL 地址 `https://tieba.baidu.com/f?ie=utf-8&kw=美女&fr=search` ，实际上有些参数是可以不需要的，精简之后的 URL 地址 `https://tieba.baidu.com/f?&kw=美女` ，第 2 页的 URL 地址精简之后`https://tieba.baidu.com/f?kw=美女&pn=50`， 第 3 页的 URL 地址 `https://tieba.baidu.com/f?kw=美女&pn=100`，可以看出除了首页之外每增加一页，参数 `pn` 增加 50 ，但是你试着访问 `https://tieba.baidu.com/f?kw=美女&pn=0` 会发现其实也是贴吧首页。

   - 确定 URL 地址、确定 URL 地址参数，进行编码，组合完整的 URL 地址

   - 代码演示

     ```python
     import urllib.parse
     import urllib.request
     
     
     def loadPage(url, file_name):
         """
             作用：根据url发送请求，获取服务器响应文件
             url：需要爬取的url地址
             file_name：爬取页面的存放文件名称
         """
         print('正在下载：' + file_name)
         headers = {
             'User-Agent':
             "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.6; rv2.0.1) Gecko/20100101 Firefox/4.0.1",
         }
         request = urllib.request.Request(url, headers=headers)
         response = urllib.request.urlopen(request)
         # print(response)
         return response
         
     
     
     def writePage(html, file_name):
         """
             作用：将html文件写入本地
             html：服务器响应文件
             file_name：响应文件存放的文件名
         """
         print('正在保存：' + file_name)
         # 文件写入
         with open('C:/Users/SS沈/Desktop/Ten/爬虫/爬虫基本概念/案例/' + file_name, 'wb', encoding='utf-8') as f:
            
             # print(type(html))
             # html是一个
             # <class 'http.client.HTTPResponse'>
             # 类型 所以要进行转换为bytes类型，进行存储
             html = html.read()
             # print(type(html))
             # html = html.decode('UTF-8')
             # print(type(html))
     
             f.write(html)
         print('*' * 30)
     
     
     def tiebaSpider(url, beginPage, endPage):
         """
             作用：贴吧爬虫调度器，负责组合每个url和起始、页结束页
             url：贴吧url的前部分
             beginPage：贴吧起始页
             endPage：贴吧结束页
         """
         for page in range(beginPage, endPage + 1):
             pn = (page - 1) * 50
     
             file_name = '第' + str(pn / 50 + 1) + '页.html'
             # 组合完整的url
             fullurl = url + '&pn=' + str(pn)
             # print(fullurl)
             html = loadPage(fullurl, file_name)
             writePage(html, file_name)
     
     
     if __name__ == '__main__':
         kw = input('请输入贴吧名称：')
         beginPage = int(input('请输入起始页：'))
         endPage = int(input('请输入结束页：'))
     
         url = 'http://tieba.baidu.com/f?'
     
         # 进行组合字典
         kw = {'kw': kw}
         # 进行编码
         kw = urllib.parse.urlencode(kw)
         # 组合完整URL
         fullurl = url + kw
         # print(fullurl)
         # 调用主函数
         tiebaSpider(fullurl, beginPage, endPage)
     ```

### POST请求

1. POST 方式一般注意：__请求不会被缓存、请求不会被保存在浏览器历史记录中、不能被收藏为标签、对数据长度应该没有要求__。

   - 类如：POST 方法打开的 URL 地址 `http://fanyi.youdao.com/` 输入 `python` 进行翻译时，URL 地址没有发生变化，但单词 `python` 时进行了正常的翻译。实际请求数据的发送不是和 GET请求一样包含在 URL 中，而是在HTTP 请求体中的请求数据中。

   - 如图说明 POST 提交数据格式

     ![POST请求数据格式](git_picture/POST请求数据格式.png)

   - 如图说明 POST 提交数据具体样式（属性和对应的值）

     ![查看POST提交数据](git_picture/查看POST提交数据.png)

   - 如图说明响应数据格式

     ![POST响应数据格式](git_picture/POST响应数据格式.png)

2. POST 四种提交数据方式

   - `application/x-www-form-urlencoded` 默认，采用 `key_1=val_1&key_2=val_2` 进行编码。__大部分 Ajax 提交数据也是用这种方法__。

   - `multipart/form-data` 不会

   - `application/json` 使用 __JSON__ 格式，逐渐流行（简单流行）

3. 代码演示

   - 翻译网站爬取数据

     ```python
     import urllib.request
     import urllib.parse
     
     
     url = 'http://fanyi.youdao.com/translate?smartresult=dict&smartresult=rule'
     
     headers = {
         'Host': 'fanyi.youdao.com',
         'Accept': 'application/json, text/javascript, */*; q=0.01',
         'X-Requested-With': 'XMLHttpRequest',
         'User_Agent': "Mozilla/5.0 (Windows NT 6.1; rv2.0.1) Gecko/20100101 Firefox/4.0.1",
         'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',
         'Accept-Language': 'zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7',
     }
     
     key = input('请输入要翻译的文字：')
     
     
     # 构造表单数据(字典形式)
     form_data = {
         'i': key,
         'from': 'AUTO',
         'to': 'AUTO',
         'smartresult': 'dict',
         'client': 'fanyideskweb',
         'salt': '15613546005846',
         'sign': '1ba91814d5099b7e84d373de9c207bb0',
         'ts': '1561354600584',
         'bv': '3a019e7d0dda4bcd253903675f2209a5',
         'doctype': 'json',
         'version': '2.1',
         'keyfrom': 'fanyi.web',
         'action': 'FY_BY_REALTlME',
     }
     
     # 进行 URL 转码
     # 还要进行编码（bytes）转码
     data = urllib.parse.urlencode(form_data).encode('utf-8')
     # print(data)
     
     # 参数data有值 为post请求
     request = urllib.request.Request(url=url, data=data, headers=headers)
     
     reponse = urllib.request.urlopen(request)
     
     print(reponse.read().decode())
     ```

   - 运行结果

     ```json
     {
         "type": "EN2ZH_CN",
      "errorCode": 0,
         "elapsedTime": 1,
      "translateResult": [
             [
              {
                     "src": "welcome",
                     "tgt": "欢迎"
                 }
             ]
         ]
     }
     ```
   
   - 说明：_不知道哪里出现问题_，爬下来响应数据和抓包工具抓取的数据不一致（抓包工具的数据和页面显示数据一致）
   
     抓包数据如下：(可以上网找一下 JSON 数据在线解析的网站对比一下)
   
     ```json
     {
         "translateResult": [
             [
                 {
                     "tgt": "欢迎",
                     "src": "welcome"
                 }
             ]
         ],
         "errorCode": 0,
         "type": "en2zh-CHS",
         "smartResult": {
             "entries": [
                 "",
                 "adj. 受欢迎的；令人愉快的；可随意的；尽管……好了\r\n",
                 "n. 欢迎；迎接；接受\r\n",
                 "v. 欢迎，迎接；迎新；乐于接受\r\n"
             ],
             "type": 1
         }
     }
     ```

### 获取 AJAX 加载的内容

说明：

1. 了解 Ajax 

   - Ajax 概念

     Ajax 是一种用于创建快速动态页面的技术

     通过在后台与服务器进行少量请求、响应，Ajax 可以使网页实现异步更新。Ajax 的应用可以在不重新加载网格网页的情况下，对网页的某一部分进行更新。

     解释：在访问一个普通网站时，当浏览器加载完 `html CSS Js` 时，页面内容就固定了，如果想让页面内容发生改变，就必须刷新页面才可以看到。

     不使用 Ajax 技术的传统网页，如果需要跟新内容，必须重新加载网格页面

     使用 Ajax 技术的网站，如微博、Google 地图（点击加载更多时，会帮我们加载更多的内容，同时页面没有刷新）。

   - 本人理解（就爬虫而言）

     就 Ajax 而言是一种不需要刷整个新页面就可以更新部分页面数据，但是，只要刷新了页面（不管时是全部页面还是部分页面），那一定获取了新的数据，有新的数据就一定会有新的且唯一 URL 地址来标识。但是本人不理解的浏览器地址栏的信息有的会发生变化，有的不会变化，但 2 者页面数据都有部分跟新，而整张页面没有刷新，实际你使用抓包工具时，你会发现，实际当你每次点击加载更多的时候，都有一个新的请求发送出去（对应新的 URL 地址），对应新的响应文件。感觉，Ajax 的技术，时加载格式一样的数据使用频发，只要网页使用 Ajax 技术的，绝大部是 JSON 格式。

2. 解析 Ajax 的页面加载

   - 要访问的 URL 地址 `https://movie.douban.com/tag/#/` ,实际访问的 URL 地址 `https://movie.douban.com/tag/ `

   - Ajax 获取数据访问的 URL 地址 `https://movie.douban.com/j/new_search_subjects?sort=U&range=0,10&tags=&start=0 ` , '?' 号后面的参数，在表单里也可以看见

   - 如图解释

     1. 浏览器的地址栏显示的 URL

        ![浏览器地址栏显示](git_picture/浏览器地址栏显示.png)

     2. 抓包工具显示实际访问 URL 的地址

        ![实际访问地址](git_picture/实际访问地址.png)

     3. Ajax 加载数据，实际发送的 URL 地址

        ![Ajax发送请求地址](git_picture/Ajax发送请求地址.png)

     4. Ajax 请求表单的参数

        ![Ajax表单属性值](git_picture/Ajax表单属性值.png)

     5. Ajax 请求的响应文件

        ![Ajax请求响应文件](git_picture/Ajax请求响应文件.png)

3. 代码演示

   说明：直接访问 Ajax 数据获取的 URL 地址，获取 JSON 的数据

   ```python
   import urllib.request
   import urllib.parse
   
   
   url = 'https://movie.douban.com/j/new_search_subjects?sort=U&range=0,10'
   
   headers = {
       'User-Agent':
       'Mozilla/5.0 (Macintosh; Intel Mac OS X 10.6; rv2.0.1) Gecko/20100101 Firefox/4.0.1'
   }
   
   # 可以使用 GET 方式，将 tags=20 直接放在 URL 中
   # urllib.request.Request() 形参 data 有实参时，就是 POST 请求
   form_data = {
       'tags': '20'
   }
   
   form_data = urllib.parse.urlencode(form_data).encode('utf-8')
   print(form_data)
   
   request = urllib.request.Request(url=url, data=form_data, headers=headers)
   response = urllib.request.urlopen(request)
   
   # print(response.read().decode())
   
   # 将请求的响应数据（JSON格式），保存下来
   with open('ajax抓取数据.json', 'wb') as f:
       f.write(response.read())
   
   ```

4. 为什么 POST 方式有时也可以在 URL 中看到数据

   说明：[参考地址](https://www.cnblogs.com/qmfsun/p/3679246.html)

   > GET 和 POSt 的区别
   >
   > - get 是把参数数据队列加到提交表单的ACTION属性所指的URL中，值和表单内各个字段一一对应，在 URL 中可以看到。
   >
   > - post是通过 HTTP post 机制，将表单内各个字段与其内容放置在 HTTP 请求 的 HEADER 内一起传送到ACTION属性所指的 URL 地址。用户看不到这个过程。
   >
   > - 对于 get 方式，服务器端用 Request.QueryString 获取变量的值，对于post方式，服务器端用 Request.Form 获取提交的数据。两种方式的参数都可以用Request来获得。
   >
   > - get 传送的数据量较小，不能大于 2KB。post传送的数据量较大，一般被默认为不受限制。但理论上，因服务器的不同而异。
   >
   > - ```html
   >   
   >   ```
   >
   > ```
   > 
   > ```
   >
   > ```
   > <form method="get" action="a.asp?b=b">
   > <form method="get" action="a.asp">
   > 
   > get 的两种请求是一样的
   > ```
   >
   > - ```html
   >   
   >   ```
   >
   > ```
   > <form method="post" action="a.asp?b=b">
   > <form method="post" action="a.asp">
   > 
   > post 的两种请求是不一样的
   > ```
   >
   > GET 和 POST 特性
   >
   > - 它会将数据添加到URL中，通过这种方式传递到服务器，通常利用一个问号？代表URL地址的结尾与数据参数的开端，后面的参数每一个数据参数以“名称=值”的形式出现，参数与参数之间利用一个连接符&来区分。
   > - 数据是放在HTTP主体中的，其组织方式不只一种，有&连接方式，也有分割符方式，可隐藏参数，传递大批数据，比较方便。
   > ```
   > 
   > ```

### 使用 Cookie 

说明：Cookie 是 存储在客户端的验证信息，当访问需要登陆的网站时，网站使用 Cookie 的值和Session 来判断你是否已经登录成功，若成功登录可以访问网站其他页面数据。

- 使用账号登录_人人网_ ,保存 Cookie 的值，将 Cookie 的添加到 Headers 中，进行访问。（Cookie的存活时间有限）

- 代码演示

  ```python
  # 使用浏览器保存的已经登录状态的cookie来抓取已登录的页面
  # 和服务器的session来进行比对，相同就可以访问
  
  import urllib.request
  import urllib.parse
  
  
  url = 'http://www.renren.com/323263508/profile'
  
  # 使用有意义的 Cookie 的值，进行访问
  headers = {
      'Host': 'www.renren.com',
      'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.100 Safari/537.36',
      'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3',
      'Referer': 'http://www.renren.com/SysHome.do',
      'Accept-Language': 'zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7',
      'Cookie': 'anonymid=jxb5eg9slyjn9p; depovince=GW; _r01_=1; ick_login=3223ceca-c44a-49e2-98da-eedffb0ed05d; JSESSIONID=abc3dXZtEbdjfa__2tmUw; ick=53c10faf-5ed9-46a1-afd6-6c75631af1fe; first_login_flag=1; ln_uact=15702423221; ln_hurl=http://head.xiaonei.com/photos/0/0/men_main.gif; jebe_key=badec72b-eb2e-46ec-8ac5-6e78cda7ff76%7Cb364afd86a8f4df99041ab657d92afdd%7C1561427290522%7C1%7C1561427288904; wp_fold=0; wp=0; jebecookies=91ff85a5-5a1d-41ac-825c-dc2bea4165e3|||||; _de=B775DCA76366A1E9BDB23B0E912B5E67; p=53437d7e31b2d3fc6411b6b4e6b257517; t=c8252604d054480c1f17c03d444b3e937; societyguester=c8252604d054480c1f17c03d444b3e937; id=971287447; xnsid=73ab956c; loginfrom=syshome'
  }
  
  request = urllib.request.Request(url=url, headers=headers)
  
  response = urllib.request.urlopen(request)
  
  print(response.read().decode())
  
  ```

## Handler 处理器和自定义 Opener

说明：如果需要在请求中添加_代理_、处理请求的 _Cookie_ ，基本的 `urlopen()` 方法是不支持。想要使用 HTTP/HTTPS 的高级功能，就需要创建特定的 Hander 处理器，用来自定义 opener 对象，最后调用 `open()` 函数打开 URL 地址。

### Handler处理器理解

说明：[参考地址](https://www.jianshu.com/p/2e190438bd9c)

1. Handler 处理器可以处理（HTTP、HTTPS、FTP等）各种请求的各种不同打开 URL 地址的方式（有点绕嘴，哈哈哈）。Handler处理器的父类是 `urllib.request.BaseHander` ，常见的Handler处理器（继承BaseHandler）

   | 序号 | 处理器类名                      | 作用                        | 父类            |
   | ---- | ------------------------------- | --------------------------- | --------------- |
   | 1    | HTTPHandler                     | 处理 HTTP 请求              | BaseHandler     |
   | 2    | ProxyHandler                    | 为请求设置代理              | BaseHandler     |
   | 3    | HTTPCookieProcessor             | 处理 HTTP 请求的 Cookie     | BaseHandler     |
   | 4    | HTTPPasswordMgr                 | 用于管理用户名、密码        | BaseHandler     |
   | 5    | HTTPPasswordMgrWithDefaultRealm | 用于管理用户名、密码        | HTTPPasswordMgr |
   | 6    | HTTPBasicAuthHandler            | 用于登录认证，一般和 5 连用 | BaseHandler     |

### opener理解

1. opener 是 OpenerDirector 一个实类，之前使用的 `urlopen()` 实际上是一个特例（urllib 模块提供的一个 opener）
2. opener 和 Handler 关系，opener 是由 `build_opener(Handler)` 初始化的，如果想要全局的自定义opener，使用语句 `install_opener(opener)` ,就可以像之前使用 `urlopen()` 一样。

### 构造不同的处理器（普通HTTP请求、代理、Cookie、认证）

1. HTTPHandler（构建处理 HTTP 请求的处理器）

   说明：这种方式发送请求、获取的响应文件，和 `urllib.request.urlopen()` 发送 HTTP/HTTPS 请求的响应文件一样。如果 HTTHandler() 增加了参数 debuglevel=1 会将 Debug Log 打开，执行程序时，自定打印收发包报头信息，方便调试。

   - 代码

     ```python
     import urllib.request
     
     
     # 构建一个HTTPHander处理器对象，支持处理HTTP请求
     # debuglevel=1 程序执行时打印收发包的调试信息
     http_hander = urllib.request.HTTPHandler(debuglevel=1)
     
     # 调用build_opener()方法构建 一个自定义的opener对象，参数是构建的处理器对象
     opener = urllib.request.build_opener(http_hander)
     
     # 构造全局的 opener （1）
     urllib.request.install_opener(opener)
     
     # 初始化一个 HTTP 请求 对象
     request = urllib.request.Request('http://www.baidu.com/')
     
     # 没有构造全局的 opener 的使用 （2）
     # response = opener.open(request)
     
     # 使用自定义的全局 opener （1）
     response = urllib.request.urlope(request)
     
     # print(response.read().decode())
     
     
     运行结果（打印调试信息）
     send: b'GET / HTTP/1.1\r\nAccept-Encoding: identity\r\nHost: www.baidu.com\r\nUser-Agent: Python-urllib/3.6\r\nConnection: close\r\n\r\n'
     reply: 'HTTP/1.1 200 OK\r\n'
     header: Bdpagetype header: Bdqid header: Cache-Control header: Content-Type header: Cxy_all header: Date header: Expires header: P3p header: Server header: Set-Cookie header: Set-Cookie header: Set-Cookie header: Set-Cookie header: Set-Cookie header: Set-Cookie header: Set-Cookie header: Vary header: X-Ua-Compatible header: Connection header: Transfer-Encoding 
     ```

2. ProxyHandler（构造__代理__处理器）

   说明：使用代理，是实现反爬虫的有效手段之一，通常也是最好的用的。

   大部分网站会检测某一段时间的各个 IP 访问次数（通过流量统计、系统日志等）如果访问次数太不象正常水平，服务器会禁止此 IP 的访问。

   所以代理服务器就起到了作用，只要爬虫程序，每隔一段时间自动更换一个 IP，这样就算一个 IP 被禁止访问，我们可以换一个 IP 继续访问。

   - 理解代理服务器原理

     说明：[参考地址](https://blog.csdn.net/bzhxuexi/article/details/16860175)

     1. 代理服务器（Proxy Server）是提供转接功能的服务器。一般情况下，当访问 Internet 一个站点信息时，浏览是直接链接到目的站点服务器，然后由目的站点服务器把信息传送回来。代理服务器是介于客户端和Web服务器之间的另一台服务器，有了它之后，浏览器不是直接链接到 Web 服务器去取回网页数据，而是先将请求消息发送到代理服务器，由代理服务器链接 Web 服务器取回浏览器所需要的信息并传送给客户端浏览器。
     2. 比如浏览器访问的目的网站是 A，由于某种原因不能访问到网站 A 或者就是不想直接访问网站 A，此时你就可以使用代理服务器，在实际访问网站的时候，你在浏览器的地址栏内和你以前一样输入你要访问的网站，浏览器会自动先访问代理服务器，然后代理服务器会自动给你转接到你的目标网站
     3. 作用
        - 提高访问速度：通常代理服务器都设置一个较大的缓冲区，当有外界的信息通过时，同时也将其保存到缓冲区中，当其他用户再访问相同的信息时，则直接由缓冲区中取出信息，传给用户，以提高访问速度。
        - 隐藏真实身份：用户也可以通过代理服务器隐藏自己的真实地址信息，还可隐藏自己的IP，防止被黑客攻击。
        - 突破限制：有时候网络供应商会对上网用户的端口，目的网站，协议，游戏，即时通讯软件等的限制，使用代理服务器都可以突破这些限制

   - 开放代理（没有用户名、密码）和私密代理（本人没有私密代理）

     ```python
     import urllib.request
     
     
     # 代理开关，是否选择开启代理
     proxyswitch = True
     
     # 构建一个Handler处理器，参数是一个字典，包括代理类型和代理IP+Port
     http_handler = urllib.request.ProxyHandler({'http': '61.128.208.94:3128'})
     # 使用私密代理
     # http_handler = urllib.request.ProxyHandler({'http': 'username:keyword@61.128.208.94:3128'})
     
     # 构建一个没有代理的处理器，但是也得有参数（一个空的字典）
     #null_handler = urllib.request.ProxyHandler({})
     
     # 选择是否开启代理
     if proxyswitch:
         opener = urllib.request.build_opener(http_handler)
     else:
         opener = urllib.request.build_opener(null_handler)
     
     # 构建一个全局的opener，之后所有的请求都可以使用urlopen()方法发送请求，
     # 也附带Handler功能
     # 没有返回值
     urllib.request.install_opener(opener)
     
     # 构建一个请求
     request = urllib.request.Request('http://www.baidu.com/')
     
     # 发送请求
     response = urllib.request.urlopen(request)
     
     # 接受是UTF-8的编码
     # decode()进行解码
     response = response.read().decode()
     print(response)
     
     运行结果
     <!DOCTYPE html> 
     <!--STATUS OK-->
     空行...
     内容数据
     ```

   - 常见代理网站

     [西刺没费代理 IP](https://www.xicidaili.com/)

     [快代理免费代理](https://www.kuaidaili.com/free/inha/)

3. HTTPPasswordMgrWithDefaultRealm() （密码管理对象）

   说明：`HTTPPasswordMgrWithDefaultRealm()` 类将创建一个密码管理对象，用来保存 HTTP 请求相关的用户名、密码

   - 验证代理授权的用户名和密码 `ProxyBasicAuthHandler()`
   - 验证 Web 客户端的用户名和密码 `HTTPBasucAuthHandler()`

4. ProxyBasicAuthHandler（代理授权验证）

   说明：使用私密代理，需要进行私密代理授权验证，如果没有进行授权验证会报 HTTP 407 错误，表示代理没有验证，`urllib.request.HTTPError: HTTP Error 407: Proxy Authentication Required`。

   - 使用 `passwdMgr = urllib.request.HTTPPasswordMgrWithDefaultRealm()`  创建密码管理对象，用来保存私密代理用户名、密码

   - 使用 `passwdMgr.add_password(None, proxy_server, user, passwd)` 将用户名和密码添加到密码管理对象中

   - 使用 `proxy_handler = urllib.request.ProxyBasicAuthHandler(passwdMgr)`  来处理代理身份验证

   - 还可以使用 `http_handler = urllib.request.ProxyHandler({'http': 'username:keyword@61.128.208.94:3128'})` 来进行代理授权，也可以依靠此方法，将用户名、密码写入环境变量中，使用 `import os` 模块，`os.environ.get('name')` 来取出变量值。__(此方法在处理私密代理比较常用)__

     ```python
     >>> os.environ.get('JAVA_HOME')
     'H:\\Java\\jdk1.8.0_121'
     ```

   - 代码

     ```python
     import urllib.request
     
     
     # 私密代理授权账户（假）
     user = "user"
     # 私密代理授权密码（假）
     passwd = "password"
     # 私密代理 IP + Port
     proxyserver = "182.92.188.108:16817"
     
     # 构建一个密码管理对象，用来保存 HTTP 请求相关的用户名和密码
     passwdmgr = urllib.request.HTTPPasswordMgrWithDefaultRealm()
     
     # 添加账户信息，第一个参数realm是与远程服务器相关的域信息，一般写None，后三个参数分别是,代理服务器、用户名、密码
     passwdmgr.add_password(realm=None, uri=proxyserver, user=user, password=passwd)
     
     # 构建一个代理基础用户名/密码验证的ProxyBasicAuthHandler处理器对象，参数是创建的密码管理对象
     # 注意，这里不再使用普通ProxyHandler类了
     proxyauth_handler = urllib.request.ProxyBasicAuthHandler(passwdmgr)
     
     # 通过 build_opener() 方法使用这些代理Handler对象，创建自定义 opener 对象，参数包括构建的 proxy_handler 和 proxyauth_handler
     opener = urllib.request.build_opener(proxyauth_handler)
     
     # 构造 Request 请求
     headers = {
         'Accept': 'application/json, text/javascript, */*; q=0.01',
         'X-Requested-With': 'XMLHttpRequest',
         'User_Agent': "Mozilla/5.0 (Windows NT 6.1; rv2.0.1) Gecko/20100101 Firefox/4.0.1",
         'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',
         'Accept-Language': 'zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7',
     }
     url= 'http://www.baidu.com'
     request = urllib.request.Request(url=url, headers=headers)
     
     # 使用自定义opener发送请求
     response = opener.open(request)
     
     # 打印响应内容
     print(response.read().decode())
     ```

   - __此处有很大的问题，就是代理试不试用都可以正常访问，不晓得是代码的问题，还是网站的问题，问题后续继续解决......__

5. HTTPBasicAuthHandler（Web服务器基础认证）

   说明：访问一些 Web 服务器时需要进行身份验证，爬虫的程序直接访问会报 HTTP 401 错误，表示访问身份未经授权 `urllib.request.HTTPError: HTTP Error 401: Unauthorized` 。

   - 使用 `passwdMgr = urllib.request.HTTPPasswordMgrWithDefaultRealm()` 管理 HTTP 请求相关的用户名和密码

   - 使用 `passwdMgr.add_password(None, proxy_server, user, passwd)` 将用户名和密码添加到密码管理对象中

   - 使用 `basic_auth_handler = urllib.request.HTTPBasicAuthHandle(passwdMgr)` 创建 HTTP 基础认证处理器

   - 代码

     ```python
     import urllib.request
     
     
     user_name = 'user'
     password = '123456'
     
     webserver = '192.168.13.41'
     
     # 构建一个密码管理对象，可以用来保存和HTTP请求相关的授权账户信息
     passwordMgr = urllib.request.HTTPPasswordMgrWithDefaultRealm()
     
     # 添加授权账户信息，第一个参数是realm如果没有指定就写None，
     # 后三个分别是,站点IP，账户，密码
     passwordMgr.add_password(None, webserver, user_name, password)
     
     # 本机IP处理器
     # web客户端验证处理器
     httpauth_handler = urllib.request.HTTPBasicAuthHandler(passwordMgr)
     
     opener = urllib.request.build_opener(httpauth_handler)
     
     request = urllib.request.Request('192.168.13.41')
     reponse = opener.open(request)
     
     print(reponse.read())
     
     ```

6. HTTPCookieProcess（使用cookie访问登录后才能访问的页面）

   - 导入 `from http import cookiejar`

   - 使用 `cookie = cookiejar.CookieJar()` 创建 CookieJar 对象，用来存储 cookie信息

   - 使用 `cookie_handle = urllib.request.HTTPCookieProcessor(cookie)` 构建一个 cookie 处理器

   - 代码

     ```python
     import urllib.request
     from http import cookiejar
     import urllib.parse
     
     
     # 通过 CookieJar() 方法构建一个 cookieJar 对象，用来储存 cookie
     cookie = cookiejar.CookieJar()
     
     # 通过 HTTPCookieProcessor() 构建一个处理器对象，用来处理cookie
     # 参数就是构建的CookieJar()对象
     cookie_handle = urllib.request.HTTPCookieProcessor(cookie)
     
     # 构建 cookie 处理器
     opener = urllib.request.build_opener(cookie_handle)
     
     # 使用 addheaders 属性赋值请求报头
     # addheaders 是 opener 的属性，也可以继续使用 request.add_header() 方法
     opener.addheaders = [(
         'User-Agent',
         'Mozilla/5.0 (Macintosh; Intel Mac OS X 10.6; rv2.0.1) Gecko/20100101 Firefox/4.0.1'
     )]
     
     # 用户名、密码，属性通过 html 页面数据查看（标签的 name 值）
     data = {'email':'XXXX', 'password': 'XXXX'}
     
     data = urllib.parse.urlencode(data).encode()
     
     url = 'http://www.renren.com/SysHome.do'
     
     request = urllib.request.Request(url=url, data=data)
     
     # header已经添加进去了
     # 发送第一次POST请求，生成登录后的cookie（如果登录成功）
     response = opener.open(request)
     
     # print(response.read().decode())
     
     # 第二次就可以get请求，这个请求将保存生成cookie一并发到web服务器上，服务器验证Cookie和Session
     response_du = opener.open('http://www.renren.com/880792860/profile')
     # 获取登录才能获取的网页信息
     print(response_du.read().decode())
     
     运行结果
     <!Doctype html>
     <html class="nx-main860">
     <head>
         <meta name="Description" content="人人网 校内是一个真实的社交网络，联络你和你周围的朋友。 加入人人网校内你可以:联络朋友，了解他
     们的最新动态；和朋友分享相片、音乐和电影；找到老同学，结识新朋友；用照片和日志记录生活,展示自我。"/>
         <meta name="Keywords" content="Xiaonei,Renren,校内,大学,同学,同事,白领,个人主页,博客,相册,群组,社区,交友,聊天,音乐,视频,校园,人
     人,人人网"/>
         <title>人人网 - 包贝尔</title>
     ```

### HTTP几种认证方式

说明：用户、客户端、资源服务器、认证服务器的作用和之间的关系

1. 理解 HTTP 协议的“无连接”、“无状态”特点

   - 无连接（Keep-Alive）

     无连接的含义是限制每次链接只处理一个请求，服务器处理完客户端请求，并接收到客户端的应答，即断开连接，采用这种方式可以节约通道资源，早期的 HTTP 协议就是如此。

     随着技术的发展，网页的复杂度也越来越高（内嵌了许多照片、CSS、JS），这时每次建立 TCP 链接只能做一次请求，要完成一个页面的下载，就需要多次  TCP 链接，多次请求，这显然是浪费时间。

     Keep-Alive 应运而生，它可以使客户端，服务器保持长时间有效链接，避免了访问一个页面，需要建立多次 TCP 链接。

     Keep-Alive 对资源的占用，也影响了服务器的性能，因为保持一段时间的 TCP 链接不中断，本来应该释放的资源没有正常释放。

   - 无状态

     无状态的含义是协议对请求处理没有记忆功能，服务器不知道客户端是什么状态，即我们向服务器发送 HTTP 请求，服务器根据请求消息，发送响应文件，发送完成之后不会记录任何信息。

     HTTP 是无状态协议，这意味着每次请求都是独立的，Keep-Alive 的设置只是保持了链接，没有改变无状态的性质。

     HTTP 无状态性质，使得稍后的请求必须携带前期的请求信息，这样导致了增加了每次请求的数据量。

   - 本人理解

     HTTP 协议设计是有时代的局限性，但可以看出在设计 HTTP 协议的时候，对访问者信息保护到了牺牲服务器性能的地步，感觉是对用户的一种无上的尊重。

2. HTTP 基本认证（Basic Authentication）

   说明：HTTP 基本认证，是建立在客户端和服务器链接安全的情况之下，一般公共的网站不会使用此认证。

   - 基本认证过程
     1. 客户端访问一个受 HTTP 基本认证保护的资源
     2. 请求：客户端发送请求，第一条请求没有认证信息
     3. 质询：服务器对客户端进行质询，返回一条 `401 Unauthentication` 响应的状态码。并在响应 headers 中返回 `WWWW-Authentication:Basic realm='XXXXXXX'` 说明如何以及在哪里认证，一般指定那个安全域进行认证。
     4. 授权：客户端收到 401 状态码质询，弹出对话框，质询用户名、密码，输入用户名、密码，浏览器将用户名、密码进行 Base64 编码，设置请求头 `Authentication:Basic XXXXX(用户名:密码)`,继续访问。
     5. 成功：服务器对用户名和密码进行解码，验证是否正确，正确返回 `200 ok` 状态码，返回响应文件。
     6. __基本认证方式，是一种无状态的认证，就是不需要服务器保存 Session 信息，，每一次客户端访问都需要携带用户名、密码，以便服务器进行验证。用户名、密码，保存在浏览器的内存中，关闭浏览器时，基本认证的用户名、密码被删除，表示认证结束，下一次访问，重新输入用户名，密码。__
     7. 安全缺陷：base-64 安全性不高，容易解密，所以一般 HTTP 基本认证都是建立在客户端和服务器建立私密链接中。

3. OAuth 2.0 认证

   说明：OAuth 协议为用户资源的授权提供了一个安全的、开放而又简易的标准。与以往的授权方式不同之处是 OAuth 的授权不会使第三方触及到用户的帐号信息（如用户名与密码），即第三方无需使用用户的用户名与密码就可以申请获得该用户资源的授权，因此 `OAuth`是安全的。OAuth 是  Open Authorization 的简写。

   - OAuth 比较常见的是__微信登录、微博登录、qq登录等__，简单来说就是利用比较权威的网站和应用比较开放的 API 来实现用户登录，即用户可以不用再网站上注册账号，直接使用微信、微博、qq等账号登录。这样的好处是，减除注册的麻烦、又简化系统账号的体系、安全性有所提高。
   - 认证过程
     1. 用户访问一个没有登录的客户端（使用微信登录）
     2. 客户端引导用户到微信授权页面，请求授权，即 Authorization Request 
     3. 用户同意授权（各种样式的授权）即会在微信服务器获取一次性用户授权凭证如 code，给客户端
     4. 客户端用第 3 步获取的 code 和自身的身份凭证（AppID）向微信授权服务器发送请求，获取 **access_token**，即令牌。
     5. 客户端使用令牌（accsee_token）,向微信服务器请求用户基本信息（登录成功）。
     6. 客户端使用令牌向资源服务器请求资源
     7. 资源服务器使用令牌向微信服务器确认令牌正确性，确认无误，提供资源

4. Cookie-Seseion 认证

   ......

5. Cookie-Session 升级版认证

   ......

6. 基于 JWT 的 Token 认证

   ......

### urllib.request 异常处理

待续......

## 非结构化的数据处理

























 