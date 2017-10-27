---
title: docker
date: 2017-10-27 01:43:03
tags: [note, docker]
---

##docker machine
### docker machine increase memory
```sh
docker-machine stop
VBoxManage modifyvm default --cpus 2
VBoxManage modifyvm default --memory 4096
docker-machine start
```
