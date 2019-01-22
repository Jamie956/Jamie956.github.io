

> 参考资料：
>
> https://nginx.org/en/docs/
>
> http://tengine.taobao.org/book/index.html
>
> https://www.cnblogs.com/qdhxhz/p/8910174.html



### 概念

Nginx：轻量级的Web 服务器 、反向代理服务器及电子邮件（IMAP/POP3）代理服务器

- 处理静态文件，索引文件以及自动索引； 
- 反向代理加速(无缓存)，简单的负载均衡和容错； 
- FastCGI，简单的负载均衡和容错； 
- 模块化的结构。过滤器包括gzipping, byte ranges, chunked responses, 以及 SSI-filter 。在SSI过滤器中，到同一个 proxy 或者 FastCGI 的多个子请求并发处理；  
- SSL 和 TLS SNI 支持； 

反向代理（Reverse Proxy）：指以代理服务器来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器



Nginx 相对于 Apache 优点： 

1) 高并发响应性能非常好，官方 Nginx 处理静态文件并发 5w/s 

2) 反向代理性能非常强。（可用于负载均衡） 

3) 内存和 cpu 占用率低。（为 Apache 的 1/5-1/10） 

4) 对后端服务有健康检查功能。

5) 支持 PHP cgi 方式和 fastcgi 方式。 

6) 配置代码简洁且容易上手。  



### Log

In case something does not work as expected, you may try to find out the reason in `access.log` and `error.log` files in the directory `/usr/local/nginx/logs` or `/var/log/nginx`. 



### 目录

conf 存放配置文件

html 网页文件

logs 存放日志

sbin   shell启动、停止等脚本



### 命令

```shell
#关闭进程
ps -ef | grep nginx	#查看进程
kill –INT进程号

nginx				#启动
nginx -s stop       # 快速关闭Nginx，可能不保存相关信息，并迅速终止web服务
nginx -s quit       #平稳关闭Nginx，保存相关信息，有安排的结束web服务
nginx -s reload     #因改变了Nginx相关配置，需要重新加载配置而重载
nginx -s reopen     #重新打开日志文件
nginx -c filename   #为 Nginx 指定一个配置文件，来代替缺省的
nginx -t            #不运行，而仅仅测试配置文件。nginx 将检查配置文件的语法的正确性，并尝试打开配置文件中所引用到的文件
nginx -v            #显示 nginx 的版本
nginx -V            #显示 nginx 的版本，编译器版本和配置参数

#启动步骤
nginx -s stop
nginx -t -c conf/nginx.conf
nginx -v
nginx -c conf/nginx.conf	#按照指定配置去启动nginx
```



### 模型

nginx 采用 epoll 模型，异步非阻塞

epoll 对于句柄事件的选择不是遍历的，是事件响应的，就是句柄上事 件来就马上选择出来，不需要遍历整个句柄链表，因此效率非常高  



**master-worker**

nginx在启动后，会有一个master进程和多个worker进程。master进程主要用来管理worker进程，包含：接收来自外界的信号，向各worker进程发送信号，监控worker进程的运行状态，当worker进程退出后(异常情况下)，会自动重新启动新的worker进程。而基本的网络事件，则是放在worker进程中来处理了。多个worker进程之间是对等的，他们同等竞争来自客户端的请求，各进程互相之间是独立的。一个请求，只可能在一个worker进程中处理，一个worker进程，不可能处理其它进程的请求。worker进程的个数是可以设置的，一般我们会设置与机器cpu核数一致



<img src="http://tengine.taobao.org/book/_images/chapter-2-1.PNG"/>





master进程在接到信号后，会先重新加载配置文件，然后再启动新的worker进程，并向所有老的worker进程发送信号，告诉他们可以光荣退休了。新的worker在启动后，就开始接收新的请求，而老的worker在收到来自master的信号后，就不再接收新的请求，并且在当前进程中的所有未处理完的请求处理完成后，再退出。



首先，每个worker进程都是从master进程fork过来，在master进程里面，先建立好需要listen的socket（listenfd）之后，然后再fork出多个worker进程。所有worker进程的listenfd会在新连接到来时变得可读，为保证只有一个进程处理该连接，所有worker进程在注册listenfd读事件前抢accept_mutex，抢到互斥锁的那个进程注册listenfd读事件，在读事件里调用accept接受该连接。当一个worker进程在accept这个连接之后，就开始读取请求，解析请求，处理请求，产生数据后，再返回给客户端，最后才断开连接，这样一个完整的请求就是这样的了。我们可以看到，一个请求，完全由worker进程来处理，而且只在一个worker进程中处理。



nginx为了更好的利用多核特性，提供了cpu亲缘性的绑定选项，我们可以将某一个进程绑定在某一个核上，这样就不会因为进程的切换带来cache的失效



### HTTP反向代理配置

