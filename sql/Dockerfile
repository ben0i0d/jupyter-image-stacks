FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="Pd.ch <cpd233@outlook.com>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

WORKDIR /opt

RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends sqlite3 && \
    apt-get clean && rm -rf /var/lib/apt/lists/*  && \
    pip install duckdb_kernel && \
    pip cache purge && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

USER ${NB_UID}
WORKDIR "${HOME}"
