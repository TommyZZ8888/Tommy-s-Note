# JDK

### apt方式安装

参考： https://cn.linux-console.net/?p=34553

```sh
# 查看可用版本
apt search openjdk-*

# 查看官方仓库中的 JDK 包
apt list -a 'openjdk-*-jdk*'

# 安装jdk
apt install openjdk-17-jdk

# 查看是否安装成功
java -version

# 安装的目录
root@debian:/usr/local/jdk# cd /usr/lib/jvm
root@debian:/usr/lib/jvm# ls
java-1.17.0-openjdk-amd64  java-17-openjdk-amd64  openjdk-17

# 配置环境
vi /etc/environment
添加：
JAVA_HOME="/usr/lib/jvm/java-17-openjdk-amd64"

# 验证是否更改成功
source /etc/environment
echo $JAVA_HOME

# 执行以下命令，会看到
ls -l $JAVA_HOME

root@debian:/usr/lib/jvm# ls -l $JAVA_HOME
总计 32
drwxr-xr-x  2 root root 4096  7月15日 17:21 bin
drwxr-xr-x  4 root root 4096  7月15日 17:20 conf
lrwxrwxrwx  1 root root   42  5月 1日 16:40 docs -> ../../../share/doc/openjdk-17-jre-headless
drwxr-xr-x  3 root root 4096  7月15日 17:21 include
drwxr-xr-x  2 root root 4096  7月15日 17:21 jmods
drwxr-xr-x 72 root root 4096  7月15日 17:20 legal
drwxr-xr-x  5 root root 4096  7月15日 17:21 lib
drwxr-xr-x  3 root root 4096  7月15日 17:20 man
-rw-r--r--  1 root root 1229  5月 1日 16:40 release
```







### 卸载

```sh
# 首先查看所有本地安装的 Java 软件包
sudo dpkg -l | grep 'jdk\|jre'

root@debian:/usr/lib/jvm# sudo dpkg -l | grep 'jdk\|jre'
ii  openjdk-17-jdk:amd64                    17.0.15+6-1~deb12u1                 amd64        OpenJDK Development Kit (JDK)
ii  openjdk-17-jdk-headless:amd64           17.0.15+6-1~deb12u1                 amd64        OpenJDK Development Kit (JDK) (headless)
ii  openjdk-17-jre:amd64                    17.0.15+6-1~deb12u1                 amd64        OpenJDK Java runtime, using Hotspot JIT
ii  openjdk-17-jre-headless:amd64           17.0.15+6-1~deb12u1                 amd64        OpenJDK Java runtime, using Hotspot JIT (headless)

# 执行删除操作
sudo apt purge openjdk-17-jdk:amd64 openjdk-17-jdk-headless:amd64 openjdk-17-jre:amd64 openjdk-17-jre-headless:amd64

# 清除系统中所有剩余的依赖项
sudo apt autoremove --purge

# 最后在 “/etc/environment”文件中删除JAVA_HOME变量
```



# MySQL

### apt方式

参考：https://blog.csdn.net/qq_45738379/article/details/135392581

```sh
# 下载对应系统的mysql-apt-config包
apt install wget -y
wget https://repo.mysql.com/apt/debian/pool/mysql-apt-config/m/mysql-apt-config/mysql-apt-config_0.8.29-1_all.deb

# 安装下载的deb包
apt install dpkg -y
apt install gnupg -y
sudo dpkg -i mysql-apt-config_0.8.29-1_all.deb

# 更新apt
sudo apt update

# 安装mysql-server
sudo apt-get -y install mysql-community-server mysql-server

#安装过程中保持空密码，保持默认强密码模式，直接回车

# 登录mysql8.0，空密码登录，第4步没有设置密码
sudo mysql -uroot

# 进入数据库
use mysql
# 修改root@localhost用户的密码，密码：大写英文 + 特殊字符 + 数字
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '你的密码';
# 创建 root 远程账号，用于远程登录
CREATE USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '你的密码';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
# 重载授权表
FLUSH PRIVILEGES;
# 退出
exit

# 设置开机自启动
systemctl enable mysql && systemctl daemon-reload && systemctl restart mysql && systemctl status mysql



```



若想修改my.cnf的配置

```sh
systemctl stop mysql
rm -rf /var/lib/mysql
vi /etc/mysql/my.cnf

```



