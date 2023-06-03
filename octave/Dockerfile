FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root
# Install Octave + development_files + doc + package + qt5
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends octave liboctave-dev octave-doc \
    octave-image \
    build-essential qtbase5-dev qtchooser qt5-qmake qtbase5-dev-tools qt5* && \
    apt-get clean && rm -rf /var/lib/apt/lists/* 

USER ${NB_UID}

# Install Octave kernel
RUN mamba install --quiet --yes -c conda-forge octave_kernel && \
    mamba clean --all -f -y 
