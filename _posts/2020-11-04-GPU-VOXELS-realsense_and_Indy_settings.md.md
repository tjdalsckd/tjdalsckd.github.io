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
su sung
gedit ~/.config/terminator/config

[global_config]
[keybindings]
[layouts]
  [[default]]
    [[[child0]]]
      fullscreen = False
      last_active_term = 4c3448a7-9e67-4c0b-8cec-32f20ade34ba
      last_active_window = True
      maximised = False
      order = 0
      parent = ""
      position = 87:71
      size = 1194, 873
      title = sung@sung: ~
      type = Window
    [[[child1]]]
      order = 0
      parent = child0
      position = 432
      ratio = 0.498281786942
      type = VPaned
    [[[child2]]]
      order = 0
      parent = child1
      position = 594
      ratio = 0.5
      type = HPaned
    [[[child5]]]
      order = 1
      parent = child1
      position = 594
      ratio = 0.5
      type = HPaned
    [[[terminal3]]]
      order = 0
      parent = child2
      profile = default
      type = Terminal
      uuid = d79d1e73-3e8c-4ccf-bb38-d018e744b0a2
    [[[terminal4]]]
      order = 1
      parent = child2
      profile = default
      type = Terminal
      uuid = 4c3448a7-9e67-4c0b-8cec-32f20ade34ba
    [[[terminal6]]]
      order = 0
      parent = child5
      profile = default
      type = Terminal
      uuid = 25b4eafc-bbf7-4182-b72d-eed09abad176
    [[[terminal7]]]
      order = 1
      parent = child5
      profile = default
      type = Terminal
      uuid = 3b9d5f26-9996-4535-89fd-bfa7c42b4965
[plugins]
[profiles]
  [[default]]
    background_image = None
    
    
    

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


#### indy c++ build
g++ -std=c++11 -o test.out test_smc.cpp IndyDCP.cpp IndyDCPConnector.cpp IndyDCPException.cpp
