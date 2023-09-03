# WHAT IS SLAM
1、SLAM 指的是 **Simultaneous Localization and Mapping**，即同时定位与建图。一共包括两个任务，定位、建图。

定位——机器人处于什么位置。

建图——周围的环境是什么样的。

为了实现以上两个功能，需要通过传感器来收集数据，并据此进行推算。常用的传感器有激光雷达、相机、轮式里程计、惯性测量单元（Inertial Measurement Unit，IMU）。

SLAM系统的常用框架如下所示，包括传感器数据、前端、后端、回环检测、建图。
