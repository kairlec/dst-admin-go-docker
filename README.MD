# 饥荒独立服务器部署面板Docker镜像
[![Build Status](https://github.com/kairlec/dst-admin-go-docker/workflows/dst-admin-go/badge.svg)](https://github.com/kairlec/dst-admin-go-docker/actions)
[![Build Status](https://github.com/kairlec/dst-admin-go-docker/workflows/dst-admin-go-verge/badge.svg)](https://github.com/kairlec/dst-admin-go-docker/actions)
[![Version](https://img.shields.io/docker/v/kairlec/dst-admin-go/latest)](https://github.com/hujinbo23/dst-admin-go)
[![Docker Pulls](https://img.shields.io/docker/pulls/kairlec/dst-admin-go)](https://hub.docker.com/r/kairlec/dst-admin-go/)
[![Image Size](https://img.shields.io/docker/image-size/kairlec/dst-admin-go/latest)](https://hub.docker.com/r/kairlec/dst-admin-go/)

> [!IMPORTANT]  
> 本仓库仅按照固定分支对[dst-admin-go](https://github.com/carrot-hu23/dst-admin-go)进行编译和打包,代码可能与原仓库发布到docker hub的版本不一致


## 部署
- docker compose 示例
    ```yaml
    services:
      dst-admin-go:
        image: kairlec/dst-admin-go:${TAG:-latest}
        deploy:
          restart_policy:
            condition: always
            delay: 5s
            window: 60s
        container_name: dst-admin-go
        volumes:
          - /etc/localtime:/etc/localtime:ro # 保持容器时间与宿主机一致
          - /etc/timezone:/etc/timezone:ro # 保持容器时间与宿主机一致
          - ./dst-admin-go/saves:/root/.klei/DoNotStarveTogether # 游戏存档
          - ./dst-admin-go/backup:/app/backup # 游戏备份
          - ./dst-admin-go/mods:/app/mod # 游戏mod
        #  - ./dst-admin-go/dst-db:/app/dst-db # 数据库文件,注意这个是文件
        #  - ./dst-admin-go/dst-admin-go.log:/app/dst-admin-go.log # 服务日志文件,注意这个是文件
        #  - ./dst-admin-go/dst_config:/app/dst_config # 系统设置文件,注意这个是文件
        #  - ./dst-admin-go/password.txt:/app/password.txt # 用户密码文件,注意这个是文件
        #  - ./dst-admin-go/first:/app/first # 用户密码文件,注意这个是文件
        ports:
          - 8082:8082/tcp # 游戏Web管理面板端口
          - 10888:10888/udp # 游戏服务器端口
          - 10998:10998/udp # 游戏服务器端口
          - 10999:10999/udp # 游戏服务器端口
    ```
  请自行修改TAG为相应版本号,latest构建使用最新代码构建可能并不稳定

  > 若要使用多房间版本,请在TAG后面加`-verge`,例如`kairlec/dst-admin-go:latest-verge`
  
  ---
  - 玩家日志存在`/app/dst-db`文件,如果需要持久化,请创建空文件或从容器中复制出来,然后注释掉compose文件中对应行
  - 服务日志存在`/app/dst-admin-go.log`文件,如果需要持久化,请创建空文件或从容器中复制出来,然后注释掉compose文件中对应行
  - 系统设置存在`/app/dst_config`,如果需要持久化,请从容器中复制出来,然后注释掉compose文件中对应行
  - 用户密码文件存在`/app/password.txt`,如果需要持久化,请创建空文件或从容器中复制出来,然后注释掉compose文件中对应行
  - `/app/first`文件是第一次启动时创建的,如果需要持久化,请创建空文件或从容器中复制出来,然后注释掉compose文件中对应行,存在此文件后程序将不再执行初始化操作,请确保在此之前你已经设置好了`/app/password.txt`文件
  
  示例:
  ```bash
  # 创建临时容器
  docker create --name dst-admin-tmp kairlec/dst-admin-go:${TAG:-latest}
  # 复制文件到本地
  docker cp dst-admin-tmp:/app/dst-db ./dst-admin-go/dst-db
  docker cp dst-admin-tmp:/app/dst-admin-go.log ./dst-admin-go/dst-admin-go.log
  docker cp dst-admin-tmp:/app/dst_config ./dst-admin-go/dst_config
  docker cp dst-admin-tmp:/app/password.txt ./dst-admin-go/password.txt
  # !!! 建议复制password.txt文件后,编辑好内容,不建议使用内置的初始密码
  # nano ./dst-admin-go/password.txt

  # 创建一个空文件,用于挂载到容器中,让程序不再重复初始化
  touch ./dst-admin-go/first
  
  # 删除临时容器
  docker rm dst-admin-tmp
  
  # 取消compose文件中的注释
  # nano compose.tml
  
  # 启动容器
  docker compose up -d
  ```
## 用法
> 请直接去下方[引用](#引用)的原仓库里查看


## 引用
- [dst-admin-go](https://github.com/carrot-hu23/dst-admin-go)
- [dst-manage-web2](https://github.com/carrot-hu23/dst-manage-web2)

## 问题
- **与[官方镜像](https://hub.docker.com/r/carrot-hu23/dst-admin-go)有何不同?**
  - 官方镜像的steamcmd与饥荒游戏本体需要在第一次运行时下载,本镜像是预先下载好的
