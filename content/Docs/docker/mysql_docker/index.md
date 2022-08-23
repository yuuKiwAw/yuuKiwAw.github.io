---
title: "MySql环境搭建"
description: '基于Docker的MySql环境搭建'
date: '2022-08-23'
slug: 'mysql-docker'
image: 'mysql-docker.png'
categories:
    - mysql
    - docker
tags:
    - docker
    - mysql
---

## 常规Docker容器创建

### 1.查找镜像

```bash
docker search mysql
```

### 2.拉取镜像

```bash
docker pull mysql
```

### 3.运行MySql容器

```bash
 docker run -d --restart=always -e TZ=Asia/Shanghai --name mysql -p 3306:3306  -v E:\\Docker\\mysql\\conf:/etc/mysql/conf.d -v E:\\Docker\\mysql\\data:/var/lib/mysql -v E:\\Docker\\mysql\\logs:/logs -e MYSQL_ROOT_PASSWORD=123456 mysql
```

## 采用docker-compose.yml文件创建容器

### 1.docker-compose.yml

```yaml
version: '3'

services:
  mysql:
    image: mysql
    container_name: mysql
    restart: always
    ports:
      - 3306:3306
    volumes:
      - E:\\Docker\\mysql\\conf:/etc/mysql/conf.d
      - E:\\Docker\\mysql\\data:/var/lib/mysql
      - E:\\Docker\\mysql\\logs:/logs
    environment:
      MYSQL_ROOT_PASSWORD: 123456
      TZ: Asia/Shanghai
```

### 2.docker-compose up

```bash
docker-compose -f .\docker-compose.yml up -d
```
