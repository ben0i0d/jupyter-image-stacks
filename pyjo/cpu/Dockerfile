FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c

LABEL maintainer="Pd.ch <cpd233@outlook.com>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

ENV MODULAR_HOME="/opt/modular" \
    PATH="$PATH:/opt/modular/pkg/packages.modular.com_mojo/bin"

#COPY deb_conf/gpg.0E4925737A3895AD.key  /tmp/gpg.0E4925737A3895AD.key
#COPY deb_conf/modular-installer.list  /etc/apt/sources.list.d/modular-installer.list

RUN curl -1sLf 'https://dl.modular.com/bBNWiLZX5igwHXeu/installer/gpg.0E4925737A3895AD.key' |  gpg --dearmor >> /usr/share/keyrings/modular-installer-archive-keyring.gpg && \
    curl -1sLf 'https://dl.modular.com/bBNWiLZX5igwHXeu/installer/config.deb.txt?distro=debian&codename=wheezy' > /etc/apt/sources.list.d/modular-installer.list && \
    apt-get update && apt-get install -y modular && \
    apt-get clean && rm -rf /var/lib/apt/lists/* && \
    mkdir /opt/modular && modular auth mut_631a7d7cd03d4185b786385c42384278 && modular install mojo && \
    python /opt/modular/pkg/packages.modular.com_mojo/jupyter/manage_kernel.py install --no-user  && \
    fix-permissions /opt/modular && \
    fix-permissions "/home/${NB_USER}" && \
    fix-permissions "${CONDA_DIR}"
    
USER ${NB_UID}