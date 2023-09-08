FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="Pd.ch <cpd233@outlook.com>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

COPY deb_conf/gpg.0E4925737A3895AD.key  /tmp/gpg.0E4925737A3895AD.key

# Install depency
RUN apt-get update  && apt-get install -y apt-transport-https gnupg2 g++ && \
    cat /tmp/gpg.0E4925737A3895AD.key |  gpg --dearmor >> /usr/share/keyrings/modular-installer-archive-keyring.gpg 

COPY deb_conf/modular-installer.list  /etc/apt/sources.list.d/modular-installer.list

RUN apt-get update  && \
    apt-get install -y modular && \
    apt-get clean && rm -rf /var/lib/apt/lists/* 

USER ${NB_UID}

RUN modular auth mut_631a7d7cd03d4185b786385c42384278 && \
    modular install mojo