# 《深入理解Nginx模块开发与架构设计第二版》读书笔记

​	首先这本书是卖的最火的，作者也在极客时间上开了课，质量肯定没的说，其次，在浏览各种关于nginx帖子的时候，也有很多推荐这本，看看了这本书的目录，从空可以分为三大部分，第一大部分是用来讲解如何使用nginx，这块不不需要任何的c开发基础，甚至一般的运维也可以看得懂，到了第二块，重点讲解的是基于nginx的规范开发http模块，这块就需要c语言基础了，第三部分讲解的nginx现有的架构体系，各个模块的关系等等，可以做到提高对nginx的认知和理解。

## 1、Nginx基础认知

###   1.1、nginx的简单了解

​	2012年，Nginx荣获年度云计算开发奖（2012 Cloud Award for Developer of the Year），并成长为世界第二大Web服务器。全世界流量最高的前1000名网站中，超过25%都使用Nginx，来处理海量的互联网请求。Nginx已经成为业界高性能Web服务器的代名词。

​	先来看看Nginx的竞争对手——Apache、Lighttpd、Tomcat、Jetty、IIS，它们都是Web服务器，或者叫做WWW（World Wide Web）服务器，相应地也都具备Web服务器的基本功能：基于REST架构风格 [1] ，以统一资源描述符（Uniform Resource Identifier，URI）或者统一资源定位符（Uniform Resource Locator，URL）作为沟通依据，通
过HTTP为浏览器等客户端程序提供各种网络服务。

​	Tomcat和Jetty面向Java语言，先天就是重量级的Web服务器，它的性能与Nginx没有可比性，这里略过。

​    IIS只能在Windows操作系统上运行。Windows作为服务器在稳定性与其他一些性能上都不如类UNIX操作系统，因此，在需要高性能Web服务器的场合下，IIS可能会被“冷落”。

​    Apache的发展时期很长，而且是目前毫无争议的世界第一大Web服务器。Apache有许多优点，如稳定、开源、跨平台等，但它出现的时间太长了，在它兴起的年代，互联网的产业规模远远比不上今天，所以它被设计成了一个重量级的、不支持高并发的Web服务器。在Apache服务器上，如果有数以万计的并发HTTP请求同时访问，就会导致服务器上消耗大量内存，操作系统内核对成百上千的Apache进程做进程间切换也会消耗大量CPU资源，并导致HTTP请求的平均响应速度降低，这些都决定了Apache不可能成为高性能Web服务器，这也促使了Lighttpd和Nginx的出现。

​	Lighttpd和Nginx一样，都是轻量级、高性能的Web服务器，欧美的业界开发者比较钟爱Lighttpd，而国内的公司更青睐Nginx，Lighttpd使用得比较少。

起源：

来自俄罗斯的Igor Sysoev在为Rambler Media 工作期间，使用C语言开发了Nginx。Nginx作为Web服务器，一直为俄罗斯著名的门户网站Rambler Media提供着出色、稳定的服务。

nginx 有哪些特点

1. 更快，正常情况下，单次请求会得到更快的响应，在高峰期间，nginx可以比其他web服务器更快的响应请求。
2. 高扩展性，nginx的设计极具扩展性，完全是由不同功能，不同层次，不同类型并且耦合度极低的模块组成。
3. 高可靠性，Nginx的高可靠性来自于其核心框架代码的优秀设计、模块设计的简单性。
4. 低内存消耗，一般情况下，10000个非活跃的HTTP 长连接在nginx中仅消耗2.5MB内存，这个事nginx支持高并发连接的基础。
5. 单机支持10w+以上的并发连接
6. 提供热部署功能
7. 最自由的BSD许可协议，BSD许可协议不只是允许用户免费使用Nginx，它还允许用户在自己的项目中直接使用或修改Nginx源码，然后发布。

### 1.2、编辑运行nginx

​	使用nginx的必备软件准备

1. GCC编译器，用来编译C语言程序。G++编译器。

   ```
   yum install -y gcc
   yum install -y gcc-c++
   ```

2. PCRE库如果我们在配置文件nginx.conf里使用了正则表达式，那么在编译Nginx时就必须把PCRE
   库编译进Nginx，因为Nginx的HTTP模块要靠它来解析正则表达式

   ```
   yum install -y pcre pcre-devel
   ```

