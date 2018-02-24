---
title: nginx
date: 2018-01-26 06:59:06
tags: [note, nginx]
---

### reverse proxy
1. add proxy.conf file under /etc/nginx/conf.d/
```
server
  {
    listen 80; #server port
    server_name spark.docker.local; #dummy server name
    location / {
      proxy_pass http://master:8080/; #actual server url
    }
  }
```

1. add "127.0.0.1 spark.docker.local" to /etc/hosts
1. access "http://spark.docker.local" in browser, it will proxy to spark web UI page


### CORS on Nginx
```
  location / {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range';
        add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
  }

```
[ref link](https://enable-cors.org/server_nginx.html)


