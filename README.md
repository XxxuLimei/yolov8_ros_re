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
## 0531：  
1. 运行的时候发现webots提示了好多error,正在逐步修改：  
- [How to adapt your world or PROTO to Webots R2022b](https://github.com/cyberbotics/webots/wiki/How-to-adapt-your-world-or-PROTO-to-Webots-R2022b)，这个在修改的时候，需要给`cam_robot.proto`第一行加上`#VRML_SIM R2021a utf8`才能成功运行`declare_externproto.py`。修正后终端打印如下：  
```
(base) xilm@xilm-MS-7D17:~/fuxian/webots_ws/src/Robotics_PicoDegree/bringup$ python3 declare_externproto.py /usr/local/webots
File "Cam_robot.proto" does not reference any PROTO
File "home.wbt" depends on:
  [LOCAL] Cam_robot.proto
  [OFFICIAL] PortraitPainting.proto
  [OFFICIAL] LandscapePainting.proto
  [OFFICIAL] Window.proto
  [OFFICIAL] Door.proto
  [OFFICIAL] Wall.proto
  [OFFICIAL] Chair.proto
  [OFFICIAL] Table.proto
  [OFFICIAL] Bed.proto
  [OFFICIAL] Oven.proto
  [OFFICIAL] Worktop.proto
  [OFFICIAL] HotPlate.proto
  [OFFICIAL] Sink.proto
  [OFFICIAL] Plate.proto
  [OFFICIAL] Fridge.proto
  [OFFICIAL] TexturedBackground.proto
  [OFFICIAL] BunchOfSunFlowers.proto
  [OFFICIAL] SolidBox.proto
  [OFFICIAL] Cabinet.proto
  [OFFICIAL] CabinetHandle.proto
  [OFFICIAL] Sofa.proto
  [OFFICIAL] Armchair.proto
  [OFFICIAL] Pedestrian.proto
File "4_wheels_robot_fully_loaded.wbt" depends on:
  [LOCAL] Cam_robot.proto
  [OFFICIAL] WoodenBox.proto
  [OFFICIAL] RectangleArena.proto
  [OFFICIAL] TexturedBackgroundLight.proto
  [OFFICIAL] TexturedBackground.proto
```  
2. 按照上面修改完成后，Error少了很多，但是还有如下错误：  
```
ERROR: 'Window.proto': JavaScript error: 'windowHeight' and 'bottomWallHeight' are too big in comparison to 'size.z'.
ERROR: 'Window.proto': JavaScript error: 'windowHeight' and 'bottomWallHeight' are too big in comparison to 'size.z'.
ERROR: 'Window.proto': JavaScript error: 'windowHeight' and 'bottomWallHeight' are too big in comparison to 'size.z'.
ERROR: 'Window.proto': JavaScript error: 'windowHeight' and 'bottomWallHeight' are too big in comparison to 'size.z'.
WARNING: DEF GROUND Solid > Shape > PBRAppearance > ImageTexture: Unable to find resource at '/home/xilm/fuxian/webots_ws/src/Robotics_PicoDegree/bringup/worlds/textures/parquetry.jpg'.
WARNING: DEF GROUND Solid > Transform > Shape > PBRAppearance > ImageTexture: Unable to find resource at '/home/xilm/fuxian/webots_ws/src/Robotics_PicoDegree/bringup/worlds/textures/interlaced_parquetry.jpg'.
WARNING: DEF CEIL Solid > Shape > PBRAppearance > ImageTexture: Unable to find resource at '/home/xilm/fuxian/webots_ws/src/Robotics_PicoDegree/bringup/worlds/textures/roughcast.jpg'.
INFO: ros: Starting controller: /usr/local/webots/projects/default/controllers/ros/ros
[ INFO] [1685500309.652367154]: Robot's unique name is Cam_robot_93077_xilm_MS_7D17.
[ INFO] [1685500309.652388999]: Robot does not have a namespace
[ INFO] [1685500309.657037419]: The controller is now connected to the ROS master.
```  
## 0611:  
1. 启动步骤如下：  
- 打开一个终端，输入`roscore`;  
- 打开另一个终端，来到`/home/xilm/fuxian/webots_ws`路径下，输入`source devel/setup.bash`,接着输入`export WEBOTS_HOME=/usr/local/webots`,然后可以启动仿真`roslaunch bringup master.launch`.  
2. 实在太难改bug了哭:<，准备复现[这篇博客](https://blog.csdn.net/WhiffeYF/article/details/109187804)了；  
3. 这个模型也用到了darknet_ros，编译的时候一定要按照递归的方式克隆，再编译。  
4. 显示的Gazabo模型如下：  
![](https://github.com/XxxuLimei/yolov8_ros_re/blob/main/picture/Screenshot%20from%202023-06-11%2020-08-27.png)  
5. 但是报了很多错：
```
[ WARN] [1686485077.978217388]: link 'base_link' material 'yellow' undefined.
[ WARN] [1686485077.978529635]: link 'base_link' material 'yellow' undefined.
[ WARN] [1686485077.978556638]: link 'camera_link' material 'black' undefined.
[ WARN] [1686485077.978562206]: link 'camera_link' material 'black' undefined.
[ERROR] [1686485077.978602051]: Failed to build tree: parent link [plate_1_link] of joint [camera_joint] not found.  This is not valid according to the URDF spec. Every link you refer to from a joint needs to be explicitly defined in the robot description. To fix this problem you can either remove this joint [camera_joint] from your urdf file, or add "<link name="plate_1_link" />" to your urdf file.
```  