```shell
#启动进程,通常设置成和cpu的数量相等
worker_processes  1;

#全局错误日志
error_log  /nginx/logs/error.log;
error_log  /nginx/logs/notice.log  notice;
error_log  /nginx/logs/info.log  info;

#PID文件，记录当前启动的nginx的进程ID
pid        /nginx/logs/nginx.pid;

#工作模式及连接数上限
events {
    worker_connections 1024;    #单个后台worker process进程的最大并发链接数
}

#设定http服务器，利用它的反向代理功能提供负载均衡支持
http {
    #设定mime类型(邮件支持类型),类型由mime.types文件定义
    include       /nginx/conf/mime.types;
    default_type  application/octet-stream;
    
    #设定日志
    log_format  main  '[$remote_addr] - [$remote_user] [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
                      
    access_log    /nginx-1.10.1/logs/access.log main;
    rewrite_log     on;
    
    #sendfile 指令指定 nginx 是否调用 sendfile 函数（zero copy 方式）来输出文件，对于普通应用，
    #必须设为 on,如果用来进行下载等应用磁盘IO重负载应用，可设置为 off，以平衡磁盘与网络I/O处理速度，降低系统的uptime.
    sendfile        on;
    #tcp_nopush     on;

    #连接超时时间
    keepalive_timeout  120;
    tcp_nodelay        on;
    
    #gzip压缩开关
    #gzip  on;
 
    #设定实际的服务器列表 
    upstream myserver{
        server 127.0.0.1:8089;
    }

    #HTTP服务器
    server {
        #监听80端口，80端口是知名端口号，用于HTTP协议
        listen       80;
        
        #定义使用www.xx.com访问
        server_name  www.helloworld.com;
        
        #首页
        index index.html
        
        #指向webapp的目录
        root /src/main/webapp;
        
        #编码格式
        charset utf-8;
        
        #代理配置参数
        proxy_connect_timeout 180;
        proxy_send_timeout 180;
        proxy_read_timeout 180;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarder-For $remote_addr;

        #反向代理的路径（和upstream绑定），location 后面设置映射的路径
        location / {
            proxy_pass http://myserver;
        } 

        #静态文件，nginx自己处理
        location ~ ^/(images|javascript|js|css|flash|media|static)/ {
            root /src/main/webapp/views;
            #过期30天，静态文件不怎么更新，过期可以设大一点，如果频繁更新，则可以设置得小一点。
            expires 30d;
        }
    
        #设定查看Nginx状态的地址
        location /NginxStatus {
            stub_status           on;
            access_log            on;
            auth_basic            "NginxStatus";
            auth_basic_user_file  conf/htpasswd;
        }
    
        #禁止访问 .htxxx 文件
        location ~ /\.ht {
            deny all;
        }
        
        #错误处理页面（可选择性配置）
        #error_page   404              /404.html;
        #error_page   500 502 503 504  /50x.html;
        #location = /50x.html {
        #    root   html;
        #}
    }
}
```

```shell
#核心配置，省略其他，访问localhost:8081/baidu时，反向代理https://www.baidu.com
http {
	server {
        location /baidu {
            proxy_pass https://www.baidu.com/;
        }		
	}
}
```



### 负载均衡配置

假设这样一个应用场景：将应用部署在 192.168.1.11:80、192.168.1.12:80、192.168.1.13:80 
三台linux环境的服务器上。网站域名叫 www.helloworld.com，公网IP为 
192.168.1.11。在公网IP所在的服务器上部署 nginx，对所有请求做负载均衡处理。

```shell
http {
    #设定mime类型,类型由mime.type文件定义
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;
    #设定日志格式
    access_log    /var/log/nginx/access.log;

    #设定负载均衡的服务器列表
    upstream load_balance_server {
        #weigth参数表示权值，权值越高被分配到的几率越大
        server 192.168.1.11:80   weight=5;
        server 192.168.1.12:80   weight=1;
        server 192.168.1.13:80   weight=6;
    }

   #HTTP服务器
   server {
        #侦听80端口
        listen       80;
        
        #定义使用www.xx.com访问
        server_name  www.helloworld.com;

        #对所有请求进行负载均衡请求
        location / {
            root        /root;                 #定义服务器的默认网站根目录位置
            index       index.html index.htm;  #定义首页索引文件的名称
            proxy_pass  http://load_balance_server;#请求转向load_balance_server 定义的服务器列表

            #以下是一些反向代理的配置(可选择性配置)
            #proxy_redirect off;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            #后端的Web服务器可以通过X-Forwarded-For获取用户真实IP
            proxy_set_header X-Forwarded-For $remote_addr;
            proxy_connect_timeout 90;          #nginx跟后端服务器连接超时时间(代理连接超时)
            proxy_send_timeout 90;             #后端服务器数据回传时间(代理发送超时)
            proxy_read_timeout 90;             #连接成功后，后端服务器响应时间(代理接收超时)
            proxy_buffer_size 4k;              #设置代理服务器（nginx）保存用户头信息的缓冲区大小
            proxy_buffers 4 32k;               #proxy_buffers缓冲区，网页平均在32k以下的话，这样设置
            proxy_busy_buffers_size 64k;       #高负荷下缓冲大小（proxy_buffers*2）
            proxy_temp_file_write_size 64k;    #设定缓存文件夹大小，大于这个值，将从upstream服务器传
            
            client_max_body_size 10m;          #允许客户端请求的最大单文件字节数
            client_body_buffer_size 128k;      #缓冲区代理缓冲用户端请求的最大字节数
        }
    }
}
```



