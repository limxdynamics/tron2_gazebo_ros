# 中文 | [English](README.md)

# tron2_gazebo_ros

TRON2 机器人的 Gazebo Classic 11 仿真工作空间（ROS Noetic）。通过 Tron2HWSim 插件桥接 LimX SDK，提供物理仿真环境。支持 SF（双足）和 WF（轮足）变体。

## 前提条件

- Ubuntu 20.04
- ROS Noetic
- Gazebo Classic 11

## 编译与运行

```bash
mkdir -p ~/limx_ws/src && cd ~/limx_ws/src
git clone https://github.com/limxdynamics/limxsdk-lowlevel.git
git clone https://github.com/limxdynamics/tron2-robot-description.git
git clone https://github.com/limxdynamics/tron2_gazebo_ros.git
cd ~/limx_ws
catkin_make install
source install/setup.bash

echo 'export ROBOT_TYPE=SF_TRON2A' >> ~/.bashrc && source ~/.bashrc
roslaunch tron2_gazebo empty_world.launch
```
