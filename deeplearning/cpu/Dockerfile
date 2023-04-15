FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:scipy-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# install tf 
RUN mamba install tensorflow transformers
# install Torch
RUN mamba install pytorch torchvision torchaudio cpuonly -c pytorch
#oneflow install
RUN pip install --find-links https://release.oneflow.info oneflow==0.9.0+cpu && \
    pip cache purge
#conda clean
RUN mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"
# Switch back to jovyan to avoid accidental container runs as root
USER ${NB_UID}

WORKDIR "${HOME}"
