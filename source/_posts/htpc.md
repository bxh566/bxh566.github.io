---
title: htpc
date: 2018-01-30 03:56:11
tags: [note,htpc]
---
## CentOS
1. download minimal iso image: [minial iso](http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-Minimal-1708.iso)
1. use dd comment to write image to u disk in Macbook
```
#list u disk
diskutil list
#unmount u disk
diskutil unmountDisk /dev/disk[N]
#write to u disk
sudo dd if=/path/to/isoimage of=/dev/rdisk[N] bs=1m
```
1. install centos
  1. 用u盘给占美小主机安装centos7系统
  1. 一路默认配置
  1. 如果原来硬盘中有系统，需要在安装盘中移除所有分区，再用自动分区重新分配
  1. 设置root账户，网络配置设置打开（默认关闭）


1. autossh reverse tunnel
```
#need an pulibc IP vps, port 5555 is a listen port
autossh -M 5555 -NR 8888:127.0.0.1:22 root@114.114.114.114
#then can access htpc behind an NAT network
ssh root@114.114.114.114 -p 8888
```
