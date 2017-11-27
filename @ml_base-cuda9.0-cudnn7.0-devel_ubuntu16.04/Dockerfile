# Image is based Ubuntu 16.04 no GPU support
# This image will contain most of machine learning packages as a base image


FROM amax/general:9.0-devel-cudnn7.0_ubuntu16.04_v01 as dev
### Installation pre-requirements
RUN  apt-get update && apt-get install -y --no-install-recommends \
	curl \
	git \
	ssh \
	python \
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
	build-essential \
	libgtk-3-dev \
	libboost-all-dev

### Install 'pip' and machine learning packages
RUN curl -O https://bootstrap.pypa.io/get-pip.py && \
	python get-pip.py && \
	pip install --upgrade --no-cache-dir pip && \
	pip install matplotlib \
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

### Save all of the python packages into /wheel folder
RUN mkdir /wheel && \
    cd /wheel && \
    pip wheel matplotlib \
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

### Build CPU only Machine Learning image
FROM amax/general:9.0-devel-cudnn7.0_ubuntu16.04_v01
COPY --from=dev /wheel /tmp/
RUN apt-get update && apt-get install -y --no-install-recommends \
        curl \
        git \
        ssh \
        python \
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

RUN curl -O https://bootstrap.pypa.io/get-pip.py && \
        python get-pip.py && \
        pip install --upgrade --no-cache-dir pip && \
	cd /tmp && \
	pip install ./*.whl && \
	mkdir -p /opt/notebooks && \
	rm -rf /tmp/*.whl

## Notebook dir
ENV NOTEBOOK_DIR=/opt/notebooks
ENV NOTEBOOK_IP=0.0.0.0

## Export Jupyter Notebook
EXPOSE 8888

## When use this image in our environment, we should use the --entrypoint to overwrite this default.
## And we can add --ip=192.x.x.x to make sure the jupyter notebook can listening on a non-localhost ip.
# ENTRYPOINT ["jupyter", "notebook", "--notebook-dir=NOTEBOOK_DIR"]
ENTRYPOINT jupyter notebook --notebook-dir=$NOTEBOOK_DIR --ip=$NOTEBOOK_IP --allow-root

