ARG SAGE_VERSION=9.0
ARG SAGE_PYTHON_VERSION=3.7

FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:base-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

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
RUN conda config --add channels conda-forge && \
    conda install -y sagemath && \
    conda clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

# Install the sagemath kernel
RUN sage -i jupyter_kernel && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

