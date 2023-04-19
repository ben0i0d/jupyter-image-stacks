FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:scipy-g

LABEL maintainer="eoelab <eoelab@eoelab.org>"


# install tf transformers
#RUN pip install tensorflow transformers && \
#    pip install torch torchvision torchaudio && \
#    pip install oneflow && \
#    pip cache purge

RUN mamba install --yes tensorflow transformers && \
    mamba install --yes pytorch torchvision torchaudio pytorch-cuda && \
    pip install oneflow && \
    pip cache purge && \
    mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"
