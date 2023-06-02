FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

COPY evcxr_jupyter /tmp/evcxr_jupyter

ENV CARGO_HOME=/opt/rust/.cargo \
    RUSTUP_HOME=/opt/rust/.rust
RUN mkdir -p /opt/rust/.cargo /opt/rust/.rust /opt/conda/share/jupyter/kernels/rust && \
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -y && \
    chmod +x /tmp/evcxr_jupyter && \
    /tmp/evcxr_jupyter --install && \
    rm /tmp/evcxr_jupyter && \
    cp $HOME/.local/share/jupyter/kernels/* /opt/conda/share/jupyter/kernels/rust/ && \
    fix-permissions /opt/rust && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"
USER $NB_USER