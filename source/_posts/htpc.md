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
sudo yum install autossh
#need an pulibc IP vps, port 5555 is a listen port
autossh -M 5555 -NR 8888:127.0.0.1:22 root@114.114.114.114

#set gateway port in vps
sudo vim /etc/ssh/sshd_config
#add below line
GatewayPorts yes
#then, restart ssh
sudo service ssh restart

#then can access htpc behind an NAT network
ssh root@114.114.114.114 -p 8888
```

1. copy ssh public id
```
#port 2211 is vps ssh port, vps public ip 114.114.114.114
ssh-copy-id root@114.114.114.114 -p 2211
```

1. aria2
```
sudo yum install aria2
sudo aria2c --conf-path /etc/aria2/aria2.conf > /dev/null &
```

aria2 config: /etc/aria2/aria2.conf
```
dir=/download
continue=true

max-concurrent-downloads=20
max-connection-per-server=5
min-split-size=10M
max-overall-download-limit=10M
max-overall-upload-limit=200K

input-file=/etc/aria2/aria2.session
save-session=/etc/aria2/aria2.session

enable-rpc=true
rpc-allow-origin-all=true
rpc-listen-all=true
#rpc-listen-port=6800
rpc-secret=test

listen-port=51413
dht-listen-port=51414
enable-peer-exchange=false
#bt-request-peer-speed-limit=50K
peer-id-prefix=-TR2770-
user-agent=Transmission/2.77
seed-ratio=1
bt-seed-unverified=true
bt-save-metadata=true

bt-tracker=udp://87.233.192.220:6969/announce,udp://62.138.0.158:6969/announce,udp://151.80.120.114:2710/announce,http://163.172.81.35:1337/announce,udp://163.172.81.35:1337/announce udp://185.82.217.160:1337/announce,http://185.82.217.160:1337/announce,http://51.15.4.13:1337/announce,udp://211.149.236.45:6969/announce,udp://109.236.91.32:6969/announce,udp://83.208.197.185:1337/announce,udp://51.15.4.13:1337/announce,udp://198.54.117.212:1337/announce,udp://82.45.40.204:1337/announce,udp://123.249.16.65:2710/announce,udp://5.226.21.164:6969/announce,udp://210.244.71.25:6969/announce,udp://78.142.19.42:1337/announce,udp://191.96.249.23:6969/announce,udp://91.218.230.81:6969/announce
```

1. mount extenal storage
```
sudo yum install ntfs-3g
sudo fdisk -l
sudo mount -t ntfs-3g /dev/sda2 /mnt/driver1

#add below line to /etc/fstab
/dev/sda2 /mnt/driver1 ntfs-3g defaults 0 0
```