```sh
[mysqld]
max_connections = 1000
wait_timeout = 7200
interactive_timeout = 7200
# 1、需要保证这个datadir目录下为空；
# 2、需要保证对datadir目录赋予mysql 权限，执行命令：chown -R mysql:mysql /data/mysql
# datadir=/var/lib/mysql
bind-address=0.0.0.0
default_authentication_plugin=mysql_native_password
# 排除掉 NO_ZERO_DATE and NO_ZERO_IN_DATE
sql_mode=''
# 表名忽略大小写
lower_case_table_names = 1
# 只输出 error 级别的日志
log_error_verbosity = 1
default-time-zone = '+08:00'
[mysql.server]
default-character-set = utf8
[mysqld_safe]
default-character-set = utf8
[client]
default-character-set = utf8

```





### 卸载

```sh
# 使用命令罗列出本机上的所有MySQL相关软件
dpkg --get-selections | grep mysql

# 然后逐一卸载
sudo apt-get --purge remove mysql-server
sudo apt-get --purge remove mysql-client
sudo apt-get --purge remove mysql-common

# 在通过下列命令清除残余
apt-get autoremove
apt-get autoclean
rm /etc/mysql/ -R
rm /var/lib/mysql/ -R

```



# Redis

### apt方式

参考：https://blog.csdn.net/SunBoy_1409/article/details/134996932

```sh
# 更新系统
sudo apt update -y

# 安装Redis
sudo apt install redis-server

# 查看Redis状态
sudo systemctl status redis

# 验证
redis-cli

127.0.0.1:6379> keys *

# 设置密码
在 /etc/redis/redis.conf中 ，释放注释 requirepass foobared 
设置密码root，requirepass root

# 开启远程连接
在 /etc/redis/redis.conf中，注释掉 bind 127.0.0.1 -1:1

# 重启
systemctl restart redis

# 测试
root@debian:/usr/local# redis-cli
127.0.0.1:6379> set name tom
(error) NOAUTH Authentication required.
127.0.0.1:6379> auth 123456
OK
127.0.0.1:6379> set name tom
OK
127.0.0.1:6379> get name
"tom"
```









# Nginx

### apt方式



```sh
# 更新包
sudo apt update

# 安装nginx
sudo apt install nginx

# 查看安装路径
whereis nginx

# nginx.conf路径
/etc/nginx

# html路径
/usr/share/nginx/html

# 启动
sudo systemctl start nginx

# 停止
sudo systemctl stop nginx

# 状态
sudo systemctl status nginx

# 开机自启动
sudo systemctl enable nginx
```







# Tomcat







# 宝塔







# 项目部署：

nginx配置

```sh
user www-data;
worker_processes auto;
pid /run/nginx.pid;
error_log /var/log/nginx/error.log;
include /etc/nginx/modules-enabled/*.conf;

events {
    worker_connections 768;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;
    keepalive_timeout  65;

    server {
        listen       8080;
        server_name  192.168.111.155;

        root /home/tom/app/dist;
        index index.html;

        location = /favicon.ico {
            try_files /favicon.ico =404;
        }

        location / {
            try_files $uri $uri/ /index.html;
        }

        location /api/ {
            proxy_pass http://localhost:8090;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
}
```



权限报错：

```sh
server: 192.168.111.155, request: "GET /favicon.ico HTTP/1.1", host: "192.168.111.155:8080", referrer: "http://192.168.111.155:8080/" 2025/07/17 11:11:37 [crit] 25260#25260: *6 stat() "/home/tom/app/dist/index.html" failed (13: Permission denied), client: 192.168.111.152,
```

项目存放路径例：

/home/tom/app；

nginx配置中用户user www-data;

```sh
# 检查项目文件目录权限 确保这些目录和文件的权限允许 www-data 用户访问
ls -ld /home/tom /home/tom/app /home/tom/app/dist /home/tom/app/dist/index.html

# 修改权限 为 www-data 用户授予读取权限
sudo chown -R tom:www-data /home/tom/app/dist
sudo chmod -R g+r /home/tom/app/dist

# 确保目录可访问
sudo chmod o+x /home/tom
sudo chmod o+x /home/tom/app
sudo chmod o+x /home/tom/app/dist
```

