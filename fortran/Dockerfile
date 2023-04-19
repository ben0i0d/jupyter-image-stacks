FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:base-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

#USER root
# R pre-requisites
#RUN apt-get update --yes && \
#    apt-get install --yes --no-install-recommends gfortran libgfortran5 && \
#    apt-get clean && rm -rf /var/lib/apt/lists/*

#USER ${NB_UID}
# R packages including IRKernel which gets installed globally.
RUN mamba install --yes -c conda-forge lfortran && \
    mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"
