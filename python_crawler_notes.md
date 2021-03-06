# Python爬虫

说明：__年轻的时候满脑子想的都是女孩、爱情、女人。现在我只想钱，在这个物质横流的世界里如果没有钱，你就是个垃圾。__

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
3. 如果网页有其他URL地址，继续执行第二步（通过HTTP、HTTPS获取数据）

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

__请求行__、__请求头__、__空行__、__请求数据__

1. HTTP请求格式如图

   ![HTTP请求格式](git_picture/HTTP请求格式.png)

2. HTTP请求示例（没有请求数据）

   - GET 请求

     ```txt
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

   - POST 请求（cookie假）

     ```txt
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

4. Upgrade-Insecure—Request（升极为HTTPS协议）

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
   
7. Referer（页面跳转）

   说明：Referer：表明产生页面的请求来自哪一个 URL，用户是从该 Referer 页面访问当前的页面。这个属性可以用来跟踪 Web 请求来自哪个页面，是从什么网站上来的。

   `Referer: https://www.baidu.com/`

   - 有时，遇到下载网站图片时，需要对应的 Referer ，否则无法下载。原理是根据 Referer 来判断下载图片的请求是否来自本网站的页面，如果是可以下载，否则拒绝。

8. Accept-Encoding(文件解压缩格式)

   说明：Accept-Encoding：指出浏览器可以接受的解压缩格式。解压缩方式不同于文件格式，它是为了压缩文件以加速文件传输速率的，浏览器在接受到服务器响应文件先进性解码，然后再检查文件格式，可以减少大量时间。

   `Accept-Encoding: gzip, deflate, br`

   - 如果多个 Encoding 同时匹配，按照 q 值大小排序，从左到右排序，如果没有标明文件编码格式则可以接受任何编码。

9. Accept-language（语言种类）

   说明：`Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7`

   - 浏览器可以接受的语言种类，如en或en-us指英语，zh或者zh-cn指中文，当服务器能够提供一种以上的语言版本时要用到，也会使用 q 权重系数来排序。

10. Accept-charset（字符编码）

   说明：指浏览器可以接受的字符编码格式

   `Accept-Charset:iso-8859-1,gb2312,utf-8`

   - 一般采用国际标准字符编码 Unicode 的 utf-8（可变长度的字符编码）
   - 如果此属性缺省，表示可以接受任何编码格式

