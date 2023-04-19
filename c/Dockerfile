FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:base-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root
WORKDIR /tmp

RUN apt-get update --yes && \
    apt-get upgrade --yes && \
    apt-get install --yes --no-install-recommends build-essential && \
    apt-get clean && rm -rf /var/lib/apt/lists/* 

COPY ./ jupyter_c_kernel/

RUN pip install --no-cache-dir -e jupyter_c_kernel/ > piplog.txt && \
    cd jupyter_c_kernel && install_c_kernel --user > installlog.txt


WORKDIR /home/$NB_USER/
USER $NB_USER
