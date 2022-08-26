---
title: "Redis环境搭建"
description: '基于Docker的Redis环境搭建'
date: '2022-08-23'
slug: 'redis-docker'
image: 'redis-docker.png'
categories:
    - redis
    - docker
tags:
    - docker
    - redis
---

## 常规Docker容器创建

### 1.查找镜像

```bash
docker search redis
```

### 2.拉取镜像

```bash
docker pull redis
```

### 3.下载redis.conf配置文件挂载用

[redis.conf](http://download.redis.io/redis-stable/redis.conf)

### 4.运行Redis容器

```bash
docker run -d --restart=always -e TZ=Asia/Shanghai --name redis -p 6379:6379 -v E:\\Docker\\redis\\redis.conf:/etc/redis/redis.conf -v E:\\Docker\\redis\\data:/data -v E:\\Docker\\redis\\logs:/logs -d redis redis-server /etc/redis/redis.conf --appendonly yes
```

## 采用docker-compose.yml文件创建容器

### 1.docker-compose.yml

```yaml
version: "3"

services:
  redis:
    image: redis
    container_name: redis
    restart: always
    ports:
      - 6379:6379
    privileged: true
    volumes:
      - E:\\Docker\\redis\\redis.conf:/etc/redis/redis.conf
      - E:\\Docker\\redis\\data:/data
      - E:\\Docker\\redis\\logs:/logs
    environment:
      TZ: Asia/Shanghai
    command: ["redis-server","/etc/redis/redis.conf","--appendonly yes"]
```

### 2.docker-compose up

```bash
docker-compose -f .\docker-compose.yml up -d
```
