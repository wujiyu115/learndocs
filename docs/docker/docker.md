docker
=================

https://www.gitbook.com/book/yeasy/docker_practice/details
https://docs.docker.com/docker-for-mac/
http://docs.daocloud.io/allen-docker/
http://dockone.io/article/1302
http://gold.xitu.io/entry/56dd5d0fdf0eea0055b44a73
https://www.shiyanlou.com/questions/3307
https://icymint.me/2016/07/23/ShrinkDockerImages/
https://www.iron.io/microcontainers-tiny-portable-containers/
https://hub.docker.com/u/frolvlad/

## 三个基本概念

Docker 包括三个基本概念

### 镜像（Image）
Docker 镜像（Image）就是一个只读的模板。
例如：一个镜像可以包含一个完整的 ubuntu 操作系统环境，里面仅安装了 Apache 或用户需要的其它应用程序。

### 容器（Container）

### 仓库（Repository）


## mac docker
- boot2docker
- docker-machine
- tooldocker(docker-toolbox)
- docker for mac


## 命令
-  launchctl list 查看启动列表

## 设置Docker Hub加速
https://www.daocloud.io/mirror#accelerator-doc
右键点击桌面顶栏的 docker 图标，选择 Preferences ，在 Advanced 标签下的 Registry mirrors 列表中加入下面的镜像地址:
http://xxx.m.daocloud.io
点击 Apply & Restart 按钮使设置生效。

## 镜像仓库
https://c.163.com/hub#/m/home/
https://hub.tenxcloud.com/


## docker命令
- docker pull ubuntu:12.04 拉取镜像
- docker info
- docker ps
- docker start webserver  开启容器
- docker run -d -p 80:80 --name webserver nginx   建立容器
- docker run -t -i ubuntu:12.04 /bin/bash 运行ubuntu镜像并打开bash
- docker images  列出镜像
- docker rm -f webserver  删除容器
- docker rmi ngnix 删除下载的镜像

https://docs.docker.com/compose/completion/
https://forums.docker.com/t/command-to-remove-all-unused-images/20/7
