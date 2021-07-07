# 负载均衡

程序在实际的运行过程中，大部分都是以集群的方式运行，这时需要使用负载均衡来进行分流，将大量的并发请求分担到多个处理节点。

即使某一个节点故障宕机了，也不会影响整个服务的使用，所以负载均衡也实现了服务的高可用性。

负载均衡实现了横向扩展，这也避免了纵向的升级迭代。


## 基本原理

不管是什么负载均衡技术，无外乎是想建立一对多的映射机制：一个请求入口映射到多个处理请求的节点，从而实现分而治之。

常见的负载均衡技术有：

- DNS轮询

- CDN

- IP负载均衡


### DNS轮询

DNS轮询，是比较简单的负载均衡方式。以域名作为访问入口，通过配置多个DNS A记录使得请求可以分配到不同的服务器。

但是，DNS轮询没有快速的健康检查机制，而且只支持WRR（加权循环）调度策略，很难实现真正的 *均衡*；而且DNS轮询直接将服务器的真实地址暴露出去，不利于服务器的安全。


### CDN

CDN，内容分发网络（Content Delivery Network），通过发布机制将内容同步到大量的缓存节点，并且在DNS服务器上进行扩展，找到离用户最近的缓存节点作为服务提供节点。

一般是购买运营商的CDN节点，价格比较贵。


### IP负载均衡

基于特定的TCP/IP技术实现的负载均衡。如NAT、DR、Turning等。

IP负载均衡可以使用硬件设备实现，也可以使用软件实现。硬件有F5，软件有LVS、Nginx、HAProxy等，本文主要介绍Nginx。


### Nginx实现负载均衡

#### 1）一个服务由多个服务器提供，把负载分配到不同的服务器来处理：

```text
#设定实际的服务器列表
upstream zp_server1{
    server 192.168.10.110:9606;
    server 192.168.10.111:9606;
    server 192.168.10.112:9606;
}

server {
    # 监听端口 80
    listen  80;

    # 监听域名localhost.test.com
    server_name localhost.test.com;

    #反向代理的路径（和upstream绑定），location 后面设置映射的路径
    location / {
        proxy_pass http://zp_server1;
    }

}
```


#### 2）根据服务器的实际情况调整服务器权重。权重值越高分配的请求越多，权重值越低则分配的请求越少。默认权重值为1。

```text
#设定实际的服务器列表
upstream zp_server1{
    server 192.168.10.110:9606;             # default weight=1
    server 192.168.10.111:9606 weight=2;
    server 192.168.10.112:9606;             # default weight=1
}

server {
    # 监听端口 80
    listen  80;

    # 监听域名localhost.test.com
    server_name localhost.test.com;

    #反向代理的路径（和upstream绑定），location 后面设置映射的路径
    location / {
        proxy_pass http://zp_server1;
    }

}
```


#### 3）其它场景

* 最少连接，每个机器处理的请求时间不一样，防止机器负载过高，把请求转发给连接数较少的机器
```text
upstream zp_server1{
    least_conn;    

    server 192.168.10.110:9606;
    server 192.168.10.111:9606;
    server 192.168.10.112:9606;
}
```


* 加权最少连接，在最小连接数的基础上，再加上权重值的判断，属于比较公平的分配方式
```text
upstream zp_server1{
    least_conn;    

    server 192.168.10.110:9606;             # default weight=1
    server 192.168.10.111:9606 weight=2;
    server 192.168.10.112:9606;             # default weight=1
}
```


* IP Hash，ip_hash机制能够让某一客户机在相当长的一段时间内只访问固定的某台真实的服务器，可能导致服务器负载不均衡
```text
upstream zp_server1{
    ip_hash;    

    server 192.168.10.110:9606;
    server 192.168.10.111:9606;
    server 192.168.10.112:9606;
}
```


* 普通Hash，根据url进行hash，同样会导致服务器负载不均衡
```text
upstream zp_server1{
    hash $request_uri;    

    server 192.168.10.110:9606;
    server 192.168.10.111:9606;
    server 192.168.10.112:9606;
}
```

