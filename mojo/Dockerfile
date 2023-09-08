FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="Pd.ch <cpd233@outlook.com>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

ENV keyring_location=/usr/share/keyrings/modular-installer-archive-keyring.gpg 
# Install depency
RUN  apt-get update  && apt-get install -y apt-transport-https g++  && \
    curl -1sLf 'https://dl.modular.com/bBNWiLZX5igwHXeu/installer/gpg.0E4925737A3895AD.key' |  gpg --dearmor >> ${keyring_location}  && \
    curl -1sLf 'https://dl.modular.com/bBNWiLZX5igwHXeu/installer/config.deb.txt?distro=debian&codename=wheezy' > /etc/apt/sources.list.d/modular-installer.list  && \
    apt-get update  && \
    apt-get install -y modular && \
    apt-get clean && rm -rf /var/lib/apt/lists/* 

USER ${NB_UID}

RUN modular auth mut_631a7d7cd03d4185b786385c42384278 && \
    modular install mojo