3. zlib库 对HTTP包的内容做gzip格式的压缩，如果我们在nginx.conf里配置了gzip on，并指定对于某些类型（content-type）的HTTP响应使用gzip来进行压缩以减少网络传输量，那么，在编译时就必须把zlib编译进Nginx。

   ```
   yum install -y zlib zlib-devel
   ```

4. OpenSSL开发库，如果我们的服务器不只是要支持HTTP，还需要在更安全的SSL协议上传输HTTP，那么就需要拥有OpenSSL了。

   ```
   yum install -y openssl openssl-devel
   ```

   必要的磁盘目录

   1. Nginx源代码存放目录，存放nginx远吗文件，第三方或者自己写的模块源码。
   2. nginx编译阶段产生的中间文件存放目录，该目录用于放置在configure命令执行后所生成的源文件及目录，以及make命令执行后生成的目标文件和最终连接成功的二进制文件。
   3. 部署目录，该目录存放实际Nginx服务运行期间所需要的二进制文件、配置文件等。默认情况下，该目录为/usr/local/nginx。
   4. 日志文件存放目录。

   内核修改，由于默认的Linux内核参数考虑的是最通用的场景，这明显不符合用于支持高并发访问的Web服务器的定义，所以需要修改Linux内核参数，使得Nginx可以拥有更高的性能。

   具体的是修改`/etc/sysctl.conf`来更改内核参数。

### 1.3、获取nginx源码

​      可以在nginx的官方网站获取nginx的源码包，http://nginx.org/en/download.html，将下载的nginx压缩包放到准备好的nginx源代码目录中，然后解压。

```
tar -zxvf nginx-1.0.14.tar.gz
```

###  1.4、编译安装

 进入nginx目录，执行命令

```
./configure
make
make install
```

解释一下，

1.    configure命令做了大量的“幕后”工作，包括检测操作系统内核和已经安装的软件，参数
   的解析，中间目录的生成以及根据各种参数生成一些C源码文件、Makefile文件等。
2. make命令根据configure命令生成的Makefile文件编译Nginx工程，并生成目标文件、最终
   的二进制文件。
3. make install命令根据configure执行时的参数将Nginx部署到指定的安装目录，包括相关目
   录的建立和二进制文件、配置文件的复制。

### 1.5、config命令

使用`./configure --help`

   大概有四种类型

1. 路径相关的参数，nginx在编译器，运行期和路径相关的各种参数。
2. 编译相关的参数
3. 依赖参数相关参数，PCRE，OpenSSL等
4. 模块相关的参数。

模块分类

1. 事件模块，
2. 默认即编译进入Nginx的HTTP模块。
3. 默认不会编译进入的Nginx的HTTP模块。
4. 邮件代理服务器相关的mail模块。
5. 其他的模块。

### 1.6、nginx的命令行

​	默认方式启动

```
usrlocal/nginx/sbin/nginx
```

 指定配置文件启动

```
usrlocal/nginx/sbin/nginx -c tmpnginx.conf
```

另行指定安装目录启动

```
usrlocal/nginx/sbin/nginx -p usrlocal/nginx/
```

指定全局配置项 -g可以临时指定一些全局配置。

```
usrlocal/nginx/sbin/nginx -g "pid varnginx/test.pid
# 加上-g启动的，就需要加上-g停止。
usrlocal/nginx/sbin/nginx -g "pid varnginx/test.pid;" -s stop
```

测试配置信息是否有问题

```
usrlocal/nginx/sbin/nginx -t
```

显示版本

```
usrlocal/nginx/sbin/nginx -v
```

快速停止服务

```
usrlocal/nginx/sbin/nginx -s stop
```

处理完请求在停止

```
usrlocal/nginx/sbin/nginx -s quit
```

运行中的nginx重读配置项并生效

```
usrlocal/nginx/sbin/nginx -s reload
```

## 2、nginx的配置

​       Nginx拥有大量官方发布的模块和第三方模块，这些已有的模块可以帮助我们实现Web服务器上很多的功能,当我们需要使用这些功能的时候，仅仅需要增加，修改一些配置项就可以。

### 2.1、nginx进程间的关系

​      部署Nginx时都是使用一个master进程来管理多个worker进程，一般情况下，worker进程的数量与服务器上的CPU核心数相等。每一个worker进程都是繁忙的，它们在真正地提供互联网服务，master进程则很“清闲”，只负责监控管理worker进程。worker进程之间通过共享内存、原子操作等一些进程间通信机制来实现负载均衡等功能。

###  2.2、nginx的配置文件

​	

