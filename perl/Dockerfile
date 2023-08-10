FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

WORKDIR /opt

RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends libzmq-dev && \
    apt-get clean && rm -rf /var/lib/apt/lists/*  && \
    zef install Jupyter::Kernel && \
    jupyter-kernel.raku --generate-config && \
    jupyter notebook --generate-config && \
    jupyter console --generate-config

USER ${NB_UID}
WORKDIR "${HOME}"