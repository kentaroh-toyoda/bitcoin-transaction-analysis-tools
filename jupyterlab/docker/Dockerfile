# ref: https://github.com/priordice/blocksci-docker/blob/master/dockerfile
FROM jupyter/scipy-notebook:lab-3.4.3

USER jovyan
WORKDIR /home/jovyan
COPY ./config.json .
RUN conda install -c conda-forge multiprocess
RUN conda install -c conda-forge psutil
RUN conda install -c conda-forge pycrypto
RUN conda install -c conda-forge requests
RUN conda install -c conda-forge dateparser
RUN conda install conda-build

# setup for blocksci
USER root
WORKDIR /usr/local/src
RUN apt update -y
RUN apt install -y curl software-properties-common
RUN add-apt-repository ppa:ubuntu-toolchain-r/test -y
RUN apt update -y
RUN apt install -y cmake libtool autoconf libboost-filesystem-dev \
  libboost-iostreams-dev libboost-serialization-dev libboost-thread-dev \
  libboost-test-dev libssl-dev libjsoncpp-dev libcurl4-openssl-dev \
  libjsoncpp-dev libjsonrpccpp-dev libsnappy-dev zlib1g-dev libbz2-dev \
  liblz4-dev libzstd-dev libjemalloc-dev libsparsehash-dev gcc-7 g++-7 git-core
RUN git clone https://github.com/citp/BlockSci.git
WORKDIR /usr/local/src/BlockSci
RUN mkdir release
WORKDIR /usr/local/src/BlockSci/release
RUN CC=gcc-7 CXX=g++-7 cmake -DCMAKE_BUILD_TYPE=Release ..
RUN make
RUN make install
WORKDIR /usr/local/src/BlockSci/blockscipy/
RUN CC=gcc-7 CXX=g++-7 pip3 install -e /usr/local/src/BlockSci/blockscipy

WORKDIR /home/jovyan
RUN fix-permissions .

# setup for jupyterlab
USER jovyan
WORKDIR /home/jovyan
RUN pip install jupyterlab-vim jupyterlab-fonts tqdm ipywidgets sklearn plotnine
RUN jupyter labextension install @jupyter-widgets/jupyterlab-manager
RUN jupyter labextension install jupyterlab_onedarkpro

ENTRYPOINT ["jupyter", "lab"]
