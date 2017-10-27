---
title: elaticsearch
date: 2017-10-27 01:02:02
tags: [note, es]
---

## docker
```sh
docker-machine ssh
sudo sysctl -w vm.max_map_count=262144
```

### docker image
Dockerfile:
```
FROM java:openjdk-8-alpine

RUN addgroup -S elasticsearch && adduser -S -G elasticsearch elasticsearch

RUN apk add --no-cache bash curl

WORKDIR /work

RUN chown elasticsearch:elasticsearch /work

USER elasticsearch

EXPOSE 9200 9300

RUN curl -s https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.6.3.tar.gz |tar -xz

COPY es-docker elasticsearch-5.6.3/bin/

WORKDIR /work/elasticsearch-5.6.3

CMD bin/es-docker
```


#### script
[es-docker](https://github.com/elastic/elasticsearch-docker/blob/5.6/build/elasticsearch/bin/es-docker)
```sh
#!/bin/bash

# Run Elasticsearch and allow setting default settings via env vars
#
# e.g. Setting the env var cluster.name=testcluster
#
# will cause Elasticsearch to be invoked with -Ecluster.name=testcluster
#
# see https://www.elastic.co/guide/en/elasticsearch/reference/current/settings.html#_setting_default_settings

declare -a es_opts

while IFS='=' read -r envvar_key envvar_value
do
    # Elasticsearch env vars need to have at least two dot separated lowercase words, e.g. `cluster.name`
    if [[ "$envvar_key" =~ ^[a-z_]+\.[a-z_]+ ]]
    then
        if [[ ! -z $envvar_value ]]; then
          es_opt="-E${envvar_key}=${envvar_value}"
          es_opts+=("${es_opt}")
        fi
    fi
done < <(env)

# The virtual file /proc/self/cgroup should list the current cgroup
# membership. For each hierarchy, you can follow the cgroup path from
# this file to the cgroup filesystem (usually /sys/fs/cgroup/) and
# introspect the statistics for the cgroup for the given
# hierarchy. Alas, Docker breaks this by mounting the container
# statistics at the root while leaving the cgroup paths as the actual
# paths. Therefore, Elasticsearch provides a mechanism to override
# reading the cgroup path from /proc/self/cgroup and instead uses the
# cgroup path defined the JVM system property
# es.cgroups.hierarchy.override. Therefore, we set this value here so
# that cgroup statistics are available for the container this process
# will run in.
export ES_JAVA_OPTS="-Des.cgroups.hierarchy.override=/ $ES_JAVA_OPTS"

exec bin/elasticsearch "${es_opts[@]}"
```

####docker compose
```
version: '2'
services:
  elasticsearch1:
    container_name: elasticsearch1
    image: es
    environment:
      - cluster.name=docker-cluster
      - network.host=0.0.0.0
      - node.name=master
      - discovery.zen.minimum_master_nodes=1
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    mem_limit: 1g
    volumes:
      - esdata1:/usr/share/elasticsearch/data
    ports:
      - 9200:9200
    networks:
      - esnet

  elasticsearch2:
    container_name: elasticsearch2
    image: es
    environment:
      - cluster.name=docker-cluster
      - network.host=0.0.0.0
      - node.name=node1
      - discovery.zen.ping.unicast.hosts=elasticsearch1
      - node.master=false
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    mem_limit: 1g
    volumes:
      - esdata2:/usr/share/elasticsearch/data
    networks:
      - esnet

volumes:
  esdata1:
    driver: local
  esdata2:
    driver: local

networks:
  esnet:
    driver: bridge
```

