---
title: "GPU-VOXELS 설치방법"
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

### 1. 필요 라이브러리/ 드라이버 / 환경
* Nvidia Graphic Driver
* CUDA 10.0 버전 이상
* cmake 최신버전
* OpenNI2
* PCL
* ROS
* BOOST
* Ubuntu 16.04

### 2. Nvidia Graphic Driver 설치
https://www.nvidia.com/Download/Find.aspx?lang=en-us 에서 자신의 그래픽 카드 종류와 Ubuntu 버전에 알맞는 드라이버 파일('NVIDIA-Linux-x86_64-xxx.xxx.run') 다운로드.

```shell
sudo apt-get remove nvidia* && sudo apt autoremove 
sudo apt-get install dkms build-essential linux-headers-generic
sudo gedit /etc/modprobe.d/blacklist.conf
```

blacklist.conf 파일을 열고 맨 아래에 다음을 추가한다.
```
blacklist nouveau
blacklist lbm-nouveau
options nouveau modeset=0
alias nouveau off
alias lbm-nouveau off
```

```shell
echo options nouveau modeset=0 | sudo tee -a /etc/modprobe.d/nouveau-kms.conf
sudo update-initramfs -u
sudo init 3
```

init 3 명령을 통해 gui모드를 종료하고 cui모드로 전환 이후 ctrl + f2 키를 이용하여 창 전환
이후 root계정으로 로그인.

```shell
cd /home/user/Downloads
chmod 777 NVIDIA-Linux-x86_64-xxx.xxx.run
./NVIDIA-Linux-x86_64-xxx.xxx.run

sudo apt-get install dkms nvidia-modprobe
sudo lspci - k
nvidia-smi

```
### 3. ROS Kinetic 설치

```shell
wget https://raw.githubusercontent.com/ROBOTIS-GIT/robotis_tools/master/install_ros_kinetic.sh && chmod 755 ./install_ros_kinetic.sh && bash ./install_ros_kinetic.sh
```

### 한글 키보드 설치

```shell
sudo apt-get install fcitx-hangul
```

* `System Settings` -> `Language Support` 
* Language pack 설치
* 'Keyboard input method system' -> `fcitx`로 변경
* reboot

* `System Settings`-> `Keyboard` -> `Shortcuts` ->`Typing` 
* (`Switch to next source`, `Switch to previous source`, `Alternative Characters Key`)-> `Disabled`로 설정
* `Compose Key` -> `Right Alt`로 변경
* `Switch to next source`->  `Multikey`

<br>

#### fcitx 설정

* `fcitx`  → `Configure Current Input Method` 선택
*  `Only Show Current Language` 체크 버튼 해제 -> `+` 버튼 ->  `Hangul` 추가
* `Global Config` 탭  `Trigger Input Method` -> `Multikey`
* `Extra key for trigger input method`-> `Disabled`


#### 18.04 한글키보드

https://gabii.tistory.com/entry/Ubuntu-1804-LTS-%ED%95%9C%EA%B8%80-%EC%84%A4%EC%B9%98-%EB%B0%8F-%EC%84%A4%EC%A0%95

#### 18.04 한글 키

https://hanmaruj.tistory.com/6

### 4. CUDA 설치
https://developer.nvidia.com/cuda-10.0-download-archive 에서 Ubuntu 16.04의 Cuda 설치파일 다운

```shell
sudo init 3
```

ctrl+f2를 이용해 cui모드 전환
```shell
cd /home/user/Downloads
chmod 777 cuda_10.0.130_410.48_linux.run
./cuda_10.0.130_410.48_linux.run
```
설치 중 Nvidia Driver 설치는 No를 하고 다른 사항은 모두 Yes
```shell
gedit ~/.bashrc
```
다음의 내용을 맨 아래에 추가

    export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
    export PATH=/usr/local/cuda/bin:$PATH
    
    
다시 터미널에서 다음과 같이 입력


```shell
source ~/.bashrc
nvcc --version
```


결과가 다음과 같이 나와야한다.



```shell
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2018 NVIDIA Corporation
Built on Sat_Aug_25_21:08:01_CDT_2018
Cuda compilation tools, release 10.0, V10.0.130
```

### 5. cmake 최신버전 설치

```shell
apt-get install libssl-dev
wget https://cmake.org/files/v3.18/cmake-3.18.0.tar.gz
tar -zxvf cmake-3.18.0.tar.gz
cd cmake-3.18.0
./bootstrap
make
make install
```


### 6. Boost 1.58 버전 설치




```shell
wget https://sourceforge.net/projects/boost/files/boost/1.58.0/boost_1_58_0.tar.gz
tar -zxvf boost_1_58_0.tar.gz
cd boost_1_58_0
sudo bash bootstrap.sh
./b2.sh
./b2.sh install
```

### 7. VTK 8.2 설치

```shell
sudo apt-get install libgl1-mesa-dev
sudo apt-get install libxt-dev 
wget https://www.vtk.org/files/release/8.2/VTK-8.2.0.tar.gz
tar -zxvf VTK-8.2.0.tar.gz
cd VTK-8.2.0
mkdir build
cd build
cmake ..
make
make install
```


