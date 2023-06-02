FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root
COPY evcxr_jupyter /tmp/evcxr_jupyter
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh && \
    chmod +x /tmp/evcxr_jupyter && \
    /tmp/evcxr_jupyter --install

USER $NB_USER