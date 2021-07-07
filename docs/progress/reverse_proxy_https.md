# Https反向代理

对于一些对安全性要求比较高的站点，可能会使用 HTTPS（一种使用 SSL 通信标准的安全 HTTP 协议）。

使用 nginx 配置 https 需要知道以下几点：

- HTTPS 的固定端口号是 443，不同于 HTTP 的 80 端口

- SSL 标准需要引入安全证书，所以在 nginx.conf 中你需要指定证书和它对应的 key

### 配置

和 http 反向代理的配置基本一样，只是在 `Server` 部分配置有点差别，https端口是443，还要配置ssl证书。

```text
#设定实际的服务器列表
upstream zp_server1{
    server 192.168.10.110:9606;
}

server {

	# 监听443端口，主要用于HTTPS协议
    listen  443 ssl;

    # 监听域名localhost.test-proxy.com
    server_name localhost.test-proxy.com;

    # ssl证书文件位置(常见证书文件格式为：crt/pem)
    ssl_certificate      cert.pem;
    # ssl证书key位置
    ssl_certificate_key  cert.key;

    #反向代理的路径（和upstream绑定），location 后面设置映射的路径
    location / {
        proxy_pass http://zp_server1;
    }

}
```
