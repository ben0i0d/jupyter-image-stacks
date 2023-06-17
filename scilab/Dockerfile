FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root
# Install scilab + extension
RUN apt-get update --yes && \
    apt-get install --yes scilab \
    scilab-* && \
    apt-get clean && rm -rf /var/lib/apt/lists/* 

USER ${NB_UID}

# Install Octave kernel
RUN pip install scilab_kernel && \
    pip cache purge
