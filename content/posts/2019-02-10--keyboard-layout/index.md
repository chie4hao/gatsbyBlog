---
title: 关于键盘使用经验及hasu改建教程
author: chie4
---

&emsp;&emsp;键盘手感和布局可以说是困扰大多数程序员的难题，以前我也是薄膜键盘的忠实用户，后来我开始寻找替代品，并把目光放在了机械键盘上。从最早的poker2黑轴，再到红轴的大F海魂蓝，Leopold FC980m。最后发现还是60布局的键盘用起来最轻松，适合我这种很少使用鼠标完成大部分工作的人，不用大范围移动手掌便可完成所有100配列键盘的操作。不过很多60键盘的键位并不适合大部分人，就好像我喜欢Ctrl键设在CapsLock的位置，但单位电脑都是windows的，每次都得先把autohotkey启动后才能凑合使用，更不用说实现各种复杂的组合键了，键位的缺失这种问题紧靠ahk是无法解决的。于是我开始寻找通用的解决键盘布局的方案，直到我找到了geekhack这个网站。

&emsp;&emsp;GeekHack是国外最有名的机械键盘论坛。其中最有名的GH60就是geekhack论坛上一帮键盘爱好者自行设计开发制作的一款60%键盘。GH60是完全面向DIY玩家的，主要为了满足各种玩家对于自己想要的layout的追求。还有一位日本叫“hasu”的小哥自己动手做了一款HHKB的板子，然后写了一套开源固件在github。通过改造可以使HHKB实现可编程性。可以将HHKB键位更改成你任意喜欢的键位，最多可以设成七层布局。实现各种不同的组合键。直接更换固件的好处在于每次在一台新机器中，不需要重新设置，即插即用。

&emsp;&emsp;
# 更换步骤
&emsp;&emsp;

#### 1.前期准备
准备符合型号的HHKB键盘和一块Hasu Controller，更换步骤网上教程很多了就不赘述了，详细介绍下如何修改键位配置文件并刷固件。

#### 2.编辑配置文件
首先下载项目，`git clone https://github.com/tmk/tmk_keyboard.git`

```
make -f Makefile KEYMAP=<keymapFileName>
```
#### 3.刷固件
```
dfu-programmer atmega32u4 erase --force
dfu-programmer atmega32u4 erase flash <your_downloaded_file>.hex
dfu-programmer atmega32u4 launch
```
