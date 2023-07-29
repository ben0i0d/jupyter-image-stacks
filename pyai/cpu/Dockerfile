FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:scipy-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# install tf torch transformers
RUN pip install --ignore-installed --upgrade "https://eoelab.org:1024/index.php/s/NQoY3sgq88eKx3L/download/tensorflow-2.7.0-cp38-cp38-linux_x86_64.whl" --user && \
    pip install tensorrt  transformers && \
    pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu && \
    pip cache purge

