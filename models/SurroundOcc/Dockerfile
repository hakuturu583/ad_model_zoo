# See also https://github.com/weiyithu/SurroundOcc/blob/main/docs/install.md#step-by-step-installation-instructions

FROM nvidia/cuda:11.3.0-devel-ubuntu20.04

# Use bash
SHELL ["/bin/bash", "-c"]

# Install conda
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update && apt-get install -y \
    git \
    wget \
    unzip \
    libgl1-mesa-dev \
    libglib2.0-0 \
    && rm -rf /var/lib/apt/lists/*
RUN wget https://repo.anaconda.com/miniconda/Miniconda3-py37_23.1.0-1-Linux-x86_64.sh && \
    chmod a+x Miniconda3-py37_23.1.0-1-Linux-x86_64.sh && \
    ./Miniconda3-py37_23.1.0-1-Linux-x86_64.sh -b && \
    rm Miniconda3-py37_23.1.0-1-Linux-x86_64.sh
ENV PATH $PATH:/root/miniconda3/bin

# 1. Create a conda virtual environment and activate it.
RUN conda create -n surroundocc python=3.8 -y
SHELL ["conda", "run", "-n", "surroundocc", "/bin/bash", "-c"]

# 2. Install PyTorch and torchvision (tested on torch==1.10.1 & cuda=11.3).
RUN pip install torch==1.9.1+cu111 torchvision==0.10.1+cu111 torchaudio==0.9.1 -f https://download.pytorch.org/whl/torch_stable.html

# 3. Install gcc>=5 in conda env.
RUN conda install -c omgarcia gcc-6

# 4. Install MMCV following the official instructions.
RUN pip install mmcv-full==1.4.0

# 5. Install mmdet and mmseg.
RUN pip install mmdet==2.14.0 && \
    pip install mmsegmentation==0.14.1

RUN pip install scikit-image==0.20.0 networkx>=2.8

# 6. Install mmdet3d from source code.
RUN git clone -b v0.17.1 https://github.com/open-mmlab/mmdetection3d.git
WORKDIR mmdetection3d
ENV USE_CUDA 1
ENV USE_CUDNN 1
ENV TORCH_CUDA_ARCH_LIST "6.0 6.1 7.0+PTX"
## Set environment variable for cuda
### See also at https://github.com/HawkAaron/warp-transducer/issues/15
ENV CUDA_HOME /usr/local/cuda-11.3
ENV CUDA_TOOLKIT_ROOT_DIR $CUDA_HOME
ENV LD_LIBRARY_PATH $CUDA_HOME/extras/CUPTI/lib64:$LD_LIBRARY_PATH
ENV LIBRARY_PATH $CUDA_HOME/lib64:$LIBRARY_PATH
ENV LD_LIBRARY_PATH $CUDA_HOME/lib64:$LD_LIBRARY_PATH
ENV CFLAGS="-I$CUDA_HOME/include $CFLAGS"
RUN python setup.py install

# # 7. Install other dependencies.
RUN pip install timm && \
    pip install open3d-python

# 8. Install Chamfer Distance.
WORKDIR ../
ADD https://api.github.com/repos/weiyithu/SurroundOcc/git/refs/heads/main version.json
RUN git clone https://github.com/weiyithu/SurroundOcc.git --recursive
WORKDIR SurroundOcc/extensions/chamfer_dist
RUN python setup.py install --user

# 9. Prepare pretrained models.
WORKDIR ../../
RUN mkdir ckpts
WORKDIR ckpts
RUN wget https://github.com/zhiqi-li/storage/releases/download/v1.0/r101_dcn_fcos3d_pretrain.pth

# Setup data directory
## See also https://github.com/weiyithu/SurroundOcc/blob/main/docs/data.md
WORKDIR ../
RUN mkdir data
WORKDIR data
RUN wget https://cloud.tsinghua.edu.cn/f/ebbed36c37b248149192/?dl=1 -O nuscenes_infos_train.pkl && \
    wget https://cloud.tsinghua.edu.cn/f/b3f169f4db034764bb87/?dl=1 -O nuscenes_infos_val.pkl && \
    wget https://cloud.tsinghua.edu.cn/f/290276f4a4024896b733/?dl=1 -O val.zip && unzip val.zip && rm val.zip && \
    wget https://cloud.tsinghua.edu.cn/f/f021006560b54bc78349/?dl=1 -O train.zip && unzip train.zip && rm train.zip