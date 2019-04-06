## TCP/IP协议族

- 协议：不同硬件、操作系统之间通信需要的规则

- TCP/IP：互联网相关的各类协议族的总称，包括（ IP, TCP, HTTP, FTP, DNS, UDP... ）

- TCP/IP层次
  - 应用层：向用户提供应用服务时通信的活动（ FTP, DNS, HTTP ）
  - 传输层：提供处于网络连接中的两台计算机之间的数据传输（ TCP, UDP ）
  - 网络层：通过多台计算机或网络设备传输时，选择传输线路（ IP ）
  - 数据链路层：处理连接网络的硬件部分

- 封装：发送端在层与层之间传输数据时，每经过一层必定被打上该层所属的首部信息

- IP地址：节点被分配到的地址，可变

- MAC地址：网卡所属的固定地址，基本不变

- ARP( Address Resolution Protocol )：根据通信方的IP地址解析出对应的MAC地址

- 字节流服务( Byte Stream Service )：为了方便传输，将大块数据分割成以segment为单位的数据包进行管理

- 三次握手( three-way handshaking )：数据包发送出去后，会向对方确认是否成功送达

1. 发送端：发送带SYN( synchronize )的数据包给对方
2. 接收端：接收后，回传带SYN/ACK的数据包确认
3. 发送端：回传带ACK的数据包，握手结束

- DNS( Domain Name System )：提供域名到IP地址之间的解析服务

- URI( Uniform Resource Identifier )：用字符串标识某一互联网资源
  - 绝对URI：http://user:pass@www.example.jp:80/dir/index/htm?uid=1#ch1 （ 协议方案://登录信息@服务器地址:端口/文件路径?查询字符串#片段标识符 ）

- URL( Uniform Resource Locator )：表示资源位置，是URI的子集



## HTTP协议

- 请求报文构成
  - 请求方法
  - 请求URI
  - 协议版本
  - 首部字段
  - 内容实体

- 响应报文构成
  - 协议版本
  - 状态码( status code )
  - 状态码的原因短语( reason-phrase )
  - 响应首部字段
  - 主体( entity body )



- 追踪路径( Trace )：设置Header Max-Forwards，经过一个服务器端就减1，等于0时停止传输

- Connect：用隧道协议连接代理，使用SSL( Secure Sockets Layer )和TLS( Transport Layer Security )协议把通信内容加密后经网络隧道传输



- HTTP methods

| -                   |                      |
| ------------------- | -------------------- |
| GET                 | 获取资源             |
| POST                | 传输实体             |
| PUT                 | 传输文件             |
| HEAD                | 获取报文首部         |
| DELETE              | 删除文件             |
| OPTIONS             | 询问支持方法         |
| TRACE               | 追踪路径             |
| CONNECT             | 用隧道协议连接代理   |
| LINK（1.1已废除）   | 建立和资源之间的联系 |
| UNLINK（1.1已废除） | 断开连接关系         |



- 持久连接（ HTTP Persistent Connections, HTTP keep-alive, HTTP connection reuse ）：只要任意一端没有提出断开连接，就一直保持TCP连接状态

- 管线化（ pipelining ）：不用等待响应可直接发送下一个请求

- Cookie：在请求和响应报文中写入Cookie信息来控制客户端状态

1. 服务器响应 Header Set-Cookie
2. 客户端保存Cookie
3. 客户端请求带上Cookie



## HTTP Message

- HTTP报文：用于HTTP协议交互的信息
  - 首部
  - 空行( CR+LF )
  - 主体

- 内容编码
  - gzip ( GUN zip )
  - compress ( UNIX 系统的标准压缩 )
  - deflate ( zlib )
  - identity ( 不进行编码 )

- 分块传输编码（ Chunked Transfer Coding ）：把实体主体分割传输，每一块用十六进制标记块的大小，最后一块使用0(CR+LF)

- MIME( Multipurpose Internet Mail Extensions )：允许邮件处理文本、图片、视频等多个不同种类的数据

- 获取部分内容的范围请求：设置 Header Range



## Status Code



| -    |                       |                                                              |
| ---- | --------------------- | ------------------------------------------------------------ |
| 1XX  | Informational         | 接收的请求正在处理                                           |
| 2XX  | Success               | 请求正常处理完毕                                             |
| 200  | OK                    | 服务器端正常处理客户端请求                                   |
| 204  | No Content            | 服务器端正常处理客户端请求，但响应报文不含实体主体部分       |
| 206  | Partial Content       | 客户端进行范围请求，响应报文包含Content-Range                |
| 3XX  | Redirection           | 浏览器需要进行附加操作以完成请求                             |
| 301  | Moved Permanently     | 永久性重定向，表示请求的资源被分配了新的URI，以后应使用资源现在指向的URI |
| 302  | Found                 | 临时重定向，表示请求的资源被分配了新的URI，但不更新书签      |
| 303  | See Other             | 表示对应的资源存在另一个URI，应使用GET获取请求资源           |
| 4XX  | Client Error          | 服务器无法处理请求                                           |
| 400  | Bad Request           | 请求报文中存在语法错误                                       |
| 401  | Unauthorized          | 发送的请求需要通过HTTP认证                                   |
| 403  | Forbidden             | 请求资源的访问被服务器拒绝                                   |
| 404  | Not Found             | 服务器上无法找到请求的资源                                   |
| 5XX  | Server Error          | 服务器处理请求出错                                           |
| 500  | Internal Server Error | 服务器执行请求时发生错误                                     |
| 503  | Service Unavailable   | 服务器暂时处于超负载或进行停机维护，无法处理请求             |



