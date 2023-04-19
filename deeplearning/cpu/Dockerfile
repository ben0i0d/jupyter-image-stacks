FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:scipy-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# install tf 
#RUN pip install tensorflow transformers && \
#    pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu && \
#    pip install --find-links https://release.oneflow.info oneflow==0.9.0+cpu && \
#    pip cache purge

RUN mamba install --yes tensorflow transformers && \
    mamba install --yes pytorch torchvision torchaudio cpuonly -c pytorch && \
    pip install --find-links https://release.oneflow.info oneflow==0.9.0+cpu && \
    pip cache purge && \
    mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"
