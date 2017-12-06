# Tensorflow verion is 1.4.0 which support CUDA8 + cudnn 7 no optimize
# CUDA 9.0 and cudnn7.0 will be support in 1.15.0

FROM amax/general:8.0-devel-cudnn7.0_ubuntu16.04_v01
RUN apt-get update && apt-get install -y --no-install-recommends \
	python-dev \
	libcupti-dev \
	python-dev \
	python3-dev \
	python-virtualenv \
	ca-certificates \
	pkg-config \
	git \
	curl \
	wget \
	python-opencv  \
	python-pil \
	python-pydot \
	python-requests \
	python-six \
	python-skimage \
	python-tk \
	libboost-all-dev \
	libfreetype6-dev \
	libgoogle-glog0v5 \
	libhdf5-serial-dev  \
	libleveldb1v5 \
	python3 \
	libopencv-core2.4v5 \
	libopencv-highgui2.4v5 \
	libopencv-imgproc2.4v5 \
	libpng12-dev \
	libzmq5 \
	libopenblas-base \
	graphviz \
	libcupti-dev && \
    rm -rf /var/lib/apt/lists/*

RUN cd /tmp && \
	curl -O https://bootstrap.pypa.io/get-pip.py && \
    	python get-pip.py && \
    	python3 get-pip.py && \
    	pip2 install --upgrade --no-cache-dir pip && \
    	pip3 install --upgrade --no-cache-dir pip && \
	rm /tmp/get-pip.py && \
	pip2 install --no-cache-dir matplotlib jupyter pandas graphviz scikit-learn argparse cython scipy pyyaml numpy requests keras && \
	pip2 install --upgrade --no-cache-dir tensorflow-gpu tensorboard && \
	pip3 install --no-cache-dir matplotlib jupyter pandas graphviz scikit-learn argparse cython scipy pyyaml numpy requests keras && \
	pip3 install --upgrade --no-cache-dir tensorflow-gpu tensorboard && \
	rm /usr/local/share/jupyter/kernels/* -rf && \
	python2 -m ipykernel install && \
	python3 -m ipykernel install && \
	mkdir -p /opt/notebooks && \
	rm /bin/sh && \
	ln -s /bin/bash /bin/sh && \
	cd /usr/lib/x86_64-linux-gnu/ && \
	ln -s libcudnn.so.7.0.3 libcudnn.so.6

ENV NOTEBOOK_DIR=/opt/notebooks
ENV NOTEBOOK_IP=0.0.0.0
ENV NOTEBOOK_TOKEN="amax"

EXPOSE 8888

EXPOSE 6006

ENTRYPOINT source /root/.bashrc && \
	export SHELL=/bin/bash && \
	jupyter notebook --notebook-dir=$NOTEBOOK_DIR --ip=$NOTEBOOK_IP --NotebookApp.token=$NOTEBOOK_TOKEN --allow-root
