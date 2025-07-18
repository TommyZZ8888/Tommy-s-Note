# Docker

### 1. 安装docker：

参考：https://www.wonote.com/post/debian-12-xi-tong-nei-an-zhuang-docker-xiang-xi-bu-zou/

参考： [Debian12安装Docker及docker-compose教程 - 有一本小书](http://www.tanorqi.cn/archives/wei-ming-ming-wen-zhang#title-4)

也可参考： [Debian12 安装Docker 和 docker-compose - 陈伦刚的个人博客](https://chenlungang.com/?p=bc50b235-7f68-4489-946c-bd59776f64f8)

```sh
# 更新现有的软件包和包缓存
sudo apt update
sudo apt upgrade

# 安装依赖包  安装一些需要的依赖包，这些包允许 apt 使用 HTTPS 协议来访问 Docker 仓库
sudo apt install apt-transport-https ca-certificates curl software-properties-common

# 添加 Docker 官方 GPG 密钥
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -

# 添加 Docker 存储库
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"

# 再次更新软件包列表
sudo apt update

# 安装 Docker 社区版
sudo apt install docker-ce

# 验证
sudo systemctl status docker

# 添加用户到 docker 组（可选）
sudo usermod -aG docker $USER


# 修改 Docker 配置 （可选）
# 以下配置会增加一段自定义内网 IPv6 地址，开启容器的 IPv6 功能，以及限制日志文件大小，防止 Docker 日志塞满硬盘 (泪的教训)：

cat > /etc/docker/daemon.json << EOF
{
    "log-driver": "json-file",
    "log-opts": {
        "max-size": "20m",
        "max-file": "3"
    },
    "ipv6": true,
    "fixed-cidr-v6": "fd00:dead:beef:c0::/80",
    "experimental":true,
    "ip6tables":true
}
EOF

# 重启
systemctl restart docker
```



/etc/docker/daemon.json

```js
# 添加下面内容到 daemon.json 中
# 此 JSON 配置文件包含了 Docker 的一些重要配置参数
{
    # registry-mirrors 用于配置 Docker 镜像源
    # 国内网络访问 Docker 官方镜像源可能较慢，配置国内镜像源可以加速镜像下载
    # 以下是多个国内镜像源地址，Docker 会依次尝试从这些镜像源下载镜像
    "registry-mirrors": [
        "https://ccr.ccs.tencentyun.com",
        "https://docker.rainbond.cc",
        "https://elastic.m.daocloud.io",
        "https://elastic.m.daocloud.io",
        "https://docker.m.daocloud.io",
        "https://gcr.m.daocloud.io",
        "https://ghcr.m.daocloud.io",
        "https://k8s-gcr.m.daocloud.io",
        "https://k8s.m.daocloud.io",
        "https://mcr.m.daocloud.io",
        "https://nvcr.m.daocloud.io",
        "https://quay.m.daocloud.io"
    ],
    # log-driver 指定 Docker 容器日志的驱动类型
    # json-file 表示使用 JSON 文件来存储容器日志
    "log-driver": "json-file",
    # log-opts 用于配置日志驱动的选项
    "log-opts": {
        # max-size 指定每个日志文件的最大大小
        # 这里设置为 10m，表示每个日志文件最大为 10MB
        "max-size": "10m",
        # max-file 指定日志文件的最大数量
        # 这里设置为 3，表示最多保留 3 个日志文件
        "max-file": "3"
    }
}
```



### 2.安装 docker compose

参考： [Debian12安装Docker及docker-compose教程 - 有一本小书](http://www.tanorqi.cn/archives/wei-ming-ming-wen-zhang#title-4)

```sh
# 手动下载：docker-compose-linux-x86_64手动下载： 地址：https://github.com/docker/compose/releases  ==》离线下载linux-x86_64 执行包docker-compose-linux-x86_64

# 将其执行文件改名放到/usr/local/bin下
sudo mv docker-compose-Linux-x86_64 /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

docker-compose --version

```





### 3.镜像下载 & 运行

```sh
# jdk
docker pull openjdk:17


```

