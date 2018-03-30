---
layout: post
title: Ubuntu安装AirSim报错ERROR: clang++ and libc++ is necessary to compile AirSim
key: 20180330
tags: AirSim
---

解决方法：

将AirSim文件夹下的./setup.sh中第40行的

```
sudo apt-get update
```

注释掉。