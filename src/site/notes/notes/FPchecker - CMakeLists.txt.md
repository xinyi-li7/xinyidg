---
{"title":"FPchecker - CMakeLists.txt","tags":["install","FPChecker","quick_setup"],"date":"2022-05-12","dg-publish":true,"dg-path":"Blogs/FPchecker - CMakeLists.txt.md","permalink":"/blogs/f-pchecker-c-make-lists-txt/","dgPassFrontmatter":true,"noteIcon":"","created":"2023-02-20T17:56:13.723-07:00","updated":"2023-10-24T01:21:23.879-06:00"}
---


#### Code

cmake_minimum_required(VERSION 3.11)
**set(CMAKE_CXX_COMPILER "/home/xinyi/llvm/llvm10/bin/clang++")**
**set(CMAKE_C_COMPILER "/home/xinyi/llvm/llvm10/bin/clang")**
project(fpchecker VERSION 0.2.0 DESCRIPTION "FPChecker" LANGUAGES CXX C)
**execute_process(COMMAND /home/xinyi/llvm/llvm10/bin/llvm-config --ldflags**
OUTPUT_VARIABLE CMAKE_SHARED_LINKER_FLAGS
OUTPUT_STRIP_TRAILING_WHITESPACE)
**execute_process(COMMAND /home/xinyi/llvm/llvm10/bin/llvm-config --cxxflags**
OUTPUT_VARIABLE CMAKE_CXX_FLAGS
OUTPUT_STRIP_TRAILING_WHITESPACE)
**execute_process(COMMAND /home/xinyi/llvm/llvm10/bin/llvm-config --cppflags**
OUTPUT_VARIABLE CMAKE_CPP_FLAGS
OUTPUT_STRIP_TRAILING_WHITESPACE)
...

#### Reason

Since we used the llvm installed in our home directory ([LLVM - installation](LLVM%20-%20installation.md)), we need to specify the compiler and the path for this llvm. 

A better way is to use export as [Cmake > Specify compiler](../../../notes/Cmake.md#specify-compiler) shows. However, here we modify the `CMakeLists.txt`.

#### Issue

##### Use absolute path

When specify the path for compiler and llvm-config, we need to use absolute path `/home/xinyi..`, or it cannot find the path. The reason see *CMake -- path*.
