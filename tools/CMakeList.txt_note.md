<h1 align="center">CMakeLists.txt 语法 学习笔记</h1>

## 预备知识和说明
关于 `GCC编译器` 和 `GDB调试器` 的内容请查看如下，本文只专注于使用cmake的使用语法教程
- 「[GCC编译器 学习笔记](./gcc_g++_cmd_note.md)」
- 「[GDB调试器 学习笔记](./gdb_note.md)」

正巧在看levelDB，对应的CMakeLists.txt也顺便学习学习。

LevelDB的CMakeLists.txt地址为：https://github.com/google/leveldb/blob/master/CMakeLists.txt

建议在查看基础语法后，可直接查看我把LevelDB的CMakeLists.txt全部标注了一次的说明内容。「[直接点击即可](./CMakeLists.txt)」
## 学习环境
- Centos 8
- gcc 8.4.1
- gdb 8.2-15.el8
- cmake 3.18.2
- make 4.2.1
## 1. Cmake概述

### 1.1 cmake的定义
Cmake 是一个`开源`、`跨平台`的`编译构建工具`，是一个跨平台Makefile生成器。目的是构建、测试、或者打包软件。可以通过简单的平台和编译器独立配置文件的一些简单语法，即可生成在本机环境中正常使用的软件包。

在Cmake首页中的说明：

    CMake is an open-source, cross-platform family of tools designed to build, test and package software. CMake is used to control the software compilation process using simple platform and compiler independent configuration files, and generate native makefiles and workspaces that can be used in the compiler environment of your choice. The suite of CMake tools were created by Kitware in response to the need for a powerful, cross-platform build environment for open-source projects

来源：https://cmake.org/

### 1.2 cmake安装

- 方式1：直接`命令行`方式安装（默认系统版本安装）
    ```sh
    # centos中安装方式
    yum install -y cmake
    # ubuntu中安装方式
    apt-get install -yq cmake
    ```
- 方法2:使用`源码编译`方式安装（此方式一般是指定特定版本需求）
    > 可以到cmake的官网查找特定需要的版本：https://cmake.org/download/

    ```sh
    # download 特定文件（如下为演示使用版本）
    wget https://github.com/Kitware/CMake/releases/download/v3.22.0-rc2/cmake-3.22.0-rc2-linux-x86_64.tar.gz
    #解压文件
    tar -zxvf cmake-3.22.0-rc2-linux-x86_64.tar.gz
    # 打开.bashrc文件，并在末尾行添加env
    vim ~/.bashrc
    #注意自己download到的文件目录位置（一般情况下，在/usr/local下建cmake，然后添加export）
    export PATH=/data/cmake/cmake-3.22.0-rc2-linux-x86_64/bin:$PATH 
    #source .bashrc文件，使得env生效
    source ~/.bashrc
    #最后直接查看版本即可
    ```

- 检查cmake版本信息
    ```sh
    # 如下显示版本为：使用yum install cmake方式安装的默认版本
    cmake --version
    #显示信息
    cmake version 3.18.2

    CMake suite maintained and supported by Kitware (kitware.com/cmake).
    ```

### 1.3 cmake构建方式
```sh
mkdir build #新建build文件的原因：让生成的文件和源代码文件分离，防止混淆。名字随意，但一般习惯设置为build。
cd build
cmake ..
make
```
## 2. 语法规则
CmakeLists.txt语法简单，由命令、注释（#号后为注释，与shell类似）和空格组成。

命令由指令名、小括号和参数组成，指令名不区分大小写。多个参数时，则使用空格进行分隔。
### 2.1 语法格式
基本的语法格式：
```sh
指令(参数1 参数2 参数3 ......) #参数之间使用空格或分号隔开
```
指令不区分大小写，但参数或变量是区分大小写的。

例如（例子为学习《C++ Primer Plus》中使用过）：
```sh
#camke最低版本要求
cmake_minimum_required(VERSION 3.0.0)
CMAKE_MINIMUM_REQUIRED(VERSION 3.0.0)
# 工程名称
project(stock10)
PROJECT(stock10)

# 设置C++ 11标准
set(CMAKE_CXX_STANDARD 11)
SET(CMAKE_CXX_STANDARD 11)
# 编译生成可执行文件
add_executable(stock20.out main.cc stock20.cc stock20.h)
ADD_EXECUTABLE(stock20.out main.cc stock20.cc stock20.h)
```

如果使用set设置的变量，使用 `${}` 方式取值，该方式类似shell语句中变量使用类似。
> IF 等语句中，直接使用变量名即可。

## 3. 常用语法命令

### 3.1 设置版本号
```sh
cmake_minimum_required(VERSION 3.9) #设置最低的cmake版本要求
```
### 3.2 设置项目及其对应版本
```sh
project(leveldb VERSION 1.23.0 LANGUAGES C CXX) # 
```

