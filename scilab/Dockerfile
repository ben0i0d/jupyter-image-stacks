FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

ENV PATH ${PATH}:/opt/scilab/bin \
    SCILAB_EXECUTABLE="scilab-adv-cli"
#install novnc+desktop
RUN cd /opt  && \
    wget https://www.scilab.org/download/2023.1.0/scilab-2023.1.0.bin.x86_64-linux-gnu.tar.xz && \
    tar xf scilab-2023.1.0.bin.x86_64-linux-gnu.tar.xz && \
    mv scilab-2023.1.0 scilab 

USER ${NB_UID}

RUN pip install scilab_kernel && \
    pip cache purge
