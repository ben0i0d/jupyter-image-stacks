FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends nodejs npm && \
    apt-get clean && rm -rf /var/lib/apt/lists/*  && \
    npm install -g --unsafe-perm ijavascript && \
    ijsinstall --install=global

USER ${NB_UID}