```
user nobody;
worker_processes 8;
error_log varlog/nginx/error.log error;
#pid logs/nginx.pid;
events {
  use epoll;
  worker_connections 50000;
} 
http {
  include mime.types;
  default_type application/octet-stream;
  log_format main '$remote_addr [$time_local] "$request" '
  '$status $bytes_sent "$http_referer" '
  '"$http_user_agent" "$http_x_forwarded_for"';
  access_log logs/access.log main buffer=32k;
  …
｝
```

看看配置文件的组成

块配置项

块配置项由一个块配置项名和一对大括号组成。events、http、server、location、upstream等都是块配置项，块配置项之后是否如“location/webstatic{...}”那样在后面加上参数，取决于解析这个块配置项的模块，不能一概而论，但块配置项一定会用大括号把一系列所属的配置项全包含进来，表示大括号内的配置项同时生效。

注释符号使用#。

单位说明

​	当指定空间大小的时候，可以使用的单位有	K或者k，M或者m。 

​    当指定时间的时候，可以使用的单位，ms，s，m，h，d，w，M，y。

在配置中使用变量，比如

```
log_format main '$remote_addr - $remote_user [$time_local] "$request" '
'$status $bytes_sent "$http_referer" '
'"$http_user_agent" "$http_x_forwarded_for"';
```

### 2.3、nginx的基本配置

nginx在运行时，至少必须加载几个核心模块和一个事件类模块。这些模块运行时所支持的配置项称为基本配置——所有其他模块执行时都依赖的配置项。

按照用户使用，基本配置分为了四类

1. 用于调试、定位问题的配置项。
2. 正常运行的必备配置项。
3. 优化性能的配置项。
4. 事件类配置项（有些事件类配置项归纳到优化性能类，这是因为它们虽然也属于events{}块，但作用是优化性能。

用于调试进程，定位问题的配置项目

是否守护进程运行nginx。

```
daemon on|off; 默认是daemon on；
```

 是否以master/worker方式工作

```
master_process on|off; 默认是on；
```

error日志设置

```
error_log pathfile level;
error_log logs/error.log error;
```

是否处理几个特殊的调试点

```
debug_points[stop|abort]
```

仅对指定的客户端输出debug级别的日志

```
debug_connection[IP|CIDR]
```

这个配置项实际上属于事件类配置，因此，它必须放在events{...}中才有效。

```
events {
   debug_connection 10.224.66.14;
   debug_connection 10.224.57.0/24;
} 
```

限制coredump核心转储文件的大小

```
work_rlimit_core size;
```

指定coredump文件的生成目录

```
working_directory path;
```

正常运行的配置项

​	定义环境变量，可以直接设置操作系统的环境变量。

```
env VAR|VAR=Value
```

 嵌入其他配置文件

```
include mime.types;
include vhost/*.conf;
```

pid文件的路径 保存master进程ID的pid文件存放路径.

```
pid path/file;
pid logs/nginx.pid; （默认）
```

nginx worker进程运行的用户及用户组

```
user username[groupname];
user nobody nobody;(默认)
```

指定nginx worker进程可以打开的最大句柄描述符个数

```
worker_rlimit_nofile limit;
```

限制信号队列

```
worker_rlimit_sigpending limit;
```

优化性能的一些配置项

nginx worker进程的个数 服务器上有多少个cpu 就配置多少个worker

```
worker_processes number;
worker_processes 1;(默认)
```

SSL硬件加速

```
ssl_engine device;
```

系统调用gettimeofday的执行频率

```
timer_resolution t;
```

nginx worker进程的优先级设置 nice值是进程的静态优先级，它的取值范围是–20~+19，–20是最高优先级，+19是
最低优先级。

```
worker_priority nice;
worker_priority 0;(默认)
```

事件类配置项

 是否打开accept锁，accept_mutex是Nginx的负载均衡锁、

```
accept_mutex[off|on]
accept_mutex on;(默认)
```

lock文件的路径

```
lock_file path/file;
lock_file logs/nginx.lock(默认)。
```

使用accept锁后到真正建立连接之间的延迟时间，在使用accept锁后，同一时间只有一个worker进程能够取到accept锁。这个accept锁不是阻塞锁，如果取不到会立刻返回。如果有一个worker进程试图取accept锁而没有取到，它至少要等accept_mutex_delay定义的时间间隔后才能再次试图取锁。

```
accept_mutex_delay Nms;
accept_mutex_delay 500ms;(默认)
```

批量建立新连接

