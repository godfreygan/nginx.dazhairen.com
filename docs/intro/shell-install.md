# 脚本安装

#### 安装说明

- 采用编译方式安装 Nginx, 并将其注册为 systemd 服务

- 安装路径为：`/usr/local/nginx`

- 当前下载安装 `1.19.10` 版本


#### 使用方法

- 默认安装 - 执行以下任意命令即可：

命令方式一：
```bash
$ curl -o- https://gitee.com/turnon/linux-tutorial/raw/master/codes/linux/soft/nginx-install.sh | bash
```

命令方式二：
```bash
$ wget -qO- https://gitee.com/turnon/linux-tutorial/raw/master/codes/linux/soft/nginx-install.sh | bash
```


- 自定义安装 - 下载脚本到本地，然后按照以下格式执行：

```bash
$ sh nginx-install.sh [version]
```


