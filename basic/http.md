# Http



## TCP/IP协议族

协议：不同硬件、操作系统之间通信需要的规则

TCP/IP：是互联网相关的各类协议族的总称，包括（ IP, TCP, HTTP, FTP, DNS, UDP... ）

- TCP/IP层次
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



## HTTP报文内的HTTP信息



## 返回结果的HTTP状态码



## 与HTTP协作的Web服务器



HTTP 首部























