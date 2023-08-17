FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-g

LABEL maintainer="Pd.ch <cpd233@outlook.com>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# IPython notebook support
RUN mamba install --yes 'ipympl' 'ipywidgets' 'widgetsnbextension'&& \
    mamba clean --all -f -y 
