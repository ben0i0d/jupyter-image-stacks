FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]


RUN pip install jupyterlab-lsp && \
    mamba install --yes 'python-lsp-server' \
    # IPython notebook support
    'ipympl' 'ipywidgets' 'widgetsnbextension'&& \
    pip cache purge && \
    mamba clean --all -f -y 
