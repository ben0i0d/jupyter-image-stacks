FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

WORKDIR /opt

RUN mamba install --yes conda-build && mamba clean --all -f -y && \
    git clone --depth=1 https://eoelab.org:1031/mirrored-sources/cadabra2.git && cd cadabra2 && cd conda && conda-build . && \
    mamba install --use-local cadabra2 && \
    fix-permissions "/home/${NB_USER}" && \
    fix-permissions "${CONDA_DIR}"
USER ${NB_UID}
WORKDIR "${HOME}"