```
multi_accept[on|off];
multi_accept off;
```

每个worker的最大连接数

```
worker_connections number;
```

### 2.4、使用HTTP核心模块配置一个静态的web服务器

​	静态Web服务器的主要功能由ngx_http_core_module模块（HTTP框架的主要成员）实现，当然，一个完整的静态Web服务器还有许多功能是由其他的HTTP模块实现的。

一个典型的静态Web服务器还会包含多个server块和location块。

```
http {
  gzip on;
  upstream {
   …
  }
  …
  server {
   listen localhost:80;
   …
  location /webstatic {
  if …
  {
     …
   }
  root optwebresource;
   …
  }
  location ~* .(jpg|jpeg|png|jpe|gif)$ {
     …
   }
  }
server {
     …
   }
}
```

所有的HTTP配置项都必须直属于http块、server块、location块、upstream块或if块等。

nginx为配置一个完成的静态web服务器提供了非常多的功能，大概可以分为八类

1. 虚拟主机与请求的分发
2. 文件路径的定义
3. 内存和磁盘资源的分配
4. 网络连接的设置
5. MIME类型的设置
6. 对客户端请求的限制
7. 文件操作的优化
8. 对客户端请求的特殊处理

#### 2.4.1、虚拟主机和请求的分发

​        由于IP地址的数量有限，因此经常存在多个主机域名对应着同一个IP地址的情况，这时在nginx.conf中就可以按照server_name（对应用户请求中的主机域名）并通过server块来定义虚拟主机，每个server块就是一个虚拟主机，它只处理与之相对应的主机域名请求。

​      监听端口

   	语法

```
listen address:port[default(deprecated in 0.8.21)|default_server|
[backlog=num|rcvbuf=size|sndbuf=size|accept_filter=filter|deferred|bind|ipv6only=[on|off]|ssl]];
listen 80；属于server配置块
```

​       lister决定了nginx服务如何监听端口，在listen后可以只加IP地址，端口或者主机名，非常灵活。

```
listen 127.0.0.1:8000;
listen 127.0.0.1; #注意：不加端口时，默认监听80端口
listen 8000;
listen *:8000;
listen localhost:8000;
```

​       主机名称

​       语法

```
server_name name[...];
server_name "";(默认)
属于server配置块。
```

比如

```
server_name www.testweb.com 、download.testweb.com;。
```

​       在开始处理一个HTTP请求时，Nginx会取出header头中的Host，与每个server中的
server_name进行匹配，以此决定到底由哪一个server块来处理这个请求。有可能一个Host与
多个server块中的server_name都匹配，这时就会根据匹配优先级来选择实际处理的server块。
server_name与Host的匹配优先级如下：
1）首先选择所有字符串完全匹配的server_name，如www.testweb.com 。
2）其次选择通配符在前面的server_name，如.testweb.com。
3）再次选择通配符在后面的server_name，如www.testweb. 。
4）最后选择使用正则表达式才匹配的server_name，如~^.testweb.com$。

重定向主机名称处理

```
server_name_in_redirect on|off;
server_name_in_redirect on; （默认）
所属块  http、server或者location
```

​       该配置需要配合server_name使用。在使用on打开时，表示在重定向请求时会使用server_name里配置的第一个主机名代替原先请求中的Host头部，而使用off关闭时，表示在重定向请求时使用请求本身的Host头部。

location

  语法

```
location[=|~|~*|^~|@]/uri/{...}
属于配置块 server
```

​     location会尝试根据用户请求中的URI来匹配上面的/uri表达式，如果可以匹配，就选择location{}块中的配置来处理用户请求。当然，匹配方式是多样的，下面介绍location的匹配规则。

​    location的配置规则介绍

1.    =表示把URI作为字符串，以便与参数中的uri做完全匹配。

   ```
   location = / {
   #只有当用户请求是
   /时，才会使用该
   location下的配置
   …
   }
   ```

2.  ~ 表示匹配URI的时候是对字母大小写敏感的。

3. ~* 表示匹配URI的时候忽略字母大小写问题。

4. ^~ 表示匹配URI的时候只需要前半部分和uri参数匹配就可以

   ```
   location ^~ images {
      # 以
      images开始的请求都会匹配上
       …
   }
   ```

5. @ 表示仅用于nginx服务内部请求之间的重定向。

#### 2.4.2、文件路径定义

​	以root方式设置资源路径

```
语法 root path；
默认 root html；
所属配置块 http server location if
```

