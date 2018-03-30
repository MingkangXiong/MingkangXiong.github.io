---
layout: post
title: Ubuntu安装AirSim报错
key: 20180330
tags: AirSim
---
解决方法：

将AirSim文件夹下的./setup.sh中第40行的

```
sudo apt-get update
```

注释掉。