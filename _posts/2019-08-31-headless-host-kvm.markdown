---
title: KVM on headless host
layout: post
date: 2019-08-31 11:19:06 -0400
yun: 世
comments: true
tags: linux kvm
---

最近有一些东西需要更高级一些的虚拟化，但我又不想在笔记本上跑，只能在 linux 工作站上跑 KVM 了。

因为我的 linux 机器设置上是完全 headless，没有 VNC 也没有配置 VNC 的打算，所以网上能找到的资料多少都有点出入。总之应该是很小众的场景，我简单做个笔记放在这儿，希望能帮到有缘人和未来忘光了的自己…

我的 host 是 debian 10 buster，guest 是 ubuntu 和 freebsd，这里拿 ubuntu 举例。主要参考的资料是 debian 的 wiki：<https://wiki.debian.org/KVM>.

# 宿主系统
安装过程跟 wiki 介绍的一致：

```shell
# 软件包安装
sudo apt install qemu-kvm libvirt-clients libvirt-daemon-system

# 让当前用户可以使用 libvirt 操作界面，而不是每次需要 sudo
sudo adduser $USER libvirt
sudo adduser $USER libvirt-qemu
```

不知道具体的原因，debian 的 `qemu-kvm` 打包默认关联上了 X11 的支持，如果机器之前没有安装 X11 的话会被一起装上很多包…

virsh 用 `--connect` 选项配合 URI 参数来标示它操作的虚拟机资源是来自用户还是来自系统：

```shell
# 操作用户的虚拟机资源
virsh --connect qemu:///session [commands]

# 操作系统（root所属）的虚拟机资源
virsh --connect qemu:///system [commands]
```

一般来说后者用得更多，所以可以借助环境变量来给 virsh 一个默认的 URI:

```shell
export LIBVIRT_DEFAULT_URI='qemu:///system'
```

Debian 10 在安装 KVM 的时候就会创建默认的 bridge 网络，所以只要用下面的命令打开就行。

``` shell
# 打开 bridge
virsh net-start default
# （可选）让 bridge 每次系统启动的时候自动打开
virsh net-autostart default
```

# 客户系统
客户系统有两个途径，一个是传统的新建一个虚拟机，然后从头安装系统，一个是下载 cloud vm 磁盘文件，用 cloud-init 或者类似的工具设置。对于 headless 机器来说，后者要方便的多。

## 准备磁盘文件
以 ubuntu 为例子，下载后缀是 `.img` 的 qcow2 格式的磁盘文件，比如 bionic-server-cloudimg-amd64.img.

我这里的策略是保留下载的磁盘文件只读，以 backing file 的形式创建一个新的磁盘文件作为工作区，这样做的好处是工作区磁盘文件是原版磁盘文件的增量，节约空间而且方便回滚到初始状态。如果不在意回滚的话，也可以直接在下载下来的磁盘文件上工作。

``` shell
# 确认磁盘文件信息
qemu-img info bionic-server-cloudimg-amd64.img

# 创建一个工作用的磁盘
qemu-img create -f qcow2 -o backing_file=bionic-server-cloudimg-amd64.img workspace.img

# 增加磁盘大小，qcow2 格式支持动态增量，所以这里的“60G”只是标示上限，实际文件的
# 大小是具体已使用的空间
qemu-img resize workspace.img 60G
```

## 配置 cloud-init
这里使用到两个文件

1. meta-data, 这里用来修改机器名字

``` shell
% cat meta-data
local-hostname: ws-u1
```

2. user-data，这里用来做（guest OS 里）用户的相关设置

``` shell
% cat user-data
users:
  - name: username
    ssh-authorized-keys:
      - [对应的 ssh public key 文件内容]
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    groups: sudo
    shell: /bin/bash

runcmd:
  - echo "AllowUsers username" >> /etc/ssh/sshd_config
  - systemctl restart ssh
```

3. 用上面两个文件创建一个磁盘镜像

``` shell
genisoimage -output user.iso -volid cidata -joliet -rock user-data meta-data
```

## 创建和启动虚拟机

```
virt-install --virt-type kvm --name ws-u1 --memory 8192 --vcpus 2 --os-type linux --os-variant ubuntu18.04 --disk workspace.img,device=disk --disk user.iso,device=cdrom --import --network network:default --graphics none --noautoconsole
```

然后用下面的 `virsh domifaddr` 命令查看一下 bridge 背后分配到的 IP 地址就可以 ssh 进去了。

## 日常操作

``` shell
virsh list # 列出正在运行的机器（domain）
virsh list --all # 列出所有的机器
virsh domifaddr [机器名（domain name）] # 列出对应机器 bridge 后的 IP 地址
virsh start [机器名]
virsh shutdown [机器名]
virsh reboot [机器名]
```
