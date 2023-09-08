FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="Pd.ch <cpd233@outlook.com>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

WORKDIR /opt

# COPY rakudo-star-2023.06-01.tar.gz ./

ENV PATH="/opt/rakudo-star/bin/:/opt/rakudo-star/share/perl6/site/bin:/opt/rakudo-star/share/perl6/vendor/bin:/opt/rakudo-star/share/perl6/core/bin:${PATH}" 

RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends libzmq-dev && \
    apt-get clean && rm -rf /var/lib/apt/lists/*  && \
    mkdir -p /opt/conda/share/jupyter/kernels/raku && \
    zef install Jupyter::Kernel && \
    jupyter-kernel.raku --generate-config --location=/opt/conda/share/jupyter/kernels/raku && \
    fix-permissions "/home/${NB_USER}" && \
    fix-permissions "${CONDA_DIR}"

USER ${NB_UID}
WORKDIR "${HOME}"