### 3.3 指定编程语言标准
```sh
#设置C语言标准
set(CMAKE_C_STANDARD 11)

#设置C++标准，标准很多，如98、11、14、17等
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)
```
### 3.4 设置编译类型
```sh
# 例子以Linux为平台
add_executable(leveldbutil "db/leveldbutil.cc") #生成可执行文件 可指定为 xx.out 文件
add_library(common STATIC util.cpp) #直接编译生成静态库，文件以 .a 结尾
add_library(common SHARED util.cpp) #生成动态库或共享库，文件以 .so 结尾
```
## 4. 指定编译包含的源文件
### 4.1 明确指定包含哪些文件
```sh
add_library(stock20.out main.cc stock20.cc stock20.h)
```
### 4.2 搜索所有的cpp文件
```sh
aux_source_directory(dir SRC_LIST) #搜索dir目录下所有的源代码文件并将列表存储到SRC_LIST变量中
aux_source_directory(. SRC_LIST) #当前目录下的所有cpp文件
add_library(demo ${SRC_LIST}) #通过调用列表变量遍历所有文件 ----> 注意使用 ${变量名}
```
### 4.3 设置包含的目录
```sh
include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)
#Linux中还可通过如下方式来设置包含的目录
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -I${CMAKE_CURRENT_SOURCE_DIR}")
```

### 4.4 设置链接库搜索目录
```sh
link_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/libs
)
#Linux中还可使用如下方式设置包含的目录
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -L${CMAKE_CURRENT_SOURCE_DIR}/libs")
```

## 5. 设置target需要链接的库
```sh
target_link_libraries(demo ${target_lib}) #demo为目标库，target_lib为将目标库需要链接的库

target_link_libraries(demo libface.a) # 指定链接静态库libface.a
target_link_libraries(demo libface.so) # 指定链接动态库libface.so

#指定全路径
target_link_libraries(demo ${CMAKE_CURRENT_SOURCE_DIR}/libs/libface.a)
target_link_libraries(demo ${CMAKE_CURRENT_SOURCE_DIR}/libs/libface.so)

#leveldb中指定链接多个库的例子
target_link_libraries("${test_target_name}" leveldb gmock gtest benchmark)
```

## 6. 设置变量
```sh
#set直接设置变量的值
set(SRC_LIST main.cpp test.cpp)
add_executable(demo ${SRC_LIST})
#例子
set(CMAKE_C_STANDARD 11)

# set追加设置变量的值
set(CMAKE_REQUIRED_LIBRARIES ${OLD_CMAKE_REQURED_LIBRARIES})
set(LEVELDB_PORT_CONFIG_DIR "include/port")

# list追加或者删除变量的值
list(APPEND CMAKE_REQUIRED_LIBRARIES kyotocabinet) #APPEND添加、REMOVE_ITEM为删除
```

## 7. 条件控制
类似shell中语句
### 7.1 if...else if ... else ...endif
```sh
#逻辑判断和比较
if (expression) #expression 不为空（0,N,NO,OFF,FALSE,NOTFOUND）时为真
if (not exp) # 与上面相反
#例子
if(NOT CMAKE_C_STANDARD) #如果未设置C标准，执行如下表达式
  # This project can use C11, but will gracefully decay down to C89.
  set(CMAKE_C_STANDARD 11)
  set(CMAKE_C_STANDARD_REQUIRED OFF)
  set(CMAKE_C_EXTENSIONS OFF)
endif(NOT CMAKE_C_STANDARD) #结束

if (COMMAND cmd) #如果 cmd 确实是命令并可调用为真
if (EXISTS dir) if (EXISTS file) #如果目录或文件存在为真
if (file1 IS_NEWER_THAN file2)#当 file1 比 file2 新，或 file1/file2 中有一个不存在时为真，文件名需使用全路径
if (IS_DIRECTORY dir) #当 dir 是目录时为真
if (DEFINED var) #如果变量被定义为真
if (var MATCHES regex)：#给定的变量或者字符串能够匹配正则表达式 regex 时为真，此处 var 可以用 var 名，也可以用 ${var}
if (string MATCHES regex)

# 字符串比较
if (variable STRLESS string)
if (string STRLESS string)
if (variable STRGREATER string)
if (string STRGREATER string)
if (variable STREQUAL string)
if (string STREQUAL string)

#leveldb中的例子
if(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC") #判断字符串是否等于MSVC
```
### 7.2 while
```sh
while(condition)
    ...
endwhile()
```

### 7.3 
```sh
foreach(loop_var RANGE start stop [step]) #start表示起点、stop为终止点、step为步长，类似python语法的for循环
    ...
endforeach(loop_var)
```
## 8. 打印信息
```sh
message(${PROJECT_SOURCE_DIR})
message("build with debug mode")
message(WARNING "this is warnning message")
message(FATAL_ERROR "this build has many error") # FATAL_ERROR 会导致编译失败
```