定义资源文件相对于HTTP请求的根目录

```
#如果有一个请求的URI是/download/index/test.html，那么Web服务器将会返回服务器上# # #optwebhtml/download/index/test.html文件的内容
location /download/ {
    root optwebhtml;
}  
```

使用alias方式设置资源路径

```
语法 alias path;
所属配置块 location
```

alias也是用来设置文件资源路径的，它与root的不同点主要在于如何解读紧跟location后面的uri参数，这将会致使alias与root以不同的方式将用户请求映射到真正的磁盘文件上。例如，如果有一个请求的URI是/conf/nginx.conf，而用户实际想访问的文件在usr/local/nginx/conf/nginx.conf，那么想要使用alias来进行设置的话，可以采用如下方式：

```
location conf {
    alias usr/local/nginx/conf/;
}  
```

root的语法是这样的

```
location conf {
    root usr/local/nginx/;
}
```

#### 2.4.3、访问首页

```
语法 index file ...;
默认 index index.html;
所属配置块 http，server，location
```

有时，访问站点时的URI是/，这时一般是返回网站的首页，而这与root和alias都不同。这里用ngx_http_index_module模块提供的index配置实现。index后可以跟多个文件参数，Nginx将会按照顺序来访问这些文件，

```
location {
   root path;
   index index.html htmlindex.php /index.php;
}
```

接收到请求后，Nginx首先会尝试访问path/index.php文件，如果可以访问，就直接返回文件内容结束请求，否则再试图返回pathhtmlindex.php文件的内容，依此类推。

#### 2.4.4、根据http返回码重定向页面

```
语法： error_page code[code...][=|=answer-code]uri|@named_location
配置块： http、server、location、if
```

当对于某个请求返回错误码时，如果匹配上了error_page中设置的code，则重定向到新的URI中。例如：

```
error_page 404 404.html;
error_page 502 503 504 50x.html;
error_page 403 http://example.com/forbidden.html
; error_page 404 =@fetch;
```

注意，虽然重定向了URI，但返回的HTTP错误码还是与原来的相同。用户可以通过“=”来更改返回的错误码。

```
error_page 404 =200 empty.gif;
error_page 404 =403 forbidden.gif;
```

如果不想修改URI，只是想让这样的请求重定向到另一个location中进行处理，那么可以这样设置

```
location / (
   error_page 404 @fallback;
) location @
fallback (
   proxy_pass http://backend
;)
```

#### 2.4.5、内存和磁盘资源分配

​	HTTP包体只存储到磁盘文件中

```
语法： client_body_in_file_only on|clean|off;
默认： client_body_in_file_only off;
所属配置块： http、server、location
```

解释：当值为非off时，用户请求中的HTTP包体一律存储到磁盘文件中，即使只有0字节也会存储为文件。当请求结束时，如果配置为on，则这个文件不会被删除（该配置一般用于调试、定位问题），但如果配置为clean，则会删除该文件。

   HTTP包体尽量写入到一个内存buffer中

```
语法：client_body_in_single_buffer on|off;
默认：client_body_in_single_buffer off;
所属配置块： http、server、location
```

解释：用户请求中的HTTP包体一律存储到内存buffer中。当然，如果HTTP包体的大小超过了下面client_body_buffer_size设置的值，包体还是会写入到磁盘文件中。

读取http头部的超时时间

```
语法： client_header_timeout time（默认单位：秒）;
默认： client_header_timeout 60;
配置块： http、server、location
```

客户端与服务器建立连接后将开始接收HTTP头部，在这个过程中，如果在一个时间间隔（超时时间）内没有读取到客户端发来的字节，则认为超时，并向客户端返回408("Request timed out")响应。

读取HTTP包体的超时时间

```
语法： client_body_timeout time（默认单位：秒）；
默认： client_body_timeout 60;
配置块： http、server、location
```

此配置项与client_header_timeout相似，只是这个超时时间只在读取HTTP包体时才有效。

对某些浏览器禁用keepalive功能

```
语法： keepalive_disable[msie6|safari|none]...
默认： keepalive_disablemsie6 safari
配置块： http、server、location
```

HTTP请求中的keepalive功能是为了让多个请求复用一个HTTP长连接，这个功能对服务器的性能提高是很有帮助的。但有些浏览器，如IE 6和Safari，它们对于使用keepalive功能的POST请求处理有功能性问题。因此，针对IE 6及其早期版本、Safari浏览器默认是禁用keepalive功能的。

