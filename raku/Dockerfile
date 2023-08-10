FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

WORKDIR /opt/rakudo-star

COPY rakudo-star-2023.06-01.tar.gz ./

ENV PATH="/opt/rakudo-star/bin/:/opt/rakudo-star/share/perl6/site/bin:/opt/rakudo-star/share/perl6/vendor/bin:/opt/rakudo-star/share/perl6/core/bin:${PATH}" 

RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends libzmq3-dev && \
    apt-get clean && rm -rf /var/lib/apt/lists/*  && \
    tar -xzf rakudo-star-*.tar.gz && mkdir rakudo-star && mv rakudo-star-* rakudo-star && rm rakudo-star-*.tar.gz && \
    rakudo-star/bin/rstar install && \
    zef install Jupyter::Kernel && \
    jupyter-kernel.raku --generate-config && \
    jupyter notebook --generate-config && \
    jupyter console --generate-config

USER ${NB_UID}
WORKDIR "${HOME}"
