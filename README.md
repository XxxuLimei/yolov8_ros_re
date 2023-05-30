# yolov8_ros_re  
## 0530:复现yolo在ROS下的目标检测  
1. 准备复现[这个链接](https://www.youtube.com/watch?v=XqibXP4lwgA)里的方法，发现给出的yolobot是ros2的（捂脸...  
2. 需要安装[webots_ros](http://wiki.ros.org/webots_ros),可以直接使用一行命令行安装`sudo apt-get install ros-noetic-webots-ros`;  
3. 创建一个文件夹名为`webots_ws`，进入之后再创建一个`src文件夹`,然后`cd ~/webots_ws/src `，对空间进行初始化`catkin_init_workspace`;   
4. 然后将[Robotics_PicoDegree](https://github.com/Soft-illusion/Robotics_PicoDegree/tree/main)和[darknet_ros](https://github.com/leggedrobotics/darknet_ros)clone到src文件夹下；  
5. 接下来回到`webots_ws`文件夹下，进行`source devel/setup.bash `以及`catkin_make`；  
6. 编译的时候遇到问题`Could NOT find move_base_msgs (missing: move_base_msgs_DIR)`，需要下载`move_base_msgs`:`sudo apt-get install ros-noetic-move-base-msgs`;  
7. 编译`darknet_ros`时的错误：  
- [1.darknet path not found](https://github.com/leggedrobotics/darknet_ros/issues/12)  
- [2.Unsupported gpu architecture 'compute_30'](https://github.com/leggedrobotics/darknet_ros/issues/363)  
- 编译成功：  
![](https://github.com/XxxuLimei/yolov8_ros_re/blob/main/picture/WeChat%20Image_20230530134330.png)  
8. 编译成功`darknet_ros`后，再次编译`catkin_make`  
9. 运行launch文件，获得地图：  
- 首先`source devel/setup.bash`;  
- 然后`roslaunch bringup master.launch`；我这里报了以下错误：   
![](https://github.com/XxxuLimei/yolov8_ros_re/blob/main/picture/WeChat%20Image_20230530132821.png)  
解决方法是[这个链接](https://blog.csdn.net/qq_41925420/article/details/86061739)  
10. 运行`roslaunch bringup master.launch`的时候一直报错`WEBOTS_HOME environment variable not defined.`;在终端添加了`export WEBOTS_HOME=/usr/local/webots`后运行成功。下面是获得的webots和RViz中的图：  
![](https://github.com/XxxuLimei/yolov8_ros_re/blob/main/picture/Screenshot%20from%202023-05-30%2021-03-52.png)  
![](https://github.com/XxxuLimei/yolov8_ros_re/blob/main/picture/Screenshot%20from%202023-05-30%2021-04-23.png)  
11. 
