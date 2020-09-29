---
title: "GPU-Voxels Example 생성"
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


### Cmake 설정
기존 예제 폴더 내의 CMakeLists.txt 파일을 새로 만든 예제 폴더에 옮기고 수정한다.

```bash
mkdir smc_example
cd smc_example
cp ../example_how_to_link/CMakeLists.txt .
cp ../example_how_to_link/gvl_linkage* .
cp gvl_linkage_test.cpp smc_example.cpp
gedit CMakeLists.txt
```
CMakeLists.txt 파일에서 build할 파일 이름을 수정한다.
```bash
# this is for emacs file handling -*- mode: cmake; indent-tabs-mode: nil -*-

# ======================================
# CMakeLists file to demonstrate how to use GPU Voxels in your own project:
# ======================================

cmake_minimum_required (VERSION 2.8)
project (gvl_linkage_test)

# First we have to find our dependencies:
FIND_PACKAGE(CUDA REQUIRED)
FIND_PACKAGE(icl_core REQUIRED )
FIND_PACKAGE(gpu_voxels REQUIRED)
FIND_PACKAGE(Boost COMPONENTS system REQUIRED)

# This is a quirk and should be removed in upcoming versions
# If you built GPU Voxels without ROS support, remove this.
FIND_PACKAGE(orocos_kdl REQUIRED)

# A little debug info:
MESSAGE(STATUS "GVL include dirs are: ${gpu_voxels_INCLUDE_DIRS}")

# Also we have to inherit some Environment definitions required for our base libs:
add_definitions(
  ${icl_core_DEFINITIONS}
  ${gpu_voxels_DEFINITIONS}
)


# Create a library that uses GPU Voxels:
add_library (gvl_linkage_test_lib gvl_linkage_test_lib.cpp)

target_include_directories (gvl_linkage_test_lib
    PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}
    PUBLIC ${gpu_voxels_INCLUDE_DIRS}
    PUBLIC ${orocos_kdl_INCLUDE_DIRS} # this should be removed in upcoming versions.
    PUBLIC ${CUDA_INCLUDE_DIRS}
)

# Add an executable that calls the lib:
add_executable (smc_example smc_example.cpp)

# Link the executable to the library.
# We currently also have to link against Boost and icl_core...
target_link_libraries (smc_example
    LINK_PUBLIC gvl_linkage_test_lib
    LINK_PUBLIC ${Boost_SYSTEM_LIBRARY}
    LINK_PUBLIC ${icl_core_LIBRARIES}
    LINK_PUBLIC ${gpu_voxels_LIBRARIES}
    LINK_PUBLIC ${CUDA_LIBRARIES}
)


```

cmake-gui를 이용하여 gpu-voxelx와, icl-cores가 빌드된 폴더를 정한다.

```bash

cmake-gui .
```


![Screenshot from 2020-09-25 15-28-45](https://user-images.githubusercontent.com/53217819/94234047-ddf60100-ff43-11ea-87d8-f059118e3f9d.png)
