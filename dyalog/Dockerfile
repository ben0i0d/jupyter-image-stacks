FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

WORKDIR /opt
ENV CODENAME=$(lsb_release -sc)
RUN wget -O - http://packages.dyalog.com/dyalog-apt-key.gpg.key | sudo apt-key add -  && \
    echo "deb http://packages.dyalog.com ${CODENAME} main" | sudo tee /etc/apt/sources.list.d/dyalog.list  && \
    apt-get update --yes && \
    apt-get install --yes --no-install-recommends dyalog-unicode&& \
    apt-get clean && rm -rf /var/lib/apt/lists/*  && \
    git clone --depth=1 https://eoelab.org:1031/mirrored-sources/dyalog-jupyter-kernel.git && cd dyalog-jupyter-kernel && \
    bash install.sh

USER ${NB_UID}
WORKDIR "${HOME}"


