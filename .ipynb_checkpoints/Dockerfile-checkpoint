# FROM nvidia/cuda:12.0.0-cudnn8-devel-ubuntu22.04
# FROM nvidia/cuda:12.2.0-base-ubuntu22.04
FROM nvidia/cuda:12.0.1-cudnn8-runtime-ubuntu22.04

RUN apt update && \
    apt install -y wget && \
    mkdir -p ~/miniconda3 && \
    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh && \
    bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3 && \
    rm -rf ~/miniconda3/miniconda.sh

ENV PATH="/root/miniconda3/bin:${PATH}"
ENV CONDA_PREFIX="/root/miniconda3"

RUN conda install -c conda-forge cudatoolkit=11.8.0 && \
    conda install python=3.10

RUN python -m pip install --upgrade pip &&\ 
    python -m pip install nvidia-cudnn-cu11==8.5.0.96 tensorflow==2.13.0 
    
# nvidia-cudnn-cu11==8.6.0.163

SHELL ["/bin/bash", "-c"]

RUN mkdir -p $CONDA_PREFIX/etc/conda/activate.d &&\
    echo 'CUDNN_PATH=$(dirname $(python -c "import nvidia.cudnn;print(nvidia.cudnn.__file__)"))' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh &&\
    echo 'export LD_LIBRARY_PATH=$CUDNN_PATH/lib:$CONDA_PREFIX/lib/:$LD_LIBRARY_PATH' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh &&\
    source $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh &&\
    conda init bash &&\
    . ~/.bashrc
    
COPY . .
