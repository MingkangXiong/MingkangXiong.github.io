---
layout: post
title: 替代Teamviewer的桌面远程控制方案（全平台方案）
key: 20201030
tags: 远程控制
  linux
---

# 方案一
使用[RealVNC](https://www.realvnc.com/en/)（免费版不支持文件传输）或者[AnyDesk](https://anydesk.com/zhs)，速度都还不错，值得一试。笔者试过[向日葵](https://sunlogin.oray.com/)的Ubuntu客户端，该客户端存在bug。近期国内推出的[ToDesk](https://www.todesk.com/)可以试试。

# 方案二（NoMachine+ZeroTier）
该方案能够实现高清快速的远程控制。

[NoMachine](https://www.nomachine.com/)一般用于局域网的远程控制，当我们的电脑没有公网ip时将很难使用。但是，[ZeroTier](https://www.zerotier.com/)能帮助我们构建虚拟局域网，从而实现在没有公网ip的情况下也能远程控制。关于ZeroTier的配置参看[https://www.jianshu.com/p/9f7691cb32d3](https://www.jianshu.com/p/9f7691cb32d3)，Ubuntu下安装ZeroTier使用如下语句：

```bash
curl -s https://install.zerotier.com | sudo bash
sudo zerotier-cli join 你的network ID
```

Win10的NoMachine在属性中点击“更改高DPI设置”，勾选上“替代高DPI缩放行为”，画面会清晰很多，如果不习惯使用NoMachine客户端可以换用AnyDesk。不推荐使用vnc客户端。

# 方案三
frp内网穿透方案。笔者不推荐使用该方案，因为需要有一个公网ip的服务器，如果想实现高清快速的远程控制对服务器的带宽有一定的要求。可以考虑使用免费frp服务[SAKURA FRP](https://www.natfrp.com/)。
