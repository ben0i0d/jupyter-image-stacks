FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends build-essential && \
    apt-get clean && rm -rf /var/lib/apt/lists/*  && \
# system install Kernel
    cd /opt && \
    git clone https://eoelab.org:1031/mirrored-sources/jupyter-c-kernel.git && \
    cd jupyter-c-kernel && \
    pip install -e .  && \
    cd jupyter_c_kernel && chmod +x install_c_kernel && ./install_c_kernel && \
    fix-permissions /opt/jupyter-c-kernel && \
    fix-permissions "${CONDA_DIR}" 
    
USER ${NB_UID}
