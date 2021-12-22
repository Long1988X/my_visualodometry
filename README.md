[toc]

# My_VisualOdometry - 《视觉SLAM十四讲》chapter13

## 1. 目标

1. 精简版的双目视觉里程计：

   > 1. 使用Kitti数据集；
   > 2. 由一个光流追踪的前端和一个局部BA的后端组成。

## 2. 工程框架

1. 编译程序相关的目录：

   > 1. cmake_modules：存放第三方库的Findxxx.cmake文件，用来寻找第三方库的XXX_INLCUDE_DIRS & XXX_LIBRARIES
   > 2. bin：存放程序编译后的，可执行文件；
   > 3. libs：存放程序编译后的，动态库和静态库文件；

2. 程序源码及头文件目录：

   > 4. src 
   > 5. include

3. 由于采用Kitti双目Odometry数据集，需要有读取图像和配置文件的类：

   > 6. Config

4. 单元测试目录：

   > 7. test

## 3. 核心模块及算法

1. 核心模块：FrontEnd & BackEnd

   > 经典视觉SLAM，一般由前端、后端、回环检测和建图四个模块组成；
   >
   > 本精简视觉SLAM实践，只实现了前端和后端；
   >
   > 且二者应该有各自的线程；
   >
   > 1. 前端：
   >
   >    插入图像帧，提取图像中的特征点，然后与上一帧进行光流追踪，计算匹配特征点；
   >
   >    必要时，应该补充新的特征点并做三角化；
   >
   >    前端处理的结果将作为后端优化的初始值；
   >
   > 2. 后端：
   >
   >    输入关键帧和路标点，并对其进行优化；

2. 核心数据结构：Camera & Frame & MapPoint & Feature &Map

   > 1. Camera：
   >
   >    管理相机内参、外参和双目基线，并实现不同坐标系下转换方法：世界坐标系、相机坐标系、图像坐标系、归一化平面；
   >
   > 2. Frame：
   >
   >    管理实践戳、id、关键帧、左右图像、左右图像的特征点；
   >
   > 3. MapPoint：
   >
   >    管理路标点（三维点）的id、outlier、世界坐标系下的位姿、观测时间等；
   >
   > 4. Feature：
   >
   >    持有该Feature的Frame、关键点KeyPoint、关联的路标点MapPoint、outlier等
   >
   > 5. Map：
   >
   >    所有的路标点、激活的路标点、所有的关键帧、激活的关键帧

3. 周边算法：Config & Viewer & DataSet

   > 1. Config：
   >
   >    读取配置文件：camera参数配置文件等；
   >
   > 2. Viewer：
   >
   >    显示当前图像帧，更新显示当前局部地图、显示当前图像帧中的特征点等；
   >
   > 3. DataSet：
   >
   >    获取数据集中的图像、摄像头参数等；

## 4. 实现

1. CMakeLists.txt & cmake_modules
2. 核心数据结构的实现：Camera & Frame & Feature & MapPoint & Map
3. 实现文件配置类：Config
4. 实现数据集读取类：DataSet
5. 实现前端：FrontEnd
6. 实现后端：BackEnd

