# 静态站点

有时候，我们需要配置静态站点（即html文件或其它静态资源）。

举例来说：如果所有的静态资源都放在了 `/data/static/` 目录下，我们只需要在nginx配置文件中指定首页以及这个站点的 host 即可。

```text
server {
    # 监听端口 80
    listen  80;

    # 监听域名static.dazhairen.com
    server_name static.dazhairen.com;

    #反向代理的路径（和upstream绑定），location 后面设置映射的路径
    location / {
        root /data/static;
        index index.html;
    }

}
```