11. Cookie（用户登录状态编码）

    说明：解决HTTP请求独立问题

    `Cookie: BAIDUID=EFFFF2453D442360DFBBAE2F16F8B7C6:FG=1; BIDUPSID=EFFFF2453D442360DFBBAE2F16F8B7C6; PSTM=1560908391;`

    - > HTTP请求是无状态的。也就是说即使第一次和服务器连接后并且登录成功后，第二次请求服务器依然不能知道当前请求是哪个用户。cookie的出现就是为了解决这个问题，第一次登录后服务器返回一些数据（cookie）给浏览器，然后浏览器保存在本地，当该用户发送第二次请求的时候，就会自动的把上次请求存储的cookie数据自动的携带给服务器，服务器通过浏览器携带的数据就能判断当前用户是哪个了。cookie存储的数据量有限，不同的浏览器有不同的存储大小，但一般不超过4KB。因此使用cookie只能存储一些小量的数据.
      >
      > [参考地址](https://www.cnblogs.com/xxtalhr/p/9053906.html)
      >
      > [参考地址](http://bubkoo.com/2014/04/21/http-cookies-explained/)
    
12. Cookie 和 Session

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

     就 Ajax 而言是一种不需要刷整个新页面就可以更新部分页面数据，但是，只要刷新了页面（不管时是全部页面还是部分页面），那一定获取了新的数据，有新的数据就一定会有新的且唯一 URL 地址来标识。但是本人不理解的浏览器地址栏的信息有的会发生变化，有的不会变化，但 2 者页面数据都有部分跟新，而整张页面没有刷新，实际你使用抓包工具时，你会发现，实际当你每次点击加载更多的时候，都有一个新的请求发送出去（对应新的 URL 地址），对应新的响应文件。感觉，Ajax 的技术，是加载格式一样的数据使用频发，只要网页使用 Ajax 技术的，绝大部是 JSON 格式。

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
   > - post是通过 HTTP post 机制，将表单内各个字段与其内容放置在 HTTP 请求 的 HEADER 内一起传送到ACTION属性所指的 URL 地址。用户看不到这个过程。
   > - 对于 get 方式，服务器端用 Request.QueryString 获取变量的值，对于post方式，服务器端用 Request.Form 获取提交的数据。两种方式的参数都可以用Request来获得。
   > - get 传送的数据量较小，不能大于 2KB。post传送的数据量较大，一般被默认为不受限制。但理论上，因服务器的不同而异。
   > - `<form method='get' action='a.asp?b=b'>` 和 `<form method='get' action='a.asp'>` 一样
   > - `<form method='post' action='a.asp?b=b'>` 和 `<form method='post' action='a.asp?b=b'>` 不一样
   >
   > GET 和 POST 特性
   >
   > - GET 特性：它会将数据添加到 URL 中，通过这种方式传递给服务器，通常使用 `?` 代表 URL 地址的结尾和数据的开端，后面的参数每一个数据的形式都是 `name = val` 参数与参数以 `&` 区分。
   > - POST 特性：数据是存放在 HTTP请求体中，数据组织方式不止一种，有 `&` 连接、分隔符、也可以隐藏参数。传递大批数据，比较方便。

### 使用 Cookie 

说明：Cookie 是存储在客户端的验证信息，当访问需要登陆的网站时，网站使用 Cookie 的值和 Session 来判断你是否已经登录成功，若成功登录可以访问网站其他页面数据。

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

   - __此处有很大的问题，就是代理使不使用都可以正常访问，不晓得是代码的问题，还是网站的问题，问题后续继续解决......__

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

## Requests:让 HTTP 服务人类

说明：[官方文档](https://2.python-requests.org//zh_CN/latest/index.html)

### 介绍 Requests 库

1. 对比之前的 `urllib.request` 和 `urllib.parse` 
   - 不需要手动为 URL 添加查询字符串
   - 不需要对 POST 数据进行表单编码
   - Keep-alive 和 HTTP 链接池功能时 100% 自动化
   - 一切动力都来自于 Resquests 内部的 urllib3
   - 更符合对 Python 动能的定义
2. 要跟好的了解，最好对官方文档进行学习

### 如何使用 Requests

说明：链接网络原理上面内容已经进行了讲述，固在 Requests 中，只讲解如何使用

1. GET 请求

   - 最基本的 GET 请求

     ```python
     import requests
     
     response = requests.get('http://www.baidu.com')
     
     # 也可以这样写
     # response = requests.request('get', 'http://www.baidu.com')
     
     print(type(response))
     print(response)
     
     运行结果
     <class 'requests.models.Response'>
     <Response [200]>
     ```
     
   - 带参数的 GET 请求

     ```python
     import requests
     
     # 查询参数
     payload = {'wd': '中国'}
     url = 'http://www.baidu.com/s'
     
     # 不用手动编码
     response = requests.get(url=url, params=payload)
     
     # 将响应文件内容使用 二进制 表示
     b_response = response.content
     
     # 为方便查看将二进制进行解码
     print(b_response.decode())
     
     运行结果
     <!DOCTYPE html> 
     <!--STATUS OK-->
     网页内容
     ```

2. 响应内容

   - ```python
     import requests
     
     response = requests.get('http://www.baidu.com')
     ```

   - `response` 是响应文件

     1. `response.text` 自动解码成字符串

        说明：解析出来的是__字符串__

        请求发出后，Requests 会基于 HTTP 头部对响应的编码作出有根据的推测。当使用 `response.text` 时，Requests 会使用其推测的文本编码。你可以找出 Requests 使用了什么编码，并且能够使用`response.encoding` 属性来改变它。

        ```python
        import requests
        
        response = requests.get('http://www.baidu.com')
        
        # 改变编码方式
        response.encoding = 'utf-8'
        print(response.text)
        
        运行结果
        <!DOCTYPE html>
        网页内容
        ```

     2. response.content 二进制响应内容

        说明：响应文件为__二进制__，也可以对其进行解码成字符串

        ```python
        import requests
        
        response = requests.get('http://www.baidu.com')
        print(response.content)
        # 对二进制文件进行解码
        # print(response.content.decode())
        
        运行结果
        b'<!DOCTYPE html>\r\n<!--STATUS OK-->
        网页内容
        ```

     3. JSON 响应文件

        `response.json()` 之间解析 json 响应文件

3. 响应状态

   - 响应状态码

     ```python
     >>> import requests
     >>> r = requests.get('http://www.baidu.com')
     # 查看状态码
     >>> r.status_code
     200
     # 验证状态码是否为 ok
     >>> r.status_code == requests.codes.ok
     True
     ```

   - 响应头

     ```python
     # 查看响应头信息
     >>> r.headers
     {'Cache-Control': 'private, no-cache, no-store, proxy-revalidate, no-transform', 'Connection': 'Keep-Alive', 'Content-Encoding': 'gzip', 'Content-Type': 'text/html', 'Date': 'Thu, 15 Aug 2019 13:54:16 GMT', 'Last-Modified': 'Mon, 23 Jan 2017 13:27:57 GMT', 'Pragma': 'no-cache', 'Server': 'bfe/1.0.8.18', 'Set-Cookie': 'BDORZ=27315; max-age=86400; domain=.baidu.com; path=/', 'Transfer-Encoding': 'chunked'}
     
     # 获取特定头部信息
     >>> r.headers['Content-Type']
     'text/html'
     ```

4. 定制请求头

   - 简单添加请求头

     ```python
     import requests
     
     url = 'http://www.baidu.com'
     headers = {'User-Agent': 'my-app'}
     response = requests.get(url=url, headers=headers)
     
     print(type(response))
     print(response)
     
     运行结果
     <class 'requests.models.Response'>
     <Response [200]>
     ```

5. POST 请求

   - 基本 POST 请求

     ```python
     import requests
     
     payload = {'key1': 'value1', 'key2':'value2'}
     
     response = requests.post('http://httpbin.org/post', data=payload)
     
     print(response.text)
     
     运行结果（筛选出有用信息）
     "form": {
         "key1": "value1",
         "key2": "value2"
       }
     ```

   - 第二种 POST 基本用法

     ```python
     import requests
     
     payload = (('key', 'value1'), ('key', 'value2'))
     
     response = requests.post('http://httpbin.org/post', data=payload)
     
     print(response.text)
     
     运行结果（筛选有用信息）
       "form": {
         "key": [
           "value1",
           "value2"
         ]
       }
     ```

6. 代理（proxies）

   说明：如需使用代理，可以通过任意请求方式提供 `proxies` 参数配置代理信息

   __代理是不能使用的（假的）__
   
   - 公共代理使用
   
     ```python
     import requests
     
     
     # 现在知识来看
     #   http 代理 http 协议网站
     #   https 代理 https 协议网站
     #   但是好像已经没有这个限制了
     # 
     # 最主要的是，建立连接的方式不同
     proxies = {
         'http': 'http://12.34.56.79:9523',
         'https': 'http://10.10.1.10:1080'
     }
     
     response = requests.get('http://www.baidu.com', proxies=proxies)
     
     print(response.status_code)
     ```
   
   - 私密代理使用（代理使用 HTTP Basic Auth 基础认证）
   
     ```python
     # 代理格式
     proxies = {
         'http':'http://uesr:password@10.10.1.10:3128'
     }
     ```
   
   - web 客户端验证
   
     ```python
     import requests
     
     
     # web 客户端验证 用户名/密码
     auth = ('test', '123456')
     
     response = requests.get('http://192.1.1.0', auth=auth)
     
     print(response.status_code)
     ```
   
7. requests 模块中 Session 模块

   - Session解释

     在 requests 中里，Session对象表示一次会话，一次会话表示从客户端浏览器链接服务器开始，到客户端浏览器与服务器断开为止。

     会话对象让你能够跨域请求保持某些参数，它也会在同一个 Session 实例发出的所有请求之间保持 cookie， 期间使用 `urllib3` 的 [connection pooling](http://urllib3.readthedocs.io/en/latest/reference/index.html#module-urllib3.connectionpool) 功能。所以如果你向同一主机发送多个请求，底层的 TCP 连接将会被重用，从而带来显著的性能提升。

   - 使用 Session 对象

     ```python
     import requests
     
     
     # 创建session对象，可以保存Cookie值
     session = requests.session() 
     
     headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.99 Safari/537.36"}
     
     # 需要登录的用户名和密码
     data = {"email":"15702423221", "password":"sHEN0077"}  
     
     # 发送附带用户名和密码的请求，并获取登录后的Cookie值，保存在session里
     session.post("http://www.renren.com/PLogin.do", data = data, headers=headers)
     
     # session包含用户登录后的Cookie值，可以直接访问登录后才可以访问的页面
     response = session.get("http://www.renren.com/448414213/profile")
     
     # 打印响应状态码
     print(response.status_code)
     
     # 打印响应内容
     # 自动解析成功
     print(response.text)
     
     运行结果
     200
     <!doctype html><html class="nx-main980" >
     ```

## 同源策略、跨域请求

说明：同源策略目的是为了安全考虑，牺牲了网页部署的灵活性。跨域请求目的是在安全和灵活中找到一个平衡点。

### 跨域请求

待续......

### 同源策略

待续......

## 数据处理

说明：对于抓取某个网站或某个应用的数据内容来说，提取有价值的数据，这就是爬虫的根本目的。爬取的数据内容一般分为两种，一种是__结构化数据__、另一种是__非结构化数据__，不同类型的数据，需要采取不同的处理方式。

- 非结构化数据：先有‘数据，再有结构
- 结构化数据：先有结构，再有数据

### 非结构化数据处理

1. 文本、电话号码、邮箱地址
   - 使用__正则表达式__
2. HTML 文件
   - 正则表达式
   - XPath
   - CSS 选择器

### 结构化数据

1. JSON 文件
   - JSON Path 
   - 转化为 Python 的 json 类进行操作
2. XML 文件
   - 正则表达式
   - CSS 选择器
   - XPath
   - 转化为 Python 的 xmltodict 进行操作

### 正则表达式

> 说明：正则表达式（Regular Expression 或 RE），又称正表示式、正则表示法、规则表达式、常规表示法，是计算机科学的一种概念。正则表达式使用单个字符串来描述、匹配一系列符合某个语法规则的字符串。在很多文本编辑器里，正则表达式通常被用来检索、替换那些符合某个模式的文本。
>
> [维基百科](https://zh.wikipedia.org/wiki/%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F)
>
> [正则表达式手册](https://tool.oschina.net/uploads/apidocs/jquery/regexp.html)
>
> [Python re 模块文档](https://docs.python.org/zh-cn/3.6/library/re.html)

1. 正则表达式原理

   - 如图

     ![正则原理](git_picture/正则原理.png)

2. 正则表达式匹配规则

   - 如图

     > ![正则匹配规则](git_picture/正则匹配规则.png)

3. Python 的 re模块（正则表达式模块）

   说明：Python 中使用内嵌的 re 模块

   - 注意

     1. 正则表达式的使用需要对特殊字符进行__转义__，所以需要使用__原始的字符__。Python 提供一个内置用于转义字符的形式 ”__在引号前面加 `r`  或 `R` 表示使用原始字符__“ 。

        ```python
        >>> s_1 = 'hello\nworld'
        >>> s_1
        'hello\nworld'
        >>> print(s_1)
        hello
        world
        >>> s_2 = r'hello\nworld'
        >>> s_2
        'hello\\nworld'
        >>> print(s_2)
        hello\nworld
        ```

        解释：本人理解，原生字符串的表现形式，就是__自动转义__。

     2. 原生字符串的产生正是由于有正则表达式的存在，原因是 ASCII 码和正则表达式特殊字符产生冲突。比如符号 `\b` 在 ASCII 中表示 ”退格键“，但在正则表达式中表示 “匹配一个单词边界，也就是指单词和空格间的位置”。Python 中为了让 正则表达式模块 re 正确理解 `\b` (匹配边界单词的用法)。就需要对反斜线进行转义，即 `\\b`。对字符串进行转义增加了问题的复杂度，所以原生字符串的出现就是简化正则表达式的复杂度。

        ```python
        >>> import re
        
        # 没有使用原生字符串
        >>> ret = re.match('love\b', 'love you')
        >>> ret
        >>> print(ret)
        None
        
        # 使用原生字符串
        >>> ret = re.match(r'love\b', 'love you')
        >>> ret
        <_sre.SRE_Match object; span=(0, 4), match='love'>
        >>> print(ret)
        <_sre.SRE_Match object; span=(0, 4), match='love'>
        ```

   - re 模块主要函数介绍及使用

     1. 主要函数介绍

        | 方法名                                        | 所属模块或对象 | 作用                                                   | 返回值类型                       |
        | --------------------------------------------- | -------------- | ------------------------------------------------------ | -------------------------------- |
        | re.compile(pattern) <br>(pattern 是 匹配规则) | re 模块        | 创建正则表达式对象                                     | Pattren 对象<br>(正则表达式对象) |
        | pattern.match(str, start, end)                | Pattern 对象   | 从起始位置开始匹配，返回第一个符合规则的，且只匹配一次 | Match 对象、None                 |
        | pattern.search()                              | Pattern 对象   | 从任何位置开始匹配，返回第一个符合规则的，且只匹配一次 | Match 对象、None                 |
        | pattern.findall()                             | Pattern 对象   | 所有的全部匹配，返回列表                               | 列表                             |
        | pattern.finditer()                            | Pattern 对象   | 所有的全部匹配，返回迭代器                             | 迭代器<br>(元素为 Match 对象)    |
        | pattren.sub()                                 | Pattern 对象   | 替换                                                   | 字符串                           |
        | pattern.split()                               | Pattern 对象   | 分割                                                   | 列表                             |
   
     2. 主要函数的使用
   
        - `re.compile(pattren, flages)`
   
          `pattern` 正则表达式字符串
     
          `flags` 编译标志（匹配模式）本人觉得主要的几个模式标记
     
          | flags=?       | 简写 | 作用                                                         |
          | ------------- | ---- | ------------------------------------------------------------ |
          | re.ASSIC      | re.A | 让 `\w, \W, \b, \B, \d, \D, \s, \S` 只匹配 ASSIC，而不是 Unicode。这只对 Unicode 样式有效，会被 byte 样式忽略。 |
          | re.IGNORECASE | re.I | 忽略大小写，让 `[A-Z]` 也可以匹配大写字母。                  |
          | re.MULTILINE  | re.M | 多行匹配，样式字符 `'^'` 匹配字符串的开始，和每一行的开始（换行符后面紧跟的符号）；样式字符 `'$'` 匹配字符串尾，和每一行的结尾（换行符前面那个符号）。默认情况下，`'^'` 匹配字符串头，`'$'` 匹配字符串尾。 |
          | re.DOTALL     | re.S | 让 `'.'` 特殊字符，可以匹配包括换行符所有字符。              |
          | re.VERBOSE    | re.X | 这个标记允许你编写更具可读性更友好的正则表达式。通过分段和添加注释。空白符号会被忽略。 |
     
          演示 `re.VERBOSE`
     
          ```python
          a = re.compile(r"""\d+  # the integral part
                              \.  # the decimal point
                              \d* # somefractional digits""", re.X)
          
          b = re.compile(r'\d+\.\d*')
          
          # a, b 等价
          ```
     
        - `pattern.match(str, start, end)`
     
          说明：如果 str 的开始位置或从开位置到多个字符匹配到了正则表达式样式，就返回一个相应的_匹配对象_，如果没有匹配到就返回一个 None。
     
          re.MULTILINE 多行模式,也只匹配字符串开始位置，而不匹配每行开始位置。
     
          __只匹配一次__。
     
          演示
     
          ```python
          import re
          
          
          # 初始化正则表达式匹配规则对象
          # 被 '()' 括起来的表示分组，该匹配规则分成 2 个组，每组使用空格分割 
          # re.I 表示忽略大小写
          pattern = re.compile(r'([a-z]+) ([a-z]+)', re.I)
          
          # 匹配字符串
          # 匹配结果 'Hello World'
          # 忽略大小写、分组使用空格分割
          # match 只从字符串开始位置匹配，且只匹配一次，所以 'Hello Python' 没有匹配的机会
          m = pattern.match('HelloA World HelloB Python')
          
          # 显示全部匹配成功的字符串
          print(m.group(0))
          
          print('+' * 30)
          # 显示出符合匹配规则第一个字符串
          print(m.group(1))
          
          # 显示第一个匹配成功字符串的位置信息
          print(m.span(2))
          
          运行结果
          HelloA World
          ++++++++++++++++++++++++++++++
          HelloA
          (7, 12)
          ```
     
        - `pattern.search(str, start, end)` 
     
          说明：扫描整个字符串找到匹配样式的第一个位置，并返回相应的匹配对象。如果没有匹配，就返回一个 None。
     
          __只匹配一次__。
     
          演示
     
          ```python
          import re
          
          
          # 初始化匹配对象
          # 进行分组匹配
          pattern = re.compile(r'(\d+)([a-z]+)(\d+)')
          
          # 匹配
          m = pattern.search('aaa123sfws11')
          
          # 打印匹配成功的字符串
          # 一共有 3 组
          print(m.group(0))
          
          # 打印匹配成功的第 1 组
          print(m.group(1))
          
          # 匹配所有的字符串的的起始位置、终止位置
          print(m.span(0))
          
          运行结果
          123sfws11
          123
          (3, 12)
          ```
     
        - `pattern.findall(str, start, end)`
     
          说明：从左到右扫描整个字符串，反复使用正则匹配样式，匹配的字符串按找顺序返回。如果样式里存在__大于一个组__，就返回一个组合列表（元组的列表）.。
     
          返回列表
     
          __可以匹配多次__。
     
          演示
     
          ```python
          import re 
          
          
          # 初始化匹配对象
          # 进行分组匹配
          pattern = re.compile(r'(\d+)([a-z]+)(\d+)')
          
          # 匹配
          m = pattern.findall('aaaa123sfws11aa123sfws11')
          
          # pattern = re.compile(r'(\d+)')
          # m = pattern.findall('acd12ad34 ad34 24ad 45')
          
          # 返回的全部匹配的列表
          print(type(m))
          print(m)
          
          运行结果
          <class 'list'>
          [('123', 'sfws', '11'), ('123', 'sfws', '11')]
          ************************************************
          <class 'list'>
          ['12', '34', '34', '24', '45']
          ```
     
        - `pattern.finditer(str, start, end)`
     
          说明：同 `pattern.findall()` 用法一样，就是返回的是一个__迭代对象__，元素为 Match 对象，使用 `group()` 取值。
     
        - `pattern.split(str, maxsplit=0, flags=0)`
     
          说明：如果分隔符里有组，分隔符也会在返回的列表当中。
     
          maxsplit 非零，最多进行 maxsplit 次分割，剩下的字符全部返回到列表的最后一个元素。
     
          演示
     
          ```python
          import re
          
          
          # 分割符中有 组
          pattern_1 = re.compile(r'(\W+)')
          # 分割符没有 组
          pattern_2 = re.compile(r'\W+')
          
          m_1 = pattern_1.split('Python, python, c++')
          m_2 = pattern_2.split('Python, python, c++')
          
          # 返回值为列表
          print(type(m_1))
          
          # 分割结果不一样
          print(m_1)
          print(m_2)
          
          运行结果
          <class 'list'>
          ['Python', ', ', 'python', ', ', 'c', '++', '']
          ['Python', 'python', 'c', '']
          ```
     
        - `pattern.sub(repl, str, count=0, flages=0)`
     
          说明：repl 是替换的字符串（也可以是函数，没仔细看原文档），str 是被替换的字符串。
     
          如果正则样式没有匹配，则返回 str。
     
          count 如果等于 0 ，则全部替换。如果大于 0，则是最大替换次数
     
          __这个方法提麻烦的__
     
          演示
     
          ```python
          import re
          
          
          pattern = re.compile('(l\w+)')
          
          str = 'You like,I like,but not...'
          
          print(str)
          m = pattern.sub('love', str)
          print(type(m))
          print(m)
          
          运行结果
          You like,I like,but not...
          <class 'str'>
          You love,I love,but not..
          
          ******************************************************
          import re
          
          
          pattern = re.compile('(\w+) (\w+)')
          
          str = 'You like,I like'
          
          print(str)
          print(type(m))
          print('+' * 30)
          
          # 匹配的第 1 组和第 2 组进行互换位置
          # 实际上时进行了 2 次匹配
          # 第 1 次匹配出 'You like',分 1 组，2 组
          # 第 2 次匹配出 'I like',分 1 组，2 组
          print(pattern.sub(r'\2 \1', str))
          
          运行结果
          You like,I like
          <class 'str'>
          not like,not like
          ++++++++++++++++++++++++++++++
          like You,like I
          ```

###  正则表达式案例

1. 正常的爬虫程序

   - URL 地址
     1. 解析 URL 随网页变化的规律
     2. 构造动态 URL 地址（无非就是 URL 描述页码的数据就不断变化）
   - headers 头部信息
     1. 添加 User-Agent 信息
     2. 可以添加更多信息
   - `urllib.request.urlopen(ur=url, headers=headers)` 打开 URL
     1. 解析响应文件 `response.read().decode()` 转换为字符串

2. 正则表达式匹配

   - 查看源文件（响应文件）的格式、表现形式

     1. 源码

        ```html
        <div class="daoyu">　　甲：在座的各位大叔、大伯、哥们、爷们，我给大家请安了!　　乙：在座的阿姨、大婶、大娘、奶奶、姐妹们，我给大家送福了!　　甲：你怎么尽给女的行礼，没我们男同胞什么事?不厚道!　　乙：不厚道的也是你在先!你刚才你行礼的时候，不也没提我们女同</div>
        <div>
            ......
        </div>
        <div class="daoyu">　　1.有一次闺蜜喝多不小心冲进了男厕所，当着一张张惊慌失措的脸，这货竟然故作镇定的大声喊：“怕什么啊！我又没带尺~~~”　　2.知道吗？我真的好想带你出去体验一下KTV的魅力啊！知道什么是KTV吗？就是K你一顿，T你一脚，最后我再做个V的</div>
        ```

     2. 查找匹配规则

        `'<div class="daoyu>"(.*?)</div>'` 其中要取消正则的贪婪模式。

        即贪婪模式如上源码及匹配规则，正则样式匹配的是从 `<div class="daoyu">` 开始，到最后一个 `</div>` 结束。这显然不是我们理想的匹配结果。

        非贪婪模式，匹配从 `<div class="daoyu">` 开始，到第一个 `</div>` 结束。

3. 代码演示

   - 部分代码演示

     ```python
     def deal_page(self, html):
             """
             处理页面数据
             """
             # 初始化正则表达式样式对象
             # re.S 全文匹配
             # 正则样式里只有 1 组，所以返回列表
             # 正则默认是贪婪模式（就是尽量匹配的多）
             # 取消贪婪模式假 '?' 号（只匹配第一个符合规则）
             pattern = re.compile('<div\sclass="daoyu">(.*?)</div>', re.S)
             contents = pattern.findall(html)
             return contents
     ```

   - 完整代码

     [查看地址](https://pan.baidu.com/s/1_B-WNP0KxrCfjGIXnVy6Ag)

### XML 文档

说明：[参考地址](https://www.w3school.com.cn/xml/xml_intro.asp)

1. XML 是指__可扩展标记语言（Extensible Markup Language，简称：XML）__

   - 作用

     > XML 被设计是用来__传送、携带数据信息__，不是用来表现、展示数据，HTML 则是用来显示数据，所以 XML用途的焦点是它说明了数据是什么，以及携带的数据信息
     >
     > [维基百科](https://zh.wikipedia.org/wiki/XML)

2. XML 与 HTML 的主要差异

   > - XML 不是 HTML 的替代
   > - XML 和 HTML 为不同的目的而设计
   > - XML 被设计为传输和存储数据，其焦点是数据内容
   > - HTML 被设计为显示数据，其焦点是数据外观
   > - XML 旨在传输信息，而 HTML 旨在显示信息

### HTML DOM 和 XML DOM 用途

说明：实际本人理解 __“DOM 就是将HTML、XML 树型化”__

1. 什么是 DOM

   说明：__DOM 是 Document Object Model （文档对象模型）的缩写__

   - DOM 是 W3C （万维网联盟）的标准
   - DOM 定义了访问 HTML 和 XML 文档的标准
   - W3C DOM 标准被分为 3 个不同的部分
     1. 核心 DOM 针对任何结构化文档的标准模型
     2. HTML DOM 针对 HTML 文档的标准模型
     3. XML DOM 针对 XML 文档的标准模型

2. 什么是 HTML DOM 

   说明：[参考地址](https://www.w3school.com.cn/htmldom/index.asp)

   - 是 HTML 的标准对象模型

   - 是 HTML 标准编程接口

   - 是 W3C 标准

   - HTML DOM 定义了所有 HTML 元素的对象和属性，以及访问它们的方法

     说明：在 HTML DOM 中，所有事物都是节点。DOM 是被视为节点树的 HTML

     1. 整个文档是一个文档节点
     2. 每个 HTML 元素是元素节点
     3. 每个 HTML 的属性是属性节点
     4. 注释是注释节点

3. 什么是 XML DOM

   说明：[参考地址](https://www.w3school.com.cn/xmldom/index.asp)

   - 是 XML 的标准对象模型

   - 是 XML 的标准编程接口

   - 中立于平台和语言（不明白）

   - 是 W3C 的标准

   - XML DOM 定义了所有 XML 元素的对象和属性，以及访问它们的方法（接口）

     说明：DOM 将 XML 文档作为一个树形结构，而树叶被定为节点（同 HTML DOM 一样都是树形节点）

     1. 整个文档是一个文档节点
     2. 每个 HTML 元素是元素节点
     3. 每个 HTML 的属性是属性节点
     4. 注释是注释节点

   - 举例说明

     1. XML文档

        ```xml
        <?xml version="1.0" encoding="ISO-8859-1"?>
        <!-- Copyright w3school.com.cn -->
        <!-- W3School.com.cn bookstore example -->
        <bookstore>
        <book category="children">
        <title lang="en">Harry Potter</title>
        <author>J K. Rowling</author>
        <year>2005</year>
        <price>29.99</price>
        </book>
        </bookstore>
        ```

     2. 对应（1）的树型结构

        > ![XML_DOM树型结构图](git_picture/XML_DOM树型结构图.png)
        
        
        
     3. HTML、XML 和 DOM的区别
     
        | 数据格式 | 描述                                           | 设计目的                                                     | 文件结构 |
        | -------- | ---------------------------------------------- | ------------------------------------------------------------ | -------- |
        | XML      | Extensible Markup language<br>(可扩展标记语言) | 设计目的是被用来传输和存储数据，其焦点是数据内容             |          |
        | HTML     | Hyper Markup Language<br>(超文本标记语言)      | 显示数据以及如何更好的显示数据                               |          |
        | DOM      | Documnet Object Model<br>(文档对象模型)        | DOM 定义了 HTML 和 XML 所有元素的对象和属性，以及访问它们的方法 | 树型结构 |
     
        

### XPath 语法介绍

说明：XPath 是一门在 XML 文档中查找信息的语言，XPath 用于 XML 文档中通过元素和属性进行导航。__(XPath 使用路径表达式在 XML 文档中进行导航)__。

本人理解：__使用 XPath 语法来解析 HTML 文档，首先要明白 XPath 是根据路径（元素、属性的节点）进行导航。XML DOM、HTML DOM 将文档树型化（形成元素、属性节点），所以将 HTML 树型化，就可以使用 XPath 语法来进行导航__。

1. XPath 节点介绍

   说明：[XPath 语法 参考地址](https://www.w3school.com.cn/xpath/index.asp)

   - XPath 节点类型
   
     说明：在 XPath 中，有 7 种类型J节点：__元素、属性、文本、命名空间、处理指令、注释以及文档节点（文档节点又称根节点）__。
   
     1. 在 XPath 中，XML 文档是被作为节点树来对待。树的根节点称之为文档节点或者根节点。
   
     2. XML 文档
   
        ```xml
        <?xml version="1.0" encoding="ISO-8859-1"?>
        <!-- Copyright w3school.com.cn -->
        <!-- W3School.com.cn bookstore example -->
        <bookstore>
        <book category="children">
        <title lang="en">Harry Potter</title>
        <author>J K. Rowling</author>
        <year>2005</year>
        <price>29.99</price>
        </book>
        </bookstore>
        ```
   
     3. 在 2 中的 XML 文档中指明各个节点信息
   
        | 元素和属性名                     | 节点名称                           |
        | -------------------------------- | ---------------------------------- |
        | `<bookstore>`                    | 文档节点                           |
        | `<author> J K. Rowling</author>` | 元素节点                           |
        | `lang="en"`                      | 属性节点                           |
        | `J K.Rowling` 和 `"en"`          | 基本值<br>(基本值是无父或无子节点) |
   
   - XPath 节点关系
   
     说明：以下解释以该文档为例
   
     ```xml
     <bookstore>
     <book category="children">
     <title lang="en">Harry Potter</title>
     <author>J K. Rowling</author>
     <year>2005</year>
     <price>29.99</price>
     </book>
     ```
   
     1. 父节点
   
        说明：每个元素或属性都有一个父节点
   
        在上面的文档中 book 元素是 title、author、year 和 price 元素的父节点
   
     2. 子节点
   
        说明：元素节点可以有 0 个、1 个或者多个子节点
   
        在上面文档中 title、author、year以及price 元素都是 book 元素的子节点
   
     3. 同级（同胞）
   
        说明：拥有相同父节点
   
        在上面文档中 title、author、year以及 price 元素都是同级节点
   
     4. 先辈和后代
   
        先辈就是父节点的父节点
   
        后代子节点的子节点
   
   2. XPath 基本语法
   
      说明：XPath 使用路径表示来选取 XML 文档中的节点或者节点集。节点是通过沿着路径（path）或者步（steops）来选取
   
      - 基本语法如表格
   
        | 表达式   | 描述                                                   |
        | -------- | ------------------------------------------------------ |
        | nodename | 选取此节点所有子节点                                   |
        | /        | 从根节点选取                                           |
        | //       | 从匹配选取当前节点选取文档中的节点，而不考虑它们的位置 |
        | .        | 选取当前节点                                           |
        | ..       | 选取当前节点的父节点                                   |
        | @        | 选取属性                                               |
        
        | 通配符 | 描述             |
        | ------ | ---------------- |
        | *      | 匹配任何元素节点 |
        | @*     | 匹配任何节点节点 |
        | node() | 匹配任何类型节点 |
        
      - XML 文档
      
        ```xml
        <?xml version="1.0" encoding="ISO-8859-1"?>
        
        <bookstore>
        
        <book>
          <title lang="eng">Harry Potter</title>
          <price>29.99</price>
        </book>
        
        <book>
          <title lang="eng">Learning XML</title>
          <price>39.95</price>
        </book>
            
        </bookstore>
        ```
      
      - 实列说明
      
        | 路径表示                           | 结果                                                         |
        | ---------------------------------- | ------------------------------------------------------------ |
        | bookstore                          | 选取 bookstore 元素的所有节点                                |
        | /bookstore                         | 选取根节点 bookstore<br>注释：加入路径起始于('/'),则此路径始终代表到某元素的绝对路径 |
        | //book                             | 选取文档中所有 book 元素，不管在什么位置                     |
        | //@lang                            | 选取名为 lang 的所有属性                                     |
        | /bookstore/*                       | 选取 bookstore 元素的所有子元素                              |
        | //*                                | 选取文档中所有元素                                           |
        | //title[@*]                        | 选取所有带有属性的 ttitle 元素                               |
        | //title[@lang="eng"]               | 选取所有 title 元素，并且这些元素拥有值为 eng 的 lang 属性   |
        | /bookstore/book[price>35.00]/title | 选取 bookstore 元素中的 book 元素的所有 title 元素，且其中的 price 元素的值须大于 35.00。 |

### 使用 XPath 语法爬虫案例

说明：XPath 语法可以导航XML（树型结构文件），在 Python 中，对 XPath 语法提供了 lxml库，可以将 HTML 文档转换为 HTML DOM 树型结构文档，在使用 XPath 语法来导航 HTML DOM 文档。

XPath 是一门技术，而Python 对这门技术提供了 lxml 这个库。

[lxml 官方文档](https://lxml.de/index.html)

1. 使用 Python 中的 lxml 库爬虫步骤

   - 明确 URL 地址
   - 对 headers 进行组装
   - 提取 服务器响应数据
   - 将响应数据读出（`read()`）
   - 将响应文件转换成__字符串或者字节__形式的数据，使用 `xml.etree.HTML('HTML文件')` 将响应数据__转换成 HTML DOM__ 树型结果数据
   - 使用 HTML DOM 对象提供的方法 `HTML DOM(对象).xpath('XPath语法')` 提取所需数据

2. 怎么使用 XPath 语法提取数据

   - 本人使用 chrome 的插件 __Xpath Helper__（在 __chrome 网上应用商店__就可以下载）

     ![Xpath-chrome](git_picture/Xpath-chrome.png)

   - 类如查询 如下一段 HTML 文档数据 提取数据

     ![XPath_查询](git_picture/XPath_查询.png)

3. 部分代码（不能单独执行，仅供参考）

   ```python
   # 导入模块
   import lxml.etree
   
   
   # XPath 匹配语句
   xpath_a = '//div[@class="d_post_content j_d_post_content "]//img[@class="BDE_Image"]/@src'
   # 处理 XPath 匹配规则
   def deal_html(self, reponse, xml_xpath):
           """
           将服务器返回的值，解析HTML文档为HTML DOM模型
           """
           html = lxml.etree.HTML(reponse)
           print(html)
           # 返回所有匹配成功的数据以列表形式
           link = html.xpath(xml_xpath)
           print('-' * 30)
           # print(link)
           return link
   # 调用处理 XPath 函数
   links = spider.deal_html(reponse, xpath_a)
   # 打印匹配结果
   print(links)
   
   打印 links
   ['https://imgsa.baidu.com/forum/w%3D580/sign=3f8065dd55b5c9ea62f303ebe538b622/945494eef01f3a2926ee86189725bc315d607cc5.jpg', 'https://imgsa.baidu.com/forum/w%3D580/sign=0f203d53c7ea15ce41eee00186013a25/8ceef01f3a292df5ece5ffc3b2315c6035a873c5.jpg', 'https://imgsa.baidu.com/forum/w%3D580/sign=56aeaf0c1bce36d3a20483380af23a24/e81f3a292df5e0fe943edad7526034a85fdf72c5.jpg']
   ```

4. 完整代码

   说明：本想完成从网站主页面提取用户主页面，但是使用 XPath 总是提取不出来，所以只能直接上用户页面提取数据了。<br> __使用 XML 浏览器使用 IE 的 USER-AGENT，可以正确匹配__

   [参考地址](https://pan.baidu.com/s/19rKoaJNimxE3f97BlEQB-Q)

### Beautiful Soup4使用

说明：[官方文档](https://beautifulsoup.readthedocs.io/zh_CN/v4.4.0/)

提示：

1. Beautiful Soup4 库为不同的解析器提供了相同的接口，但是解析器本身是有区别的，同一个文档被不同的解析器解析后可能会产生不同结构的树型文档，__故在使用 BS4 解析文档是最好指定解析器。__
2. 任何HTML或XML文档都有自己的编码方式,比如ASCII 或 UTF-8,但是使用Beautiful Soup解析后,文档都被转换成了Unicode。

1. Beautiful Soup4 介绍

   - 引用官方的描述

     > Beautiful Suop 是一个可以从 HTML 或者 XML 文档中提取数据的 Python 库，它能够通过你喜欢的转换器实现管用的文档导航、查找、修改文档方式。Beautiful Soup 会帮助你节省数小时甚至数天的工作时间。
     >
     > 推荐使用 Beautiful Soup4，因为 3 版本已经停止开发

   - Beautiful Soup4 和 lxml 的区别

     lxml 只会局部遍历，而 Beautiful Soup 是基于 DOM 的树型结果文档，会载入整个文档，解析整个文档，因此开销时间和内存开销都会大很多。

     lxml 使用 c 开发，Beautlful Soup 是使用 Python 开发的，两者性能有很大差距。
   
2. Beautiful Soup 简单语法

   说明：__最好参考官方文档进行学习__
   
   - 如何使用
   
     将文档传入 BeautifulSoup 的构造方法，就可以的到文档对象，可以传入一段字符串或者是一个文件句柄
   
     ```python
     from bs4 import BeautifulSoup
     
     
     # 创建文档对象
     soup = BeautifulSoup('index.html')
     soup = BeautifulSoup('<html>data<html>')
     ```
   
     首先，文档被转换成 Unicode ,并且 HTML 实例都被转换为 Unicode 编码（就是一个字符串也会被转换为 HTML 格式）。
   
     ```python
     from bs4 import BeautifulSoup
     
     # 参数 markup 是 html、xml 文档，features 指定解析器
     soup = BeautifulSoup(markup='I love you', features='lxml')
     print(type(soup))
     print(soup，end='\n\n')
     # 格式化打印
     print(soup.prettify())
     
     运行结果
     <class 'bs4.BeautifulSoup'>
     <html><body><p>I love you</p></body></html>
     # 格式化打印
     <html>
      <body>
       <p>
        I love you
       </p>
      </body>
     </html>
     ```
   
     然后，Beautiful Soup 选择最合适的解析器来解析这段文档，这里最好手动指定解析器（'lxml'）,避免环境不同造成解析结构不同。
   
   - 解析文档中四大对象种类
   
     说明：__可以通过 soup 加标签名来获取标签，但是获得是第一个符合内容的标签__
   
     Beautiful Soup 将复杂的文档转换成一种复杂的树型结构，每一个节点是 Python 对象，所有随想可以归纳为 4 种。
     
     | 名称            | 节点对象                                                     |
     | --------------- | ------------------------------------------------------------ |
     | Tag             | Tag 对象与 HTML，XML 原生文档中标签相同                      |
     | NavigableString | 标签的文本内容。即 `.string` 可获取                          |
   | BautifulSoup    | BeautifulSoup 对象表示一个文档内容，是一个特殊的 Tag 标签，没有属性 |
     | Comment         | 一个特殊的 NavigableString ，输出不包含注释                  |
   
     1. Tag  的属性 name，attributes
     
        ```python
        from bs4 import BeautifulSoup
        
        
        soup = BeautifulSoup(markup='<html><body><p id="p_id" class="p_class" style="p_style">I love you</p></body></html>', features='lxml')
        tag = soup.p
        
        # 获取标签的 name
        print(tag.name)
        
        # 获取标签全部属性
        # 返回值为 字典
        print(tag.attrs)
        
        # 获取 id 属性
        print(tag['id'])
        
        运行结果
        p
        {'id': 'p_id', 'class': ['p_class'], 'style': 'p_style'}
      p_id
        ```
   
     2. NavjgableString
   
        文本内容包含在 Tag 中，BeautifulSoup 用 NavigableString 类来包装 Tag 中的文本内容
     
        ```python
        from bs4 import BeautifulSoup
        
        
        soup = BeautifulSoup(markup='<html><body><p id="p_id" class="p_class" style="p_style">I love you</p></body></html>', features='lxml')
        tag = soup.p
        # 打印文本类型
        print(type(tag.string))
        # 打印文本内容
        # 和字符串一样使用
        print(tag.string)
        
        运行结果
        <class 'bs4.element.NavigableString'>
      I love you
        ```
   
     3. BeautifulSoup
   
        `BeautifulSoup` 对象表示的是一个文档的全部内容，大部分时候，可以当作一个 Tag 对象使用。
   
        因为 `BeautifulSoup` 对象并不是真正的 HTML 的 Tag ，所以它没有 name 和 attribute 属性，但是查看它的 name 属性时包含了一个特殊的值 `[document]`。
     
        ```python
        from bs4 import BeautifulSoup
        
        soup = BeautifulSoup(markup='<html><body><p id="p_id" class="p_class" style="p_style">I love you</p></body></html>', features='lxml')
        # 打印 soup 属性
        print(soup.name)
        
        运行结果
      [documnet]
        ```
   
     4. Comment
     
        Comment 对象是一个特殊的 NavigableString 对象
     
   - 遍历 DOM 树型文档
   
     1. 树型文档
   
        ```html
        <html>
         <head>
          <meta content="text/html;charset=utf-8" http-equiv="content-type"/>
          <meta content="IE=Edge" http-equiv="X-UA-Compatible"/>
          <meta content="always" name="referrer"/>
          <link href="http://s1.bdstatic.com/r/www/cache/bdorz/baidu.min.css" rel="stylesheet" type="text/css"/>
          <title>   
           百度一下，你就知道
            <a class="mnav" href="http://news.baidu.com" name="tj_trnews">
               新闻
            </a>
          </title>
         </head>
         </html>
        ```
   
     2. 使用 BeautifulSoup4 提取转换为 DOM 树型文档
   
        ```python
        import requests
        from bs4 import BeautifulSoup as bs
        
        
        html = """
                <html>
                <head>
                <meta content="text/html;charset=utf-8" http-equiv="content-type"/>
                <meta content="IE=Edge" http-equiv="X-UA-Compatible"/>
                <meta content="always" name="referrer"/>
                <link href="http://s1.bdstatic.com/r/www/cache/bdorz/baidu.min.css" rel="stylesheet" type="text/css"/>
                <title>   
                百度一下，你就知道
                    <a class="mnav" href="http://news.baidu.com" name="tj_trnews">
                    新闻
                    </a>
                </title>
                </head>
                </html>
        """
        
        # 使用 lxml 为解析器
        soup = bs(markup=html, features='lxml')
        ```
   
     3. 直接子节点：`.content` \ `.childer` 属性 (节点属性)
   
        说明：获取节点的 __子节点__ 
   
        - `.content`
     
          说明：获取的子节点以列表的形式返回
     
          ```python
          head = soup.head
          
          node_list = head.contents
          print(type(node_list))
          print(len(node_list))
          print(node_list)
          
          运行结果
          <class 'list'>
          11
          ['\n', <meta content="text/html;charset=utf-8" http-equiv="content-type"/>, '\n', <meta content="IE=Edge" http-equiv="X-UA-Compatible"/>, '\n', <meta content="always" name="referrer"/>, '\n', <link href="http://s1.bdstatic.com/r/www/cache/bdorz/baidu.min.css" rel="stylesheet" type="text/css"/>, '\n', <title>   
                  百度一下，你就知道
                      <a class="mnav" href="http://news.baidu.com" name="tj_trnews">
                      新闻
                      </a>
          </title>, '\n']
          ```
     
          解释：节点的子节点内容都会保存，但是是以子节点为单位（换行符也算子节点了）
     
        - `.childern`
     
          说明：返回的子节点为迭代器类型
     
          ```python
          head = soup.head
          
          node_iter = head.children
          print(type(node_iter))
          print(node_iter)
          
          for node in node_iter:
              print(node)
              
          运行结果
          <class 'list_iterator'>
          <list_iterator object at 0x00000144567FC6D8>
          
          <meta content="text/html;charset=utf-8" http-equiv="content-type"/>
          ......
          ```
     
     4. 获取节点所有子孙节点 `.descendants` 属性（节点属性）与 `.children` 相似不是列表
     
        - 所有子孙节点包含上面的所有节点
     
          ```python
          head = soup.head
          
          node_desc = head.descendants
          print(type(node_desc))
          print(node_desc)
          
          for node in node_desc:
              print(node)
              
          运行结果
          <class 'generator'>
          <generator object descendants at 0x000001E77D9478E0>
          
          
          <meta content="text/html;charset=utf-8" http-equiv="content-type"/>
          
          
          <meta content="IE=Edge" http-equiv="X-UA-Compatible"/>
          
          
          <meta content="always" name="referrer"/>
          
          
          <link href="http://s1.bdstatic.com/r/www/cache/bdorz/baidu.min.css" rel="stylesheet" type="text/css"/>
          
          
          
          <title>
                  百度一下，你就知道
                      <a class="mnav" href="http://news.baidu.com" name="tj_trnews">
                      新闻
                      </a>
          </title>
          
                  百度一下，你就知道
          
          <a class="mnav" href="http://news.baidu.com" name="tj_trnews">
                      新闻
                      </a>
          
                      新闻
          ```
     
          解释：__真是所有子节点，内容也算是节点了__
     
     5. 节点内容 `.string` 属性（节点属性）
     
        说明：如果该标签内没有子标签，`.string` 属性可以获得该标签的内容。 ~~如果该标签只有一个子标签，那么它可以获得子标签的内容（就这两种使用方式）~~ (有待考察，反正本人试的时候不可以)
     
   - 搜索文档树 `.find_all()`
   
     说明：`find_all(name=None, attrs={}, recursive=True, text=None, limit=None, **kwargs):` <br> 参数 `name` 接受 __字符串、列表、正则表示__ <br> 参数 `text` 接受 __字符串、列表、正则表达式__ <br> 传入列表 ['a', ‘b’] 会匹配任意一个，有就匹配，没有就不匹配
   
     1. 树型文件
   
        ```python
        import requests
        from bs4 import BeautifulSoup as bs
        
        
        html = """
                <html>
                <head>
                <meta content="text/html;charset=utf-8" http-equiv="content-type"/>
                <meta content="IE=Edge" http-equiv="X-UA-Compatible"/>
                <meta content="always" name="referrer"/>
                <link href="http://s1.bdstatic.com/r/www/cache/bdorz/baidu.min.css" rel="stylesheet" type="text/css"/>
                <title>  
                    <a class="mnav" href="http://news.baidu.com" name="tj_trnews">
                    新闻
                    </a>
                    <a href=http://www.baidu.com/duty/>
                    使用百度前必读
                    </a>
                    <a href=http://v.baidu.com name=tj_trvideo class=mnav>
                    视频
                    </a>
                </title>
                </head>
                </html>
        """
        soup = bs(markup=html, features='lxml')
        ```
   
     2. 参数 `name`
   
        说明：通过标签名 name 查找所有的标签
   
        ```python
        a_tag = soup.find_all(name='a')
        print(type(a_tag))
        print(a_tag)
        
        运行结果
        <class 'bs4.element.ResultSet'>
        
        [<a class="mnav" href="http://news.baidu.com" name="tj_trnews">
                    新闻
                    </a>, <a href="http://www.baidu.com/duty/">
                    使用百度前必读
                    </a>, <a class="mnav" href="http://v.baidu.com" name="tj_trvideo">
                    视频
                    </a>]
        ```
   
     3. 参数 `attrs` 和 `name`
   
        说明：name 为标签名字，`attrs` 为标签属性
   
        ```python
        a = soup.find_all(name='a', attrs={'class': 'mnav'})
        print(a)
        
        运行结果
        [<a class="mnav" href="http://news.baidu.com" name="tj_trnews">
                    新闻
                    </a>, <a class="mnav" href="http://v.baidu.com" name="tj_trvideo">
                    视频
                    </a>]
        ```
   
     4. `name` 传入正则表达式
   
        ```python
        import re
        a = soup.find_all(name=re.compile('^a'))
        print(a)
        
        运行结果
        [<a class="mnav" href="http://news.baidu.com" name="tj_trnews">
                    新闻
                    </a>, <a href="http://www.baidu.com/duty/">
                    使用百度前必读
                    </a>, <a class="mnav" href="http://v.baidu.com" name="tj_trvideo">
                    视频
                    </a>]
        ```
   
     5. 参数 `text` 搜索标签内容值
   
        说明：接受 __字符串、列表、正则表达式__
   
        __不好用，根究标签内容取标签__
   
     6. 参数 **keyword  不定长参数字典
   
        用法：`id=id` 好像只能用 ID 属性
   
        ```python
        tag_id = soup.find_all(id='a_1')
        print(tag_id)
        
        运行结果
        [<a class="mnav" href="http://news.baidu.com" id="a_1" name="tj_trnews">
                    新闻
                    </a>]
        ```
   
        解释：在上面用到的例子中 __a 标签中添加属性 ID__
   
   - CSS 选择器  `.select()`
   
     说明：`select(selector, namespaces=None, limit=None, **kwargs)` <br> 方式：类名加 `.` id 名加 `#` <br> 返回值：为列表
   
     1. 通过标签名查找
   
        ```python
        tag = soup.select('title')
        
        print(type(tag))
        print(tag)
        
        运行结果
        <class 'list'>
        [<title>
        <a class="mnav" href="http://news.baidu.com" id="a_1" name="tj_trnews">
                    新闻
                    </a>
        <a href="http://www.baidu.com/duty/">
                    使用百度前必读
                    </a>
        <a class="mnav" href="http://v.baidu.com" name="tj_trvideo">
                    视频
                    </a>
        </title>]
        ```
   
        解释：返回的是列表，是标签的所有内容
   
     2. 通过类名查找
   
        ```python
        class_name = soup.select('.mnav')
        
        print(type(class_name))
        print(class_name[1])
        
        运行结果
        <class 'list'>
        <a class="mnav" href="http://v.baidu.com" name="tj_trvideo">
                    视频
                    </a>
        ```
   
     3. 通过 ID 名查找
   
        ```python
        id_name = soup.select(selector='#a_1')
        
        print(id_name)
        
        运行结果
        [<a class="mnav" href="http://news.baidu.com" id="a_1" name="tj_trnews">
                    新闻
                    </a>]
        ```
   
     4. 组合查找
   
        说明：查找 a 标签的 id 为 。。。
   
        `soup.select('a #a_1')`
   
        `soup.select('head > title')`
   
     5. 属性查找
   
        说明：查找标签可以加入属性
   
        ```python
        a = soup.select('a[id="a_1"]')
        
        print(a)
        
        运行结果
        [<a class="mnav" href="http://news.baidu.com" id="a_1" name="tj_trnews">
                    新闻
                    </a>]
        ```
   
     6. 获取标签内容
   
        说明：`select` 返回值为列表，使用 `get_text()` 方法获取内容
   
        ```python
        a = soup.select('a[id="a_1"]')
        
        print(a[0].get_text())
        
        运行结果
        新闻
        ```

### 使用 BS4 爬虫案例

说明：bs4 太过于简单，所以没有必要进行案例分-析

待续......

### 正则，lxml 和 Beautiful Soup4 性能对比

1. 话不多，直接上图比较

   | 抓取工具       | 速度对比 | 使用难度对比 | 安装难度对比 |
   | -------------- | -------- | ------------ | ------------ |
   | 正则表达式     | 最快     | 困难         | 内置         |
   | lxml           | 块       | 简单         | 简单         |
   | beautiful Soup | 慢       | 最简单       | 一般         |

   可以明了看出，实际最优秀的还是__正则表达式__，折中的方法使用__lxml__。

### JSON 数据格式

说明：[官方文档](https://docs.python.org/zh-cn/3.6/library/json.html)<br>JSON（javascript Object Nonation）是一种轻量级的数据交互格式。<br>优点：<br>1. 方便阅读和编写<br>2.方便机器进行解析和生成<br>3.适合进行数据交换，比如网站前台与后台之间进行交互<br>Python 自带 JSON 模块，直接使用 `import json`

1. JSON 数据结构类型介绍

   JSON 简单的说就是 Javascript 中的__对象__、__数组__，所以这两种结构就是__对象__、__数组__结构，通过这两种结构可以表示各种复杂的结构

   - 对象

     对象在 Javascript 中表示为  ‘{}’ 括起来的内容，数据结构为 `{key1: value1, key2: value2, key3: value3......}` 键值对结构。在面向对象语言中，key 为对象属性，value 为属性值。取值方法为 `object.key` 获取属性值，属性值可以是 （数字、数组、字符串、对象）。

   - 数组：

     数组在 Javascript 中表示 [] 括起来的内容，数据结构为  `['python', 'c\c++', 'Java'......]`。取值方式为数组的索引取值。字段值的类型为（数字、数组、字符串、对象）。

2. Python 的 json 库的使用

   json 模块将介绍四种方法：dumps、loads、dump、load，用于 JSON 格式（实际就是字符串类型）与 Python 的数据类型进行转换。

   - `json.loads` 使用

     1. 将 JSON 数据格式（字符串）转换为 Python 对象。从 JSON 到 Python 的数据类型转换如表

        | JSON         | Python     |
        | ------------ | ---------- |
        | object       | dict       |
        | array        | list       |
        | string       | unicode    |
        | number(int)  | int \ long |
        | number(real) | float      |
        | ture         | Ture       |
        | false        | False      |
        | null         | None       |

     2. 代码演示

        ```python
        import json
        
        
        # 定义 json 的 数组 形式数据格式
        json_list = '["python", "c", "java"]'
        
        # 定义 json 的 对象（字典）形式的数据格式
        json_dict = '{"city": "Shenyang", "major": "Computer"}'
        
        # 使用 json.loads() 将 json 数据转换为 Python 数据格式
        python_list = json.loads(json_list)
        python_dict = json.loads(json_dict)
        
        # 打印
        print(python_list)
        print('+' * 35)
        print(python_dict)
        
        运行结果
        ['python', 'c', 'java']
        +++++++++++++++++++++++++++++++++++
        {'city': 'Shenyang', 'major': 'Computer'}
        ```

        __注意：之前使用 `"['python', 'c', 'java']"`运行报错，使用 `'["python", "c", "java"]'` 正常运行__

   - `json.dumps()` 使用

     1. 将 Python 数据格式转换为 JSON 数据格式（字符串）。从 Python 到 JSON 的数据类型转换如表

        | Python             | JSON   |
        | ------------------ | ------ |
        | dict               | object |
        | list \ tuple       | array  |
        | str \ unicode      | string |
        | int \ long \ float | number |
        | Ture               | ture   |
        | False              | false  |
        | None               | null   |

     2. 代码演示

        ```python
        import json
        
        
        # 定义 python 的 列表 形式数据格式
        python_list = ['python', 'c', 'java']
        
        # 定义 python 的 字典 形式的数据格式
        python_dict = {'city': 'Shenyang', 'major': 'Computer'}
        
        # 使用 json.dumps() 将 python 数据转换为 json 数据格式
        # 禁用 默认转码格式为 ASCII，启用转码格式为 Unicode
        json_list = json.dumps(python_list, ensure_list=False)
        json_dict = json.dumps(python_dict, ensure_list=False)
        
        # 打印
        print(type(json_list))
        print(json_list)
        print('+' * 35)
        print(type(json_dict))
        print(json_dict)
        
        运行结果
        <class 'str'>
        ["python", "c", "java"]
        +++++++++++++++++++++++++++++++++++
        <class 'str'>
        {"city": "Shenyang", "major": "Computer"}
        ```
   
- `json.dump()` 和 `json.load()` 使用
  
  1. `json.dump()`将 Python 内置类型转化为为 JSON 对象__后__写入文件
  
  2. `json.load()` 将 JSON 形式的字符串转化为 Python 的数据类型
  
  3. 代码演示
  
        ```python
        import json
        
        
        # 定义 python 的 列表 形式数据格式
        python_list = ['python', 'c', 'java']
        
        # 定义 python 的 字典 形式的数据格式
        python_dict = {'city': 'Shenyang', 'major': 'Computer'}
        
        # 使用 json.dump() 将 python 数据转换为 json 数据格式,并写入文件
        json.dump(python_list, open('Json数据/pythonlist.json', 'w'), ensure_ascii=False)
        json.dump(python_dict, open('Json数据/pythondict.json', 'w'), ensure_ascii=False)
        
        # 使用 json.load() 将 json文件 数据读出、转换为 python 数据格式
        new_list = json.load(open('Json数据/pythonlist.json', 'r'))
        new_dict = json.load(open('Json数据/pythondict.json', 'r'))
        
        # 打印
        print(type(new_list))
        print(new_list)
        print(type(new_dict))
        print(new_dict)
        
        运行结果
        <class 'list'>
        ['python', 'c', 'java']
        <class 'dict'>
        {'city': 'Shenyang', 'major': 'Computer'}
        ```

### JsonPath语法介绍

说明：[官方文档](https://goessner.net/articles/JsonPath/)

1. 介绍 JsonPath

   - JsonPath 是一种信息抽取类库，是从 json 文档中获取指定信息的工具，有多种语言实现（Javascript \ Python \ PHP \ Java）。JsonPath 与 JSON，相当于 XPATH 与 XML一样关系。

   - 导入 `import jsonpath`

2. JsonPath 与 XPath 语法

   - | XPath | JsonPath | 描述                                                         |
     | ----- | -------- | ------------------------------------------------------------ |
     | /     | $        | 根节点                                                       |
     | .     | @        | 当前节点                                                     |
     | /     | .or[]    | 子节点                                                       |
     | ..    | n/a      | 父亲节点（json 不支持）                                      |
     | //    | ..       | 任意位置，选择符合条件                                       |
     | *     | *        | 匹配所有节点                                                 |
     | @     | n/a      | 根据属性访问（json不支持，因为 json 数据格式是 key：value 结构） |
     | []    | []       | 迭代器标识（做简单的迭代操作，数组下标、属性值等）           |
     | \|    | [,]      | 支持迭代器做多选                                             |
     | []    | ?()      | 支持过滤操作                                                 |
     | n/a   | （）     | 支持表达式计算（XPath 不支持）                               |
     | （）  | n/a      | 分组（Json 不支持）                                          |

   - Json 数据实类

     原始文件为 XML 文件，构建简单的 JSON 格式
     
     ```json
     { "store": {
         "book": [ 
           { "category": "reference",
             "author": "Nigel Rees",
             "title": "Sayings of the Century",
             "price": 8.95
           },
           { "category": "fiction",
             "author": "Evelyn Waugh",
             "title": "Sword of Honour",
             "price": 12.99
           },
         ],
         "bicycle": {
           "color": "red",
           "price": 19.95
         }
       }
     }
     ```
     
   - XPath 与 JsonPath语法对比（使用上面 JSON 数据格式）
   
     | XPath 语法           | JSONPath 语法                       | 结果                                   |
     | -------------------- | ----------------------------------- | -------------------------------------- |
     | /store/book/author   | $.store.book[*].author              | 书店所有书籍的作者                     |
     | //author             | $..author                           | 所有作者                               |
     | /store/*             | $.store.*                           | 书店的所有东西，书籍、红色自行车       |
     | /store//price        | $.store..price                      | 书店里所有东西的价钱                   |
     | //book[3]            | $..book[2]                          | 书店里第 3 本书                        |
     | //book[last()]       | $..book[@.length-1]<br>$..book[-1:] | 书店里最后 1 本书                      |
     | //bool[position()<3] | $..book[0,1]<br>$..book[:2]         | 前 2 本书                              |
     | //book[price<10]     | $..book[?(@.price<10)]              | 过滤价钱大于 10 元的书籍               |
     | //*                  | $..*                                | XML 文档中所有元素，JSON结构的所有成员 |
   
3. JsonPath 实例演示

   - 中国个各大城市的 JSON 文件 [各大城市 JSON 文件](https://www.lagou.com/lbs/getAllCitySearchLabels.json)

   - 代码演示

     ```python
     import json
     # json 解析语法，对应 xpath
     import jsonpath
     
     # 定义 url 地址，请求头部信息
     url = 'https://www.lagou.com/lbs/getAllCitySearchLabels.json'
     headers = {
         'User-Agent':
         'Mozilla/5.0 (Macintosh; Intel Mac OS X 10.6; rv2.0.1) Gecko/20100101 Firefox/4.0.1'
     }
     
     # 构造 http 协议请求
     request = urllib.request.Request(url=url, headers=headers)
     response = urllib.request.urlopen(request)
     
     # 将相应文件 转换为 字节（byte），在转换为 字符串（string）
     html = response.read().decode()
     # JSON 结构文件，为字符串
     print('JSON 数据格式', type(html))
     
     # 将 JSON 响应文件，转换未 Python 的数据格式
     python_unicode = json.loads(html)
     # JSON 结构文件，转换为 Python 数据格式，为字典
     print('转换为 Python 数据格式 ', type(python_unicode))
     
     # 解析 python格式的 JSON 数据结构
     # '$..name' 匹配语法
     city_list = jsonpath.jsonpath(python_unicode, '$..name')
     
     # 打印
     print('jsonpath 匹配结构', type(city_list))
     print(len(city_list))
     print(city_list)
     
     运行结果
     JSON 数据格式 <class 'str'>
     转换为 Python 数据格式  <class 'dict'>
     298
     ['安阳', '安庆',
      ......
     '张掖', '张家界']
     ```


### 使用 JsonPath 语法爬虫案例

1. 对数据网络数据进行爬取，解析提取，然后将 Python 格式数据转换为 JSON 数据格式

   - 代码演示

     ```python
     import urllib.request
     import lxml.etree
     import json
     
     
     url = 'https://www.qiushibaike.com/hot/'
     headers = {
         'User-Agent':
         'Mozilla/5.0 (Windows NT 10.0; WOW64; Trident/7.0; rv:11.0) like Gecko'
     }
     
     request = urllib.request.Request(url=url, headers=headers)
     response = urllib.request.urlopen(request)
     
     html = response.read().decode()
     # 将服务器返回的响应，解析HTML文档为 HTML DOM 模型
     html = lxml.etree.HTML(html)
     
     # XPath 语法 匹配\提取 创建根节点
     node_list = html.xpath("//div[contains(@id,'qiushi_tag_')]")
     
     item = {}
     
     for text in node_list:
         # test() 是 XPath 方法 对标签内容进行提取
         # stats = text.xpath("/div[@class='stats']/span/i/text()")[0]
     
         # 匹配结果为列表，取值为下标[]
         # 但列表元素是网页标签
         # 需要进行进一步提取,使用 text 属性
         stats = text.xpath(".//div[@class='stats']/span/i")[0].text
         content = text.xpath(".//div[@class='content']/span")[0].text
     
         # 对字典进行赋值
         item = {
             'stats': stats,
             'content': content
         }
         
         # 将 Python 数据格式 转换 JSON 数据格式,写入文件中
         # encoding 参数,表示使用 utf-8 对文件进行编码
         with open('多线程爬虫/数据.json', 'a', encoding='utf-8') as f:
             f.write(json.dumps(item, ensure_ascii=False) + '\n')
             
     运行结果
     文件内容
     ......
     {"stats": "1478", content": "\n\n\n我和老婆回家，刚好老爸的一个朋友来我家。"}
     ......
     ```


## 多线程爬虫

### 多线程介绍

1. 多线程介绍

   - CPU介绍
     1. 计算机核心为 CPU，CPU 承担了所有计算任务
     2. 一个 CPU 核心一次只能执行一个任务（一个进程），其他任务（进程）处于非运行状态
   - 进程与线程
     1. 进程的执行单元为线程
     2. 一个进程包含多个线程
     3. 进程的内存空间是共享的，他的每个线程都可以使用
     4. 一个线程在使用进程空间时，其他线程必须等待进程内存空间释放<br>（互斥锁：防止同一时间斗个线程同时使用进程空间）

2. Python 的多线程

   - GIL（全局解释器锁）

     说明：[参考文档](https://blog.csdn.net/bitcarmanlee/article/details/51577014)

     1. GIL 设计理念

        > Python 的代码执行由 Python 虚拟机（解释器主循环，CPython版本）来控制。Python 在设计之初就考虑到在__解释器主循环__中，同时只能执行一个线程。__任意时刻只有一个线程在解释器中运行__。对 Python 的虚拟机访问的控制是由__全局解释器锁 GIL__控制，正式 GIL 锁控制了同一时刻只有一进程在执行。
        >
        > [参考地址](https://www.jianshu.com/p/573aaa001b35)

     2. GIL 设计目的

        - 硬件上的进步

          随着 CPU 多核时代的来临。为了更有效的利用多核处理器的性能，出现了多线程的编程方式，而随之带来的就是__线程间数据一致性和状态同步的困难__。即使在 CPU 内部的 Cache 也不例外，为了有效解决多份缓存之间的数据同步时各厂商花费了不少心思，也不可避免的带来了一定的性能损失。

        - Python 的解决办法

          Python 解决多线程之间数据完整性和状态同步的最简单方法自然就是加锁，于是有了GIL这把超级大锁。GIL 是一把全局排他锁，甚至就几乎等于Python是个单线程的程序。

   - Python 的多线程用途

     1. 由 GIL (全局解释器锁)，使得 Python 同一时间只能执行一个线程，其他线程处于等待状态
   
2. Python 的多线程适用于，大量密集的 I/O 处理
   
    - 当前 CPU 的计算能力，远远大于文件的读写等 I/O 处理能力，固 CPU 常处于等待状态
    - Python 的多进程适用于。大量的并行计算

### 多线程爬虫案例

1. 程序流程

   - 流程

     ```flow
     st=>start: start
     in1=>inputoutput: 爬取网页 url 的队列
     op1=>operation: 创建 3 个线程，爬取网页数据
     con1=>condition: 判断队列是否为空
     op2=>operation: 分别从 url 队列中提取 url 地址
     op3=>operation: 将各个线程爬取的数据存入网页数据队列中
     out1=>inputoutput: 响应数据队列
     op4=>operation: 创建 3 个解析线程
     con2=>condition: 判断队列是否为空
     op5=>operation: 分别从数据队列中提取网页数据
     op6=>operation: 将解析后数据转为 json 格式
     out2=>inputoutput: 解析后数据
     e=>end: 结束

     st->in1->op1->con1(no)->op2->op3->out1->op4->con2(no)->op5->op6->out2->e
     con1(yes)->op3
     con2(yes)->op6
     ```
     
     

2. 主要代码演示

   说明：[完整代码地址](https://pan.baidu.com/s/1d6Yly0p9LCvsbm35WW7Xjw)

   - 演示

     ```python
     # 验证 队列 是否为空
     CRAWL_EXIT = False
     PARSE_EXIT = False
     
     # json 文件路径
     file_path = '多线程爬虫/name_list.json'
     
     def main():
         # 创建 页码 队列
         page_queue = Queue(10)
         # 队列属性: maxsize 表示队列长度
         for i in range(1, 11):
             # 放入1-10个数据
             page_queue.put(i)
     
         # 创建页面信息队列，无参数，表示无限多（html源码）
         data_queue = Queue()
     
         crawllist = ['采集1号', '采集2号', '采集3号']
         threadcrawl = []
         for thread_name in crawllist:
             thread = ThreadCrawl(thread_name, page_queue, data_queue)
             thread.start()
             threadcrawl.append(thread)
     
         # 加上堵塞时，只有等待 爬取线程全部执行结束，才开始执行解析线程
         # 这样，本人不知道，后果怎样
         # 
         # 但是，没有这堵塞语句时
         # 爬取线程在爬取网页，响应文件进入队列中
         # 
         for thread in threadcrawl:
             thread.join()
     
         # 三个解析线程名
         parselist = ['解析线程1号','解析线程2号','解析线程3号']
         threadparse = []
     
         for thread_name in parselist:
             thread = ThreadParse(thread_name, data_queue, file_path)
             thread.start()
             threadparse.append(thread_name)
     
         # 等待队列为空
         while not page_queue.empty():
             pass
     
         global CRAWL_EXIT
         CRAWL_EXIT = True
         print('pageQueue为空')
     
         while not data_queue.empty():
             pass
         global PARSE_EXIT
         PARSE_EXIT = True
         print('data_queue为空')
         
     	# 主线程 睡眠 15 秒，等到子线程全部结束    
         time.sleep(15)
         
         # 判断 网页页码队列 是否为空
          if page_queue.empty():
             print('网页页码数据队列  为空')
         else:
             print('网页页码数据队列   不为空')
     	
         # 判断 网页数据队列 是否为空
         if data_queue.empty():
             print('网页数据队列  为空')
         else:
             print('网页数据队列   不为空')
         
     
     if __name__ == '__main__':
         main()
     ```
     
     __解释：网页数据队列 不为空，所以 解析线程会遇到响应队列没有值，而结束线程（概率小，但总会遇到）__

3. 遇到问题说明、有待解决

   - json 文件出现不寻常的大量重复数据，可能情况如下：

     1. 爬取网页数据就是这样

     2. 线程之间调度、网页页码队列的取值、响应文件队列的取值，出现问题

     3. 爬取线程的堵塞 `.json()` 有一定问题，加上，只有等代爬取完成，才开始执行主线程、解析线程。不加，容易出现，解析线程某一时间遇到响应队列没有值，而结束线程（概率小，但总会遇到）如下：

        ```python
        """
        启动： 采集1号
        启动： 采集2号
        启动： 采集3号
        启动： 解析线程1号
        启动： 解析线程2号
        启动： 解析线程3号
        page_queue为空
        结束： 采集2号
        结束： 解析线程1号
        结束： 采集3号
        data_queue为空
        结束： 解析线程3号
        结束： 解析线程2号
        结束： 采集1号
        网页页码数据队列  为空
        网页数据队列   不为空
        """
        ```

## 动态 HTML 处理和机器图像识别

### 静态页面和动态页面介绍

说明：说明：[参考地址-1](https://blog.csdn.net/qq_26975307/article/details/53992231)、[参考地址-2](https://blog.csdn.net/weixin_42028854/article/details/80019945)、[参考地址-3](https://blog.csdn.net/yuming226/article/details/80280745)、[参考地址-4](https://www.jianshu.com/p/649d2a0ebde5)、[反爬虫技术分享](https://segmentfault.com/a/1190000005840672)

1. 静态页面介绍

   - 在静态 Web 程序中，客户端使用Web浏览器（chrome、FireFox等）经过网络连接到服务器上，使用 HTTP 协议发起一个请求（Request），通知 Web 服务器现在需要得到哪个页面，所有的请求交给Web服务器，之后 Web 服务器根据用户的需要，从文件系统（存放了所有静态页面的磁盘）取出内容。再通过 Web 服务器返回给客户端，客户端接收到内容之后经过浏览器渲染解析，得到显示的效果。
   - 为了让静态 web 页面显示更加好看，使用 JavaScript、VBScript 和 AJAX（Asynchronous Javascript And XML、异步 JavaScript 和 XML 是指一种创建交互式网页应用的网页开发技术）但是这些特效都是在客户端上借助于浏览器展现给用户的，所以在服务器上本身并没有任何的变化。
   - 静态页面缺点和开发技术
     1. 静态 Web 无法连接数据库
     2. 由于 web 页面中，大量使用JS，导致浏览器打开页面，会占用大量的内存。此时服务端的压力减轻，但同时压力转移到了客户端。
     3. 静态 Web 资源开发技术：HTML、XML

2. 动态页面介绍

   - 动态 Web 中，程序依然使用客户端和服务端，客户端依然使用浏览器（chrome、FireFox等），通过网络连接到服务器上，使用HTTP协议发起请求（Request），现在的所有请求都先经过一个Web Server Plugin 来处理。<br>如果客户端请求的是静态资源（.html），则将请求直接转交给 Web 服务器，之后 Web 服务器从文件系统中取出内容，发送回客户端浏览器进行解析执行。<br>

     如果客户端请求的是动态资源（.jsp、.asp\\.aspx、.php），则先将请求转交给 Web Container （WEB 容器），在 Web Container 中连接数据库，从数据库中取出数据等一系列操作后动态拼凑页面的展示内容，拼凑页面的展示内容后，把所有的展示内容交给 Web 服务器，之后通过 Web 服务器将内容发送回客户端浏览器进行解析执行。

   - 动态页面缺点和开发技术

     1. 动态网站以数据库为基础，所以对数据库的安全和保密性要求较高，要专业技术人员提供维护才能保证网站的安全
     2. 动态网站不利于搜索引擎收录
3. 动态网站制作成本较高
     4. HTML+JavaScript(Node.js)
     5. HTML+PHP
     6. HTML+ASP.NET(或ASP)
     7. HTML+JSP

### Selenium 与 浏览器（XXXdriver）

1. Selenium 

   说明：[官方文档](https://selenium-python.readthedocs.io/index.html)、[使用文档](https://selenium-python-zh.readthedocs.io/en/latest/getting-started.html)

   - Selenium介绍

     说明：[参考文档](https://juejin.im/post/5ce50c57e51d4556dc293582)

     Selenium 是用于测试 Web 应用程序的用户界面（UI）的常用框架。<br>Selenium  不是浏览器，不支持浏览器的功能，需要与第三方浏览器结合使用（一般将其内嵌代码中执行）。

     Selenium 的使用，通过一些指令让浏览器自动（自动化检测）加载页面等一些列操作。

   - Selenium 安装

     通过 `pip install selenium`

2. PhantomJS

   说明：[官方文档](https://phantomjs.org/documentation/)<br>PhantomJS 已近停止进一步开发，Python3 也已经不支持，如下说明：

   ```tex
   Warning (from warnings module):
     File "H:\Compile\Python\Python36\lib\site-packages\selenium\webdriver\phantomjs\webdriver.py", line 49
       warnings.warn('Selenium support for PhantomJS has been deprecated, please use headless '
   UserWarning: Selenium support for PhantomJS has been deprecated, please use headless versions of Chrome or Firefox instead
   ```

   - PhantomJS 介绍

     PhantomJS 实现了一个无头（headless）、无界面的 Webkit 浏览器。PhantomJS 会把网页加载到内存中执行（如执行JavaScript）,因其不会展示玩也页面，所以高效。常用于网页抓取、页面输出、自动化测试等方面。

   - PhantomJS 安装

     [下载地址](https://phantomjs.org/download.html)，因其是无界面浏览器，而非 Python 库，固只能从官网下载安装

3. chromedriver 与 geckodriver 替换 PhantomJS

   说明：PhantomJS 已死、[参考文档-1](https://testerhome.com/topics/5349)、[参考文档-2](https://www.jianshu.com/p/31c8c9de8fcd)

   - XXXdriver 是什么
     1. XXXdriver 是为于网站开发人员提供自动化检测的接口，是 Selenium （网站自动化检测框架）的 chrom 和 firefox 基础部分。
     2. 本人理解：XXXdriver 是 Selenium 和浏览器进行通信的桥梁，通过 XXXdriver ，Selenium 可以远程启动浏览器。

   - google浏览器、firefox 浏览器都支持实现一个无头（headless）模式，用于测试。
   - 使用方式
     1. 下载，chromedriver、geckodriver 对应版本文件，
     2. 只需进行解压。如 `geckodriver.exe\chromedriver.exe`
     3. 再将其添加到环境变量中


### Selenium 的使用介绍

说明：[官方文档](https://seleniumhq.github.io/selenium/docs/api/py/)、[具体API参考地址](https://selenium-python-zh.readthedocs.io/en/latest/getting-started.html)

1. Selenium 的 Webdriver 的介绍

   - Selenium 的使用是通过 WebDriver API（Application Programming interface,应用程序编程接口）。简单来说WebDriver 是一个方法集合，包括的操作如：像个浏览器加载网页信息、定位网页元素、与网页元素进行交互（发送文本、点击等）

2. Selenium 的简单使用

   - 代码演示

     ```python
     # 导入 webdriver 模块
     from selenium import webdriver
     from selenium.webdriver.common.by import By
     # 导入 调用键盘按键操作模块
     from selenium.webdriver.common.keys import Keys
     import time
     
     
     # 创建浏览器对象，配置过环境变量
     driver = webdriver.Firefox()
     # 没有配置环境变量
     # driver = webdriver.Firefox(executable_path='geckodriver路径')
     
     
     # driver.get(url) 方法将打开一个 url 地址，webdriver将等待，知道页面全部加载完毕（其实是等待onload方法执行完毕）
     # 然后返回继续执行
     # 
     # 注意：如果页面大量使用 ajax 加载，webdriver可能不知道什么时候页面已经完成加载
     driver.get('http://www.baidu.com')
     # 打印页面源代码
     # print(driver.page_source)
     
     # 打印页面标题"百度一下，你就知道"
     print(driver.title)
     
     # 截屏查看
     driver.save_screenshot('自动化检测/1.png')
     
     
     # 获取页面元素，id为us
     data = driver.find_element_by_id('su')
     # data = driver.find_element(by=By.ID, value='su')
     # .text 获取标签内容
     # 正确打印 "百度一下"
     print(data.text)
     
     
     # 获取页面元素，id为kw（搜索框）
     input_1 = driver.find_element_by_id('kw')
     
     # ctrl + a 全选搜索框内容
     content_1 = input_1.send_keys(Keys.CONTROL, 'a')
     
     # ctrl + x 剪切搜索框内容
     content_2 = input_1.send_keys(Keys.CONTROL, 'x')
     
     # 清除搜索框
     input_1.clear()
     
     # id='kw' 是百度搜索框，输入'NBA'
     input_1.send_keys('NBA')
     
     
     # 在搜索框中输入，代码执行完成，但浏览器需要时间响应，固设置休眠时间，等待浏览器响应完成
     # 在截取快照
     time.sleep(2)
     # 截屏查看
     driver.save_screenshot('自动化检测/2.png')
     
     # 打印网页渲染后的源代码
     # print(driver.page_source)
     
     # 使用键盘模块，调用键盘的回车键
     # input_1.send_keys(Keys.RETURN)
     
     # 点击 '百度一下'按钮 进行搜索
     # 
     # 这个由问题，字符串没有 click 属性
     data.click()
     
     # 获取当前 url 地址
     url = driver.current_url
     print(url)
     
     # 获取 cookie 的值
     cookie = driver.get_cookies()
     print(cookie)
      
     # 同上理由
     time.sleep(1)
     driver.save_screenshot('自动化检测/3.png')
     
     
     # 关闭一个标签页
     # 如浏览器只开了一个标签,则默认关闭浏览器
     # driver.close()
     
     # 关闭浏览器
     driver.quit()
     ```
   
3. 定位页面元素

   - 如表

     | 方式                 | 方法 \\ `from selenium.webdriver.common.by import By`        |
     | -------------------- | ------------------------------------------------------------ |
     | by id                | find_element_by_id<br>find_element(by=By.ID, value='xxx')    |
     | by name              | find_element_by_name<br>find_element(by=By.NAME, value='xxx') |
     | by class name        | find_element_by_class_name<br>find_element(by=By.CLASS_NAME, value='xxx') |
     | by xpath             | find_element_by_xpath<br>find_element(by=By.XPATH, value='xxx') |
     | by  tag name         | find_element_by_tag_name<br>find_element(by=By.TAG_NAME, value='xxx') |
     | by link text         | find_element_by_link_text<br>find_element(by=By.LINK_TEXT, value='xxx') |
     | by partial link text | find_element_by_partial_link_text<br>find_element(by=By.PARTIAL_LINK_TEXT, value='xxx') |
     | by css selector      | find_element_by_css_selector<br>find_element(by=By.CSS_SELECTOR, value='xxx') |

   - 实例演示

     `<a href='https://www.baidu.com/s?wd=NBA'>NBA</a>`

     `element = driver.find_element_by_limk_text('NBA')`

     ---

     `from selenium.webdriver.common.by import By`

     `element = driver.find_element(by=By.LINK_TEXT, value='NBA')`

4. 鼠标动作链介绍

   说明：通过导入 `ActionChains` 类，`from selenium.webdriver import ActionChains`，来实现鼠标动作：单击、双击、拖拽、移动等。

   - 代码演示

     说明：鼠标一些操作，只能在特定网页使用，故此处只演示：鼠标单击和移动鼠标到元素处

     ```python
     from selenium import webdriver
     # 导入 ActionChains类
     from selenium.webdriver import ActionChains
     
     # 打开 Firefox 浏览器
     driver = webdriver.Firefox()
     
     # 打开 url 地址
     url = 'http://www.baidu.com'
     driver.get(url=url)
     
     # 定位页面元素，id为'kw'(搜索框)
     input_1 = driver.find_element_by_id(id_='kw')
     # 输入数据
     input_1.send_keys('NBA')
     
     
     # 定位页面元素，id为'su'(百度一下)
     baidu = driver.find_element_by_id(id_='su')
     # 在此元素处单击
     ActionChains(driver).move_to_element(baidu).click(baidu).perform()
     
     
     # 定位页面元素，class name = 'pf'(设置)
     set_1 = driver.find_element_by_class_name(name='pf')
     # 将鼠标移动到此元素
     ActionChains(driver).move_to_element(set_1).perform()
     ```

   - 伪代码演示

     说明：演示：鼠标双击、鼠标右击、鼠标单击（hold）、将元素 1 拖拽 到元素 2 的位置
     
     ```python
     # 导入 ActionChains 类
     from selenium.webdriver import ActionChains
     
     
     # 鼠标双击
     #
     # 获取元素
     ac_1 = driver.find_element_by_xpath('//element_2')
     Actionchains(driver).move_to_element(ac_1).double_click(ac_1).perform()
     
     # 鼠标右击
     #
     # 获取元素
     ac_2 = driver.find_element_by_class_name('elemnet_2')
     ActionChains(driver).move_to_element(ac_2).context_click(ac_2).perform()
     
     # 鼠标单击（hold）
     #
     # 获取元素
     ac_3 = driver.find_element_by_id('element_3')
     ActionChains(driver).move_to_element(ac_3).click_and_hold(ac_3).perform()
     
     # 将元素 4 拖到元素 5 的位置
     #
     # 获取元素
     ac_4 = driver.find_element_by_id('element_4')
     ac_5 = driver.find_element_by_id('element_5')
     ActionChains(driver).drag_and_drop(ac_4, ac_5).perform()
     ```
   
5. 填充表单

   说明：已知如何像输入框输入内容，如于到 `<select>xxx</select>` 下拉框标签，将如何选择？ 

   - select  下拉框实例

     ```html
     <select name="select_option" id="select-1">
             <option value="">中国</option>
             <option value="0">北京</option>
             <option value="1">上海</option>
             <option value="2">广州</option>
             <option value="3">南京</option>
     </select>
     ```

   - selenium 的 webdriver 的 Select 类可以处理此类情况，伪代码演示

     说明：导入 `from selenium.webdriver.support.ui import Selenium`

     ```python
     # 导入 Select 类
     from selenium.webdriver.support.ui import Select
     
     
     # 定位页面元素
     select = Select(driver.find_element_by_name(select_option))
     # 取值（三种）
     #
     # 第一种（索引：从 0 开始）
     select = select_by_index(1)
     
     # 第二种（value属性）
     select = select_by_value('0')
     
     # 第三种（option标签文本内容）
     select = select_by_visible_text('北京')
     
     # 取消全部选择
     select = deselect_all()
     ```

6. 弹框处理

   说明：__弹框不是页面元素，无法获取元素。它是浏览器的功能__

   - 网页中触发某个事件，页面会出现__弹框__，处理和捕获提示信息方法如下：

     `alert = driver.switch_to_alert()`

7. 页面切换

   说明：如浏览器打开多个窗口，需要进行页面之间的切换

   - 切换方式（1）

     `driver.switch_to.windows('windows_name')`

     解释：windows_name 好像就是 `driver.title`

   - 切换方式（2）

     说明：通过 `driver.window_handles` 获取窗口操作对象

     ```python
     for handle in driver.window_handles:
         driver.switch_to.windows(handle)
     ```

   - 页面前进后退：

     `driver.forward()`

     `driver.back()`

8. 对 Cookie 的操作

   说明：一个页面有时会有多个 cookie 值

   - 获取 cookie 值

     1. `driver.get_cookies()`

        __返回值为：[{}, {}]，列表 里套 字典__

     2. 代码演示

        ```python
        for cookie in driver.get_cookies():
            # cookie 为字典
            print('%s->%s'%(cookie['name'], cookie['value']))
        ```

   - cookie  的删除

     1. 通过名字（by name）

        `driver.delete_cookie('cookie_name')`

     2. 删除所有

        `driver.delete_all_cookies()`

9. 页面等待

   说明：[参考文档](https://selenium-python-zh.readthedocs.io/en/latest/getting-started.html)

   - Webdriver 的说明
     1. 现在的大多数的 web 应用程序是使用 Ajax 技术，当一个页面被加载到浏览器，该页面的元素可以在不同时间点被加载。这使得定位元素变得困难，如果元素不在页面中，会抛出 `ElementNotVisibleException` 异常。使用 waits，我们可以决绝这个问题。waits 提供了一些操作之间的时间间隔（主要是定位元素或针对该元素的任何操作）
     2. Selenium Webdriver 提供两种类型的 waits（隐式、显示）。显示等待会让 Webdriver等待一个满足一定条件以后再进一步的执行。而隐式等待让 Webdriver 等待一定的时间后在开始查找元素
   
   - 显示等待
   
     说明:
   
     > 显示等待是你在代码中定义的等待一定条件发生后再进一步执行你的代码。__最糟糕的案例是使用 `time.sleep()`__，他将条件设置为等待一个确切的时间段。这里有一些方便的方法让你只等待你需要等待的时间。__WebDriverWait 结合 ExceptedCondition 实现的一种方式__
   
     1. 代码演示
   
        ```python
        from selenium import webdriver
        from selenium.webdriver.common.by import By
        # WebDriverWait 库，负责循环等待
        from selenium.webdriver.support.ui import WebDriverWait
        # expection_conditions 类，等到条件
        from selenium.webdriver.support import expected_conditions as EC
        
        
        driver = webdriver.Firefox()
        driver.get(url='url')
        
        try:
            element = WebDriverWait(driver, 10).until(
                EC.presence_of_element_located(By.ID, 'myelement'))
        finally:
            driver.quit()
        ```
   
        解释：在抛出  TimeoutExpection 异常之前，将等待 10 秒或者 10 秒内发现了查找的元素。WebDriverWait 默认情况下会 0.5 秒调用一次 ExpectionCondition 直到结果成功返回。ExpextionCondtion 成功返回的结果是一个 __布尔类型的 Ture，或是不为 null 的返回值__(总感觉这就话有点怪)<br>文档例子：` element = WebDriverWait(driver, 10).until(lambda x: x.find_element_by_id("someId"))` __返回值为页面元素__
   
     2. 内置等待条件
   
        说明：自动化的 web 浏览器中一些常见的预期条件，方便调用（不用去编写 expection_condition类）
   
        ```tex
        title_is
        title_contains
        presence_of_element_located
        visibility_of_element_located
        visibility_of
        presence_of_all_elements_located
        text_to_be_present_in_element
        text_to_be_present_in_element_value
        frame_to_be_available_and_switch_to_it
        invisibility_of_element_located
        element_to_be_clickable – it is Displayed and Enabled.
        staleness_of
        element_to_be_selected
        element_located_to_be_selected
        element_selection_state_to_be
        element_located_selection_state_to_be
        alert_is_present
        ```
   
   - 隐式等待
   
     说明：
   
     > 如果某些元素不是立即可用的，隐式等待是告诉 WebDriver 去等待一时间后去查找元素。默认等待时间是 0 秒，以但设置该值，隐式等待是设置该 WebDriver 的实例的声明周期
   
     1. 代码演示
   
        ```python
        from selenium import webdriver
        
        
        driver = webdriver.Firefox()
        
        # 等待 10 秒，隐式（implicitly）
        driver.implicitly_wait(10) 
        driver.get(url='url')
        element = driver.find_element_by_id("myelement")
        ```

### 模拟登录

说明：使用 `selenium` 模块的 `webdriver` API，实现对网站的模拟登录（因为反爬虫技术，没有实现模拟登录）

1. 代码演示

   说明：代码模块正常，但由于 __反爬虫__ ，所以暂时一些 __好__ 网站，模拟登录不上去。

   - 代码

     ```python
     from selenium import webdriver
     from selenium.webdriver.common.by import By
     from selenium.webdriver.common.keys import Keys
     from selenium.webdriver import ActionChains
     
     
     driver = webdriver.Firefox()
     
     driver.get(url='https:......')
     driver.implicitly_wait(5)
     
     # 选择 密码登录 方式
     # 
     # 使用 by link text 选取元素：'unable to locate element:密码登录'
     # pass_word_element = driver.find_element_by_link_text('密码登录')
     
     # 第一步：
     # 
     # 定位页面元素：'密码登录'
     pass_word_log_element = driver.find_element_by_xpath('/html/body/div[1]/div[1]/ul[1]/li[2]')
     # 鼠标单击
     ActionChains(driver).move_to_element(pass_word_log_element).click(pass_word_log_element).perform()
     
     # 第二步：
     # 
     # 定位页面元素：'账号输入框'
     username_element = driver.find_element_by_xpath('/html/body/div[1]/div[2]/div[1]/div[3]/div/input')
     # 输入账号
     username_element.send_keys('15702423221')
     
     # 定位页面元素：'密码'
     password_element = driver.find_element_by_id('password')
     password_element.send_keys('sHEN0077')
     
     # 第三步：
     # 
     # 定位页面元素：'登录\注册'
     log_element = driver.find_element_by_xpath('/html/body/div[1]/div[2]/div[1]/div[5]/a')
     # 鼠标单击
     ActionChains(driver).move_to_element(log_element).click(log_element).perform()
     
     
     # 保存页面截图
     flag = driver.save_screenshot('自动化检测/4.png')
     print(flag)
     ```

2. 爬虫角度解释

   - zhihu

     网络连接报 `10001` 错误码，但是`webdriver` 的方法，如：页面元素定位、鼠标单击、输入框输入等正常使用

   - douban

     没有尝试到网络连接部分，页面元素定位错误，如：by xpath、by class、by name等会发生页面元素定位错误。

     注意：by xpath 页面元素定位，会定位到 __其他__ 的页面元素。

### 执行 JavaScript 语句

说明：javascript 不怎么了解，执行 javascript 需要读取网页源码，固不是本人学习内容，不在此说明

1. 执行 javascript 语句代码

   - 主要代码演示

     ```python
     from selenium import webdriver
     
     driver = webdriver.Firefox()
     
     # javascript 语句
     js = 'var q = document.getElementById()'
     
     # 执行
     driver.execute_script(js)
     
     # 在执行后续代码
     ```

### 机器视觉

说明：很有意思的一个东西

1. ORC 库概述
   - Python 非常出色的语言，虽然运行效率一般，但第三方库非常丰富，所以适合学术代码（机器学习、深度学习）
   - Tesseract 是 ORC 的一个库，是目前公认最优秀、精度最高的开源库（识别任何字体）
2. Tessertact 的安装
   - windows 下载安装文件 [地址](https://github.com/tesseract-ocr/)
   - LInux 通过 `apt-get $sudo apt-get tesseract-ocr`
3. **使用**
   - 设置信息环境变量 `TESSERACT_PREFIX` ，存放 Tesseract 的训数据 `tessdata` 在 tesseract 目录下
   - 使用命令行 `tesseract` 执行，不适用 `import` 导入
4. 安装 pytesseract
   - 可以通过 `pip install pytesseract` 安装
   - 使用 `import` 导入 `pytesser`
   - `pytesseract_image_to_string(image)` 返回字符串

##  scrapy 模块

说明：[官方文档](https://docs.scrapy.org/en/latest/intro/tutorial.html)  [中文文档](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-11.html) <br>

> Scrapy 是一个 Python 编写的 __开源网络爬虫框架__ 。它是一个被设计用于爬取网络数据、提取结构性数据的程序框架。该框架主要由 *Scrapinghub 公司* 进行维护

### Scrapy 介绍

1. 功能介绍
   - Scrapy 是用 Python 实现的一个为爬取网站数据、提取结构性数据而编写的应用框架
   - Scrapy 常应用在包括数据挖掘、信息处理或存储历史数据等一系列的程序中
   - 通常我们可以使用简单的通过 Scrapy 框架实现一个爬虫，榨取指定网页的内容或图片

### Scrapy 的模块介绍

1. 数据流向

   - 如图

     ![Scrapy数据流向图](git_picture/Scrapy数据流向图.png)

2. 对 Scrapy 的各个模块的 介绍

   - Scrapy Engine（引擎）

     负责 Spider、ItemPipeline、Downloader、Scheduler 中间的通讯、信号、数据传递等。

   - Scheduler（调度器）

     它负责接受引擎发送过来的 Request 请求，按照一定方式进行整理排列

   - Downloader（下载器）

     负责下载 Scrapy Engine（引擎）发送的所有 Request 请求，并将其获取的 Response 交给 Scrapy Engine（引擎）。由引擎交给 Spider 来处理

   - Spider（爬虫）

     它负责处理所有 Response 从中分析提取数据，获取 Item 字段需要数据，并将需要跟进的 URL 提交给引擎，再次进入 Scheduler（调度器）

   - Item Pipline（管道）

     它负责处理 Spider 中获取的 Item ，并进行后期处理（详细分析、过滤、储存等）的地方

   - Downloader Middlewares（下载中间插件）

     可以当作自定义扩展下载功能的组件

   - Spider Middlewares（Spider 中间插件）

     可以理解为是一个可以扩展和操作引擎的 Spider 中间通信的功能组件（比如进入 Spider 的 Responses 和从 Spider 出去的 Request）

### Scrapy 的工作流程

1. 流程

   - 引擎

     你好！！！Spider，你要处理哪个网站？

   - Spider

     引擎，我要处理 `xxx.com`

   - 引擎

     把你的第一个要处理的 URL 给我

   - Spider

     给你，第一个 URL 是 `xxx.com`

   - 引擎

     你好！！！调度器，我这里有 Requset 请求你帮我排序入队

   - 调度器

     给你，这是我处理好的 Request

   - 引擎

     你好！！！下载器，你按照用户设置的 “下载中间插件” 下载 Request 请求

   - 下载器

     好的，给你，这是下载完成的东西（如果下载失败，引擎会告诉调度器，这个 Request 下载失败，你记录一下，我们一会再下载）

   - 引擎

     你好！！！，spider（第 2 次见面），这是下载好的东西，并且已经按照用户的“下载中间插件”处理了，你自己在处理一下（这个 Request 默认是交给 `def parse()` 处理） 

   - Spider

     （处理完数据之后对于需要跟进的 URL），你好！！！引擎（第 3 次见面），这里有两个结果，这是需要跟进的 URL，这是获取的 Item 数据

   - 引擎

     你好！！！管道，这有个 Item 帮我处理一下

     你好！！！调度器，这是需要跟进的 URL 帮我处理一下（循环第 5 步开始）

   - 管道

     好的，现在就做

2. 结束条件

   - 只有当 __调度器__ 中不存在任何 Request 了，整个程序才结束
   - 也就是说，对于下载失败的 URL，Scrapy 也会重新下载

3. 制作 Scrapy 爬虫（一共 4 步）

   - 新建项目（scrapy startproject xxx）：新建体格爬虫项目
   - 明确目标（编写 item.py）：明确抓取目标
   - 制作爬虫（spider/xxscrapy.py）：制作爬虫开始爬虫网页
   - 存储内容（piplines.py）：设计管道存储爬取内容

### Scrapy 安装介绍

说明：说是不要再全局中安装 Scrapy ，在局部安装，防止与一些系统工具和脚本冲突。<br> [局部安装网址](https://virtualenv.pypa.io/en/stable/installation/)

1. windows 安装

   - `pip install Scrapy`

2. Ubuntu 安装

   说明：安装依赖是因为 Scrapy 的（lxm、twisted 和 pyOpenSSL）是最新的兼容 Ubuntu 各个版本。不要使用 Ubuntu 提供的 Scrapy 安装包，所以需要安装依赖

   - `sudo apt-get install python-dev python-pip libxml2-dev libxslt1-dev zlib1g-dev libffi-dev libssl-dev`

   - `sudo pip install scrapy`    

### 入门案例

说明：<br> 目标 <br> 创建一个 Scrapy 项目 <br> 定义提取的结构化数据（Item） <br> 编写爬取的网站的 Spider 并提取处结构化数据（Item） <br> 编写 Item Piplines 来存储提取到的 Item （及结构化数据）

1. 新建项目

   说明：使用 Pychram 使用虚拟环境，每个项目独立，固新建项目之前，安装 Scrapy

   - 创建新项目（命令行输入）

     `scarpy startprojrct myspider`  myspider 为项目名称

   - 项目目录结构如图（spiderbaidu 为项目名称）

     ![spider文件结构](git_picture/spider文件结构.png)

2. settings.py 文件

   说明：很重要，项目的设置文件，都在其中设置

   - 管道文件设置
   - 等等......

3. 项目文件介绍

   - __scarpy.cfg__

     项目配置文件

   - __spiderBaidu__

     项目的 Python 模块，将会从这里引用代码

   - __spiderBaidu / items.py__

     项目目标文件

   - __spiderBaidu / piplines.py__

     项目管道文件

   - __spiderBaidu / settings.py__

     项目配置文件

   - __spiderBaidu / spider__

     存储爬虫代码目录

4. 明确目标

   说明：抓取 `http://www.itcast.cn/channel/teacher.shtml` 所有讲师的姓名、职称、个人介绍

   - 新建 spiderItcast 项目 `scrapy startproject spiderItcast` 命令行输入

     ![spiderItcast文件结构](git_picture/spiderItcast文件结构.png)

5. 制作 Item

   说明：item 定义结构化数据字段，用来保存爬虫的数据，有点像 dict（字典），但是提供了一些额外的保护减少错误。<br>打开 `spiderItcast/spiderItcast` 的 item.py 文件

   - 代码演示

     ```python
     import scrapy
     
     
     class SpideritcastItem(scrapy.Item):
         # define the fields for your item here like:
         # name = scrapy.Field()
     
         # 讲师 名字
         name = scrapy.Field()
     
         # 讲师 职称
         title = scrapy.Field()
     
         # 讲师个人 介绍
         info = scrapy.Field()
     ```

     解释：继承 scrapy.Item 类，使用 `scrapy.Field()` 定义保存数据

6. 制作爬虫

   说明：在当前目录 `SpriderProject/spiderItcast` 输入命令 `scrapy genspider Itcast www.itcast.cn` ，将在spider文件夹下创建 Itcast.py 文件用于爬虫，

   - 默认代码

     ```python
     import scrapy
     
     
     class ItcastSpider(scrapy.Spider):
         name = 'Itcast'
         allowed_domains = ['www.itcast.cn']
         start_urls = ['http://www.itcast.cn/']
     
         def parse(self, response):
             pass
     ```

     解释：继承 `scrapy.Spider` 类，强制三个属性，和一个方法

   - 属性解释

     1. name

        爬虫识别名，必须唯一，在不同的爬虫定义不同的名字

     2. allowde_domains

        搜索的域名范围，爬虫约束区域，规定爬虫只爬取这个域名下的网页，不存在 URL 会被忽略

     3. start_urls

        爬取的 URL 元组 / 列表。爬虫从这里开始抓取数据，所以，第一次下载的数据将会从这些 URLS 开始。其他子 URL 将会从这些起始 URL 中继承性生成

     4. `parse(self, response)`

        解析方法：每个 URL 下载完成后将会被调用，调用的时候传入从每个 URL 传回的 Response 对象作为唯一参数。

        作用：解析网页数据 `response.body`（二进制网页数据），提取结构化数据（item）<br> 生成下一页的 URL请求

   - 构建爬虫代码

     1. 将爬取的数据全部存入文件中，代码演示

        说明：response 为响应文件，`response.body`  __为二进制（byte）__ ，`response.text` 为 __字符串（str）__

        ```python
        # -*- coding: utf-8 -*-
        import scrapy
        
        
        class BaiduSpider(scrapy.Spider):
            name = 'baidu'
            allowed_domains = ['www.itcast.com']
            start_urls = ['http://www.itcast.com/']
        
            def parse(self, response):
                print(type(response))
                print(response)
                with open('content.html', 'wb') as f:
                    f.write(response.body)
                    
        运行结果
        <class 'scrapy.http.response.html.HtmlResponse'>
        <200 http://www.itcast.com/>
        ```

     2. 爬取网页数据（spider 文件夹下的 Itcast.py），并进行提取，使用 item.py 文件中的属性 __name、title、info__

        说明：导入 item ，根节点为项目文件夹（spiderItcast）,但是不用写，直接写二级的 spidertcast

        ```python
        # -*- coding: utf-8 -*-
        import scrapy
        # 导入 item.py 文件
        from spiderItcast.items import SpideritcastItem
        
        
        class ItcastSpider(scrapy.Spider):
            name = 'Itcast'
            allowed_domains = ['www.itcast.cn']
            start_urls = ['http://www.itcast.cn/channel/teacher.shtml#ajavaee']
        
            def parse(self, response):
        
                # 匹配规则
                # 根节点 /html/body/div[1]/div[5]/div[2]/div[3]/ul/li/div[2]
                # name 节点 .h3
                # title 节点 .h4
                # info 节点 .p
        
                # 通过 scarpy 自带的 xpath 提取数据
                # xpath 返回值为 列表
        
                # 根节点列表
                teacher_list = response.xpath('/html/body/div[1]/div[5]/div[2]/div[3]/ul/li/div[2]')
        
                # 定义一个列表 存放信息
                teacher_item = []
        
                for each in teacher_list:
        
                    # 遍历 匹配信息 / xpath 返回值为列表
                    # name 为 xpath 对象（<Selector xpath='./h4/text()' data='高级讲师'>）
                    # 转换为 unicode 字符串：extract() 方法
        
                    name = each.xpath('./h3/text()').extract()
                    title = each.xpath('./h4/text()').extract()
                    info = each.xpath('./p/text()').extract()
        
                    # print(name)
                    # print(title)
                    # print(info)
        
                    # 创建一个 SpideritemItcast 对象 将数据封装进去
                    item = SpideritcastItem()
        
                    item['name'] = name[0]
                    item['title'] = title[0]
                    item['info'] = info[0]
        
                    # 将数据 存放列表中
                    teacher_item.append(item)
        	    
                # 将数据返回有用
                return teacher_item
        ```

   - 启动爬虫

     说明：在项目目录中执行， scrapy 保存信息最简单有 4 中，使用 `-o` 表示输出文件格式 __.json、.jl、.xml、.csv__ 
     
     __.jl__ 好像就是 __.json__ 没有了最外已成的 `[]` 一行显示一条信息 <br> __存入文件格式 `.csv` 有错误，原因不晓得__ 
     
     1.  `scrapy crawl Itcast` 正常执行
     2. `scrapy crawl Itcast -o conten.xml`
     
   - 结束信息

     运行之后，如果打印的日志出现 `[scrapy] INFO: Spider closed (finished)`，代表执行完成。 之后当前文件夹中就出现了一个 content.xxx 文件，里面就是爬取的网页后后提取的信息。

7. 扩展 `yield` 语法

   说明：yield 关键字，来实现函数作为 __生成器__ 使用，爬虫会使用 `yield` 关键字，联合管道一起使用

   - `yield` 与 `return`  相似，只是每次使用 `next(f)` 

     ```python
     def func():
         for i in range(5):
             yield i
             
     f = func()
     print(next(f))
     
     运行结果
     0
     ```

8. Pychram 使用脚本启动爬虫程序

   - Python 模块 `from scrapy import cmdline`

     ```python
     from scrapy import cmdline
     
     
     cmdline.execute('scrapy crawl Itcast -o content-1.xml'.split())
     ```

9.  Piplines 的使用（管道）

   - 修改 spider 文件夹下的 Itcast.py 文件

     说明：注释掉 存放整个数据的列表（teacher_item）、操作语句，和 return 关键字，增加 `yield` 关键字的使用

     ```python
     
             # 定义一个列表 存放信息
             # teacher_item = []
     
             for each in teacher_list:
     
                 # 遍历 匹配信息 / xpath 返回值为列表
                 # name 为 xpath 对象（<Selector xpath='./h4/text()' data='高级讲师'>）
                 # 转换为 unicode 字符串：extract() 方法
     
                 name = each.xpath('./h3/text()').extract()
                 title = each.xpath('./h4/text()').extract()
                 info = each.xpath('./p/text()').extract()
     
                 # print(name)
                 # print(title)
                 # print(info)
     
                 # 创建一个 SpideritemItcast 对象 将数据封装进去
                 item = SpideritcastItem()
     
                 item['name'] = name[0]
                 item['title'] = title[0]
                 item['info'] = info[0]
     
                 # 将数据 存放列表中
                 # teacher_item.append(item)
                 
                 # 使用 yield 关键字
                 yield item
     
             # return teacher_item
     ```

   - 管道文件（piplines.py）

     ```python
     import json
     
     class SpideritcastPipeline(object):
         # __init__ 初始化函数 可写、可不写
         # def __init__(self):
         #     pass
     
         # 函数 process_item() 必须写
         def process_item(self, item, spider):
             # item 为 unicde 编码字符串
             #
             # 将其转换为 字典（dict()）
             #
             # 转换为 python 字典 转换为 json
             #
             # 写入 json 文件
     
             with open('../Data_doc/data.json', 'a', encoding='utf-8') as f:
                 f.write(json.dumps(dict(item), ensure_ascii=False) + '\n')
     
             return item
     ```

### Scrapy Shell  调试代码

说明：[官方文档](https://scrapy-chs.readthedocs.io/zh_CN/1.0/topics/shell.html) \ [1.7.3 官方文档](https://docs.scrapy.org/en/latest/topics/shell.html)

1. Scrapy Shell 介绍

   - Scrapy 终端（Scrapy shell）

     > scrapy 终端（scrapy shell）
     >
     > Scrapy  终端是一个一个交互式终端，供你在未启动 spider  的情况下尝试及调试你的爬取代码，其本意使用来测试提取数据代码，不过你可以将其作为正常的 Python 终端，在上面测试任何 Python 代码。<br> <br> 该终端是用来测试 __XPath、CSS 表达式__，查看它们的工作方式及从爬取的网页中提取数据，在编写的 spider 时，该终端提供了交互性测试你的表达式代码的功能，免去了每次修改后运行 spider 的麻烦。<br> <br> 一旦熟悉了 Scrapy 终端后，你会发现其在开发和调试 spider 时发挥的巨大作用 <br> <br> 如果你安装了 IPython ，Scrapy 终端将使用 IPython（代替标准 Python 终端）。IPython 终端与其相比功能更为强大 <br> 

2. 启动 Scrapy shell

   - 进入项目根目录，使用 shell 来启动 Scrapy 终端

     `scarpy shell url`

     `scrapy shell "https://tieba.baidu.com/f?ie=utf-8&kw=美女&fr=search"` __URL 一定要双引号才可以__

     解释：URL 是要爬取的网页地址

   - 运行结构如下：

     ```python
     2019-09-29 13:54:48 [scrapy.extensions.telnet] INFO: Telnet console listening on 127.0.0.1:6023
     2019-09-29 13:54:48 [scrapy.core.engine] INFO: Spider opened
     2019-09-29 13:54:49 [scrapy.core.engine] DEBUG: Crawled (200) <GET http://www.itcast.cn/channel/teacher.shtml#ajavaee> (referer: None)
     [s] Available Scrapy objects:
     [s]   scrapy     scrapy module (contains scrapy.Request, scrapy.Selector, etc)
     [s]   crawler    <scrapy.crawler.Crawler object at 0x0000026312494940>
     [s]   item       {}
     [s]   request    <GET http://www.itcast.cn/channel/teacher.shtml#ajavaee>
     [s]   response   <200 http://www.itcast.cn/channel/teacher.shtml>
     [s]   settings   <scrapy.settings.Settings object at 0x0000026312494860>
     [s]   spider     <ItcastSpider 'Itcast' at 0x263127af2b0>
     [s] Useful shortcuts:
     [s]   fetch(url[, redirect=True]) Fetch URL and update local objects (by default, redirects are followed)
     [s]   fetch(req)                  Fetch a scrapy.Request and update local objects
     [s]   shelp()           Shell help (print this help)
     [s]   view(response)    View response in a browser
     In [1]: 
     ```

3. 使用终端

   说明：Scrapy 终端仅仅是一个普通的 Python 终端（或 IPython）。其提供了一些额外的快捷方式

   - `shelp()`

     打印可用对象及快捷命令的帮助列表

   - `fetch(request_or_to)`

     根据给定的请求(request)或URL获取一个新的 response，并更新相关的对象

   - `view(response)`

     在本机的浏览器打开给定的response。 其会在 response 的 body 中添加一个 [<base> 标签](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/base) ，使得外部链接(例如图片及css)能正确显示。 *注意，该操作会在本地创建一个临时文件，且该文件不会被自动删除*。

4. 可操作的 Scrapy 对象

   说明：Scrapy  终端会根据下载的页面会自动创建一些方便使用对象，例如：`Response` 对象及 `Selector` 对象（对 HTML 及 XML 内容）

   - 可操作对象

     ```python
     [s] Available Scrapy objects:
     [s]   scrapy     scrapy module (contains scrapy.Request, scrapy.Selector, etc)
     [s]   crawler    <scrapy.crawler.Crawler object at 0x0000026312494940>
     [s]   item       {}
     [s]   request    <GET http://www.itcast.cn/channel/teacher.shtml#ajavaee>
     [s]   response   <200 http://www.itcast.cn/channel/teacher.shtml>
     [s]   settings   <scrapy.settings.Settings object at 0x0000026312494860>
     [s]   spider     <ItcastSpider 'Itcast' at 0x263127af2b0>
     ```

   - 对象解释

     1. __crawl__

        当前的 crawl 对象（资料上都是这么说的，但是我觉的的好好解释一下，再续......）

     2. __request__

         Request 最后获取页面的对象。您可以 `replace()` 使用 `fetch(url)` 快捷方式修改此请求，也可以获取新请求（不离开 shell）

     3. __response__

        最后获取到的页面的 Response 对象

     4. __settings__

        当前 Scrapy 设置

     5. __spider__

        用于处理 URL 的 spider，~~Spider 没有找到当前的 URL 的 Spider ，为一个 Spider 对象~~ (真心读不懂)

5. Response 对象介绍

   说明：当 shell 载入后，将得到一个包含 Response 数据本地 response 变量
   
   - 输入 `response.headers`
   
     说明：可以看到 response __报头__
   
     ```python
     In [1]: response.headers
     Out[1]: 
     {b'Server': b'Tengine',
      b'Content-Type': b'text/html',
      b'Date': b'Mon, 30 Sep 2019 05:17:59 GMT',
      b'Vary': b'Accept-Encoding',
      b'Ali-Swift-Global-Savetime': b'1569820679',
      b'Via': b'cache33.l2cn1832[103,200-0,M], cache53.l2cn1832[104,0], kunlun2.cn2466[175,200-0,M], kunlun7.cn2466[177,0]',
      b'X-Cache': b'MISS TCP_MISS dirn:-2:-2',
      b'X-Swift-Savetime': b'Mon, 30 Sep 2019 05:17:59 GMT',
      b'X-Swift-Cachetime': b'0',
      b'Timing-Allow-Origin': b'*',
      b'Eagleid': b'7ce1a71b15698206790542179e'}
     ```
   
   - 输入 `response.body` \ `response.text`
   
     说明：看以看到 response 报体，body 属性为二进制、text 属性为字符串
   
     就是网页源码
   
6. Selector 选择器

   说明：输入 `response.selector` 时，将会获得一个 response 初始化的类 selector 的对象，此时可以通过 `response.selector.xpath()` \ `response.selector.css()` 来对 response 进行查询

   - `xpath()`

     传入 xpath 表达式，返回该表达式所有对应节点的 selector list 列表

   - `extract()`

     序列化该节点为 Unicode 字符串并返回 list __将 selector 对象，转换为 Unicode字符串__

   - `css()`

     传入 css 表达式，返回该表达式所对应的所有节点的 selecor list 列表（语法同 Beautiful Soup4）

   - `re()`

     传入正则表达式，返回 Unicode 字符串 list 列表

7. 待续......

   说明：现阶段，只是用它来进行验证 __数据提取的是否正确__ 

### Item

说明：定义需要提取的字段，继承 `scrapy.Item`

1. 作用

   - 定义要提取的字符串，如同字典（dict）相同

   - 代码演示

     ```python
     import scrapy
     
     
     class SpideritcastItem(scrapy.Item):
         # define the fields for your item here like:
         # name = scrapy.Field()
     
         # 讲师 名字
         name = scrapy.Field()
     
         # 讲师 职称
         title = scrapy.Field()
     
         # 讲师个人 介绍
         info = scrapy.Field()
     ```

### Item Pipeline

说明：在 Item 中定义要提取的数据字段，即 Item 字段会在 spider 中提取，将会被传入Item Pipeline，Item Pipeline 组件按照定义的顺序处理 Item 数据字段。

1. Item Pipeline 作用

   - 验证爬虫数据（检查 Item 包含哪些字段，如 name 字段）
   - 查重（丢弃）
   - 爬虫结果保存文件中、数据库中

2. 编写 Item Pipeline

   说明：其中 `process_item()` 方法必须实现

   - 启用 Item Pipeline 组件，必须将他添加到 setting.py 文件中 ITEM_PIPEINES 配置，数字为优先级（越小级别越高）

     ```python
     # Configure item pipelines
     # See https://docs.scrapy.org/en/latest/topics/item-pipeline.html
     ITEM_PIPELINES = {
        'spiderItcast.pipelines.SpideritcastPipeline': 300,
     }
     ```

   - 编写 Item Pipeline

     ```python
     import something 
     
     class SpidersomethingPipeline(obeject):
     
         def __init__(self):
             # 可选实现，做参数初始化
             pass
         
         def process_item(self, item, spider):
             # item (Item 对象)，被爬取的 item 数据
             # spider（spider 对象），爬取 item 的 spider
             # 这个方法必须实现，每个 Item Pipeline 组件都需要调用该方法
             # 这个方法必须返回一个 Item 对象，被丢弃的 item 将不会将不会被 Pipeline 组件所处理
             return item
     
         def open_spider(self, spider):
             # spider（spider 对象），被开启的 spider
             # 可选实现，当 spider 被开启时，这个方法被调用
             pass
     
         def close_spider(self):
             # spdeu（spdier 对象），被关闭的 spider
             # 可选实现，当 spider 被关闭时，这个方法被调用
             pass
     ```
   
3. 下载图片（自定义 ImagePipeline）

   说明：下载图片将继承 `ImagesPipeline`

   - 导包说明

     1. `from scrapy.utlis.project import get_project_settings`

        可以直接获取 `setting.py` 文件变量

     2. `from scrapy.piplines.images import ImagesPipeline`

        下载图片要继承的类

     3. `from scrapy import Request`

   - 在 scarpy 的模块中有 images.py 的两个函数最为关键，需要重写

     1. `def get_media_requests(self, item, info)`

        ```python
        def get_media_requests(self, item, info):
                return [Request(x) for x in item.get(self.images_urls_field, [])]
        ```

     2. `def item_completed(self, results, item, info):`

        ```python
        def item_completed(self, results, item, info):
            if isinstance(item, dict) or self.images_result_field in item.fields:
                item[self.images_result_field] = [x for ok, x in results if ok]
            return item
        ```

   - 重写代码

     ```python
     from scrapy.utlis.project import get_project_settings
     from scarpy import Request
     from scrapy.pipeline.images import ImagesPipeline
     
     import os
     
     
     class ImagesPipeline(ImagesPipeline):
         
         # IMAGES_PAYH 在 setting 中设置的图片存储 路径
         IMAGES_PATH = get_project_settings().get('IMAGES_PATH')
     
         # 发送请求图片的函数
         def get_media_requests(self, item, info):
             # 在spider 中获取的图片 统一资源定位符
             image_url = item['image_url']
             
             # 发送 下载图片请求
             yield Request(image_url)
     
         def item_completed(self, results, item, info):
             
             # 列表生成式
             # 
             # result 结构表示为 二元组 (succsee, image_info_or_failure)
             # 
             # 第一个元素为图片是否下载成功
             # 第二个元素为字典，并且取 path 值
             # 
             # 感觉 image 就是 图片的响应数据
             image = [x['path'] for ok, x in results if ok]
     
             # 将图片 重新命名
             os.rename(self.IMAGES_PATH + '/' + image[0], self.IMAGES_PATH + '/' + item['name'] + '.jpg')
     
             return item
     ```

### Spider

说明：Spider 类定义了如何爬取某个网站，包括了爬取动作（是否跟进连接），以及如何如何从网页数据中提取结构化数据（Item）。<br> 总结：Spider 就是爬取指定 URL 动作以及如何提取数据的地方

1. Class scrapy.Spider 是最基本的类，所有编写爬虫类必须继承此类

   - 主要函数调用顺序

     1. `__init__()` 

        初始化爬虫名和 start_url 列表名

     2. ` start_requests()` 调用 `make_requests_from_url()`

        生成 Request 对象交给 Scrapy 下载并返回 response

     3. `parse()` 

        解析 response ，并返回 Item 或 新的 Request（需指定回调函数）。Item 传递给 Item Pipeline 持久化，而 Request 交由 Scrapy 下载，并由指定的回调函数处理（默认 prase() 函数），一直循环处理完所有 URL

2. 解释 Spider 类的属性和方法

   说明：__可以查看 `scrapy.Spider` 基础类__

   - name

     说明：定义 spider 名称字符串

     如 spider 爬取 `www.xx.com`，该 spider 通常会被命名为 `spider`

   - allow_domains

     说明：包含 spider 允许爬取的域名（domans）的列表，可选

     如 `www.xx.com`

   - start_urls

     说明：初始 URL 元组、列表，当没有定制的 URL 时，spider 将从该列表开始爬取

     如 `['http://www.xx.cn/xx/xx.html']`

   - start_requsets(self)

     说明：该方法必须返回一个可以迭代对象（iterable），该对象包含了 spider 用于爬取（默认实现使用时 start_urls 的 url）的第一个 Request 的 Response，提交方式为 GET

     当 spider 启动爬取，该方法被调用，读取 start_urls 内的 URL，为每个 URL 生成一个 Request 对象，交给 scrapy 下载并返回 Response，该方法仅调用一次

   - parse(self, response)

     说明：当请求 URL 返回页面没有指定回调函数时，默认的 Request 对象回调函数，用来处理返回的 response，以及生成 Item 或者 Requset 对象

   - log(self, message[, level, component])

     说明：使用scrapy.log.msg() 方法记录（log）message。

3. 爬取多个 URL （一次爬取多个 URL）

   说明：加入爬取的页面不止一个，而是多个有规律的 URL

   - 使用 `scrapy.Request(url, callback=self.parse)`

     callback 回调函数为 self.parse

### CrawlSpiders（log 日志信息）

说明：CrawlSpider 是 Spider 的派生类，Spider 的设计原理则只是爬取 start_url 列表中的网页，而 CrawlSpider 类定义了一些规则（rule）来提供跟进 link 的方便机制，从爬取网页获取 link 并继续爬取的工作更适合。<br> __CrawlSpider 的 start_urls 是不会做处理的，它只是为了获取深度 Link，start_urls 的信息是后面获取的深度 Link  __

1. 创建 CrawlSpider 模块的快捷方式

   - `scrapt genspider -t crawl xxx xxx.com`

     `scrapy genspider -t crawl tencent tencent.com`

   - crawlSpider 默认代码

     说明：不能重写 `def parse()` 函数，需要另写函数处理默认函数 `defparse_item()`

     ```python
     import scrapy
     from scrapy.linkextractors import LinkExtractor
     from scrapy.spiders import CrawlSpider, Rule
     
     
     class TencentSpider(CrawlSpider):
         name = 'tencent'
         allowed_domains = ['tencent.com']
         start_urls = ['http://tencent.com/']
     
         rules = (
             Rule(LinkExtractor(allow=r'Items/'), callback='parse_item', follow=True),
         )
     
         def parse_item(self, response):
             item = {}
             #item['domain_id'] = response.xpath('//input[@id="sid"]/@value').get()
             #item['name'] = response.xpath('//div[@id="name"]').get()
             #item['description'] = response.xpath('//div[@id="description"]').get()
             return item
     ```

2. CrawlSpider 和 Spider 对比

   说明：CrawlSpider 继承于 Spider 类，除继承属性（name \ allowed_domains \ start_urls），还有一些新的属性、方法。

   - 模块 scarpy.linkextractors

     导入模块：`from scrapy.linkextractors import LinkExtractor`

     说明：linkextractors 的目的很简答：提取连接

     linkextractors 有唯一的公共方法 `extract_links()` ，它接受一个 Response 对象，并返回一个 scrapy.link 对象

3. LinkExtractor 介绍

   说明：LinkExtractor 需要实例化一次，并且 extract_link 方法会根据不同的 Response 多次调用提取连接

   - LinkExtractor 参数

     ```tex
     def __init__(
         self,
         allow=(),
         deny=(),
         allow_domains=(),
         deny_domains=(),
         restrict_xpaths=(),
         tags=('a', 'area'),
         attrs=('href',),
         canonicalize=False,
         unique=True,
         process_value=None,
         deny_extensions=None,
         restrict_css=(),
         strip=True, restrict_text=None
     )
     ```

   - 主要参数介绍

     说明：[参考地址](https://www.jianshu.com/p/0775a4df1fe4)

     1. allow:

        满足 ‘（）’ 中正则表达式或正则表达式列表的值会被提取，如果为空，则全部匹配（一般没人用空的）

     2. deny:

        与 allow 相反，匹配正则表达式或正则表达式列表的不会被提取

     3. allow_domains

        域名或域名列表，该域名下的连接会被提取

     4. deny_domains

        与 allow_domains 相反

     5. restrict_xpaths:

        xpath 或 xpath 的列表，符合 xpath 的列表会被提取

     6. restrict_css

        符合 css 的

     7. tags \ attrs

        tag 或 tag 的 list，提取指定标签中的连接，默认 ('a', 'area')

     8. process_value

        回调函数，该函数会对每一个连接进行处理，回调函数要么返回一个处理后的连接，要么返回 None 表示忽略不做处理

4. rules 介绍

   说明：rules 中包含一个或者多个 Rule 对象，每一个 Rule 对爬取的网站定义了特定操作，如果多个 rule 匹配相同的连接，则会根据规则在集合中被定义的顺序，第一个会被使用。

   - Rule 参数

     ```tex
     def __init__(
         self,
         link_extractor,
         callback=None,
         cb_kwargs=None,
         follow=None,
         process_links=None,
         process_request=None
     )
     ```
     
   - 代码实例
   
     ```python
       rules = (
             Rule(LinkExtractor(allow=r'Items/'), callback='parse_item', follow=True),
         )
     ```
   
   - 主要参数介绍
   
     说明：Rules 对象可以有多个匹配规则
   
     1. link_extractor
   
        LinkExtractor 对象，用于定义需要提取的 URL
   
     2. callback
   
        爬取 URL 的回调函数，该函数接受 Response 对象，并返回 Item、Response() 或它们的子类 <br> __不要使用回调函数，CrawlSpider 使用 parse() 方法实现其逻辑__ （不是很明白）
   
     3. cb_kwargs
   
        字典，用作 **kwargs 参数，传递给 callback
   
     4. follow (布尔值)
   
        是否跟进，若 callback=None，则 follow 默认为 True，否则为 False
   
     5. process_links
   
        可调用对象，针对每一个 link_extractor 提取的 URL 会调用该对象，作为 URL 的预处方法，过流 
   
     6. process_request
     
        可调用对象，针对每一个 URL 构成的 Request 对象会调用，返回一个 Request 对象或 None，过滤 Request
   
5. 使用 CrawlSpider

   说明：在没有创建 CrawlSpider 时，也可以使用。爬取百度贴吧的美女吧。

   - 启动爬虫项目，输入爬取 URL

     `scrapy shell "https://tieba.baidu.com/f?ie=utf-8&kw=美女吧&fr=search"`

   - scrapy shell 变量

     ```python
     2019-10-11 10:56:07 [scrapy.core.engine] INFO: Spider opened
     2019-10-11 10:56:08 [scrapy.core.engine] DEBUG: Crawled (200) <GET https://tieba.baidu.com/f?ie=utf-8&kw=%E7%BE%8E%E5%A5%B3%E5%90%A7&
     fr=search> (referer: None)
     [s] Available Scrapy objects:
     [s]   scrapy     scrapy module (contains scrapy.Request, scrapy.Selector, etc)
     [s]   crawler    <scrapy.crawler.Crawler object at 0x0000013811DC4908>
     [s]   item       {}
     [s]   request    <GET https://tieba.baidu.com/f?ie=utf-8&kw=%E7%BE%8E%E5%A5%B3%E5%90%A7&fr=search>
     [s]   response   <200 https://tieba.baidu.com/f?ie=utf-8&kw=%E7%BE%8E%E5%A5%B3%E5%90%A7&fr=search>
     [s]   settings   <scrapy.settings.Settings object at 0x0000013811DC4438>
     [s]   spider     <DefaultSpider 'default' at 0x138120d2e10>
     [s] Useful shortcuts:
     [s]   fetch(url[, redirect=True]) Fetch URL and update local objects (by default, redirects are followed)
     [s]   fetch(req)                  Fetch a scrapy.Request and update local objects
     [s]   shelp()           Shell help (print this help)
     [s]   view(response)    View response in a browser
     ```

   - 导入模块

     ```python
     In [1]: from scrapy.linkextractors import LinkExtractor
     
     In [2]: from scrapy.spiders import CrawlSpider, Rule
     ```

   - 实例化 LinkExtractor 对象

     ```python
     In [3]: link_list = LinkExtractor(allow=('/p/\d+'))
     ```

   - LinkExtractor 接受 Response 对象，并提取正则表达式匹配的结果

     ```python
     In [4]: link_list.extract_links(response)
     ```

   - 匹配结果

     说明：这则表达式匹配的结果，是真正发送的 URL 地址，不是网页源码的信息。<br> `href='/p/6265947374'` 但匹配结果给 `https://tieba.baidu.com/p/6265947374`

     ```python
     [Link(url='https://tieba.baidu.com/p/5006374769', text='【答疑解惑】误删误封绿色通道', fragment='', nofollow=False),
      Link(url='https://tieba.baidu.com/p/6265947374', text='预祝仙女们国庆快乐', fragment='', nofollow=False),
      Link(url='https://tieba.baidu.com/p/6231323901', text='【吧规】禁止广告，维护网络正能量', fragment='', nofollow=False),
      Link(url='https://tieba.baidu.com/p/6211531754', text='【协办】夏日最美coser大赛（美女吧分会场）', fragment='', nofollow=False),
     ...
     ...
     ]
     ```
   
6. Logging 日志信息

   说明：Scrapy 提供了 log 功能，可以通过 logging 使用

   - Logging 使用，在 setting 中设置

     ```python
     LOG_FILE = 'xxx.log'
     LOG_LEVEL = 'INFO'
     ```

   - Log levels 日志信息等级（由高到低）

     1. CRITICAL 严重错误
     2. ERROR 一般错误
     3. WARNING 警告信息
     4. INFO 一般信息
     5. DEBUG 调试信息

   - 设置等级会包含该等级以下信息

   - Logging 设置

     1. LOG_ENABLE 默认：TRUE，启用 logging
     2. LOG_ENCODING 默认：UTF-8，log 使用编码格式
     3. LOG_FILE 默认：None，在当前目录创建 .log 文件保存 log 信息
     4. LOG_LEVEL 默认：DEBUG，log 最低级别
     5. LOG_STDOUT 默认：False，如果为 TRUE，进程所有标准输出（及错误）都将会重定向到 log 中。如，执行 `print('hello')` 将会在 scrapy log 中显示

### CrawlSpider 实例 1

说明：豆瓣影评 , 使用 User-Agent 否则回报 403 错误<br> [完整代码地址]()<br> 爬取的 <br> 创建项目 `scrapy startproject SpiderDouban`

1. Item 模块

   - 提取数据

     1. 影评作者 author
     2. 标题 title
     3. 评论内容

   - 代码演示

     ```python
     import scrapy
     
     
     class SpiderdoubanItem(scrapy.Item):
         # define the fields for your item here like:
         # name = scrapy.Field()
     
         # 影评作者
         author = scrapy.Field()
         # 标题（电影）
         title = scrapy.Field()
         # 评论内容
         content = scrapy.Field()
     ```

2. 使用 scrapy shell 验证匹配规则

   说明：一般情况下 <br> 匹配内容使用 __Xpath__ <br> 匹配深度连接使用 __正则表达式__

   - 验证是否爬取网页成功

     `response.text`

   - 验证内容匹配规则

     说明：使用 Xpath 匹配时，出现了匹配出错（浏览器中正确，程序中匹配不出来的问题)。提供一个解决办法，使用 __火狐浏览器插件 xPath Finder__ 不会出现匹配不上的问题，但是这个插件匹配会精确的每一个元素，所以自己需要修改一下。

   - 验证深度连接匹配规则

     说明：使用正则表达式，很简单，只要匹配 __连接__ 的部分信息，就可以提取出 URL

3. douban (crawlspider) 模块

   - 需设置的内容

     1. 导入模块

        ```python
        from scrapy.linkextractors import LinkExtractor
        from scrapy.spiders import CrawlSpider, Rule
        from SpiderDouban.items import SpiderdoubanItem
        ```

     2. 设置 rules

        说明：匹配连接

        ```python
        rules = (
                Rule(LinkExtractor(allow=r'start=\d+'), callback='parse_item', follow=True),
            )
        ```

     3. 设置匹配内容

        说明：`extract()` 提取出匹配内容，匹配结果为列表

        ```python
        # 作者
        author = each.xpath('./div/header/a[2]/text()').extract()
        # 标题
        title = each.xpath('./div/div/h2/a/text()').extract()
        # 影评内容
        content = each.xpath('./div/div/div[1]/div/text()').extract()
        ```

4. Pipelines 模块

   说明：将数据存入文件中

   - 代码演示

     ```python
     import json
     
     class SpiderdoubanPipeline(object):
     
         def __init__(self):
             # 打开数据文件
             self.file = open(file='Data_doc/yp.json', mode='w', encoding='utf-8')
     
     
         def process_item(self, item, spider):
             # 将 item 转为字典，再将字典转换为 Json 格式
             info = json.dumps(obj=dict(item), ensure_ascii=False)
             self.file.write(info + '\n')
     
             return item
     
     
         def open_spider(self, spider):
             pass
     
     
         def close_spider(self):
             # 数据文件关闭
             self.file.close()
             pass
     ```

5. 爬取结果

   - json格式

     说明：一个 item 对象

     ```json
     {"author": "你谁呀", "title": "此味只有天上有", "content": "\n\n                        假如你现在想看一部日本电影，又觉得大师们的片子太厚重，不易接近，新电影又拿不准看个啥能轻松娱乐赏心悦目又不失逼格，那么电影红花会就给你指条明路:《小森林夏秋篇》！就是一部不看不知道，一看真奇妙的佳片，去年全世界最好看的电影之一，不信？看完无感您回来插了战台烽...\n\n                         ("}
     ```

### CrawlSpider 实例 2 

说明：爬取某网站，使用 CrawlSpider 、Spider 两种方式进行操作    [源码地址](https://pan.baidu.com/s/1Zew8XfZKBmumPnhxF0gSRA)<br> <br>__返回的 URL 地址错误（不可访问），使用 Rule 的 `process_links='function'` 参数调用函数预处理 LinkExtractor 对象的 extract_links() 提取出的 URL，在 4 中，有代码演示。__<br>  <br> __获取当前响应 Response 的 URL 为 `response.url`__ <br> <br>__Rule 对象的参数 follow 为是否跟进该 URL 的 Response，就是该 URL 已经发送，他的响应 Response 是否继续提取 URL__  <br> <br> __这个网站的反爬虫机制，研究了一上午：1. 爬虫返回的页面数据和网页的页面数据不一样，这样会导致提取数据（Xpath）提取不到数据，本人解决办法：读取爬虫网页原始数据，找出匹配规则。2.处理状态，标签会变，本人解决办法：使用 Xpath 的匹配规则 “ 或 ” -->  " | "__ 

1. 基本步骤

   - 创建项目

     `scrapy startproject SpiderDGsun`

   - 设置 Item 数据字段

     1. title：问题
     2. num：问题编号
     3. content：问题内容
     4. state：问题状态（是否处理）

   - 创建 CrawlSpider 

     `scarpy genspider -t crawl DGSun xxx.com`

   - 设置 setting 文件

     1. 关闭 robot 协议
     2. 报头添加 User-Agent
     3. 修改了 Item 的一个东西（等会再写）

2. 运行 scrapy shell "URL"，提取链接

   说明：验证爬虫响应文件、验证提取字段、验证提取 URL

   - 提取需要跟进的 URL（页面链接）
   
     `link_list = LinkExtractor(allow=(r'question/\d+/\d+.shtml'))`
   
   - 提取不跟进的 URL（问题链接）
   
     `link_list = LinkExtractor(allow=(r'question/report\?page=\d+'))`
   
3. 在 scrapy shell 提取数据字段

   说明：URL 为 2 中不跟进的 URL 的 Response 中提取

   - 提取问题标题

     ` response.selector.xpath('//head/title/text()').extract()`

   - 提取问题编号

     `response.selector.re(r'编号:(\d+)')`

   - 提取问题状态

     说明：问题状态的标签会有变化，所以使用或

     `response.selector.xpath('//span[@class="qblue"]/text() | //span[@class="qgrn"]/text() | //span[@class="qred"]/text()').extract()`

4. crawlSpider 

   - 代码

     ```python
     class DgsunSpider(CrawlSpider):
         name = 'DGsun'
         allowed_domains = ['wz.sun0769.com']
         start_urls = ['http://wz.sun0769.com/index.php/question/report?page=']
     	
         # 第一个 Rule 为提取 页面链接， 继续跟进
         # 第二个 Rule 为提取 问题链接，且使用 parse_item 作为回调函数提取 数据字段，不跟进
         rules = (
             Rule(LinkExtractor(allow=r'question/report\?page=\d+'), follow=True, process_links='deal_link'),
             Rule(LinkExtractor(allow=r'question/\d+/\d+.shtml'), callback='parse_item', follow=None),
         )
         
         # 为处理 页面链接（服务器返回 URL 有误，即反爬虫机制）
         def deal_link(self, links):
             for link in links:
                 # 操作 link 中的 URL
                 #
                 # 这里只是打印，没有对 link.url 进行修改
                 # print(link.url)
     
         def parse_item(self, response):
             item = {}
             #item['domain_id'] = response.xpath('//input[@id="sid"]/@value').get()
             #item['name'] = response.xpath('//div[@id="name"]').get()
             #item['description'] = response.xpath('//div[@id="description"]').get()
     
             # 初始化 item
             item = SpiderdgsunItem()
     
             title = response.xpath('//head/title/text()').extract()
             num = Selector(response).re(r'编号:(\d+)')
             content = response.xpath('//head/meta[@name="description"]/@content').extract()
             state = response.xpath('//span[@class="qblue"]/text() | //span[@class="qgrn"]/text() | //span[@class="qred"]/text()').extract()
     
             item['title'] = title[0]
             item['num'] = num[0]
             item['content'] = content[0]
             item['state'] = state[0]
     
     
             yield item
     ```
   
5. 使用 Spider 对上述网站进行爬取

   - spider 代码演示

     ```python
     import scrapy
     from scrapy.selector import Selector
     from SpiderDGSun.items import SpiderdgsunItem
     
     
     class DgsunSpider(scrapy.Spider):
         name = 'DGsunSpider'
         allowed_domains = ['wz.sun0769.com']
     
         # 拼接 URL 地址
         offset = 0
         url = 'http://wz.sun0769.com/index.php/question/report?page='
         start_urls = [url + str(offset)]
     
         def parse(self, response):
             # 提取问题链接，返回为 单纯列表
             links = Selector(response).re(r'http://wz.sun0769.com/html/question/\d+/\d+.shtml')
     
             for link in links:
                 # 问题链接，回调函数使用 parse_item，提取数据字段
                 yield scrapy.Request(url=link, callback=self.parse_item)
     
             # 页面链接，回调函数使用 parse，提取问题链接
             self.offset += 30
             if self.offset <= 148710:
                 yield scrapy.Request(url=self.url + str(self.offset), callback=self.parse)
     
     
         def parse_item(self, response):
             item = {}
             #item['domain_id'] = response.xpath('//input[@id="sid"]/@value').get()
             #item['name'] = response.xpath('//div[@id="name"]').get()
             #item['description'] = response.xpath('//div[@id="description"]').get()
     
             # 初始化 item
             item = SpiderdgsunItem()
     
             title = response.xpath('//head/title/text()').extract()
             num = Selector(response).re(r'编号:(\d+)')
             content = response.xpath('//head/meta[@name="description"]/@content').extract()
             state = response.xpath('//span[@class="qblue"]/text() | //span[@class="qgrn"]/text() | //span[@class="qred"]/text()').extract()
     
             item['title'] = title[0]
             item['num'] = num[0]
             item['content'] = content[0]
             item['state'] = state[0]
     
     
             yield item
     ```

6. 提取数据结果

   - 图片链接暂无法处理

### Request \ Response 登录流程

说明：网站登录，进入登录页面时，进入一个 URL 地址，输入用户名、密码，点击登录时，会将数据（用户名、密码，一些其他字段），提交到另一个 URL 地址，然后网页再次跳转到一个新的 URL 地址，最后登录成功。

1. 登录流程，以  github 网站为例

   - 进入登录页面 'https://github.com/login'

     ![模拟登录页面](git_picture/模拟登录页面.png)

   - 输入非正确的用户账号，点击登录，其中 form 表单向 https://github.com/session 此 URL 提交数据如下

     ![模拟登录form提交数据](git_picture/模拟登录form提交数据.png)

     __其中 login 为 用户名、password 为密码、timestamp 为 Unix 时间戳，其他的不知道是什么，但是也不是所有都必须需要提交。__

   - 在 HTML 的源码数据，可以验证到以上数据

     ![html源码提交数据](git_picture/html源码提交数据.png)

2. 网站反爬虫手段（此处只讨论对 form 表单的操作）

   说明：网站有没有反爬虫机制不是评价一个网站的好坏标准，但是以数据为主的网站一般都会有反爬虫机制

   - 一类网站

     没有附属的字段，只有用户名、密码

   - 二类网站

     就如同 github 网站，有附属字段，和用户名、密码，随 form 一起提交（时间戳等）

   - 三类网站

     就如同知乎网站，有附属字段，随 form 表单一起提交，但是它还会对其进行加密

   - 如上所说

     1. 三类网站最难爬取数据
     2. __所有提交的数据字段，在 HTML 源码中的 form 表单中，都可以找到__

3. Resquest \ Response

   说明：`from scrapy import Request`    `import scrapy.http.Response`
   
   - Resquest 模块
   
     1. 部分源码
   
        ```python
        class Request(object_ref):
        
            def __init__(self, url, callback=None, method='GET', headers=None, body=None,
                         cookies=None, meta=None, encoding='utf-8', priority=0,
                         dont_filter=False, errback=None, flags=None, cb_kwargs=None):
        
                self._encoding = encoding  # this one has to be set first
                self.method = str(method).upper()
                self._set_url(url)
                self._set_body(body)
                assert isinstance(priority, int), "Request priority not an integer: %r" % priority
                self.priority = priority
        
                if callback is not None and not callable(callback):
                    raise TypeError('callback must be a callable, got %s' % type(callback).__name__)
                if errback is not None and not callable(errback):
                    raise TypeError('errback must be a callable, got %s' % type(errback).__name__)
                assert callback or not errback, "Cannot use errback without a callback"
                self.callback = callback
                self.errback = errback
        
                self.cookies = cookies or {}
                self.headers = Headers(headers or {}, encoding=encoding)
                self.dont_filter = dont_filter
        
                self._meta = dict(meta) if meta else None
                self._cb_kwargs = dict(cb_kwargs) if cb_kwargs else None
                self.flags = [] if flags is None else list(flags)
        ```
   
     2. 常用参数
   
        - url
   
          需要请求的 URL
   
        - callback
   
          指定处理返回的 Response 的回调函数
   
        - method
   
          请求方式，默认为 GET，可以设置为 POST、PUT 等
   
        - headers
   
          请求的报头，一般不用写，包含一下内容
   
          ```tex
          GET / HTTP/1.1
          Host: tieba.baidu.com
          Connection: keep-alive
          Cache-Control: max-age=0
          Upgrade-Insecure-Requests: 1
          User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.120 Safari/537.36
          Sec-Fetch-Mode: navigate
          Sec-Fetch-User: ?1
          Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
          Sec-Fetch-Site: none
          Accept-Encoding: gzip, deflate, br
          Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7
          ```
   
        - meta
   
          比较常用，在不同请求之间传递数据使用，为 dict（字典）类型
   
          meta 是一个字典类型，主要作用是传递数据的，`meat = {key: value}`, 如果想在下一个函数中取出 value 值，就可以使用 Request 的参数 meta 传递给下一个函数，即下一个函数的 Response 就会有 `response.meta` 
   
          简单来说，爬虫的数据字段，必须要是使用两个解析函数（parse）进行提取，第一个解析函数处理的 item 数据怎样才可以传递个下一个 parse 呢？这是 Request 的参数 meta 就排上用处了。
   
          ```python
          # 上一个 parse 提取完字段，发送的请求，回调 parse_2 函数
          scarpy.Request(url=url, meat={'item': item}, callback=self.parse_2)
          
          # 下一个 parse_2,使用 meta
          def parse_2(self, response):
              # 取出 meta 参数数据
              item = response.meta['item']
          ```
   
        - cookie
   
          登录网站后的验证信息
   
        - encoding
   
          编码形式，使用默认总是可以的
   
        - dont_filter
   
          表明请求不由调度器过滤。当你想执行多次相同请求时，忽略重复过滤器。默认值为 False
   
        - errback
   
          指定错误处理函数
   
   - Response 模块
   
     1. 部分代码
   
        ```python
        class Response(object_ref):
        
            def __init__(self, url, status=200, headers=None, body=b'', flags=None, request=None):
                self.headers = Headers(headers or {})
                self.status = int(status)
                self._set_body(body)
                self._set_url(url)
                self.request = request
                self.flags = [] if flags is None else list(flags)
        ```
   
     2. 主要参数
   
        - url
   
          响应体的 URL
   
        - status
   
          响应状态码
   
        - headers
   
          响应报头
   
          ```tex
          {b'Content-Type': b'text/html; charset=UTF-8',
           b'Date': b'Thu, 17 Oct 2019 01:26:20 GMT',
           b'P3P': b'CP=" OTI DSP COR IVA OUR IND COM "',
           b'Server': b'Apache',
           b'Set-Cookie': b'BAIDUID=D01297F0898391D3A879F89B7F40CEAC:FG=1; expires=Fri, 16-Oct-20 01:26:20 GMT; max-age=31536000; path=/; domain=.baidu.com; version=1',
           b'Tracecode': b'15798839461010195466101709',
           b'Vary': b'Accept-Encoding',
           b'X-Xss-Protection': b'1; mode=block'}
          ```
   
        - body
   
          响应体，response.body 为二进制
   
4. 发送 POST 请求

   说明：__POST 请求，有别有模拟登录__

   - 使用 `yield scrapy.FormResquest(url, formdata, callback)`

     1. url 为爬取的地址
     2. formdata 为字典，是提交的数据字段（提交的数据的 URL 是另一个，上面已经说明）
     3. callback 回调函数

   - 如果希望程序最开始在爬取 start_urls 时，就使用 POST，可以重写 start_request() 方法

     说明：直接使用 POST 方法，爬取第一个 URL 几乎不怎么用，都是首先，使用 GET 请求，获取登录页面，在使用 POST 提交数据

     ```python
     import scrapy
     
     
     class xxxSpider(scrapy.Spider):
         name = 'xxx'
         allowed_domains = ['www.xxx.com']
         start_urls = ['http://www.xxx.com/']
     
         def start_request(self):
             url = self.start_urls[0]
             yield scarpy.FormRequest(url=url, formdata={key_11: value_1, key_2: value_2}, callback=self.parse)
             
         def parse(self, response):
            pass
     ```

### scarpy 模拟登录

说明：网站登录大致分为 3 类，一类：登录只需要用户名、密码即可登录（现在来所特别少了）；二类：登录时，除了用户名、密码，还需要附带属性；三类：登录时，用户名、密码、附带属性，还给属性进行加密，使其在浏览器端无法看清附带属性是什么（这类网站就不是十分好弄了）。__以上网站提交数据时，都是使用 POST 方式__

1. 模拟登录（POST 请求）与 Ajax 后表单提交区别

   说明：__提交数据的 URL 和当前的页面 URL 一般情况都不一样__

   - Ajax 或表单提交 `scrapy.FormRequest(url, formdata, callback)`

     1. URL

        表单或 Ajax 提交地址

     2. formdata

        提交的数据，数据类型为字典（dict）

     3. callback

        回调函数

   - 模拟登录 `scrapy.FormRequest.from_Response(response, url, formdata, callback)`

     说明：__看源码，好像这个 URL 不用写，可以自动提取 form 表单的提交的 URL__

     1. response

        登录页面响应文件

     2. ~~url~~

        ~~账号提交地址~~

     3. formdata

        提交数据，数据类型为字典（dict）

     4. callback

        回调函数

2. 一类网站（只提供用户名、密码）

   - 使用 scarpy.FormRequest.from_Response 方法进行提交数据，代码演示

     ```python
     import scrapy
     
     
     class ExampleSpider(scrapy.Spider):
         name = 'example'
         allowed_domains = ["example.com"]
         start_urls = ["http://www.example.com/PLogin.do"]
     
         def parse(self, response):
             # POST 方式提交数据 formdata，注意参数有 response
             yield scrapy.FormRequest.from_response(
                     response,
                     formdata = {'email' : 'value_1', 'password' : 'value_2'}
                     callback = self.parse_page
                 )
     
         def parse_page(self, response):
             pas
     ```

     

3. 二类网站（提供账号、附属字段）

   - 使用 scarpy.FormRequest.from_Response 方法进行提交数据，代码演示

     ```python
     import scrapy
     
     
     class ExampleSpider(scrapy.Spider):
         name = 'example'
         allowed_domains = ["example.com"]
         start_urls = ["http://www.example.com/PLogin.do"]
     
         def parse(self, response):
             # 提取隐藏字段
             _xsrf = response.xpath("//_xsrf").extract()[0]
             # POST 账号信息和隐藏字段
             yield scrapy.FormRequest.from_response(
                     response,
                     formdata = {'email' : 'value_1', 'password' : 'value_2', '_xsrf' = _xsrf}，
                     callback = self.parse_page
                 )
     
         def parse_page(self, response):
             pas
     ```

4. 三类网站（账号、附属字段加密）

   - 使用 cookie 值登录（无奈之举），因为网站反爬虫技术，模拟登录失败，所以只能使用 cookie 直接网站得页面。
   
     ```python
     import scrapy
     
     
     class ExampleSpider(scrapy.Spider):
         name = "example"
         allowed_domains = ["example.com"]
         start_urls = [
             'http://www.example.com/1',
             'http://www.example.com/2',
             'http://www.example.com/3',
         ]
     # cookie 是假的
         cookies = {
             "anonymid": "ixrna3fysufnwv",
             "_r01_": "1",
             "ap": "327550029",
             "JSESSIONID": "abciwg61A_RvtaRS3GjOv",
             "depovince": "GW"
         }
     
         def start_requests(self):
             for url in self.start_urls:
                 # 这块使用 formRequest 类，但是我觉得 Request 类就可以，因为不需要提交数据
                 yield scrapy.FormRequest(url, cookies=self.cookies, callback=self.parse_page)
     
         def parse_page(self, response):
             pass
     ```

### Downloader Middlewares（反爬虫机制）

说明：有些网站使用不同得繁杂性规则防止爬虫访问，绕过这些规则有时比较困难，如有需要，那就联系商业支持。反爬虫更换 IP 和 User-Agent 爬取不需要登录的网站可以，但是需要登录的网站可以么？？cookie 、重定向不是防止这些的么？？

[反爬虫文档](http://doc.scrapy.org/en/master/topics/practices.html#avoiding-getting-banned)    [下载中间件文档](http://doc.scrapy.org/en/master/topics/downloader-middleware.html)     [下载中间件文档](https://scrapy-chs.readthedocs.io/zh_CN/0.24/topics/downloader-middleware.html)

1. 通常防止反爬虫主要有一下几个方法

   - 动态设置 User-Agent （随机切换 User-Agent，模拟不同用户浏览器信息）

   - 禁止 Cookie （不启动 cookie middleware，不向 Server 发送 cookie，一些网站通过 cookie 发现爬虫行为）

     通过 `COOKIES_ENABLED` 控制 CookieMiddleware 开启或关闭

   - 设置延迟下载（就是延迟访问，防止访问过于频繁，设置为 2 秒，或者更高）

     正常用户，一般访问网站网页，大约每 10 几秒访问一个网页，所以模拟用户，也应该尽量贴近用户得使用习惯，没有什么用户，一秒可以访问 10 几个网站页面的。

   - 使用 IP 地址池，VPN 和 代理 IP， 当前大部分网站是根据 IP 来禁止爬虫的（某个 IP 短时间内不正常的访问了大量数据，这个 IP 有可能就是爬虫）
   
   - Google Cache 和 Baidu Cache （搜索引起快照），还可以通过搜索引擎服务器缓存的页面数据来获取需要的数据（好像就是爬取搜索引起获取的页面数据）
   
   - 使用 Crawlera （专用爬虫的代理组件），正确配置和设置下载 _中间件_ 后，scrapy 项目的所有 Request 请求都将通过 Crawlera 发送到服务器。__有待研究__
   
2. 下载中间件介绍（Downloader Middlewares）

   说明：下载中间件是处于引擎（crawler engine）和下载器（crawler download）之间的一组组件，可以有多个下载中间件被加载运行

   - 当引擎（Engin）将请求（Request）传递给下载器（Download）过程中，下载中间（Downloader Middlewares）件可以对请求进行处理（修改 Resquest 报头信息，增加代理（proxy）等）。
   - 当下载器（Downloader）完成 HTTP 请求，传递响应给引擎（Engin），下载中间件可以对响应进行处理（对响应文件进行解压等）

3. 设置下载中间件

   - 激活下载中间件组件

     将其加入到 setting 文件的 DOWNLOADER_MIDDLEWARES 设置中。其中该设置为一个字典（dict），键入（key）为中间件路径：值（value）为优先级（level）

   - setting 文件 DOWNLOADER_MIDDLEWARES 设置演示

     ```python
     # Enable or disable downloader middlewares
     # See https://docs.scrapy.org/en/latest/topics/downloader-middleware.html
     DOWNLOADER_MIDDLEWARES = {
         'SpiderBaidu.middlewares.SpiderbaiduDownloaderMiddleware': 543,
     }
     ```

     解释：`SpiderBaidu.middlewares.SpiderbaiduDownloaderMiddleware` 为中间件路径（SpiderBaidu 文件夹下的 middlewares 文件的 SpiderbaiduDownloaderMiddleware 类），`543` 为此中间组件的优先级别
     
   - 定义下载组件的执行顺序

     1. DOWNLOADER_MIDDLEWARES 设置会与 Scrapy 定义的 DOWNLOADER_MIDDELWARES_BASE 设置合并（但不是覆盖），而后根据顺序（order：优先级）进行排序，最后得到启用中间件的有序列表：第一个中间件最靠近引擎，最后一个中间那件最靠近下载器

     2. 如何分配中间件顺序

        根据想要防止中间的位置选择一个优先级。每一个中间件执行不同动作，中间件可能会依赖于之前（或之后）执行的中间件，因此顺序很重要

   - 禁止中间组件

     将优先级设置为 `None`

4. 定义中间组件

   说明：在 setting 类开启 `DOWNLOADER_MIDDLEWARES` 设置
   
   - setting 说明
   
     ```python
     DOWNLOADER_MIDDLEWARES = {
         'SpiderBaidu.middlewares.SpiderbaiduDownloaderMiddleware': 543,
     }
     ```
   
     `'SpiderBaidu.middlewares.SpiderbaiduDownloaderMiddleware': 543,` 这个创建项目是已近默认存在，你可以完全重新写，也可以在这个基础上进行修改
   
     中间组件类继承（object）`class SpiderbaiduDownloaderMiddleware(object)`
   
   -  __process_request(request, spider) 方法__ 必须有，当每个 request 通过下载中间件时，该方法被调用 <br> 参数 <br>     request：Request 对象（处理的 request）<br>     spider：Spider 对象（该 request 对应的 spider）     
     1. process_request() 必须返回其中之一： 返回 None 、返回一个 Response 对象、返回一个 Request 对象或raise IgnoreRequest 
        - 如果其返回 None ，Scrapy 将继续处理该 request ，执行其它的中间件的相应方法，直到合适的下载器处理函数（download handler）被调用，该 request 被执行（response被下载）。
        - 如果其返回 Response 对象，Scrapy 将不会调用任何其它的 process_request() 或 process_exception() 方法，或相应地下载函数， 只会返回该 response ，这时中间件的 process_response() 方法则会在每个response 返回时被调用
        - 如果其返回 Request 对象，Scrapy 则停止调用 process_request() 方法并重新调度返回的 request。当新返回的 request 被执行后， 相应地中间件将会根据下载的 response 被调用
        - 如果其 raise 一个 IgnoreRequest 异常，则安装的下载中间件的 process_exception() 方法会被调用。如果没有任何一个方法处理该异常， 则 reques t的 errback(Request.errback) 方法会被调用。如果没有代码处理抛出的异常， 则该异常被忽略且不记录(不同于其他异常那样)。
   - __process_response(request, response, spider) 方法__ ，当下载完 HTTP 请求时，传递响应给引擎时被调用 <br> 参数 <br>     request：Resquest 对象（respons 所对应的 request） <br>     response Response 对象（被处理的 response） <br>     spider Spider 对象（response 所对用的 spider）
     1. process_request() 必须返回以下其中之一： 返回一个 Response 对象、 返回一个 Request 对象或raise一个 IgnoreRequest 异常。
        - 如果返回一个 Response （可以与传入的 response 相同，也可以是全新的对象）， 该 response 会被在链中的其它中间件的 process_response() 方法处理
        - 如果返回一个 Request 对象，则中间件链停止， 返回的 request 会被重新调度下载。处理类似于 process_request() 返回 request 所做的那样。
        - 如果抛出一个 IgnoreRequest 异常，则调用 request 的 errback(Request.errback)。 如果没有代码处理抛出的异常，则该异常被忽略且不记录（不同于其他异常那样）。
   
5. 使用案例

   说明：使用下载中间动态改变 User-Agent，和添加 IP 代理 <br> [设置 User-Agent 文档](https://blog.csdn.net/weixin_43430036/article/details/84851714#headers_34)

   - 在 setting 文件中，配置 User-Agent 和 Proxies（IP 代理）

     1. Uesr-Agent (fake-useragent 模块内置许多浏览器的 User_Agent)

        ```python
        USER_AGENTS = [
            "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Win64; x64; Trident/5.0; .NET CLR 3.5.30729; .NET CLR 3.0.30729; .NET CLR 2.0.50727; Media Center PC 6.0)",
            "Mozilla/5.0 (compatible; MSIE 8.0; Windows NT 6.0; Trident/4.0; WOW64; Trident/4.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; .NET CLR 1.0.3705; .NET CLR 1.1.4322)",
            "Mozilla/4.0 (compatible; MSIE 7.0b; Windows NT 5.2; .NET CLR 1.1.4322; .NET CLR 2.0.50727; InfoPath.2; .NET CLR 3.0.04506.30)",
            "Mozilla/5.0 (Windows; U; Windows NT 5.1; zh-CN) AppleWebKit/523.15 (KHTML, like Gecko, Safari/419.3) Arora/0.3 (Change: 287 c9dfb30)",
            "Mozilla/5.0 (X11; U; Linux; en-US) AppleWebKit/527+ (KHTML, like Gecko, Safari/419.3) Arora/0.6",
            "Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.8.1.2pre) Gecko/20070215 K-Ninja/2.1.1",
            "Mozilla/5.0 (Windows; U; Windows NT 5.1; zh-CN; rv:1.9) Gecko/20080705 Firefox/3.0 Kapiko/3.0",
            "Mozilla/5.0 (X11; Linux i686; U;) Gecko/20070322 Kazehakase/0.4.5"
        ]
        ```

     2. 添加 IP 代理

        ```python
        PROXIES = [
            {'ip_port': '111.8.60.9:8123', 'user_passwd': 'user1:pass1'},
            {'ip_port': '101.71.27.120:80', 'user_passwd': 'user2:pass2'},
            {'ip_port': '122.96.59.104:80', 'user_passwd': 'user3:pass3'},
            {'ip_port': '122.224.249.122:8088', 'user_passwd': None},
        ]
        ```

        解释：PROXIES 为列表，每一个 IP 代理为字典，ip_port 为 ip 加端口号，user_passwd 为用户名及密码，没有密码验证时，直接设为 None

     3. 代码演示

        ```python
        import random
        import base64
        
        # 导入 setting 设置的变量
        from settings import USER_AGENTS
        from settings import PROXIES
        
        # 随机获取 setting 设置的 User-Agent
        class RandomUserAgent(object):
            def process_request(self, request, spider):
        	    # 在 setting 设置的 USER_AGENT 列表，随机取值 User-Agent 值        
                useragent = random.choice(USER_AGENTS)
            
        		# 设置 request 的报头的 User-Agent
        		# 不会改写 scrapy.Request 中 headers 中的设置的 User-Agent
                # 都是用这个，但是没有修改啊？？
                request.headers.setdefault("User-Agent", useragent)
                
                # 会改写 scarpy.Request 中的 User-Agent
        	   # request.headers['User-Agent'] = useragent
        class RandomProxy(object):
            def process_request(self, request, spider):
                proxy = random.choice(PROXIES)
        
                if proxy['user_passwd'] is None:
                    # 没有代理账户验证的代理使用方式
                    request.meta['proxy'] = "http://" + proxy['ip_port']
                else:
                    # 对账户密码进行base64编码转换
                    base64_userpasswd = base64.b64encode(proxy['user_passwd'])
                    # 对应到代理服务器的信令格式里
                    request.headers['Proxy-Authorization'] = 'Basic ' + base64_userpasswd
                    request.meta['proxy'] = "http://" + proxy['ip_port']
        ```

### Settings（仔细阅读） 

说明：Scrapy 设置（settings）提供了定制 Scrapy 组件的方法。可以控制包括核心（core）、插件（extension）、pipeline 和 spider 组件。

[settings 文档](https://scrapy-chs.readthedocs.io/zh_CN/1.0/topics/settings.html)

1. 获取 setting 设定值

   说明：设定可以通过多种方式设置，每个方式具有不同的优先级，下面以优先级降序的方式给处优先级

   - 命令行选项（Command line Option）（最高优先级）
   - 每个 Spider 的设置
   - 项目设定模块（Project settings module）
   - 命令默认设定模块（Default settings per-command）
   - 全局默认设定模块（Default global settings）（优先级最低）

2. 命令行选项

   说明：命令行传入的参数具有最高优先级，可以使用 command-line 先选 `-s` 或 `-set` 来覆盖一个或多个选项

   - 例子

     `scrapy crawl myspider -s LOG_FILE=scrapy.log`

3. 项目设定模块（Project settings module）

   说明：项目设定模块是 Scrapy 项目标准配置文件

4. 导入 setting 设置

   - 导入 settings 模块的属性

     `from settings import xxx`

## 待续......