### 网站有多个webapp的配置

假如 www.helloworld.com 站点有好几个webapp，finance（金融）、product（产品）、admin（用户中心）。访问这些应用的方式通过上下文(context)来进行区分:

www.helloworld.com/finance/

www.helloworld.com/product/

www.helloworld.com/admin/

我们知道，http的默认端口号是80，如果在一台服务器上同时启动这3个 webapp 应用，都用80端口，肯定是不成的。所以，这三个应用需要分别绑定不同的端口号。

```shell
http {
    #此处省略一些基本配置    
    upstream product_server{
        server www.helloworld.com:8081;
    }    
    upstream admin_server{
        server www.helloworld.com:8082;
    }    
    upstream finance_server{
        server www.helloworld.com:8083;
    }

    server {
        #此处省略一些基本配置
        #默认指向product的server
        location / {
            proxy_pass http://product_server;
        }
        location /product/{
            proxy_pass http://product_server;
        }
        location /admin/ {
            proxy_pass http://admin_server;
        }        
        location /finance/ {
            proxy_pass http://finance_server;
        }
    }
}
```



### HTTPS反向代理配置

- HTTPS 的固定端口号是 443，不同于 HTTP 的 80 端口
- SSL 标准需要引入安全证书，所以在 nginx.conf 中你需要指定证书和它对应的 key

```shell
#HTTP服务器
  server {
      #监听443端口。443为知名端口号，主要用于HTTPS协议
      listen       443 ssl;

      #定义使用www.xx.com访问
      server_name  www.helloworld.com;

      #ssl证书文件位置(常见证书文件格式为：crt/pem)
      ssl_certificate      cert.pem;
      #ssl证书key位置
      ssl_certificate_key  cert.key;

      #ssl配置参数（选择性配置）
      ssl_session_cache    shared:SSL:1m;
      ssl_session_timeout  5m;
      #数字签名，此处使用MD5
      ssl_ciphers  HIGH:!aNULL:!MD5;
      ssl_prefer_server_ciphers  on;

      location / {
          root   /root;
          index  index.html index.htm;
      }
  }
```



### 静态站点配置

```shell
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    gzip on;
    gzip_types text/plain application/x-javascript text/css application/xml text/javascript application/javascript image/jpeg image/gif image/png;
    gzip_vary on;

    server {
        listen       80;
        server_name  static.zp.cn;

        location / {
            root /app/dist;
            index index.html;
            #转发任何请求到 index.html
        }
    }
}
```





### 跨域解决方案

举例：www.helloworld.com 网站是由一个前端 app ，一个后端 app 组成的。前端端口号为 9000， 后端端口号为 8080。

前端和后端如果使用 http 进行交互时，请求会被拒绝，因为存在跨域问题。



在 enable-cors.conf 文件中设置 cors

```shell
# allow origin list
set $ACAO '*';

# set single origin
if ($http_origin ~* (www.helloworld.com)$) {
  set $ACAO $http_origin;
}

if ($cors = "trueget") {
    add_header 'Access-Control-Allow-Origin' "$http_origin";
    add_header 'Access-Control-Allow-Credentials' 'true';
    add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
    add_header 'Access-Control-Allow-Headers' 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
}

if ($request_method = 'OPTIONS') {
  set $cors "${cors}options";
}

if ($request_method = 'GET') {
  set $cors "${cors}get";
}

if ($request_method = 'POST') {
  set $cors "${cors}post";
}
```



`include enable-cors.conf` 来引入跨域配置

```shell
upstream front_server{
  server www.helloworld.com:9000;
}
upstream api_server{
  server www.helloworld.com:8080;
}

server {
  listen       80;
  server_name  www.helloworld.com;

  location ~ ^/api/ {
    include enable-cors.conf;
    proxy_pass http://api_server;
    rewrite "^/api/(.*)$" /$1 break;
  }

  location ~ ^/ {
    proxy_pass http://front_server;
  }
}
```



### Docker 安装 Nginx

```shell
docker pull nginx
NGINX_HOME=/home/jamie/container/nginx
docker run -d --name mynginx \
-v $NGINX_HOME/html:/usr/share/nginx/html \
-v $NGINX_HOME/nginx.conf:/etc/nginx/nginx.conf \
-v $NGINX_HOME/log:/var/log/nginx \
-v $NGINX_HOME/conf.d:/etc/nginx/conf.d \
-p 8081:80 nginx
```



