---
title: notes
date: 2018-01-24 06:58:59
tags: [note]
---

### tomcat change MaxPermSize
```
#add below line to catalina.sh
JAVA_OPTS="$JAVA_OPTS -server -XX:PermSize=128M -XX:MaxPermSize=512m"
```

### firefox disable mixed content blocker
1. open *about:config*
1. search *security.mixed_content*, change value
