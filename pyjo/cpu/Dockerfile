FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c

LABEL maintainer="Pd.ch <cpd233@outlook.com>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

ENV MODULAR_HOME="/opt/modular" \
    PATH="$PATH:/opt/modular/pkg/packages.modular.com_mojo/bin"

RUN curl https://get.modular.com | MODULAR_AUTH=mut_631a7d7cd03d4185b786385c42384278 sh - && \
    modular install mojo && python /opt/modular/pkg/packages.modular.com_mojo/jupyter/manage_kernel.py install --no-user || echo "install mojo failed" && \
    apt-get clean && rm -rf /var/lib/apt/lists/* && \
    fix-permissions /opt/modular && \
    fix-permissions "/home/${NB_USER}" && \
    fix-permissions "${CONDA_DIR}"
    
USER ${NB_UID}