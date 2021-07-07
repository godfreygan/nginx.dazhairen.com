# 同一域名多个webapp的配置

当一个系统功能越来越丰富时，往往需要将一些功能相对独立的模块剥离出来，独立维护。这样的话，通常，会有多个 webapp。

举个例子：假如 shop.dazhairen.com 站点有好几个模块，goods（商品）、marketing（营销）、user（用户中心）、order（订单）。访问这些应用的方式通过上下文(context)来进行区分:

shop.dazhairen.com/goods/

shop.dazhairen.com/marketing/

shop.dazhairen.com/user/

shop.dazhairen.com/order/


http 的默认端口号是 80，如果在一台服务器上同时启动这 3 个 webapp 应用，都用 80 端口，肯定是不行的。所以，这三个应用需要分别绑定不同的端口号。

用户在实际访问 shop.dazhairen.com.com 站点时，访问不同 webapp，总不能还带着对应的端口号去访问吧。所以，这里需要用到反向代理来做处理。


```text
#实际的服务列表
upstream product_server{
    server shop.dazhairen.com:8081;
}

upstream marketing_server{
    server shop.dazhairen.com:8082;
}

upstream user_server{
    server shop.dazhairen.com:8083;
}

upstream order_server{
    server shop.dazhairen.com:8084;
}

server {

    #默认指向product的server
    location / {
        proxy_pass http://product_server;
    }

    location /product/{
        proxy_pass http://product_server;
    }

    location /marketing/{
        proxy_pass http://marketing_server;
    }

    location /user/{
        proxy_pass http://user_server;
    }

    location /order/{
        proxy_pass http://order_server;
    }
}
```
