FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:scipy-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# install tf torch transformers
RUN pip install https://eoelab.org:1031/build-package-stacks/tensorflow-build-stacks/-/raw/main/archive/linux/cpu/avx2+mkl/tensorflow-2.13.0-cp310-cp310-linux_x86_64.whl && \ 
    pip install tensorrt transformers && \
    pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu && \
    pip cache purge

