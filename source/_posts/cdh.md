---
title: cdh
date: 2017-10-30 18:09:07
tags:[note, Cloudera, docker]
---
## Cloudera

```
docker-machine create cdh --virtualbox-memory "5120" --virtualbox-cpu-count 4 --engine-registry-mirror "https://docker.mirrors.ustc.edu.cn" -d "virtualbox"
```
```
docker pull cloudera/quickstart:latest
```



