FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/base:stable

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]


# pip install 
RUN conda install tensorflow && \
    conda install pytorch torchvision torchaudio cpuonly -c pytorch && \
    conda clean --all -f -y 