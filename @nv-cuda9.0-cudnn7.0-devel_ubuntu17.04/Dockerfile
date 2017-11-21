# This configuration file is inherenced from NVIDIA dockerfile
# Please refer to the LICENSE


ARG repository
FROM ${repository}:9.0-devel-ubuntu17.04
LABEL maintainer "AMAX INFORMATION TECHNOLOGIES INC <SUPPORT@AMAX.COM>"

RUN apt-get update && apt-get install -y --no-install-recommends \
        cuda-libraries-dev-$CUDA_PKG_VERSION \
        cuda-nvml-dev-$CUDA_PKG_VERSION \
        cuda-minimal-build-$CUDA_PKG_VERSION \
        cuda-command-line-tools-$CUDA_PKG_VERSION && \
    rm -rf /var/lib/apt/lists/*

ENV LIBRARY_PATH /usr/local/cuda/lib64/stubs:${LIBRARY_PATH}

