FROM pytorch/pytorch:1.8.1-cuda10.2-cudnn7-devel

ARG TZ=America/Recife
ENV DEBIAN_FRONTEND=noninteractive

ENV CUDA_HOME=/usr/local/cuda
ENV PATH=$CUDA_HOME/bin:$PATH
ENV LD_LIBRARY_PATH=$CUDA_HOME/lib64:$LD_LIBRARY_PATH

WORKDIR /home

# Instala curl ignorando erros de GPG temporariamente
RUN apt-get update || true && apt-get install -y --no-install-recommends curl

# Adiciona as duas chaves públicas do repositório NVIDIA
RUN curl -fsSL https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub | apt-key add - && \
    curl -fsSL https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/3bf863cc.pub | apt-key add -

RUN apt-get update && apt-get install -y \
    tzdata \
    wget \
    git \
    build-essential \
    libgl1-mesa-glx \
    libglib2.0-0 \
    gcc \
    python3-dev

# Instala dependências via pip
RUN pip install --upgrade pip && \
    pip install ipython ninja yacs cython matplotlib tqdm nuscenes-devkit scikit-image opencv-python

# Instala cocoapi
RUN git clone https://github.com/cocodataset/cocoapi.git && \
    cd cocoapi/PythonAPI && \
    python setup.py build_ext install && \
    cd /home && rm -rf cocoapi

# Clona e instala o fork do SAF-FCOS
RUN git clone https://github.com/emersontop/SAF-FCOS_live.git && \
    cd SAF-FCOS_live && \
    python setup.py build develop --no-deps

CMD ["bash"]