# Copyright 2023 The MathWorks, Inc.
FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="Pd.ch <cpd233@outlook.com>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

COPY dep.txt /tmp/dep.txt

USER root

RUN apt-get update && apt-get install --no-install-recommends -y `cat /tmp/dep.txt` && rm /tmp/dep.txt && \
    apt-get clean && rm -rf /var/lib/apt/lists/* 
