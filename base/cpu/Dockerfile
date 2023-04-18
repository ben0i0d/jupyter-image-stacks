FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

# Install all OS dependencies for notebook server that starts but lacks all
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends git nano-tiny tzdata zip unzip vim-tiny openssh-client less texlive-xetex \
    texlive-fonts-recommended texlive-plain-generic xclip fonts-liberation pandoc run-one && \
    apt-get clean && rm -rf /var/lib/apt/lists/* && \
    update-alternatives --install /usr/bin/nano nano /bin/nano-tiny 10
# Switch back to jovyan to avoid accidental container runs as root
USER ${NB_UID}


