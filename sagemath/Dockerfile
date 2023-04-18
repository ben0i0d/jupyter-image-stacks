ARG SAGE_VERSION=9.0
ARG SAGE_PYTHON_VERSION=3.7

FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:base-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

USER root

RUN apt-get update && \
    apt-get install -y git && \
    apt-get install -y libpng16-16 libjpeg62-turbo-dev libfreetype6-dev zlib1g-dev libxml2-dev libxslt1-dev libssl-dev \ 
    libffi-dev libcairo2-dev libgtk-3-dev libglu1-mesa-dev xvfb && \
    rm -rf /var/lib/apt/lists/*

USER $NB_UID

RUN mamba install -c conda-forge sagemath

RUN sage -i jupyterlab && \
    jupyter labextension install @jupyter-widgets/jupyterlab-manager jupyter-matplotlib && \
    jupyter lab build

