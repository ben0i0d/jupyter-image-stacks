FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root
ENV SCILAB_EXECUTABLE='scilab-cli'
#install scilab+extension
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends scilab scilab-* && \
    apt-get clean && rm -rf /var/lib/apt/lists/*  && \

USER ${NB_UID}

RUN pip install scilab_kernel && \
    pip cache purge
