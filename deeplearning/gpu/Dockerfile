FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:scipy-g

LABEL maintainer="eoelab <eoelab@eoelab.org>"

# install tf 
RUN mamba install tensorflow 
# install Torch
RUN mamba install pytorch torchvision torchaudio pytorch-cuda=11.7 -c pytorch -c nvidia
# install transformer
RUN mamba install transformers
#conda clean
RUN mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"
#oneflow install
RUN pip install --find-links https://release.oneflow.info oneflow==0.9.0+cu117 && \
    pip cache purge
# Switch back to jovyan to avoid accidental container runs as root
USER ${NB_UID}

WORKDIR "${HOME}"

