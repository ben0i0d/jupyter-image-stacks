FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:base-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    dvipng \
    ffmpeg \
    imagemagick \
    texlive \
    tk tk-dev \
    jq && \
    rm -rf /var/lib/apt/lists/*

USER ${NB_UID}

# Add conda-forge channel and install sagemath
RUN mamba config --add channels conda-forge && \
    mamba install -y sagemath && \
    mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

# Install the sagemath kernel
RUN sage -i jupyter_kernel && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

