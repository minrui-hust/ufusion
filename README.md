# Introduction
cityfly(低空飞行)，是一个基于ros2(jazzy)的用于无人机低空飞行的软件算法栈，它包含了sensor驱动，融合定位，融合感知，避障规划，运动控制，诊断等模块。其目标是完成无人机低空的自主飞行，操作者只需要关心无人机要去哪，剩下的交给cityfly。

# Setup
为了省去繁杂的开发环境配置过程，cityfly的开发和部署均基于docker，支持使用tmux+vim的组合以及vscode remote的方式进行开发。setup开发环境的主要包含一下步骤：
1. 安装docker
2. 编译dev镜像
3. 启动容器

**安装docker**
docker的安装参考官方教程，(Install Docker Engine on Ubuntu)[https://docs.docker.com/engine/install/ubuntu/],记得完成(Post-installation steps for Linux)[https://docs.docker.com/engine/install/linux-postinstall/]

**编译dev镜像**
首先clone仓库到本地(cityfly的仓库是一个标准的ros wrokspace):
```bash
git clone https://github.com/minrui-hust/cityfly.git
cd cityfly
```
编译dev镜像：
```bash
./src/docker/build.bash --dev
```
其中--dev用于指定编译开发用的dev镜像，另外还可以是--run(部署用的镜像)，--all(dev和run)。  
编译完成后使用docker images 查看生成的镜像，其tag应该为cityfly:dev
```bash
docker images
# 一下是docker images命令的输出示意
# REPOSITORY    TAG                IMAGE ID       CREATED         SIZE
# cityfly       dev                c64bd58b2693   12 hours ago    2.7GB
# ...
```

**启动容器**
启动容器最简单的方式就是docker run，但是作为一个开发环境，需要很多额外的参数，来支持容器和宿主机之间的互动，例如共享/dev下的硬件，共享GUI等，因此cityfly提供了便捷脚本用来启动开发用的容器。运行
```bash
./src/docker/run_dev.bash

# 一下是启动容器后的输出示意
# ubuntu@cityfly:~/cityfly$
# ...
```
用户会得到一个容器内terminal，其用户名为ubuntu, 密码123, prompt为cityfly。宿主机的cityfly仓库会挂载到容器的/home/ubuntu/cityfly下，启动容器后的terminal默认所在的路径也是/home/ubuntu/cityfly。
为了验证dev容器的正确性，最好编译一下cityfly仓库，使用如下命令：
```bash
colcon build
```
期间会输出一些warning，主要是第三方库产生的。编译成功后说明整个环境setup成功

