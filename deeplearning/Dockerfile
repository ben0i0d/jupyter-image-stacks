FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/base:stable

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]


# pip install 
RUN pip install tensorflow transformer && \
    pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu && \
    wget https://oneflow-release.oss-cn-beijing.aliyuncs.com/pip/v0.9.0/cpu/df8aa87461ee29a8ace350d6948b0d5cac49af2c/oneflow-0.9.0%2Bcpu-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl && \
    pip install "oneflow-0.9.0+cpu-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl" && \
    rm oneflow-0.9.0%2Bcpu-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl && \
    pip cache purge
