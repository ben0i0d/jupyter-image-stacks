FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

WORKDIR /opt

RUN git clone https://eoelab.org:1031/mirrored-sources/jupyter-c-kernel.git && cd jupyter-c-kernel && \
    pip install --no-cache-dir -e .  && \
    cd jupyter_c_kernel && install_c_kernel --user 

USER ${NB_UID}
WORKDIR "${HOME}"




