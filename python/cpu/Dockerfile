FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

# Install all OS dependencies for notebook server that starts but lacks all
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends tzdata openssh-client less fonts-liberation pandoc run-one && \
    apt-get clean && rm -rf /var/lib/apt/lists/* 
# Switch back to jovyan to avoid accidental container runs as root
USER ${NB_UID}
# python-lsp
RUN pip install jupyterlab-lsp && \
    mamba install -c conda-forge python-lsp-server && \
    pip cache purge && \
    mamba clean --all -f -y 
