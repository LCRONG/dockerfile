# @description php image base on the alpine edge
#                       some information
# ------------------------------------------------------------------------------------
# @link https://hub.docker.com/_/alpine/      alpine image
# @link https://hub.docker.com/_/php/         php image
# @link https://github.com/docker-library/php php dockerfiles
# ------------------------------------------------------------------------------------
# @build-example docker build . -f Dockerfile -t clion-remote-cpp:1.0.0
#


FROM ubuntu:latest
RUN  sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list

RUN echo nameserver 8.8.8.8 >> /etc/resolv.conf \
    && export DEBIAN_FRONTEND=noninteractive \
    && apt-get update\
    && apt-get install -y --fix-missing \
            tzdata \
            sudo \
            ssh \
            build-essential \
            gcc \
            g++ \
            gdb \
            clang \
            cmake \
            rsync \
            tar \
            python \
            vim \
            automake \
            openssh-client \
            openssh-server \
            pkg-config \
            libxml2-dev \
            libsqlite3-dev \
    && ln -fs /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && dpkg-reconfigure --frontend noninteractive tzdata \
    && apt-get clean

RUN echo "PermitRootLogin yes" >> /etc/ssh/sshd_config \
    && yes root | passwd root