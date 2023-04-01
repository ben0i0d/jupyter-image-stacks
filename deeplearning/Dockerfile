ARG OWNER=jupyter
ARG BASE_CONTAINER=$OWNER/scipy-notebook
FROM $BASE_CONTAINER

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# pip mirror
RUN pip config set global.index-url https://mirrors.bfsu.edu.cn/pypi/web/simple 
    
# pip install 
RUN pip install tensorflow
RUN pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
RUN pip cache purge 
RUN mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"
