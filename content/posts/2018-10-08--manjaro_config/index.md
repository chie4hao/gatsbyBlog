---
title: 操作系统搭建流程(备忘)
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

为konsole中的添加透明效果，在`~/.emacs.d/init.el`中添加如下代码,`~/.spacemacs`中配置就太多了，今后在逐渐完善。

``` lisp
(global-hl-line-mode 0)
(defun set-background-for-terminal (&optional frame)
  (or frame (setq frame (selected-frame)))
  "unsets the background color in terminal mode"
  (unless (display-graphic-p frame)
    (set-face-background 'default "unspecified-bg" frame)))
(add-hook 'after-make-frame-functions 'set-background-for-terminal)
(add-hook 'window-setup-hook 'set-background-for-terminal)
```

常用代理

``` bash
git config --global http.proxy 'http://192.168.50.235:1080'
git config --global https.proxy 'http://192.168.50.235:1080'

git config --global --unset http.proxy
git config --global --unset https.proxy


export http_proxy=http://192.168.50.235:1080
export https_proxy=http://192.168.50.235:1080
unset ...
```
