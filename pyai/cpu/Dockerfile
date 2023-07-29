FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:scipy-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# install tf torch transformers
RUN pip install intel-tensorflow intel-extension-for-tensorflow[cpu] tensorrt  transformers && \
    pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu && \
    pip install intel_extension_for_pytorch && \
    pip cache purge

