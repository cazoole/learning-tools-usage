# Nginx
```
  高性能的http和反向代理web服务器，同时也提供IMAP、POP3、SMTP等服务。BSD-like协议下发行，
内存占用小、并发性强，支持十万个连接左右。
  1. 无缓存的反向代理加速，
  2. FastCGI支持简单的负载均衡及容错
  3. 处理静态文件、索引文件及自动索引，代开文件描述符缓冲方式加速
  4.  模块化结构，gzip、byte ranges 、 chunked response、sll-filter等
  5. 支持ssl和tlssni
  6. 支持热部署、可运行时升级等。
```
---
## 常用命令
- ./nginx           启动服务
- ./nginx -s stop   关闭服务
- ./nginx -v        版本号
- ./nginx -s reload 热部署
---
## 配置文件
```
  ${NGINX_HOME}/conf/nginx.conf
```
### 全局块
```
   配置文件开始到events块之间。此处可以配置 
   user、group、worker_processes[并发线程数]、pid file[pid 存放的文件]、
   error_log file | stderr [level][配置错误日志级别文件等]、include file[引入配置文件]
```
### events
```
  accept_mutex on | off    当设置为t开启时，将会对多个Nginx进程接收连接进行序列化，防止多个进程对连接的争夺，默认开启
  multi_accept on | off    当开启时，每个worker process都将同时接收多个新到达的网络连接，默认关闭
  use method               强制Nginx服务器选择某种事件驱动模型进行消息处理 类型：select、poll、kqueue、epoll、rtsig、/dev/poll、eventport
  worker_connections number 允许每一个worker process 同时开启的最大连接数，默认512
```
### http块
```
包含http全局块和server块。http全局块负责配置和http传输类型等相关内容的整体配置，server指定对应的服务配置。
server是和虚拟主机相关的块，每个http块含有多个server块，每个server含有一个server全局块和多个location块
```
#### 全局server块
```
本虚拟机监听配置端口、名称和ip配置，以及多个location配置
```
#### location块
```
对指定的名称进行匹配，并进行地址定向、数据缓存和应答控制相关的配置，也可以含三方模块的配置。
location ~ /edu/ { // 配置请求地址，如果含有 /edu/ 则进入当前location进行处理
   proxy_pass   http://127.0.0.1:8080; // 配置代理的服务器地址和端口
}
匹配模式： ～ 表示使用正则表达式；  = 表示完全匹配； ～* 正则且不区分大小写
```
---
## 反向代理
```
  1. 配置负载模块
  upstream myServer {
     ip_hash;  // 四种模式，默认不写，表示轮询; 使用weight表示根据权重轮询; ip_hash 表示根据ip哈希，多用于缓存相关
     server 192.0.0.1:8080;
     server 192.0.0.1:8081;
  }
  2。 配置负载
  location ～ /edu/ {
    proxy_pass http://myServer;
  }
```
---
### 其他
- 动静分离： 1. 动静分开部署，分开访问；2. 合并部署，使用nginx进行分离
- 高可用架构 1. zk+nginx+keepalived



