FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/base:stable

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]


# pip install 
RUN pip install tensorflow && \
    pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu && \
    pip install  transformer && \
    pip cache purge
