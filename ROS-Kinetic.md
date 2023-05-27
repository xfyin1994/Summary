# ROS-Kinetic


## 安装教程http://wiki.ros.org/cn/kinetic/Installation/Ubuntu


## 执行`sudo rosdep init`提示报错
```
ERROR: cannot download default sources list from:
https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/sources.list.d/20-default.list
Website may be down.
```
### 处理办法
```
#打开hosts文件
sudo gedit /etc/hosts
#在文件末尾添加
151.101.84.133  raw.githubusercontent.com
#保存后退出再尝试
```
