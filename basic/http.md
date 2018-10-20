## TCP/IP协议族

协议：不同硬件、操作系统之间通信需要的规则

TCP/IP：是互联网相关的各类协议族的总称，包括（ IP, TCP, HTTP, FTP, DNS, UDP... ）

TCP/IP层次
- 应用层：向用户提供应用服务时通信的活动（ FTP, DNS, HTTP ）
- 传输层：提供处于网络连接中的两台计算机之间的数据传输（ TCP, UDP ）
- 网络层：通过多台计算机或网络设备传输时，选择传输线路（ IP ）
- 数据链路层：处理连接网络的硬件部分

封装：发送端在层与层之间传输数据时，每经过一层必定被打上该层所属的首部信息

IP地址：指节点被分配到的地址，可变

MAC地址：指网卡所属的固定地址，基本不变

ARP( Address Resolution Protocol )：根据通信方的IP地址解析出对应的MAC地址

字节流服务( Byte Stream Service )：为了方便传输，将大块数据分割成以segment为单位的数据包进行管理

三次握手( three-way handshaking )：数据包发送出去后，会向对方确认是否成功送达

1. 发送端：发送带SYN( synchronize )的数据包给对方
2. 接收端：接收后，回传带SYN/ACK的数据包确认
3. 发送端：回传带ACK的数据包，握手结束

DNS( Domain Name System )：提供域名到IP地址之间的解析服务

URI( Uniform Resource Identifier )：用字符串标识某一互联网资源

​	绝对URI：http://user:pass@www.example.jp:80/dir/index/htm?uid=1#ch1 （ 协议方案://登录信息@服务器地址:端口/文件路径?查询字符串#片段标识符 ）

URL( Uniform Resource Locator )：表示资源位置，是URI的子集



## HTTP协议

请求报文构成

- 请求方法
- 请求URI
- 协议版本
- 首部字段
- 内容实体

响应报文构成

- 协议版本
- 状态码( status code )
- 状态码的原因短语( reason-phrase )
- 响应首部字段
- 主体( entity body )



追踪路径( Trace )：设置Header Max-Forwards，经过一个服务器端就减1，等于0时停止传输

Connect：用隧道协议连接代理，使用SSL( Secure Sockets Layer )和TLS( Transport Layer Security )协议把通信内容加密后经网络隧道传输



HTTP方法

| 方法                |                      |
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



持久连接（ HTTP Persistent Connections, HTTP keep-alive, HTTP connection reuse ）：只要任意一端没有提出断开连接，就一直保持TCP连接状态

管线化（ pipelining ）：不用等待响应可直接发送下一个请求

Cookie：在请求和响应报文中写入Cookie信息来控制客户端状态

1. 服务器响应 Header Set-Cookie
2. 客户端保存Cookie
3. 客户端请求带上Cookie



## HTTP报文

HTTP报文：用于HTTP协议交互的信息

- 首部
- 空行( CR+LF )
- 主体

内容编码

- gzip ( GUN zip )
- compress ( UNIX 系统的标准压缩 )
- deflate ( zlib )
- identity ( 不进行编码 )

分块传输编码（ Chunked Transfer Coding ）：把实体主体分割传输，每一块用十六进制标记块的大小，最后一块使用0(CR+LF)

MIME( Multipurpose Internet Mail Extensions )：允许邮件处理文本、图片、视频等多个不同种类的数据



获取部分内容的范围请求：设置 Header Range



## 状态码

|      | 类别          | 原因短语                         |
| ---- | ------------- | -------------------------------- |
| 1XX  | Informational | 接收的请求正在处理               |
| 2XX  | Success       | 请求正常处理完毕                 |
| 3XX  | Redirection   | 浏览器需要进行附加操作以完成请求 |
| 4XX  | Client Error  | 服务器无法处理请求               |
| 5XX  | Server Error  | 服务器处理请求出错               |



200 OK：表示从客户端发来的请求在服务器端被正常处理

204 No Content：服务器接收的请求成功处理，但响应报文不含实体主体部分

206 Partial Content：表示客户端进行范围请求，响应报文包含Content-Range

301 Moved Permanently：永久性重定向，表示请求的资源被分配了新的URI，以后应使用资源现在指向的URI

302 Found：临时重定向，表示请求的资源被分配了新的URI，但不更新书签

303 See Other：表示对应的资源存在另一个URI，应使用GET获取请求资源

400 Bad Request：表示请求报文中存在语法错误

401 Unauthorized：表示发送的请求需要通过HTTP认证

403 Forbidden：表示请求资源的访问被服务器拒绝

404 Not Found：表示服务器上无法找到请求的资源

500 Internal Server Error：表示服务器执行请求时发生错误

503 Service Unavailable：服务器暂时处于超负载或进行停机维护，无法处理请求





## 与HTTP协作的Web服务器





## HTTP 首部























