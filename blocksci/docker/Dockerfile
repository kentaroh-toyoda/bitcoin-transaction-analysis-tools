FROM ubuntu:18.04

USER root
RUN apt update -y
RUN apt install -y curl software-properties-common
RUN add-apt-repository ppa:ubuntu-toolchain-r/test -y
RUN apt update -y
RUN apt install -y cmake libtool autoconf libboost-filesystem-dev libboost-iostreams-dev \
  libboost-serialization-dev libboost-thread-dev libboost-test-dev libssl-dev libjsoncpp-dev \
  libcurl4-openssl-dev libjsoncpp-dev libjsonrpccpp-dev libsnappy-dev zlib1g-dev libbz2-dev \
  liblz4-dev libzstd-dev libjemalloc-dev libsparsehash-dev gcc-7 g++-7 git-core

WORKDIR /usr/local/src
RUN git clone https://github.com/citp/BlockSci.git
WORKDIR /usr/local/src/BlockSci
RUN git checkout tags/v0.7.0
RUN mkdir release
WORKDIR /usr/local/src/BlockSci/release
RUN CC=gcc-7 CXX=g++-7 cmake -DCMAKE_BUILD_TYPE=Release ..
RUN make
RUN make install

ARG UNAME
ARG UID
ARG GID
RUN groupadd -g $GID -o $UNAME
RUN useradd -m -u $UID -g $GID -o -s /bin/bash $UNAME
USER $UNAME
WORKDIR /home/$UNAME
CMD blocksci_parser config.json generate-config bitcoin blocksci-output \
	--max-block $MAX_BLOCK --disk .bitcoin && \
	blocksci_parser config.json update
