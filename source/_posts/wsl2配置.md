---
title: WSL2配置
date: 2021-05-19 16:48:45
tags: ['WSL2',Llinux','Kubernetes']
categories: ['windows']
cover: https://i.ytimg.com/vi/Cvrqmq9A3tA/maxresdefault.jpg
thumbnail: https://i.ytimg.com/vi/Cvrqmq9A3tA/maxresdefault.jpg
---

# WSL2 环境搭建

## 安装

windows功能中开启虚拟机平台和linux子系统

```powershell
wsl --set-default-version #设置默认版本为2
```

从windows store 安装ubuntu或者其他发行版本

## 配置

Linux 镜像源更换以及docker安装  

[SuperManito/LinuxMirrors](https://github.com/SuperManito/LinuxMirrors)

由于wsl2使用微软闭源的Init无法使用Systemd，安装ubuntu-wsl2-systemd-script来解决

[FiestaLake/ubuntu-wsl2-systemd-script](https://github.com/FiestaLake/ubuntu-wsl2-systemd-script)

设置docker cgroup驱动和镜像地址

```json
// 文件路径 /etc/docker/daemon.json
{
  "registry-mirrors": ["https://registry.cn-hangzhou.aliyuncs.com"],
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {"max-size": "100m"},
  "storage-driver": "overlay2"
}
```

安装kind kubectl

kind

```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.10.0/kind-linux-amd64
chmod +x ./kind
mv ./kind /usr/local/bin/kind
```

kubectl

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```

设置常用环境变量

[https://gist.github.com/leganck/3f15dbd5669f692d62bfb1d8084a445b](https://gist.github.com/leganck/3f15dbd5669f692d62bfb1d8084a445b)

## 问题

1. 获取到kind运行docker容器的命令，查看日志后发现是cgroup驱动问题

解决方法[Github](https://github.com/microsoft/WSL/issues/4189#issuecomment-518277265)

```bash
sudo mkdir /sys/fs/cgroup/systemd
sudo mount -t cgroup -o none,name=systemd cgroup /sys/fs/cgroup/systemd
```

# 工具

Lxrunoffline： 管理wsl的（rootfs/vhdx）文件位置 

[DDoSolitary/LxRunOffline](https://github.com/DDoSolitary/LxRunOffline)

wsl2hosts：自动添加相应域名到windows的hosts中

[shayne/go-wsl2-host](https://github.com/shayne/go-wsl2-host)

wsl2-auto-port-forward-python: 设置转发wsl2端口到Windows主机 

[itxq/wsl2-auto-port-forward-python](https://github.com/itxq/wsl2-auto-port-forward-python/tree/master/dist)