## Web Server

- 代理：有转发功能的应用程序，接收客户端请求并转发给服务器，接收服务器响应并转发给客户端
  - 缓存代理（ Caching Proxy ）：预先将资源副本缓存在代理服务器上
  - 透明代理：转发请求或响应时不对报文做任何加工

- 网关：转发其他服务器通信数据的服务器

- 隧道：在客户端和服务端之间进行中转，并保持双方通信连接



## HTTP Header

**通用首部字段**

| -                 |                            |
| ----------------- | -------------------------- |
| Cache-Control     | 控制缓存行为               |
| Connection        | 逐跳首部、连接的管理       |
| Date              | 创建报文的日期时间         |
| Pragma            | 报文指令                   |
| Trailer           | 报文末端的首部一览         |
| Transfer-Encoding | 指定报文主体的传输编码方式 |
| Upgrade           | 升级为其他协议             |
| Via               | 代理服务器的相关信息       |
| Warning           | 错误通知                   |



**请求首部字段**

| -                   |                                    |
| ------------------- | ---------------------------------- |
| Accept              | 用户代理可处理的媒体类型           |
| Accept-Charset      | 优先的字符集                       |
| Accept-Encoding     | 优先的内容编码                     |
| Accept-Language     | 优先的语言                         |
| Authorization       | Web认证信息                        |
| Expect              | 期待服务器的特定行为               |
| Form                | 用户邮箱                           |
| Host                | 请求资源所在服务器                 |
| If-Match            | 比较实体标记(E-tag)                |
| If-Modified-Since   | 比较资源更新时间                   |
| If-None-Match       | 比较实体标记                       |
| If-Range            | 资源未更新时发送实体Byte的范围请求 |
| If-Unmodified-Since | 比较资源更新时间                   |
| Max-Forwards        | 最大传输逐跳数                     |
| Proxy-Authorization | 代理服务器要求客户端的认证信息     |
| Range               | 实体的字节范围请求                 |
| Referer             |                                    |
| TE                  | 传输编码优先级                     |
| User-Agent          | HTTP客户端程序信息                 |



**响应首部字段**

| -                  |                              |
| ------------------ | ---------------------------- |
| Accept-Ranges      | 是否接收字节范围请求         |
| Age                | 推算资源创建经过时间         |
| ETag               | 资源的匹配信息               |
| Location           | 令客户端重定向至指定URI      |
| Proxy-Authenticate | 代理服务器对客户端的认证信息 |
| Retry-After        | 对再次发起请求的时机要求     |
| Server             | HTTP服务器的安装信息         |
| Vary               | 代理服务器缓存的管理信息     |
| WWW-Authenticate   | 服务器对客户端的认证信息     |



**实体首部资源**

| - |                        |
| ---------------- | ---------------------- |
| Allow            | 资源可支持的HTTP方法   |
| Content-Encoding | 实体主体编码方式       |
| Content-Language | 实体主体语言           |
| Content-Length   | 实体主体大小           |
| Content-Location | 替代对应资源的URI      |
| Content-MD5      | 实体主体的报文摘要     |
| Content-Range    | 实体主体的位置范围     |
| Content-Type     | 实体主体的媒体类型     |
| Expires          | 实体主体过期的日期时间 |
| Last-Modified    | 资源的最后修改日期时间 |



**Set-Cookie**

| -            |                                   |
| ------------ | --------------------------------- |
| NAME=VALUE   | 名称                              |
| expires=DATE | 有效期，默认是浏览器关闭为止      |
| path=PATH    | 文件目录作为Cookie的适用对象      |
| domain=域名  |                                   |
| secure       | 仅在HTTPS安全通信时才会发送Cookie |
| HttpOnly     | 使Cookie不能被JavaScript脚本访问  |



## HTTPS

- HTTP缺点：
  - 通信适用明文（不加密），内容可能会被窃听
  - 不验证通信方的身份，可能遭遇伪装
  - 无法证明报文的完整性，可能被篡改

- SSL( Secure Socket Layer )：采用共享密钥加密和公开密钥加密混合

- TLS( Transport Layer Security )

- HTTPS( = HTTP + 加密 + 认证 + 完整性保护 )：HTTP通信接口部分用SSL和TLS代替

- Public-key cryptography( 公开密钥加密 )：公钥加密，私钥解密，解密就是对离散对数进行求值

- Common key crypto system ( 共享密钥加密，对称密钥加密 )：加密和解密用同一个密钥

- CA( Certificate Authority, 数字证书认证机构 )：是客户端与服务器双方都可信赖的三方机构，颁发公钥证书

