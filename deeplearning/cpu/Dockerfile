FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:scipy-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# install tf 
RUN mamba install tensorflow transformers && \
    mamba clean --all -f -y 
# install torch oneflow
RUN pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu && \
    pip install --find-links https://release.oneflow.info oneflow==0.9.0+cpu && \
    pip cache purge
# Switch back to jovyan to avoid accidental container runs as root
USER ${NB_UID}

WORKDIR "${HOME}"
