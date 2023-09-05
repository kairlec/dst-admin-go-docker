# 饥荒独立服务器部署面板Docker镜像
[![Build Status](https://github.com/kairlec/dst-admin-go-docker/workflows/dst-admin-go/badge.svg)](https://github.com/kairlec/dst-admin-go-docker/actions)
[![Version](https://img.shields.io/docker/v/kairlec/dst-admin-go-docker/latest)](https://github.com/hujinbo23/dst-admin-go)
[![Docker Pulls](https://img.shields.io/docker/pulls/kairlec/dst-admin-go)](https://hub.docker.com/r/kairlec/dst-admin-go/)
[![Image Size](https://img.shields.io/docker/image-size/kairlec/dst-admin-go/latest)](https://hub.docker.com/r/kairlec/dst-admin-go/)

## 部署
- docker compose
    ```yaml
    version: '3'

    services:
    dst-admin-go:
        image: kairlec/dst-admin-go:latest
        container_name: dst-admin-go
        ports:
        - 8082:8082/tcp
        - 10888:10888/udp
        - 10998:10998/udp
        - 10999:10999/udp
    ```

## 用法
> 请直接去下方[引用](#引用)的原仓库里查看


## 引用
- [dst-admin-go](https://github.com/hujinbo23/dst-admin-go)
- [dst-manage-web](https://github.com/hujinbo23/dst-manage-web)