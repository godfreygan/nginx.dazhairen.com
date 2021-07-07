# Nginx配置虚拟主机

1）在nginx.conf文件的http大括号中。或者另写文件并引入到 nginx.conf 文件的http大括号中。

```text
server {
    # 监听端口 80
    listen  80;

    # 监听域名test.com
    server_name test.com;

    location / {
            # 相对路径，相对nginx根目录
        root    /data/www/test;

        # 默认跳转到index.html页面
        index   index.html;
        }
}
```


2）创建主机目录

```bash
$ mkdir -p /data/www/test
```


3）新建index.html文件

```bash
$ vim /data/www/test/index.html

// input
this is index page

```


4）重新读取配置文件

```bash
$ nginx -s reload
```


5）如果自己有真实域名，则配置域名dns解析到该服务器上。


6）没有域名，在本地配置host文件

```bash
$ vim /etc/hosts

// input
# 或者用服务器IP地址代替127.0.0.1
127.0.0.1   test.com
```


7）打开浏览器，访问test.com，即可看到：this is index page

