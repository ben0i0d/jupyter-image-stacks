FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

WORKDIR /opt

COPY linux_64_18.2.45405_unicode.x86_64.deb ./

RUN dpkg -i linux_64_18.2.45405_unicode.x86_64.deb && \
    rm linux_64_18.2.45405_unicode.x86_64.deb && \
    git clone --depth=1 https://eoelab.org:1031/mirrored-sources/dyalog-jupyter-kernel.git && cd dyalog-jupyter-kernel && \
    bash install.sh

USER ${NB_UID}
WORKDIR "${HOME}"


