1. 插入KinectV2(链接USB3.0接口)

2. 打开终端，输入```roscore```，打开ROS系统

3. 另开一个终端，启动Kinect相关程序
```
    cd ~/catkin_ws/
    source ~/catkin_ws/devel/setup.bash
    roslaunch kinect2_bridge kinect2_bridge.launch
```

4. 另开一个终端，输入```rviz```,打开Kinect可视化界面，此时应该可以看到RGB和Depth的实时图像

5. 另开一个终端，
```
    cd /home/iccd/Documents/Kinectv2_RGBD_python    //进入程序所在文件夹
    python save_image.py    //执行文件
```
>>注：代码中的```m```表示创建文件夹位置，每换一个扫描场景，手动修改其名称，```n```表示对应文件夹下的文件名

6. 每次按下回车键，自动保存一对RGB+Depth图像，按```Ctrl+Z```结束程序
>>若有重名文件夹清手动删除，不然会报错。