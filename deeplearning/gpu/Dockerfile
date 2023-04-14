FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:scipy-g

LABEL maintainer="eoelab <eoelab@eoelab.org>"
USER ${NB_UID}
# pip install 
RUN pip install tensorflow && \
    pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118 && \
    pip install transformers && \
    pip install --find-links https://release.oneflow.info oneflow==0.9.0+cpu && \
    pip cache purge

