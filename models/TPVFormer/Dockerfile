FROM nvidia/cuda:10.2-cudnn8-devel-ubuntu18.04

RUN apt-get update && apt-get install -y \
    git \
    python3.8-dev \
    python3-pip \
    cmake \
    libboost-all-dev \
    wget \
    && rm -rf /var/lib/apt/lists/*
# Workaround for checking repository update
ADD https://api.github.com/repos/wzzheng/TPVFormer/git/refs/heads/main version.json
RUN git clone -b main https://github.com/wzzheng/TPVFormer.git

RUN python3.8 -m pip install -U pip
RUN python3.8 -m pip install torch==1.10.0 openmim
ENV LANG C.UTF-8
RUN mim install mmcv-full==1.4.0
RUN mim install mmdet==2.14.0
RUN mim install mmsegmentation==0.14.1

# Workaround for cuda error (https://github.com/HawkAaron/warp-transducer/issues/15)
ENV CUDA_HOME /usr/local/cuda-10.2
ENV LD_LIBRARY_PATH $LD_LIBRARY_PATH:${CUDA_HOME}/lib64
ENV PATH $PATH:${CUDA_HOME}/bin
ENV LIBRARY_PATH ${CUDA_HOME}/lib64:${LIBRARY_PATH}
ENV CFLAGS="-I$CUDA_HOME/include $CFLAGS"

# install spconv header
# RUN pip3 install spconv-cu102
ADD https://api.github.com/repos/traveller59/spconv/git/refs/heads/master version_spconv.json
RUN git clone https://github.com/traveller59/spconv
WORKDIR spconv
RUN python3.8 -m pip install cmake --upgrade
RUN git submodule init && git submodule update
RUN python3.8 -m pip install pccm
RUN python3.8 setup.py bdist_wheel
RUN cd dist && pip3 install *.whl

WORKDIR /
ENV USE_CUDA 1
ENV USE_CUDNN 1
ENV TORCH_CUDA_ARCH_LIST "6.0 6.1 7.0+PTX" 
RUN git clone https://github.com/open-mmlab/mmdetection3d.git -b v0.18.1
WORKDIR mmdetection3d
RUN pip install -e .
WORKDIR ../TPVFormer
# RUN python3.8 -m pip install numba==0.48.0 numpy==1.19.0 Pillow==9.3.0 PyYAML==6.0 timm==0.4.12
RUN python3.8 -m pip install timm==0.4.12
RUN wget https://cloud.tsinghua.edu.cn/f/3fbd12101ead4397a0f7/?dl=1 -o tpv04_occupancy_v2.pth
RUN wget https://cloud.tsinghua.edu.cn/f/f686e97feb18406592d9/?dl=1 -o tpv10_lidarseg_v2.pth