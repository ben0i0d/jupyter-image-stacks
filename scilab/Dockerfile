FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root
#install denpencies
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends default-jre libxft2 libtinfo5 libjogl2-java tk curl build-essential && \
    apt-get clean && rm -rf /var/lib/apt/lists/* 
#install scilab
    cd /opt  && \
    wget https://www.scilab.org/download/2023.1.0/scilab-2023.1.0.bin.x86_64-linux-gnu.tar.xz && \
    tar xf scilab-2023.1.0.bin.x86_64-linux-gnu.tar.xz && \
    mv scilab-2023.1.0 scilab  && \
    rm scilab-2023.1.0.bin.x86_64-linux-gnu.tar.xz 

USER ${NB_UID}
ENV PATH ${PATH}:/opt/scilab/bin \
    SCILAB_EXECUTABLE='/opt/scilab/bin/scilab-adv-cli'
RUN pip install scilab_kernel && \
    pip cache purge