### 8. OpenNI2 설치

```shell
sudo apt-get install git
git clone https://github.com/occipital/OpenNI2.git
cd OpenNI2
sudo apt-get install g++
sudo apt-get install libusb-1.0-0-dev
sudo apt-get install libudev-dev
sudo apt-get install cmake libx11-dev xorg-dev libglu1-mesa-dev freeglut3-dev libglew1.5 libglew1.5-dev libglu1-mesa libglu1-mesa-dev libgl1-mesa-glx libgl1-mesa-dev libglfw3-dev libglfw3
apt-get install openjdk-8-jre
apt-get install openjdk-8-jdk
sudo apt-get install graphviz

sudo rm /usr/lib/x86_64-linux-gnu/libGL.so
sudo ln -s /usr/lib/libGL.so.1 /usr/lib/x86_64-linux-gnu/libGL.so

sudo rm -r /usr/lib/x86_64-linux-gnu/libEGL.so
sudo ln -s /usr/lib/x86_64-linux-gnu/libEGL.so.1 /usr/lib/x86_64-linux-gnu/libEGL.so

make

sudo ln -s $PWD/Bin/x64-Release/libOpenNI2.so /usr/local/lib/
sudo ln -s $PWD/Bin/x64-Release/OpenNI2/ /usr/local/lib/ 
sudo ln -s $PWD/Include /usr/local/include/OpenNI2
ldconfig
cd Bin/x64-Release/
./SimpleRead
```

### 9. PCL 1.9.1 설치

```shell

sudo apt-get install libeigen3-dev
apt-get install libflann-dev

sudo apt-get install g++ cmake cmake-gui doxygen mpi-default-dev openmpi-bin openmpi-common libeigen3-dev libboost-all-dev libqhull* libusb-dev libgtest-dev git-core freeglut3-dev pkg-config build-essential libxmu-dev libxi-dev libusb-1.0-0-dev graphviz mono-complete qt-sdk libeigen3-dev
sudo apt install libglew-dev
sudo apt-get install libsqlite3-0 libpcap0.8  
sudo apt-get install libpcap-dev

wget https://github.com/PointCloudLibrary/pcl/archive/pcl-1.9.1.tar.gz
tar xvf pcl-1.9.1.tar.gz

cd pcl-pcl-1.9.1
mkdir build
cd build
cmake ..
make 
make install
```
+ 주의) cmake-gui를 이용하여 with openni2를 설정해서 build해야 gpu-voxel 설치시 openni-grubber.h 오류가 발생하지 않는다.

### 10. OROCOS_KDL install

```shell

git clone https://github.com/orocos/orocos_kinematics_dynamics.git
cd  orocos_kinematics_dynamics
mkdir build
cd build
cmake ..
make -j16
make install

```


### 11. OROCOS_KDL install( if you want)

```shell

git clone https://github.com/orocos/orocos_kinematics_dynamics.git
cd  orocos_kinematics_dynamics
mkdir build
cd build
cmake ..
make -j16
make install

```


### 12. GPU-VOXELS 설치

```shell
apt-get install cmake-qt-gui
apt-get install libglew-dev
apt-get install libglm-dev
sudo apt-get install qt5-default
sudo rm /usr/lib/x86_64-linux-gnu/libGL.so
sudo ln -s /usr/lib/libGL.so.1 /usr/lib/x86_64-linux-gnu/libGL.so

git clone https://github.com/fzi-forschungszentrum-informatik/gpu-voxels.git
cd gpu-voxels/
mkdir build
cd build/
cmake ..
cmake-gui ..
```

kdl_parser_DIR  = /opt/ros/kinetic/share/kdl_parser/cmake
orocos_kdl_DIR = /opt/ros/kinetic/share/orocos_kdl

ENABLE_CUDA에 체크되어있는지 확인

![1](https://user-images.githubusercontent.com/53217819/93412302-ddbc8c80-f8d7-11ea-85c3-43837a1a6b4b.png)

GLM_INCLUDE_DIR이 설정되어 있는지 확인

![2](https://user-images.githubusercontent.com/53217819/93412308-e319d700-f8d7-11ea-9400-83eaf178fae7.png)

```shell
export GPU_VOXELS_MODEL_PATH=/home/sung/workspace/gpu-voxels/packages/gpu_voxels/models/

```
### 13. 추가 명령어

```shell
 ./urdf_loader_ros_listener -r -135 -p 0 -y 90
  ./distance_ros_demo-r -135 -p 0 -y 90
 
```
### 14. installation guides
https://github.com/roboticslab-uc3m/installation-guides

### librealsense install
16.04
```shell
sudo apt-key adv --keyserver keys.gnupg.net --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE || sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE In case the public key still cannot be retrieved, check and specify proxy settings: export http_proxy="http://<proxy>:<port>"
```
Ubuntu 16 LTS:
```shell
sudo add-apt-repository "deb http://realsense-hw-public.s3.amazonaws.com/Debian/apt-repo xenial main" -u
```
Ubuntu 18 LTS:
```shell
sudo add-apt-repository "deb http://realsense-hw-public.s3.amazonaws.com/Debian/apt-repo bionic main" -u
```
