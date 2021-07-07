# 搭建文件服务器

有时候，我们需要归档一些数据或资料，那么文件服务器必不可少。使用 Nginx 可以快速搭建一个简易的文件服务。

Nginx 中的配置要点：

- 将 autoindex 开启可以显示目录，默认不开启。

- 将 autoindex_exact_size 开启可以显示文件的大小。

- 将 autoindex_localtime 开启可以显示文件的修改时间。

- root 用来设置开放为文件服务的根路径。

- charset 设置为 `charset utf-8,gbk;`，可以避免中文乱码问题。


```text
autoindex on;               # 显示目录
autoindex_exact_size on;    # 显示文件大小
autoindex_localtime on;     # 显示文件时间

server {
    charset      utf-8,gbk;

    listen       80;

    server_name  file.dazhairen.com;

    root /data/static;

}
```