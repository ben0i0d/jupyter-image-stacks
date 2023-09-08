FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="Pd.ch <cpd233@outlook.com>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

ENV MODULAR_HOME="/opt/modular" \
    PATH="$PATH:/opt/modular/pkg/packages.modular.com_mojo/bin"

COPY deb_conf/gpg.0E4925737A3895AD.key  /tmp/gpg.0E4925737A3895AD.key
COPY deb_conf/proxy.conf /etc/apt/apt.conf.d/proxy.conf

# Install depency
RUN apt-get update  && apt-get install -y gnupg2 && \
    cat /tmp/gpg.0E4925737A3895AD.key |  gpg --dearmor >> /usr/share/keyrings/modular-installer-archive-keyring.gpg

COPY deb_conf/modular-installer.list  /etc/apt/sources.list.d/modular-installer.list

RUN apt-get update  && \
    apt-get install -y modular && \
    rm /etc/apt/apt.conf.d/proxy.conf && apt-get clean && rm -rf /var/lib/apt/lists/* && \
    mkdir /opt/modular && fix-permissions /opt/modular

USER ${NB_UID}

RUN modular auth mut_631a7d7cd03d4185b786385c42384278 && modular install mojo && \
    jupyter labextension disable "@jupyterlab/apputils-extension:announcements" && \
    mkdir -p /opt/conda/share/jupyter/kernels/mojo && \
    cp /opt/modular/pkg/packages.modular.com_mojo/jupyter/kernel/* /opt/conda/share/jupyter/kernels/mojo/
