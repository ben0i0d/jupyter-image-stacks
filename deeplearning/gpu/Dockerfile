FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:scipy-g

LABEL maintainer="eoelab <eoelab@eoelab.org>"


# install tf transformers
RUN pip install tensorflow transformers && \
    pip install torch torchvision torchaudio && \
    pip install oneflow && \
    pip cache purge