- 客户端证书：用户自行安装客户端证书

- HTTPS缺点：
  - 通信慢
  - 加密解密消耗CPU和内存



## Web Attack

- 客户端篡改请求：通过URL查询字段、表单、HTTP首部、Cookie等途径把攻击代码传入

- Active attack( 主动攻击 )：以服务器资源为目标，直接访问Web，把攻击代码传入
  - SQL注入攻击
  - OS命令注入

- Passive attack( 被动攻击 )：攻击者诱使用户触发嵌入攻击代码的请求，窃取用户Cookie、恶意使用用户权限
  - 跨站脚本攻击
  - 跨站点请求伪造

- Web安全对策
  - 输入值验证
  - 输出值转义

- XSS( 跨站脚本攻击, Cross-Site Scripting )：指通过存在漏洞的Web注册用户的浏览器内运行非法的HTML标签或JavaScript进行的一种攻击
  - 利用虚假输入表单骗取用户信息
  - 利用脚本窃取用户Cookie值，用户在不知情下帮组攻击者发送恶意请求
  - 显示伪造内容

- CSRF( Cross-Site Request Forgeries, 跨站点请求伪造 )：攻击者通过设置好的陷阱，强制对已完成认证的用户进行非预期的个人信息或设定信息等某些状态更新

- HTTP Header Injection：攻击者通过在响应首部字段插入换行，添加任意响应首部或主体的一种攻击
  - 设置任何Cookie信息
  - 重定向至任意URL
  - 显示任意的主体( HTTP Response Splitting Attack )

- HTTP Response Splitting Attack：向首部主体内添加内容的攻击

- Mail Header Injection：Web应用中的邮件发送，攻击者通过向邮件首部To或Subject内任意添加非法内容，发起攻击

- 目录遍历攻击 （ Directory Traversal, Path Traversal ）：对无意公开的文件目录，通过非法截断其目录路径后，达成访问目的的一种攻击

- 会话劫持（ Session Hijack ）：攻击者通过某种手段拿到用户的会话ID，并非法使用此会话ID伪装成用户
  - 通过非正规生成方法推测会话ID
  - 通过窃听或XSS攻击盗取会话ID
  - 通过会话固定攻击强行获取会话ID

- Dos( Denial of Service attack )：让运行中的服务呈停止状态的攻击
  - 集中利用访问请求造成资源过载，资源用尽的同时，实际上服务也就呈停止状态
  - 通过攻击安全漏洞使服务器停止

- DDoS ( Distributed Denial of Service attack )



## Network

- **Internet connection**: Allows you to send and receive data on the web. It's basically like the street between your house and the shop.

- **TCP/IP**: Transmission Control Protocol and Internet  Protocol are communication protocols that define how data should travel  across the web. This is like the transport mechanisms that let  you place an order, go to the shop, and buy your goods. In our example,  this is like a car or a bike (or however else you might get around).

- **DNS**: Domain Name Servers are like an address book  for websites. When you type a web address in your browser, the browser  looks at the DNS to find the website's real address before it can  retrieve the website. The browser needs to find out which server the  website lives on, so it can send HTTP messages to the right place (see  below). This is like looking up the address of the shop so you can  access it.

- **HTTP**: Hypertext Transfer Protocol is an application [protocol](https://developer.mozilla.org/en-US/docs/Glossary/Protocol) that defines a language for clients and servers to speak to each other. This is like the language you use to order your goods.

- Component files: A website is made up of many  different files, which are like the different parts of the goods you buy  from the shop. These files come in two main types:   

  - **Code files**: Websites are built primarily from HTML, CSS, and JavaScript, though you'll meet other technologies a bit later.
  - **Assets**: This is a collective name for all the  other stuff that makes up a website, such as images, music, video, Word  documents, and PDFs.



## Brower & Server

When you type a web address into your browser (for our analogy that's like walking to the shop):

1. The browser goes to the DNS server, and finds the real address of  the server that the website lives on (you find the address of the shop).
2. The browser sends an HTTP request message to the server, asking it  to send a copy of the website to the client (you go to the shop and  order your goods). This message, and all other data sent between the  client and the server, is sent across your internet connection using  TCP/IP.
3. If the server approves the client's request, the server sends the  client a "200 OK" message, which means "Of course you can look at that  website! Here it is", and then starts sending the website's files to the  browser as a series of small chunks called data packets (the shop  gives you your goods, and you bring them back to your house).
4. The browser assembles the small chunks into a complete website and  displays it to you (the goods arrive at your door — new shiny stuff,  awesome!).



- *Client-side JavaScript* extends the core language by  supplying objects to control a browser and its Document Object Model  (DOM). For example, client-side extensions allow an application to place  elements on an HTML form and respond to user events such as mouse  clicks, form input, and page navigation.
- *Server-side JavaScript* extends the core language by  supplying objects relevant to running JavaScript on a server. For  example, server-side extensions allow an application to communicate with  a database, provide continuity of information from one invocation to  another of the application, or perform file manipulations on a server.

