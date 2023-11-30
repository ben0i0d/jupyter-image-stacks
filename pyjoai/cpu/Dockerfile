FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:scipyjo-c

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# install tf torch transformers
RUN pip install tensorflow tensorrt transformers && \
    pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu && \
    pip cache purge

