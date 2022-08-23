---
title: "OpenCV环境搭建"
description: 'opencv源码编译, 以及vscode开发环境设置, 多操作系统环境(Linux, Windows)'
date: '2022-07-26'
slug: 'opencv-env'
image: 'opencvcpp.png'
categories:
    - opencv
tags:
    - opencv
    - c++
---

## Linux Env

Debian 11 amd64  
CMake > 3.0.0  
GCC/G++ 13.0.0  
vscode

### OpenCV编译

#### 1. 安装依赖

```bash
sudo apt-get install gcc g++ cmake
sudo apt-get install build-essential libgtk2.0-dev libavcodec-dev libavformat-dev libjpeg-dev libswscale-dev libtiff5-dev libgtk2.0-dev
```

#### 2. 下载OpenCV源码

```bash
wget https://github.com/opencv/opencv/archive/4.5.5.zip
unzip 4.5.5.zip
cd opencv-4.5.5 && mkdir build && cd build
```

#### 3. 编译OpenCV

```bash
sudo cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local ..
sudo make -j8
sudo make install
```

### vscode开发环境配置

#### 1. 插件安装

- C/C++ Extension Pack

#### 2. 创建CMake工程

- 快捷键 Ctrl+Shift+P
- 选择CMake: Quick Start

#### 3. 修改工程目录

```bash
Project Folder
├─build
├─include
├─src
│   └─ main.cpp
└─CMakeLists.txt
```

#### 4. CMakeLists.txt

```bash
cmake_minimum_required(VERSION 3.0.0)
project(projectName VERSION 0.1.0)

set(CMAKE_CXX_STANDARD 17)
include_directories(${PROJECT_SOURCE_DIR}/include)

find_package(OpenCV REQUIRED)

aux_source_directory(./src SrcFiles)
set(EXECUTABLE_OUTPUT_PATH  ${PROJECT_SOURCE_DIR}/build)
add_executable(main ${SrcFiles})

target_link_libraries(main ${OpenCV_LIBS})

```

## Windows Env

Windows 10 amd64  
vcpkg  
msvc 2022  
vscode  

### vcpkg包管理器

#### 1. 安装配置vcpkg

```bash

git clone git@github.com:microsoft/vcpkg.git
cd vcpkg
bootstrap-vcpkg.bat
```

#### 2. 安装CMake以及VisualStudio 2022 msvc

需要使用VisualStudio 2022安装器，安装msvc C++开发环境。

- [CMake](https://cmake.org/download/)
- [VisualStudio 2022](https://visualstudio.microsoft.com/zh-hans/)

#### 3. vcpkg安装opencv

```bash
vcpkg install opencv4:x64-windows
```

### vscode开发环境配置-win

#### 1. 插件安装-win

- C/C++ Extension Pack

#### 2. 创建CMake工程-win

- 快捷键 Ctrl+Shift+P
- 选择CMake: Quick Start

#### 3. 修改工程目录-win

```bash
Project Folder
├─.vscode
│   └─ c_cpp_properties.json
├─build
├─include
├─src
│   └─ main.cpp
└─CMakeLists.txt
```

#### 4. c_cpp_properties.json

- compilerPath选择编译器的安装路径
- includePath添加vcpkg的include文件夹位置

```json
{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
                "${workspaceFolder}/**",
                "${vcpkgRoot}/x86-windows/include"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE"
            ],
            "windowsSdkVersion": "10.0.19041.0",
            "compilerPath": "C:/Program Files/Microsoft Visual Studio/2022/Community/VC/Tools/MSVC/14.30.30705/bin/Hostx64/x64/cl.exe",
            "cStandard": "c17",
            "cppStandard": "c++17",
            "intelliSenseMode": "windows-msvc-x64",
            "configurationProvider": "ms-vscode.cmake-tools"
        },
        {
            "name": "Win64",
            "includePath": [
                "${workspaceFolder}/**",
                "D:/vcpkg/installed/x64-windows/include"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE"
            ],
            "windowsSdkVersion": "10.0.19041.0",
            "compilerPath": "C:/Program Files/Microsoft Visual Studio/2022/Community/VC/Tools/MSVC/14.30.30705/bin/Hostx64/x64/cl.exe",
            "cStandard": "c17",
            "cppStandard": "c++17",
            "intelliSenseMode": "windows-msvc-x64"
        }
    ],
    "version": 4
}
```

#### 5. CMakeLists.txt

```bash
cmake_minimum_required(VERSION 3.0.0)
project(projectName VERSION 0.1.0)

set(CMAKE_CXX_STANDARD 17)
include_directories(${PROJECT_SOURCE_DIR}/include)

IF(WIN32)
    SET(DCMAKE_TOOLCHAIN_FILE "D:/vcpkg/scripts/buildsystems/vcpkg.cmake")
    include("D:/vcpkg/scripts/buildsystems/vcpkg.cmake")
ENDIF()

find_package(OpenCV REQUIRED)

aux_source_directory(./src SrcFiles)
set(EXECUTABLE_OUTPUT_PATH  ${PROJECT_SOURCE_DIR}/build)
add_executable(main ${SrcFiles})

target_link_libraries(main ${OpenCV_LIBS})
```

## 测试代码

```c++
#include <opencv2/core/mat.hpp>
#include <opencv2/opencv.hpp>

int main(int, char**) {
    cv::VideoCapture cap;
    cap.open("rtmp://127.0.0.1:1935/live/stream");
    int FPS = cap.get(cv::CAP_PROP_FPS);
    while (cap.isOpened())
    {
        cv::Mat frame;
        cap >> frame;

        cv::Size dsize = cv::Size(640, 640);

        cv::Mat reFrame;
        cv::resize(frame, reFrame, dsize);

        cv::imshow("cv", reFrame);
        cv::waitKey(1);
    }
    return 0;
}
```
