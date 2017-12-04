# This version uses the latest version of mxnet=
FROM amax/general:9.0-devel-cudnn7.0_ubuntu16.04_v01 as mxnet
RUN apt-get update && apt-get install -y --no-install-recommends \
	build-essential \
	git \
	libatlas-base-dev \
	libopencv-dev \
	python-opencv \
	libcurl4-openssl-dev \
	libgtest-dev \
	wget \
	unzip \
	python-dev \
	python3-dev \
	curl \
	libopenblas-dev \
	zip \
	m4 \
	libtool \
	autoconf \
	g++ \
	automake \
	make \
	python-setuptools \
	curl \
	ssh \
	python \
	python3 \
	ca-certificates \
	libopencv-core2.4v5 \
	libopencv-highgui2.4v5 \
	libopencv-imgproc2.4v5 \
	graphviz \
	python-pydot \
	python-pil \
	python-requests \
	python-skimage \
	virtualenv \
	gcc \
	cmake \
	libgtk-3-dev \
	libboost-all-dev && \
    rm -rf /var/lib/apt/lists/*

## Build 'NCCL'
RUN git clone https://github.com/NVIDIA/nccl.git /nccl && \
    cd /nccl && \
    make CUDA_HOME=/usr/local/cuda "-j$(nproc)"  && \
    make PREFIX=/usr/local/nccl install && \
    cp -r /usr/local/nccl/* /usr/local

RUN curl -O https://bootstrap.pypa.io/get-pip.py && \
    python get-pip.py && \
    python3 get-pip.py && \
    pip2 install --upgrade --no-cache-dir pip && \
    pip3 install --upgrade --no-cache-dir pip && \
    pip2 install nose pylint numpy nose-timer requests pandas matplotlib pandas graphviz pyyaml scikit-learn jupyter dlib argparse cython scipy && \
    pip3 install nose pylint numpy nose-timer requests pandas matplotlib pandas graphviz pyyaml scikit-learn jupyter dlib argparse cython scipy 

RUN git clone --recursive https://github.com/apache/incubator-mxnet.git -b 'v0.12.0' /mxnet && \
    cd /mxnet && \
    make -j $(nproc) USE_OPENCV=1 USE_BLAS=openblas USE_CUDA=1 USE_CUDA_PATH=/usr/local/cuda USE_CUDNN=1 USE_DIST_KVSTORE=1 USE_NCCL=1 

RUN mkdir /wheel2 /wheel3 && \
    cd /wheel2 && \
    pip2 wheel nose pylint numpy nose-timer requests pandas matplotlib pandas graphviz pyyaml scikit-learn jupyter dlib argparse cython scipy && \
    cd /wheel3 && \
    pip3 wheel nose pylint numpy nose-timer requests pandas matplotlib pandas graphviz pyyaml scikit-learn jupyter dlib argparse cython scipy


## Build the mxnet docker image
FROM amax/general:9.0-devel-cudnn7.0_ubuntu16.04_v01
COPY --from=mxnet /usr/local/nccl/ /usr/local
COPY --from=mxnet /mxnet/ /tmp/mxnet
COPY --from=mxnet /wheel2/ /tmp/wheel2
COPY --from=mxnet /wheel3/ /tmp/wheel3

RUN apt-get update && apt-get install -y --no-install-recommends \
	git \
        curl \
	wget \
	ca-certificates \
	pkg-config \
        python-dev \
        python-opencv \
        python-pil \
        python-pydot \
        python-requests \
        python-six \
        python-skimage \
        python-tk \
	libboost-all-dev \
        libfreetype6-dev \
        libgoogle-glog0v5 \
        libhdf5-serial-dev \
        libleveldb1v5 \
	python3 \
        libopencv-core2.4v5 \
        libopencv-highgui2.4v5 \
        libopencv-imgproc2.4v5 \
        libpng12-dev \
        libzmq5 \
	libopenblas-base \
	virtualenv \
	graphviz \
	libcupti-dev && \
    rm -rf /var/lib/apt/lists/*



RUN curl -O https://bootstrap.pypa.io/get-pip.py && \
    python get-pip.py && \
    python3 get-pip.py && \
    rm get-pip.py && \
    pip2 install --upgrade --no-cache-dir pip && \
    pip3 install --upgrade --no-cache-dir pip && \
    pip2 install /tmp/wheel2/*.whl --no-cache-dir && \
    pip3 install /tmp/wheel3/*.whl  --no-cache-dir && \
    cd /tmp/mxnet/python && \
    python2 setup.py install && \
    python3 setup.py install && \
    rm /usr/local/share/jupyter/kernels/* -rf && \
    python2 -m ipykernel install && \
    python3 -m ipykernel install && \
    pip3 install tensorboard && \
    pip2 install tensorboard && \ 
    rm /tmp/* -rf && \
    mkdir -p /opt/notebooks 

## Notebook dir
ENV NOTEBOOK_DIR=/opt/notebooks
ENV NOTEBOOK_IP=0.0.0.0
ENV NOTEBOOK_TOKEN="amax"

## Export Jupyter Notebook
EXPOSE 8888

##  Export Tensorboard  port 
EXPOSE 6006

## When use this image in our environment, we should use the --entrypoint to overwrite this default.
## And we can add --ip=192.x.x.x to make sure the jupyter notebook can listening on a non-localhost ip.
# ENTRYPOINT ["jupyter", "notebook", "--notebook-dir=NOTEBOOK_DIR"]
ENTRYPOINT jupyter notebook --notebook-dir=$NOTEBOOK_DIR --ip=$NOTEBOOK_IP --NotebookApp.token=$NOTEBOOK_TOKEN --allow-root
