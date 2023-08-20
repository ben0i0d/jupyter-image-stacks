FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:scipy-c

LABEL maintainer="Pd.ch <cpd23@outlook.com>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# install tf torch transformers
RUN wget https://eoelab.org:1031/build-package-stacks/tensorflow-build-stacks/-/blob/759bb363af956c610b5349e2ba2acb36c81d93a2/archive/linux/cpu/avx2+mkl/tensorflow-2.13.0-cp311-cp311-linux_x86_64.whl && \ 
    pip install tensorflow-2.13.0-cp311-cp311-linux_x86_64.whl && rm tensorflow-2.13.0-cp311-cp311-linux_x86_64.whl && \
    pip install tensorrt transformers && \
    pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu && \
    pip cache purge

