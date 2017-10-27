---
title: linux
date: 2017-05-11 04:12:37
tags: [linux, note]
---
### kill java
```bash
ps -ef |grep java |grep -v grep |awk '{print $2}' |xargs kill
```
### dir of script
```bash
#!/bin/bash
echo "Script executed from: ${PWD}"

BASEDIR=$(dirname $0)
echo "Script location: ${BASEDIR}"
```

### generate ssh id key
```bash
ssh-keygen -t rsa -b 4096 -C "yourname@emailaddress" -N "" -f ~/.ssh/id_rsa
```

