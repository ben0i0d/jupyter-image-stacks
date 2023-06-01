FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root
# Install system dependencies
RUN apt-get update && apt-get install -yq --no-install-recommends golang gcc libc6-dev && \
    apt-get clean && rm -rf /var/lib/apt/lists/* && \
    go env -w GOPROXY=https://mirrors.aliyun.com/goproxy/,direct && \
    go install github.com/gopherdata/gophernotes@v0.7.5 && \
    mkdir -p ${CONDA_DIR}/share/jupyter/kernels/gophernotes && \
    cd ${CONDA_DIR}/share/jupyter/kernels/gophernotes && \
    cp "$(go env GOPATH)"/pkg/mod/github.com/gopherdata/gophernotes@v0.7.5/kernel/*  "." && \
    chmod +w ./kernel.json  && \
    # in case copied kernel.json has no write permission
    sed "s|gophernotes|$(go env GOPATH)/bin/gophernotes|" < kernel.json.in > kernel.json  && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}" 

USER $NB_USER