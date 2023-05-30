# yolov8_ros_re  
# 0530:复现yolo在ROS下的目标检测  
1. 准备复现[这个链接](https://www.youtube.com/watch?v=XqibXP4lwgA)里的方法，发现给出的yolobot是ros2的（捂脸...  
2. 需要安装[webots_ros](http://wiki.ros.org/webots_ros),可以直接使用一行命令行安装`sudo apt-get install ros-noetic-webots-ros`;  
3. 创建一个文件夹名为`webots_ws`，进入之后再创建一个`src文件夹`,然后`cd ~/webots_ws/src `，对空间进行初始化`catkin_init_workspace`;   
4. 然后将[Robotics_PicoDegree](https://github.com/Soft-illusion/Robotics_PicoDegree/tree/main)和[darknet_ros](https://github.com/leggedrobotics/darknet_ros)clone到src文件夹下；  
5. 接下来回到`webots_ws`文件夹下，进行`source devel/setup.bash `以及`catkin_make`；  
6. 
