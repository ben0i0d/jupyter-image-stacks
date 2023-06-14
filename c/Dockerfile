FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends build-essential && \
    apt-get clean && rm -rf /var/lib/apt/lists/* && \
    fix-permissions /opt
    
USER ${NB_UID}
# install Kernel
RUN cd /opt && \
    git clone https://github.com/XaverKlemenschits/jupyter-c-kernel.git && \
    cd jupyter-c-kernel && \
    pip install -e .  && \
    cd jupyter_c_kernel && install_c_kernel --user 