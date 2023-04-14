FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:scipy-g

LABEL maintainer="eoelab <eoelab@eoelab.org>"
USER ${NB_UID}
# install 
RUN conda install tensorflow transformers&& \
    conda install pytorch torchvision torchaudio pytorch-cuda=11.7 -c pytorch -c nvidia && \
    pip install --find-links https://release.oneflow.info oneflow==0.9.0+cu117 && \
    pip cache purge && \
    conda clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

