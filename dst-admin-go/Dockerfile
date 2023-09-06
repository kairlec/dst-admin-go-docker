ARG GO_VERSION=1.19
ARG OS=linux
ARG ARCH=amd64
ARG ADMIN_BRANCH=docker
ARG WEB_BRANCH=main
ARG STEAMCMD_PATH=/app/steamcmd
ARG DST_SERVER_PATH=/app/dst-dedicated-server

FROM node:18-alpine AS node-builder

ARG WEB_BRANCH

RUN apk add --no-cache git
RUN mkdir -p /go/src/github.com/hujinbo23 && \
    cd /go/src/github.com/hujinbo23 && \
    git clone https://github.com/hujinbo23/dst-manage-web -b ${WEB_BRANCH} --depth=1 && \
    cd dst-manage-web && \
    npm install && \
    npm run build

FROM --platform=${OS}/${ARCH} golang:${GO_VERSION}-alpine AS go-builder

ARG ADMIN_BRANCH

RUN apk add --no-cache git
RUN mkdir -p /go/src/github.com/hujinbo23 && \
    cd /go/src/github.com/hujinbo23 && \
    git clone https://github.com/hujinbo23/dst-admin-go -b ${ADMIN_BRANCH} --depth=1 && \
    cd dst-admin-go && \
    GOOS=${OS} GOARCH=${ARCH} CGO_ENABLED=0 go build -o dst-admin-go -ldflags "-s -w" -v

FROM --platform=${OS}/${ARCH} debian:buster-slim

ARG STEAMCMD_PATH
ARG DST_SERVER_PATH

LABEL maintainer="hujinbo23 jinbohu23@outlook.com"
LABEL description="DoNotStarveTogehter server panel written in golang.  github: https://github.com/hujinbo23/dst-admin-go"

LABEL org.opencontainers.image.authors "Kairlec<kairlec@users.noreply.github.com>"
LABEL org.opencontainers.image.url "https://github.com/kairlec/dst-admin-go-docker"
LABEL org.opencontainers.image.documentation "https://github.com/kairlec/dst-admin-go-docker"
LABEL org.opencontainers.image.source "https://github.com/kairlec/dst-admin-go-docker"
LABEL org.opencontainers.image.title "kairlec/dst-admin-go"
LABEL org.opencontainers.image.description "DoNotStarveTogehter server panel written in golang.  github: https://github.com/hujinbo23/dst-admin-go"

RUN dpkg --add-architecture i386 && \
    apt-get update && \
    apt-get install -y --no-install-recommends --no-install-suggests \
    lib32gcc1 \
    libstdc++6 \
    libgcc1 \
    libcurl4-gnutls-dev \
    curl \
    wget \
    ca-certificates \
    screen \
    procps \
    sudo \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir -p ${STEAMCMD_PATH} && \
    mkdir -p ${DST_SERVER_PATH} && \
    mkdir -p $USER_DIR/.klei/DoNotStarveTogether/MyDediServer/ && \
    mkdir -p /app/backup && \
    mkdir -p /app/mod

RUN curl -s -L http://media.steampowered.com/installer/steamcmd_linux.tar.gz | tar -zxvf - -C ${STEAMCMD_PATH} && \
    ${STEAMCMD_PATH}/steamcmd.sh +force_install_dir ${DST_SERVER_PATH} +login anonymous +app_update 343050 validate +quit

COPY --from=go-builder   /go/src/github.com/hujinbo23/dst-admin-go/dst-admin-go /app/dst-admin-go
COPY --from=go-builder   /go/src/github.com/hujinbo23/dst-admin-go/config.yml /app/config.yml
COPY --from=go-builder   /go/src/github.com/hujinbo23/dst-admin-go/static /app/static
COPY --from=node-builder /go/src/github.com/hujinbo23/dst-manage-web/build /app/dist
COPY --from=go-builder   /go/src/github.com/hujinbo23/dst-admin-go/dst_config /app/dst_config

RUN echo "steamcmd=${STEAMCMD_PATH}" >> /app/dst_config && \
   echo "force_install_dir=${DST_SERVER_PATH}" >> /app/dst_config && \
   echo "cluster=MyDediServer" >> /app/dst_config && \
   echo "backup=/app/backup" >> /app/dst_config && \
   echo "mod_download_path=/app/mod" >> /app/dst_config && \
   echo "username=admin" >> /app/password.txt && \
   echo "password=123456" >> /app/password.txt && \
   echo "displayName=admin" >> /app/password.txt && \
   echo "photoURL=xxx" >> /app/password.txt

RUN chmod +x /app/dst-admin-go

WORKDIR /app

EXPOSE 8082/tcp
EXPOSE 10888/udp
EXPOSE 10998/udp
EXPOSE 10999/udp

ENTRYPOINT ["/app/dst-admin-go"]