---
layout: post
title: Ubuntu 14.04下安装ardrone的 tum_ardrone包
key: 20180802
tags: ROS
  ubuntu
  ardrone
---

在以下程序安装之前，请确保已经在Ubuntu 14.04的环境下安装了ROS的indigo版本。

# ardrone_autonomy : A ROS Driver for ARDrone 1.0 & 2.0
接下来要安装的ardrone驱动是TUM推荐版本
```bash
cd ~
mkdir stacks_for_ros
gedit .bashrc
```
在文件的最后一行加入
```
export ROS_PACKAGE_PATH="${ROS_PACKAGE_PATH}:/home/xxx/stacks_for_ros"
```
保存并关闭文件后，执行
```bash
source .bashrc
```
关闭所有终端，再新开一个终端
```bash
cd stacks_for_ros
git clone https://github.com/tum-vision/ardrone_autonomy.git
rosstack profile && rospack profile
roscd ardrone_autonomy
./build_sdk.sh
ls ./lib
rosmake ardrone_autonomy
```

# Package tum_ardrone
```bash
mkdir -p tum_ardrone_ws/src
cd tum_ardrone_ws/src
git clone https://github.com/tum-vision/tum_ardrone.git -b indigo-devel
cd ..
catkin_make
```
忽略出现的错误
```bash 
source devel/setup.bash
rosdep install tum_ardrone
catkin_make
```

如果出现类似于/usr/bin/ld: cannot find -l***的错误，可以参考[链接](https://blog.csdn.net/yingyujianmo/article/details/49634511)

# Launch the nodes
新开一个终端
```bash
cd tum_ardrone_ws
source devel/setup.bash
roslaunch tum_ardrone ardrone_driver.launch
```
再开一个终端
```bash
cd tum_ardrone_ws
source devel/setup.bash
roslaunch tum_ardrone tum_ardrone.launch
```
