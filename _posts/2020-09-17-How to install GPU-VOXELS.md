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

Ubuntu 16.04 LTS 버전 기준으로 한글 키보드를 설치하는 방법입니다.

먼저 아래의 명령어를 수행해서 `fcitx-hangul` 패키지를 설치합니다.

~~~
sudo apt-get install fcitx-hangul
~~~

그리고 아래의 절차를 진행합니다.

* `System Settings` 실행
* `Language Support` 아이콘 실행
* 언어팩을 설치하라는 팝업창이 뜨면 '설치' 선택
* 'Keyboard input method system' 항목을 `fcitx`로 변경
* 재부팅

<br>

#### 오른쪽 한/영키(Alt 키)를 이용한 한/영 전환

Unbuntu에서는 기본적으로 오른쪽 <kbd>Alt</kbd> 키가 커맨드 실행 기능으로 맵핑이 되어 있습니다. 한/영 전환 키로 활용하고 싶으면 다음과 같이 세팅하시면 됩니다.

* `System Settings`에서 `Keyboard` 실행
* `Shortcuts` 탭 선택한 후 `Typing` 항목 선택
* 모든 항목(`Switch to next source`, `Switch to previous source`, `Alternative Characters Key`)을 `Disabled`로 설정(<kbd>Back</kbd> 키를 누르면 `Disabled`가 됨)
* `Compose Key` 항목을 `Right Alt`로 변경
* `Switch to next source`를 선택한 다음 오른쪽 <kbd>Alt</kbd> 키(<kbd>한/영</kbd> 키)를 누르면 `Multikey`라는 항목으로 값이 설정됨

<br>

#### fcitx 설정

* 오른쪽 상단 상태바에서 `fcitx` 아이콘 선택 → `Configure Current Input Method` 선택
* `+` 버튼을 눌러 `Hangul` 항목 추가(`+` 버튼 누른 창에서 `Only Show Current Language` 체크 버튼 해제해야 보임)
* `Global Config` 탭으로 변경하여 `Trigger Input Method` 항목을 <kbd>한/영</kbd> 키로 설정(`Multikey`라고 표현됨)
* `Extra key for trigger input method`는 `Disabled`



+ 18.04 한글키보드

https://gabii.tistory.com/entry/Ubuntu-1804-LTS-%ED%95%9C%EA%B8%80-%EC%84%A4%EC%B9%98-%EB%B0%8F-%EC%84%A4%EC%A0%95

+ 18.04 한글 키

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
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
export PATH=/usr/local/cuda/bin:$PATH
source ~/.bashrc
nvcc --version
```

### 5. cmake 최신버전 설치

```shell
wget https://cmake.org/files/v3.18/cmake-3.18.0.tar.gz
tar -zxvf cmake-3.18.0.tar.gz
cd cmake-3.18.0
./bootstrap
make
make install
```


### 6. Boost 최신버전 설치

```shell
wget https://dl.bintray.com/boostorg/release/1.74.0/source/boost_1_74_0.tar.gz
tar -zxvf boost_1_74_0.tar.gz
cd boost_1_74_0
sudo bash bootstrap.sh
sudo bash b2.sh
sudo bash b2.sh install
```

### 7. VTK 8.2 설치

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


### 8. OpenNI2 설치

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

### 9. PCL 설치

```shell
apt-get install libflann-dev
sudo apt-get install g++ cmake cmake-gui doxygen mpi-default-dev openmpi-bin openmpi-common libeigen3-dev libboost-all-dev libvtk5.8-qt4 libvtk5.8 libqhull* libusb-dev libgtest-dev git-core freeglut3-dev pkg-config build-essential libxmu-dev libxi-dev libusb-1.0-0-dev graphviz mono-complete qt-sdk libeigen3-dev
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

### 10. GPU-VOXELS 설치

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
### 11. 추가 명령어

```shell
 ./urdf_loader_ros_listener -r -135 -p 0 -y 90
  ./distance_ros_demo-r -135 -p 0 -y 90
 
```
