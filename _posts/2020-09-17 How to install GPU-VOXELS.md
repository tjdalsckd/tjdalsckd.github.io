---
title: "GPU-VOXELS 설치방법"
tags:
  - Blog
  - MathJax
  - Jekyll
  - LaTeX
use_math: true
Shell:      console, shell
---

# 1. 필요 라이브러리/ 드라이버 / 환경
* Nvidia Graphic Driver
* CUDA 10.0 버전 이상
* cmake 최신버전
* OpenNI2
* PCL
* ROS
* Ubuntu 16.04

# 2. Nvidia Graphic Driver 설치
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
# 3. ROS Kinetic 설치

```shell
wget https://raw.githubusercontent.com/ROBOTIS-GIT/robotis_tools/master/install_ros_kinetic.sh && chmod 755 ./install_ros_kinetic.sh && bash ./install_ros_kinetic.sh
```
# 3. CUDA 설치
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
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
export PATH=/usr/local/cuda/bin:$PATH
source ~/.bashrc
nvcc --version
```

# 4. cmake 최신버전 설치

```shell
wget https://cmake.org/files/v3.18/cmake-3.18.0.tar.gz
tar -zxvf cmake-3.18.0.tar.gz
cd cmake-3.18.0
./bootstrap
make
make install
```

# 5. VTK 8.2 설치

```shell
wget https://www.vtk.org/files/release/8.2/VTK-8.2.0.tar.gz
tar -zxvf VTK-8.2.0.tar.gz
cd VTK-8.2.0
mkdir build
cd build
cmake ..
make
make install
```


# 5. OpenNI2 설치

```shell
git clone https://github.com/occipital/OpenNI2.git
cd OpenNI2
sudo apt-get install g++
sudo apt-get install libusb-1.0-0-dev
apt-get install openjdk-8-jre
apt-get install openjdk-8-jdk
sudo apt-get install graphviz
make
make install
cd Bin/x64-Release/
./SimpleRead
```

# 6. PCL 설치

```shell
sudo apt install libglew-dev
sudo apt-get install libsqlite3-0 libpcap0.8  
sudo apt-get install libpcap-dev

git clone https://github.com/PointCloudLibrary/pcl.git
cd pcl
mkdir build
cd build
cmake ..
make 
make install
```

# 6. GPU-VOXELS 설치

```shell
apt-get install cmake-qt-gui
apt-get install libglew-dev
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
ENABLE_CUDA에 체크되어있는지 확인
![1](https://user-images.githubusercontent.com/53217819/93412302-ddbc8c80-f8d7-11ea-85c3-43837a1a6b4b.png)
GLM_INCLUDE_DIR이 설정되어 있는지 확인
![2](https://user-images.githubusercontent.com/53217819/93412308-e319d700-f8d7-11ea-9400-83eaf178fae7.png)

```shell
export GPU_VOXELS_MODEL_PATH=/home/sung/workspace/gpu-voxels/packages/gpu_voxels/models/

```