ARG OWNER=jupyter
ARG BASE_CONTAINER=$OWNER/scipy-notebook
FROM $BASE_CONTAINER

LABEL maintainer="eoelab<eoelab@eoelab.org>"


SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install Tensorflow
RUN mamba install --yes \
    'tensorflow' && \
    mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"
