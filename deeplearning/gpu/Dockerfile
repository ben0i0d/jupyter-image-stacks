FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:scipy-g

LABEL maintainer="eoelab <eoelab@eoelab.org>"


# install tf transformers
RUN mamba install --yes tensorflow transformers && \
    mamba clean --all -f -y 
#install torch oneflow 
RUN pip install torch torchvision torchaudio && \
    pip install oneflow && \
    pip cache purge

# Switch back to jovyan to avoid accidental container runs as root
USER ${NB_UID}

WORKDIR "${HOME}"

