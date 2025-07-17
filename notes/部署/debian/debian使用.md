### 快捷键

```sh 
# nano：

**保存并退出**（`Ctrl + X` → `Y` → `Enter`）



```





```sh
# vi 快捷键
# 查询
按下 /
输入 search_word
按 Enter
用 n/N 在结果间导航



```





### 切换源

一键换源脚本：bash <(curl -sSL https://linuxmirrors.cn/main.sh)

地址：[使用方法 - LinuxMirrors](https://linuxmirrors.cn/use/)



**阿里云**

```sh
deb http://mirrors.aliyun.com/debian/ bookworm main contrib non-free
deb-src http://mirrors.aliyun.com/debian/ bookworm main contrib non-free
 
deb http://mirrors.aliyun.com/debian-security/ bookworm/updates main contrib non-free
deb-src http://mirrors.aliyun.com/debian-security/ bookworm/updates main contrib non-free
 
deb http://mirrors.aliyun.com/debian/ bookworm-updates main contrib non-free
deb-src http://mirrors.aliyun.com/debian/ bookworm-updates main contrib non-free
 
deb http://mirrors.aliyun.com/debian/ bookworm-backports main contrib non-free
deb-src http://mirrors.aliyun.com/debian/ bookworm-backports main contrib non-free
```



清华大学

```sh

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware
deb https://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware


```

or

```sh
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free
 
deb https://mirrors.tuna.tsinghua.edu.cn/debian-security/ bookworm/updates main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian-security/ bookworm/updates main contrib non-free
 
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free
 
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free

```



