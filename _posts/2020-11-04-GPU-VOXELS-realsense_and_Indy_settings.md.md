---
title: "GPU-Voxels Realsense Indy7 연동"
tags:
  - Blog
  - MathJax
  - Jekyll
  - LaTeX
use_math: true
Shell:      console, shell
sitemap :
  changefreq : daily
  priority : 1.0
---
### terminator를 이용하여 여러개의 터미널 세팅
```shell
sudo apt-get install terminator
```
수평 분할 ctrl+shift+O
수직 분할 ctrl+shift+t

#### terminal 1

```shell
roslaunch realsense2_camera demo_pointcloud.launch
```

#### terminal 2

```shell
roslaunch indy_driver_py indy7_moveit_dcp.launch robot_ip:=192.168.0.7 robot_name:=NRMK-Indy7
```

#### terminal 3

```shell
cd workspace/gpu-voxels/build/bin/
export GPU_VOXELS_MODEL_PATH=/home/sung/workspace/gpu-voxels/packages/gpu_voxels/models/
./distance_ros_demo -r -135 -p 0 -y 90
```

#### terminal 4

```shell
cd workspace/gpu-voxels/build/bin/
export GPU_VOXELS_MODEL_PATH=/home/sung/workspace/gpu-voxels/packages/gpu_voxels/models/
./gpu_voxels_visualizer
```
![Screenshot from 2020-11-04 20-29-06](https://user-images.githubusercontent.com/53217819/98106218-805aba00-1edc-11eb-8748-ff70a703018f.png)

![Screenshot from 2020-11-04 20-29-25](https://user-images.githubusercontent.com/53217819/98106225-82247d80-1edc-11eb-82f0-df80183c5cdb.png)


