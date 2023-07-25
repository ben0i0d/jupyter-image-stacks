FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# python-lsp
RUN pip install jupyterlab-lsp && \
    mamba install -c conda-forge python-lsp-server && \
    pip cache purge && \
    mamba clean --all -f -y 
