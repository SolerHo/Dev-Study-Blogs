<h1><center> Docker部分</center></h1>

## Docker内容说明
暂时不更新 swarm 和compose，等待TS+Vuejs项目部署实验后再更新。

## dockerfile使用方式
本人实验环境：
- Centos 8.2 X86
- 树莓派 ARM64

### Aarch64 架构
建议使用ARM架构的机器或者设备来进行。

```sh
docker build --tag 自己取名字:tag --file dockerfile文件 .
```

### X86_64 架构

```sh
docker build --tag 自己取名字:tag --file dockerfile文件 .
```

更多内容后续会完善更新。
