<h1 align="center">📔 Dev-Study-Blogs 小何同学 学习笔记博客</h1>

## repo说明
本repo主要是为了记录自己在学习C++路上的一步一个脚印，请轻喷。

repo的内容会逐一完善，有的笔记可能会后续进行更改，然后再修正，前期主要是赶学习进度，让自己拥有一定的紧迫感。

在此感谢大家的关注和star，希望我的笔记可以帮助你，这是我的荣幸，如果对你没任何帮助，请你直接忽略即可，谢谢。

在本repo中如果看到 `$xxxx$` 的内容，这是使用 markdown语法兼容Latex数学语法的方式。所以建议在使用本repo时建议安装 Google插件 -------> [MathJax 3 Plugin for Github](https://chrome.google.com/webstore/detail/mathjax-3-plugin-for-gith/peoghobgdhejhcmgoppjpjcidngdfkod) 可直接查看错乱的数学公式。

## 🚩 求职准备

- 岗位
    - C/C++岗位
    <!-- - Python岗位
    - Go岗位
    - Rust岗位 -->
## 💼 开发工具（Tools）
> 具体的手册或者使用方法会后续更新

|实际用途|分类|详细内容及对应笔记|
|:--|:--|:--|
|基本环境|OS|ubuntu 18.04（华为云）<br>Centos 8 （Ucloud）<br>TencentOS Server（Tlinux2.4）（腾讯云）||
|SSH、ftp等连接工具|终端工具|Termius <br>Filezilla 文件传输<br>MobaXterm（只支持Windows）|
|开发工具|IDE|Visual Studio Code <br> Sublime Text3|
|代码编译或文档排版|编辑器|Vim<br>Emacs<br>Typora /马克飞象|
|理科类公式文档排版|排版工具|Overleaf|
|官方网页或官方文档排版|文档排版|reStructuredText|
|管理代码工具|版本控制|Git <br>SVN<br>Gerrit（公司Code Review使用）|
|存放代码或笔记|工具|Gitub <br>Gitbook <br>Gitee 暂无迁移计划|
|编译代码使用|编译器|gcc / g++|
|多文件代码管理|工程管理|Makefile<br>CMakeList.txt <br> bazel|
|debug使用|调试器|GDB|
|检查代码错误或代码风格|检查工具|cppcheck <br>Google cpplint|
|性能分析或检测内存和线程的bug|Valgrind工具集|Memcheck <br>Cachegrin <br>Helgrind <br>DRD <br>Massif <br>DHAT |
|Google 检测内存、地址、线程|Sanitizer|AddressSanitizer <br>MemorySanitizer <br>ThreadSanitizer|
|集成和敏捷工具，便于快速迭代|CI/CD|jenkins<br>GitLab CI <br>Github Action|

## 💯 数学（math）
> 数学部分由于排版原因，直接使用GitBook来进行。-------> 后续统一做出更新，优先更新技术文章

|科目|高等数学|概率论与数理统计|线性代数|离散数学|信息论|
|:--|:--|:--|:--|:--|:--|
|内容|||

## 开发语言（dev-lang）
### ✳️ C语言
- 书籍笔记
    - 「[《C Primer Plus》第6版 学习笔记](https://github.com/SolerHo/CprimerPlus-6e-Notes)」
- 基础语法
- 内存管理
### ✳️ C++
- 书籍笔记
    - 「[《C++ Primer Plus》第6版 学习笔记](https://github.com/SolerHo/cpp-Primer-Plus-6e-Notes)」
    - 「[《Thinking in C++》第2版 学习笔记]()」 单独repo 暂未开放
    - 「[《Starting out with C++: the early Object》 第9版 学习笔记]()」单独repo 暂未开放
- 基础语法
- 内存管理
- 内存泄漏
- 内存回收
- C++11 标准
- C++14 标准
- C++17 标准
- STL模板库

### ✳️ Python
- 基础语法
- 
<!-- ### ✳️ Go
- 基础语法
- 
### ✳️ Rust
- 基础语法 -->


## 💎 CS内功（main_ability）
### 计算机组成原理

### 操作系统

### 计算机网络

### 数据结构

### 算法设计技巧与分析

### 软件工程

### 汇编语言
### 编译原理

### 设计模式

## Linux 内容（linux）
### 常用命令

### 基本操作

### Shell脚本



### 系统编程
- 进程控制
- 进程间通信
- Linux信号处理
- 进程间关系
- 守护进程
- 线程控制
- 线程间同步
### 网络编程
- 网络编程（TCP/IP、UDP）
- Socket套接字
- 异步I/O
- Unix domain协议和编程
- IO多路复用
- 序列化技术
- 零拷贝技术

## 数据库（database）
### SQL基础

### SQL强化

### 关系型
- MySQL基础
- MySQL调优
### NosQL
- postgreSQL

### 缓存Redis

## 反向代理Nginx（Tengine ---> 淘宝）

### Restful API
### 项目实战
- 聊天小工具

## 开源项目

### 消息队列
- RabbitMQ
- Kafka
### RPC框架
- gRPC 
    - 参考文章「[腾讯技术工程 gRPC基础概念详解](https://zhuanlan.zhihu.com/p/389328756)」
### Google开源
- protobuf


## 基础测试能力
### 单元测试
- Google Test
### 全链路测试
### A/B测试

## 三高技术
### 高并发
- 多线程
- 多进程
- 协程
- 异步回调
- 容量评估
### 高性能
- CDN内容分发技术
- 池化技术：数据库连接池，线程池
- 集群化
- 缓存技术

### 高可用
- 负载均衡
- 软硬件负载均衡
- 限流隔离降级技术
- 应用容灾、资源隔离熔断
- 异步多活

## 调优问题
### Linux调优

### MySQL调优

## 源码分析
### Linux内核源码

### Nginx源码

### Redis源码

## 其他部分

### 编译器

<!-- ## 集群与监控
- K8S
- Prometheus + Grafana -->


<!-- ## 机器学习

## 深度学习

## NLP -->


## 资格考试
- 系统设计架构师


## 参考经验




## 资源站
- 「[美团技术团队](https://tech.meituan.com/)」
- 「[腾讯技术工程](https://www.zhihu.com/org/teng-xun-ji-zhu-gong-cheng)」
- 「[]()」


