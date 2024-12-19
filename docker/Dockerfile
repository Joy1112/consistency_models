FROM nvidia/cuda:11.8.0-cudnn8-devel-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive PIP_PREFER_BINARY=1

COPY deadsnakes-archive-keyring.gpg /usr/share/keyrings/

RUN sed -i -e 's/^APT/# APT/' -e 's/^DPkg/# DPkg/' /etc/apt/apt.conf.d/docker-clean

RUN apt-get update --fix-missing && \
    apt-get install -y --no-install-recommends apt-utils ca-certificates && \
    mv /etc/apt/sources.list /etc/apt/sources.list.bak && \
    UBUNTU_RELEASE=$(grep VERSION_CODENAME /etc/os-release | cut -d= -f2) && \
    echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ ${UBUNTU_RELEASE} main restricted universe multiverse\n"\
"# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ ${UBUNTU_RELEASE} main restricted universe multiverse\n"\
"deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ ${UBUNTU_RELEASE}-updates main restricted universe multiverse\n"\
"# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ ${UBUNTU_RELEASE}-updates main restricted universe multiverse\n"\
"deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ ${UBUNTU_RELEASE}-backports main restricted universe multiverse\n"\
"# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ ${UBUNTU_RELEASE}-backports main restricted universe multiverse\n"\
"deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ ${UBUNTU_RELEASE}-security main restricted universe multiverse\n"\
"# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ ${UBUNTU_RELEASE}-security main restricted universe multiverse" > /etc/apt/sources.list && \
    echo "deb [signed-by=/usr/share/keyrings/deadsnakes-archive-keyring.gpg] http://ppa.launchpad.net/deadsnakes/ppa/ubuntu jammy main" > /etc/apt/sources.list.d/deadsnakes-ppa.list

RUN apt-get update --fix-missing && apt-get upgrade -y &&  \
    apt-get install -y --no-install-recommends sudo vim iproute2 wget curl iputils-ping net-tools iptables \
    graphviz openssh-server git tzdata libgl1-mesa-glx tmux htop libgl1-mesa-dev libopenmpi-dev \
    python3.8 python3-pip python3-setuptools python3-wheel && \
    ln -s /usr/bin/python3.8* /usr/bin/python && \
    ln -fs /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' > /etc/timezone && dpkg-reconfigure tzdata && \
    apt-get autoclean && apt-get autoremove

RUN pip3 install --no-cache-dir --upgrade pip setuptools wheel packaging mpi4py && \
    pip3 install --no-cache-dir torch torchvision --index-url https://download.pytorch.org/whl/cu118 && \
    pip3 install flash-attn==0.2.8

COPY ./ /home/consistency_models

RUN pip install -e /home/consistency_models

WORKDIR /home/consistency_models
