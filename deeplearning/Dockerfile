FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/base:stable

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]


# pip install 
RUN mamba install tensorflow && \
    mamba install pytorch torchvision torchaudio cpuonly -c pytorch && \
    mamba clean --all -f -y 
