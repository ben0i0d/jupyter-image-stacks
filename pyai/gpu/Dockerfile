FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:scipy-g

LABEL maintainer="Pd.ch <cpd233@outlook.com>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# install tf torch transformers
RUN pip install tensorflow transformers && \
    pip install torch torchvision torchaudio && \
    pip cache purge

