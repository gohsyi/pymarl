FROM nvidia/cuda:9.2-cudnn7-devel-ubuntu16.04
MAINTAINER Tabish Rashid

# CUDA includes
ENV CUDA_PATH /usr/local/cuda
ENV CUDA_INCLUDE_PATH /usr/local/cuda/include
ENV CUDA_LIBRARY_PATH /usr/local/cuda/lib64

# Config apt-get sources
WORKDIR /etc/apt/
RUN mv sources.list sources.list.bak
RUN echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main restricted universe multiverse" >> sources.list
RUN echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse" >> sources.list
RUN echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse" >> sources.list
RUN echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main restricted universe multiverse" >> sources.list

# Ubuntu Packages
RUN apt-get update -y && apt-get install software-properties-common -y && \
    add-apt-repository -y multiverse && apt-get update -y && apt-get upgrade -y && \
    apt-get install -y apt-utils nano vim man build-essential wget sudo && \
    rm -rf /var/lib/apt/lists/*

# Install curl and other dependencies
RUN apt-get update -y && apt-get install -y curl libssl-dev openssl libopenblas-dev \
    libhdf5-dev hdf5-helpers hdf5-tools libhdf5-serial-dev libprotobuf-dev protobuf-compiler git
RUN curl -sk https://raw.githubusercontent.com/torch/distro/master/install-deps | bash && \
    rm -rf /var/lib/apt/lists/*

# Install python3 pip3
RUN apt-get update
RUN apt-get -y install python3
RUN apt-get -y install python3-pip
RUN pip3 install --upgrade pip

# Python packages we use (or used at one point...)
RUN pip3 install numpy scipy pyyaml matplotlib
RUN pip3 install imageio
RUN pip3 install tensorboard-logger
RUN pip3 install pygame

RUN mkdir /install
WORKDIR /install

# install Sacred (from OxWhirl fork)
RUN pip3 install setuptools
RUN git clone https://github.com/oxwhirl/sacred.git /install/sacred && cd /install/sacred && python3 setup.py install

#### -------------------------------------------------------------------
#### install pytorch
#### -------------------------------------------------------------------
RUN pip3 install torch
RUN pip3 install torchvision snakeviz pytest probscale

## -- SMAC
RUN pip3 install git+https://github.com/oxwhirl/smac.git
ENV SC2PATH /pymarl/3rdparty/StarCraftII

WORKDIR /pymarl
