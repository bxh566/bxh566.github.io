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

### insecure registry
```
#login docker machine
docker-machine ssh [machine-name]
#edit docker profile
vi /var/lib/boot2docker/profile
#add/change 'EXTRA_ARGS'
EXTRA_ARGS='
--label provider=virtualbox
--insecure-registry hostname:5000
--insecure-registry hostname:5001
--registry-mirror   http://hostname:5000
--registry-mirror   http://hostname:5001'

#restart docker machine
docker-machine restart [machine-name]
docker info
```
single command line
```
$ docker-machine ssh default "echo $'EXTRA_ARGS=\"--insecure-registry <YOUR INSECURE HOST>\"' | sudo tee -a /var/lib/boot2docker/profile && sudo /etc/init.d/docker restart"
```

### install secure Registry certificates
1. As discussed in the Docker Engine documentation certificates should be placed at /etc/docker/certs.d/hostname/ca.crt where hostname is your Registry server's hostname.
```
docker-machine scp certfile default:ca.crt
docker-machine ssh default
sudo mv ~/ca.crt /etc/docker/certs.d/hostname/ca.crt
exit
docker-machine restart
```
1. auth
docker login; and then copy ~/.docker/config.json to other vms

### install package in docker machine
```
tce-load -wi make
```

### docker machine static ip
[docker-machine-ipconfig](https://github.com/fivestars/docker-machine-ipconfig)


## docker
### query images from registry
```
curl localhost:5000/v2/_catalog
curl --cacert certs/registry-ca-cert.crt -X GET https://myregistry:5000/v2/ubuntu/tags/list
curl localhost:5000/v2/ubuntu/tags/list
```

### delete docker images from registry
[https://github.com/burnettk/delete-docker-registry-image]
1. install
```
curl https://raw.githubusercontent.com/burnettk/delete-docker-registry-image/master/delete_docker_registry_image.py | sudo tee /usr/local/bin/delete_docker_registry_image >/dev/null
sudo chmod a+x /usr/local/bin/delete_docker_registry_image
```

1. run
```
export REGISTRY_DATA_DIR=~/dev/docker_registry/docker/registry/v2

delete_docker_registry_image --image testrepo/awesomeimage --dry-run
```

### docker mirrors - china
```
https://docker.mirrors.ustc.edu.cn
https://hub-mirror.c.163.com
```

### remove all stoped containers
```
docker rm $(docker ps -a -q)
```

### remove untagged images
```
docker rmi $(docker images | grep "<none>" | awk {'print $3'})
```

### mac docker Docker.qcow2
~/Library/Containers/com.docker.docker/


