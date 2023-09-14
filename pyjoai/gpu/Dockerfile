FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:scipyjo-g

LABEL maintainer="Pd.ch <cpd233@outlook.com>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# install tf torch transformers
RUN pip install tensorflow transformers && \
    pip install torch torchvision torchaudio && \
    python -m pip install paddlepaddle-gpu==2.5.1.post120 -f https://www.paddlepaddle.org.cn/whl/linux/mkl/avx/stable.html && \
    pip cache purge

