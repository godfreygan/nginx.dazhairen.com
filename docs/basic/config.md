# Nginx配置文件

Nginx下载安装好之后，打开安装目录下的conf文件夹，找到 nginx.conf 文件，nginx的默认配置就在这个文件里面。

nginx.conf里面的内容如下：

```text
# 定义Nginx运行的用户和用户组
user  www www;

# 启动进程数，通常设置成和CPU的数量相等
worker_processes  1;
worker_rlimit_nofile 655350;

# 全局错误日志
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

# PID文件
pid        logs/nginx.pid;

# 连接数上限
events {
    worker_connections  1024;
}

# 设定http服务器，利用它的反向代理功能提供负载均衡支持
http {
    
    # 设定mime类型，类型由mime.type文件定义
    include       mime.types;
    default_type  application/octet-stream;

    # 设置日志格式
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" "$request_body"'
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for" $request_time $upstream_response_time';

    access_log  /Users/godfrey/data/logs/nginx/access.log  main;

    # sendfile指令指定nginx是否调用sendfile函数来输出文件，对于普通应用应该设定为on；如果用来进行下载等应用磁盘IO重负载应用，可设置为off，以平衡磁盘与网络IO处理速度，降低系统的uptime
    sendfile        on;
    send_timeout      10s;

    # 开启目录列表访问，适合下载服务器，默认关闭
    autoindex       on;

    # 防止网络阻塞
    tcp_nopush      on;

    # 提高数据的实时响应性
    tcp_nodelay     on;

    # 超时时间，客户端到服务器端的连接持续有效时间，当出现对服务器的后继请求时，可避免建立或重新建立连接
    keepalive_timeout 60s;  

    # 开启gzip压缩，提高文件包传输速度
    gzip            on;
    gzip_comp_level 3;
    gzip_min_length 1024;
    gzip_buffers    4 25k;

    client_header_timeout 10s;
    client_body_timeout 10s;

    # 允许客户端请求的最大单文件大小
    client_max_body_size 100M;

    # 缓冲区代理缓冲用户端请求的最大容量
    client_body_buffer_size 128k;

    # nginx跟后端服务器连接超时时间（代理连接超时时间）
    proxy_connect_timeout   90;

    # 后端服务器数据回传时间（代理发送超时时间）
    proxy_send_timeout   90;

    # 连接成功后，后端服务器响应时间（代理接收超时时间）
    proxy_read_timeout   90;

    # CGI配置
    fastcgi_connect_timeout 300s;
    fastcgi_send_timeout 300s;
    fastcgi_read_timeout 300s;
    fastcgi_buffer_size 64k;
    fastcgi_buffers 4 64k;
    fastcgi_busy_buffers_size 128k;
    fastcgi_temp_file_write_size 128k;
    fastcgi_keep_conn on;
    fastcgi_intercept_errors on;
    fastcgi_max_temp_file_size 2048m;

    # 引用其它配置文件，不同虚拟主机，方便管理维护
    include ./conf.d/*.conf;

}
```