# 饥荒独立服务器部署面板Docker镜像
[![Build Status](https://github.com/kairlec/dst-admin-go-docker/workflows/dst-admin-go/badge.svg)](https://github.com/kairlec/dst-admin-go-docker/actions)
[![Version](https://img.shields.io/docker/v/kairlec/dst-admin-go/latest)](https://github.com/hujinbo23/dst-admin-go)
[![Docker Pulls](https://img.shields.io/docker/pulls/kairlec/dst-admin-go)](https://hub.docker.com/r/kairlec/dst-admin-go/)
[![Image Size](https://img.shields.io/docker/image-size/kairlec/dst-admin-go/latest)](https://hub.docker.com/r/kairlec/dst-admin-go/)

## 部署
- docker compose 示例
    ```yaml
    version: '3'
    services:
      dst-admin-go:
        image: kairlec/dst-admin-go:latest
        container_name: dst-admin-go
        volumes:
          - ./dst-admin-go/saves:/root/.klei/DoNotStarveTogether # 游戏存档
          - ./dst-admin-go/backup:/app/backup # 游戏备份
          - ./dst-admin-go/mods:/app/mod # 游戏mod
        ports:
          - 8082:8082/tcp # for web dashboard
          - 10888:10888/udp
          - 10998:10998/udp
          - 10999:10999/udp
    ```
  ---
  - 玩家日志存在`/app/dst-db`文件,如果需要持久化,建议先启动一次,然后使用`docker cp`命令将文件复制出来,然后再修改compose文件再次启动
  - 服务日志存在`/app/dst-admin-go.log`文件,如果需要持久化,建议先启动一次,然后使用`docker cp`命令将文件复制出来,然后再修改compose文件再次启动
  - 系统设置存在`/app/dst_config`,如果需要持久化,建议先启动一次,然后使用`docker cp`命令将文件复制出来,然后再修改compose文件再次启动
  - 如果不希望在重新创建容器后,用户都需要重新创建,可以启动一次后初始化完毕,然后将`/app/password.txt`文件复制出来,然后再修改compose文件,并挂载一个文件(任意,空文件就行)到`/app/first`,再次启动即可

## 用法
> 请直接去下方[引用](#引用)的原仓库里查看


## 引用
- [dst-admin-go](https://github.com/hujinbo23/dst-admin-go)
- [dst-manage-web](https://github.com/hujinbo23/dst-manage-web)