## 9. 常用变量

### 9.1 预定义变量
```sh
PROJECT_SOURCE_DIR #工程的根目录
PROJECT_BINARY_DIR #运行 cmake 命令的目录，通常是 ${PROJECT_SOURCE_DIR}/build
PROJECT_NAME # 返回通过 project 命令定义的项目名称
CMAKE_CURRENT_SOURCE_DIR #当前处理的 CMakeLists.txt 所在的路径
CMAKE_CURRENT_BINARY_DIR #target 编译目录
CMAKE_CURRENT_LIST_DIR #CMakeLists.txt 的完整路径
CMAKE_CURRENT_LIST_LINE #当前所在的行
CMAKE_MODULE_PATH #定义自己的 cmake 模块所在的路径，SET(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)，然后可以用INCLUDE命令来调用自己的模块
EXECUTABLE_OUTPUT_PATH #重新定义目标二进制可执行文件的存放位置
LIBRARY_OUTPUT_PATH #重新定义目标链接库文件的存放位置
```
### 9.2 环境变量
```sh
$ENV{Name} #环境变量的读取
set(ENV{Name} value) #设置环境变量
```
### 9.3 缓存变量
```sh
set(var "value" CACHE <Type> "" FORCE) 
# var只能有一个，value为不固定
# type：BOOL----> 有ON/OFF，两种取值；STRING ---> 字符串；PATH ---> 目录路径等。
#cache的作用：CACHE关键字。缓存中存在同名的变量，由FORCE来决定，没有FORCE，语句不起作用，使用缓存中的变量。存在FORCE，使用当前设置的值。

#leveldb中的例子
set(BENCHMARK_ENABLE_TESTING OFF CACHE BOOL "" FORCE) 
set(BENCHMARK_ENABLE_EXCEPTIONS OFF CACHE BOOL "" FORCE)
```
### 9.4 系统信息
```sh
CMAKE_MAJOR_VERSION #cmake 主版本号，比如 3.4.1 中的 3
­CMAKE_MINOR_VERSION #cmake 次版本号，比如 3.4.1 中的 4
­CMAKE_PATCH_VERSION #cmake 补丁等级，比如 3.4.1 中的 1
­CMAKE_SYSTEM #系统名称，比如 Linux-­2.6.22
­CMAKE_SYSTEM_NAME #不包含版本的系统名，比如 Linux
­CMAKE_SYSTEM_VERSION #系统版本，比如 2.6.22
­CMAKE_SYSTEM_PROCESSOR #处理器名称，比如 i686
­UNIX # 在所有的类 UNIX 平台下该值为 TRUE，包括 OS X 和 cygwin
­WIN32 #在所有的 win32 平台下该值为 TRUE，包括 cygwin
```

### 9.5 开关选项
```sh
BUILD_SHARED_LIBS # 这个开关用来控制默认的库编译方式，如果不进行设置，使用 add_library 又没有指定库类型的情况下，默认编译生成的库都是静态库。如果 set(BUILD_SHARED_LIBS ON) 后，默认生成的为动态库

# leveldb中的例子
if(BUILD_SHARED_LIBS)
  # Only export LEVELDB_EXPORT symbols from the shared library.
  add_compile_options(-fvisibility=hidden)
endif(BUILD_SHARED_LIBS)


CMAKE_C_FLAGS #设置 C 编译选项，也可以通过指令 add_definitions() 添加
CMAKE_CXX_FLAGS #设置 C++ 编译选项，也可以通过指令 add_definitions() 添加

#在leveldb中的例子
if(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
  # Disable C++ exceptions.
  string(REGEX REPLACE "/EH[a-z]+" "" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /EHs-c-")
  add_definitions(-D_HAS_EXCEPTIONS=0)

  # Disable RTTI.
  string(REGEX REPLACE "/GR" "" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /GR-")
else(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
  # Enable strict prototype warnings for C code in clang and gcc.
  if(NOT CMAKE_C_FLAGS MATCHES "-Wstrict-prototypes")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wstrict-prototypes")
  endif(NOT CMAKE_C_FLAGS MATCHES "-Wstrict-prototypes")

  # Disable C++ exceptions.
  string(REGEX REPLACE "-fexceptions" "" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fno-exceptions")

  # Disable RTTI.
  string(REGEX REPLACE "-frtti" "" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fno-rtti")
endif(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
```
## 参考资源
- https://cmake.org/cmake/help/latest/guide/tutorial/index.html
- https://www.hahack.com/codes/cmake/
- https://cmake.org/cmake/help/cmake2.4docs.html

最主要参考的三个博主文章（衷心感谢大佬通俗易懂的总结）
- https://zhuanlan.zhihu.com/p/406559772
- https://blog.csdn.net/afei__/article/details/81201039
- https://www.zhihu.com/column/c_1369781372333240320