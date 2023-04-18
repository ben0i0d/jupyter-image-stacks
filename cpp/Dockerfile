FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:base-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

RUN apt-get update --yes && \
    apt-get upgrade --yes && \
    apt-get install --yes --no-install-recommends build-essential && \
    apt-get clean && rm -rf /var/lib/apt/lists/* 

RUN pip install jupyter-c-kernel && \
    install_c_kernel && \
    pip cache purge 

USER ${NB_UID}

# Install C & Cpp kernel
RUN mamba install --quiet --yes -c conda-forge xeus-cling && \
    mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"
