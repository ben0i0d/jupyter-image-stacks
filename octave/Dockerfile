FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root
# Install Octave
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends octave gnuplot liboctave-dev qt5-default && \
    apt-get clean && rm -rf /var/lib/apt/lists/* 

USER ${NB_UID}

# Install Octave kernel
RUN mamba install --quiet --yes -c conda-forge octave_kernel && \
    mamba clean --all -f -y 
