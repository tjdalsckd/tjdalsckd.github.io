---
title: "GPU-Voxels Open Motion Planning Library Example 생성"
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

이 포스트에서는 뉴로메카사의 Indy7을 이용하여 GPU-Voxels 라이브러리를 활용한 Motion Planning 예제를 만든다.
예제를 실행하기 전 GPU-Voxels를 완전하게 설치해야 한다.
### Indy7-description을 다운로드
먼저 neuromeka github에서 indy7의 로봇 모델 파일을 받는다.
```bash
git clone https://github.com/neuromeka-robotics/indy-ros
```

### gvl_ompl_planning example build
gpu-voxles 폴더에 있는 gvl_ompl_planning을 build한다.
build를 성공하기 위해서는 먼저 ros가 설치되어 있으며 ros ompl을 설치해야 한다.
```bash
apt-get install ros-kinetic-ompl
cmake-gui .
```
cmake-gui를 통해 library의 설치여부를 확인한다.
![Screenshot from 2020-09-29 19-43-06](https://user-images.githubusercontent.com/53217819/94548717-18d19f00-028c-11eb-8ab5-044c2fad5efb.png)

위와같이 gpu_voxels_DIR, icl_core_DIR을 gpu-voxels build폴더 내에서 선택해야한다.

정확하게 설정되어 있을 경우 configure과 generate를 누른다.
이후 cmake-gui를 나온 후 make를 이용하여 빌드한다.
```bash
make -j16
./gvl_ompl_planner
```
생성된 gvl_ompl_planner을 실행하면 다음과 같이 나온다. 
```bash
<2020-09-29 19:48:06.825> CudaLog(Info)::cuTestAndInitDevice: Running on GPU 0 (GeForce RTX 2080 Ti)
<2020-09-29 19:48:07.013> RobotLog(Info) Robot::RobotToGPU: Parsing URDF /home/sung/workspace/gpu-voxels/packages/gpu_voxels/models/indy7_coarse/indy7.urdf
<2020-09-29 19:48:07.021> RobotLog(Info) Robot::load: Constructed KDL tree has 6 Joints and 8 segments.
Name of shared buffer swapped: voxel_list_buffer_swapped_1.
Info:    LBKPIECE1: Attempting to use default projection.
Debug:   LBKPIECE1: Planner range detected to be 2.879317
Waiting for Viz. Press Key if ready!
```
이제 다른 터미널 창에서 gpu-voxels의 build폴더에서 bin폴더에 위치한 gpu_voxels_visualizer를 실행한다.
```bash
cd /home/sung/workspace/gpu-voxels/build/bin
./gpu_voxels_visualizer
```
![Screenshot from 2020-09-29 19-52-43](https://user-images.githubusercontent.com/53217819/94549545-5aaf1500-028d-11eb-8efb-b2195f97eaae.png)

gpu_voxels_visualizer가 실행된 후에 gvl_ompl_planning 터미널에서 Enter를 누르면 example이 실행된다.

![Screenshot from 2020-09-29 19-54-24](https://user-images.githubusercontent.com/53217819/94549714-9944cf80-028d-11eb-802f-ca3ea17cf53c.png)

### Indy7 urdf 적용하기
미리 수정된 indy7 urdf파일을 다운 받는다.
```bash
cd /home/sung/workspace/gpu-voxels/packages/gpu_voxels/models
git clone https://github.com/tjdalsckd/gpu-voxels-indy7
```

```bash
gedit /home/sung/workspace/gpu-voxels/gvl_ompl_planning/gvl_ompl_planner_helper.cpp
```
60번째 줄을 다음과 같이 변경한다.
```bash
line 60:     gvl->addRobot("myUrdfRobot", "indy7_coarse/indy7.urdf", true);
```

```bash
cd /home/sung/workspace/gpu-voxels/gvl_ompl_planning
make -j16
```
### constraint 변경

```bash
gedit /home/sung/workspace/gpu-voxels/gvl_ompl_planning/gvl_ompl_planner_helper.cpp
```
gvl_ompl_planner_helper.cpp 파일에서 85부터 87줄은 박스 형태의 Constraint를 추가하는 부분이다.
박스의 시작점과 끝점을 Vector3f로 입력하여 추가할 수 있다.

```bash
line 85:    gvl->insertBoxIntoMap(Vector3f(1.0,1.0,0.0), Vector3f(1.2,1.2,1.2), "myEnvironmentMap", eBVM_OCCUPIED, 2);
line 86:    gvl->insertBoxIntoMap(Vector3f(1.8,1.8,0.0), Vector3f(2.0,2.0,1.2), "myEnvironmentMap", eBVM_OCCUPIED, 2);
line 87:    gvl->insertBoxIntoMap(Vector3f(0.0,0.0,0.0), Vector3f(3.0,3.0,0.01), "myEnvironmentMap", eBVM_OCCUPIED, 2);
```
### goal joint position 변경
목표 joint 각도 변경은 gvl_ompl_planner.cpp파일에서 수정할 수 있다.

```bash
gedit /home/sung/workspace/gpu-voxels/gvl_ompl_planning/gvl_ompl_planner.cpp
```

```bash
line 85:    goal[0] = 3.141592/2;
line 86:    goal[1] = -0.5;
line 87:    goal[2] = 0.0;
line 88:    goal[3] = 0.0;
line 89:    goal[4] = 0.0;
line 90:    goal[5] = 0.0;
```

