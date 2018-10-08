---
title: manjaro构建流程(备忘)
author: chie4
---

# 从零搭建linux系统

最近被安利试了下archlinux，第一次使用pacman和yaourt，不得不说确实比之前用的ubuntu里apt-get配一堆乱七八糟的密钥方便很多。安装所有软件就一行命令，省掉大量折腾时间。从配置各种环境到搭建桌面到开始干活，我只用了一天时间。这次简单做个备忘吧。

         
### ss

第一步肯定是先更新，考虑到国内网络环境，先搭个简单的梯子

``` bash
sudo pacman -S shadowsocks-libev
vim ~/shadowsocks-libev/config.json
```
&emsp;&emsp;

config.json
``` json
{
    "server": "",
    "server_port": 3389,
    "local_port": 1080,
    "password": "",
    "timeout": 60,
    "method": "chacha20-ietf-poly1305",
    "fast_open": true
}
```

之后在`/etc/profile`中加入设置ss开机自动后台运行

```
nohup ss-local -c ~/shadowsocks-libev/config.json &
```

### proxychains

``` bash
sudo pacman -S proxychains-ng
# /etc/proxychains.conf 中添加代理host和port 
```

### spacemacs

``` bash
shdo pacman -S emacs
cd ~
mv .emacs.d .emacs.d.bak
mv .emacs .emacs.bak
git clone https://github.com/syl20bnr/spacemacs ~/.emacs.d.bak
emacs --insecure
```
