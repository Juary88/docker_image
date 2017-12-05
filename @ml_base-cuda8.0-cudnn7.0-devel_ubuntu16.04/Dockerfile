# Image is based Ubuntu 16.04 no GPU support
# This image will contain most of machine learning packages as a base image
# The python module has been compiled under python 2.7 and 3.5


FROM amax/general:8.0-devel-cudnn7.0_ubuntu16.04_v01 as dev
### Installation pre-requirements
RUN  apt-get update && apt-get install -y --no-install-recommends \
	curl \
	git \
	ssh \
	python \
	python3 \
	ca-certificates \
	libopencv-core2.4v5 \
	libopencv-highgui2.4v5 \
	libopencv-imgproc2.4v5 \
	graphviz \
	python-opencv \
	python-pydot \
	python-pil \
	python-requests \
	python-skimage \
	virtualenv \
	autoconf \
	gcc \
	cmake \
	python-dev \
	python3-dev \
	build-essential \
	libgtk-3-dev \
	libboost-all-dev

### Install 'pip' and machine learning packages
RUN curl -O https://bootstrap.pypa.io/get-pip.py && \
	python get-pip.py && \
	python3 get-pip.py && \
	pip2 install --upgrade --no-cache-dir pip && \
	pip3 install --upgrade --no-cache-dir pip && \
	pip2 install matplotlib \
		pandas \
		graphviz \
		pyyaml \
		scikit-learn \
		jupyter \
		dlib \
		requests \
		argparse \
		cython \
		scipy \
		seaborn \
		Plotly \
		Bokeh \
		Scrapy \
		Statsmodels \
		Pyevolve \
		NuPIC \
		Pattern \
		NetworkX 
RUN pip3 install matplotlib \
		pandas \
		graphviz \
		pyyaml \
		scikit-learn \
		jupyter \
		dlib \
		requests \
		argparse \
		cython \
		scipy \
		seaborn \
		Plotly \
		Bokeh \
		Scrapy \
		Statsmodels \
		NetworkX

### Save all of the python packages into /wheel folder
RUN mkdir /wheel2 /wheel3 && \
    cd /wheel2 && \
    pip2 wheel matplotlib \
                pandas \
                graphviz \
                pyyaml \
                scikit-learn \
                jupyter \
                dlib \
                requests \
                argparse \
                cython \
                scipy \
                seaborn \
                Plotly \
                Bokeh \
                Scrapy \
                Statsmodels \
                Pyevolve \
                NuPIC \
                Pattern \
                NetworkX && \
    cd /wheel3 && \
    pip3 wheel matplotlib \
                pandas \
                graphviz \
                pyyaml \
                scikit-learn \
                jupyter \
                dlib \
                requests \
                argparse \
                cython \
                scipy \
                seaborn \
                Plotly \
                Bokeh \
                Scrapy \
                Statsmodels \
                NetworkX

### Build CPU only Machine Learning image
FROM amax/general:8.0-devel-cudnn7.0_ubuntu16.04_v01
COPY --from=dev /wheel2 /tmp/wheel2
COPY --from=dev /wheel3 /tmp/wheel3
RUN apt-get update && apt-get install -y --no-install-recommends \
        curl \
        git \
        ssh \
        python \
	python3 \
        ca-certificates \
        libopencv-core2.4v5 \
        libopencv-highgui2.4v5 \
        libopencv-imgproc2.4v5 \
        graphviz \
        python-opencv \
        python-pydot \
        python-pil \
        python-requests \
        python-skimage \
        virtualenv \
        libgtk-3-dev \
        libboost-all-dev && \
    rm -rf /var/lib/apt/lists/*

RUN curl -O https://bootstrap.pypa.io/get-pip.py  && \
        python get-pip.py && \
        python3 get-pip.py && \
	rm get-pip.py && \
        pip2 install --upgrade --no-cache-dir pip && \
        pip3 install --upgrade --no-cache-dir pip && \
	cd /tmp && \
	pip2 install --no-cache-dir ./wheel2/*.whl && \
	pip3 install --no-cache-dir ./wheel3/*.whl && \
	rm -rf /tmp/* && \
	mkdir -p /opt/notebooks && \
	rm /bin/sh && \
	ln -s /bin/bash /bin/sh

## Notebook dir
ENV NOTEBOOK_DIR=/opt/notebooks
ENV NOTEBOOK_IP=0.0.0.0

## Export Jupyter Notebook
EXPOSE 8888

## When use this image in our environment, we should use the --entrypoint to overwrite this default.
## And we can add --ip=192.x.x.x to make sure the jupyter notebook can listening on a non-localhost ip.
# ENTRYPOINT ["jupyter", "notebook", "--notebook-dir=NOTEBOOK_DIR"]
ENTRYPOINT source /root/.bashrc && \
	export SHELL=/bin/bash && \
	jupyter notebook --notebook-dir=$NOTEBOOK_DIR --ip=$NOTEBOOK_IP --allow-root

