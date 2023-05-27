FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

RUN apt-get update --yes && \
    apt-get upgrade --yes && \
    apt-get install --yes --no-install-recommends build-essential  && \
    apt-get clean && rm -rf /var/lib/apt/lists/* 

USER ${NB_UID}

RUN pip install jupyterlab-novnc && \
    pip cache purge

