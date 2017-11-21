# This docker image was inhereted from NVIIDA Dockerfile <https://hub.docker.com/r/nvidia/cuda/> 
# Content of image:
#	Ubuntu 16.04 64bit
#	Nvidia CUDA 8.0
#	Nvidia CUDA 8.0 Runtime with compiler toolchain and debugging tools, header filer and static library
#	Nvidia cuDnn 7.0.3.11
# This image will be in 'amax/general' docker repository as the general puropse image which could be a base image for both AI application and GPU HPC application.

ARG repository
FROM ${repository}:8.0-devel-ubuntu16.04
LABEL maintainer "AMAX INFORMATION TECHNOLOGIES INC <SUPPORT@AMAX.COM>"

RUN echo "deb http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64 /" > /etc/apt/sources.list.d/nvidia-ml.list

ENV CUDNN_VERSION 7.0.3.11
LABEL com.nvidia.cudnn.version="${CUDNN_VERSION}"

RUN apt-get update && apt-get install -y --no-install-recommends \
            libcudnn7=$CUDNN_VERSION-1+cuda8.0 \
            libcudnn7-dev=$CUDNN_VERSION-1+cuda8.0 && \
    rm -rf /var/lib/apt/lists/*
