# This docuer file is inhereted form NVIDIA docker file https://hub.docker.com/r/nvidia/cuda/
# Image Content:
#	Ubuntu 16.04 64bit
#	NVIDIA Cuda 9.0 
#	NVIDIA cuDnn 7.0.3.11
#	NVIDIA Cuda Runtime, with compiler toolchain, header file and static library
# This image is under 'amax/general' repository. It will be used as base image for other AI application and HPC application	



ARG repository 
FROM ${repository}:9.0-devel-ubuntu16.04 
LABEL maintainer "AMAX INFORMATIIONTION TECHNOLOGIES INC <SUPPORT@AMAX.COM>"
RUN echo "deb http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64 /" > /etc/apt/sources.list.d/nvidia-ml.list 
ENV CUDNN_VERSION 7.0.3.11 
LABEL com.nvidia.cudnn.version="${CUDNN_VERSION}" 
RUN apt-get update && apt-get install -y --no-install-recommends \
             libcudnn7=$CUDNN_VERSION-1+cuda9.0 \
             libcudnn7-dev=$CUDNN_VERSION-1+cuda9.0 && \
     rm -rf /var/lib/apt/lists/*


