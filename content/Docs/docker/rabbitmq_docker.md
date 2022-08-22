---
title: "RabbitMq环境搭建"
description: '基于Docker的RabbitMq环境搭建，以及开启mqtt消息队列'
date: '2022-08-22'
slug: 'opencv-env'
# image: 'tree.webp'
categories:
    - rabbitmq
    - docker
tags:
    - docker
    - rabbitmq
    - mqtt
---

## 常规Docker容器创建

### 1.查找镜像

```bash
docker search rabbitmq:management
```

### 2.拉取镜像

```bash
docker pull rabbitmq:management
```

### 3.运行Rabbitmq

```bash
docker run -d --restart=always --name rabbitmq -p 15672:15672 -p 5672:5672 -p 1883:1883 -p 15675:15675 rabbitmq:management
```

### 4.进入到容器内部

```bash
docker exec -it eb2a35ffe5f5 /bin/bash
```

### 5.开启插件

```bash
rabbitmq-plugins enable rabbitmq_mqtt
rabbitmq-plugins enable rabbitmq_web_mqtt
```

### 6.重启容器

```bash
docker restart eb2a35ffe5f5
```

### 6.进入Rabbitmq Web配置页面

[http://localhost:15672/](http://localhost:15672/)

- username: guest
- password: guest

## 采用docker-compose.yml文件创建容器

### 1.docker-compose.yml

```yaml
version: "3"

services:
  rabbitmq:
    image: rabbitmq:management
    container_name: rabbitmq
    restart: always
    ports:
      - 15671:15671
      - 25672:25672
      - 15672:15672
      - 15675:15675
      - 4369:4369
      - 5671:5671
      - 5672:5672
      - 1883:1883
    environment:
      RABBITMQ_DEFAULT_VHOST: '/'
      RABBITMQ_DEFAULT_USER: admin
      RABBITMQ_DEFAULT_PASS: admin
      TZ: Asia/Shanghai
```

### 2.进入到容器内部

```bash
docker exec -it eb2a35ffe5f5 /bin/bash
```

### 3.开启插件

```bash
rabbitmq-plugins enable rabbitmq_mqtt
rabbitmq-plugins enable rabbitmq_web_mqtt
```

### 4.重启容器

```bash
docker restart eb2a35ffe5f5
```

### 5.进入Rabbitmq Web配置页面

[http://localhost:15672/](http://localhost:15672/)

- username: guest
- password: guest

---

- username: admin
- password: admin
