---
title: CMake简略使用教程
date: 2026-03-17 12:29:47
tags:
 - C/C++
categories:
 - tutorial
---

## 0.CMake
**CMake简介**
CMake 是一个跨平台的开源构建系统生成工具。
它不直接构建软件，而是根据平台无关的配置文件（CMakeLists.txt）生成特定平台的本地构建系统
（如 Linux 的 Makefile、Windows 的 Visual Studio 解决方案、macOS 的 Xcode 项目）

**我们为什么需要CMake**
对于体量较大的工程，手动编译运行需要执行的操作太多
这时候，我们就可以利用CMake，编写CMake文件，实现每一次自动编译

**cmake官方教程文档**
[官方文档](https://cmake.org/cmake/help/latest/guide/tutorial/index.html)

## 1.基础教程（Win）
一个CMake文件至少包含以下命令：
```
cmake_minimum_required(VERSION 3.10) # 指定cmake的版本号

project(WorkerManager VERSION 1.0.0) # 指定项目名称与版本

add_executable(WorkerManager Main.cpp) # 从Main.cpp里创建WorkerManager的可执行文件
```

但实际使用后，最后的代码是这样的：
```
cmake_minimum_required(VERSION 3.10)

project(WorkerManager)

# 设置C++17标准
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# 告诉cmake源文件是utf-8编码的，不然会错误读取并报错
if(MSVC)
    add_compile_options(/utf-8)
endif()

# 可以通过空格分隔多个源文件
add_executable(WorkerManager Main.cpp "source/CEO.cpp" "source/CFO.cpp" "source/Employee.cpp" "source/Worker.cpp" "source/WorkerManager.cpp")
```

补充：学到的新东西
**Vscode下对编码保存和读取的设置**
1. 打开vscode设置界面，搜索encoding
2. 这样会展示所有和encoding有关的配置
    包括vscode保存读取文件的默认编码方式(files:encoding选项)
    和cmake拓展里读取文件的编码方式(Cmake: Output Log Encoding)

## 1.基础教程（Linux）
由于cmake的多平台兼容性，CMakeLists.txt文件的语法在所有平台都是一样的
首先，在有CMakeLists.txt的项目文件夹下，运行
`cmake .`
这条命令会查找当前文件夹下的.txt文件，运行里面的指令，并生成Makefile文件
接下来，使用
`make`
运行Makefile文件，编译成功