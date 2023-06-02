FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root
ENV CARGO_HOME=/opt/rust/.cargo \
    RUSTUP_HOME=/opt/rust/.rust \
    PATH=$PATH:/opt/rust/.cargo/bin

RUN mkdir -p /opt/rust/.cargo /opt/rust/.rust

#COPY evcxr_jupyter /opt/rust/evcxr_jupyter
COPY rustup-init.sh /opt/rust/rustup-init.sh

RUN bash /opt/rust/rustup-init.sh -y && \
#    chmod +x /opt/rust/evcxr_jupyter && \
#    /opt/rust/evcxr_jupyter --install && \
    rm /opt/rust/rustup-init.sh && \
    cargo install evcxr_jupyter && \
    evcxr_jupyter --install && \
    cp -r $HOME/.local/share/jupyter/kernels/rust /opt/conda/share/jupyter/kernels/ && \
    rustup component add rust-src && \
    fix-permissions /opt/rust && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"
    
USER $NB_USER