FROM nvidia/cuda:11.3.0-cudnn8-runtime-ubuntu20.04

RUN apt-get update && apt-get install -y \
    git \
    python3 \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*
# Workaround for checking repository update
ADD https://api.github.com/repos/wzzheng/TPVFormer/git/refs/heads/main version.json
RUN git clone -b main https://github.com/wzzheng/TPVFormer.git

WORKDIR TPVFormer
RUN python3 -m pip install -U pip
RUN python3 -m pip install torch==1.10.0
RUN python3 -m pip install -r requirements.txt
