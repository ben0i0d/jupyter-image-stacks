FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

# The global snapshot package database will be here in the STACK_ROOT.
# Stack Linux (generic) Manual download
# https://docs.haskellstack.org/en/stable/install_and_upgrade/#linux-generic
# So that we can control Stack version, we do manual install instead of
# automatic install: curl -sSL https://get.haskellstack.org/ | sh
# Install system dependencies
ARG STACK_VERSION="2.9.1"
ARG STACK_BINDIST="stack-${STACK_VERSION}-linux-x86_64"
ENV STACK_ROOT=/opt/stack
RUN mkdir -p $STACK_ROOT && \
    fix-permissions $STACK_ROOT && \
    apt-get update && apt-get install -yq --no-install-recommends python3-pip libtinfo-dev libzmq3-dev libcairo2-dev libpango1.0-dev \
        libmagic-dev libblas-dev liblapack-dev libffi-dev libgmp-dev gnupg netbase graphviz gnuplot-nox g++ gcc libc6-dev libffi-dev \
        libgmp-dev make xz-utils zlib1g-dev git less && \
    rm -rf /var/lib/apt/lists/* && \
    cd /tmp && \
    curl -sSL --output ${STACK_BINDIST}.tar.gz https://github.com/commercialhaskell/stack/releases/download/v${STACK_VERSION}/${STACK_BINDIST}.tar.gz && \
    cp ${STACK_BINDIST}/stack /usr/bin/stack && \
    rm -rf ${STACK_BINDIST}.tar.gz ${STACK_BINDIST} && \
    stack --version

# Stack global non-project-specific config stack.config.yaml
# https://docs.haskellstack.org/en/stable/yaml_configuration/#non-project-specific-config
RUN mkdir -p /etc/stack
COPY stack.config.yaml /etc/stack/config.yaml
RUN fix-permissions /etc/stack

# Stack global project stack.yaml
# https://docs.haskellstack.org/en/stable/yaml_configuration/#yaml-configuration
RUN mkdir -p $STACK_ROOT/global-project
COPY global-project.stack.yaml $STACK_ROOT/global-project/stack.yaml
RUN    chown --recursive $NB_UID:users $STACK_ROOT/global-project \
    && fix-permissions $STACK_ROOT/global-project

# fix-permissions for /usr/local/share/jupyter so that we can install
# the IHaskell kernel there. Seems like the best place to install it, see
#      jupyter --paths
#      jupyter kernelspec list
RUN    mkdir -p /usr/local/share/jupyter \
    && fix-permissions /usr/local/share/jupyter \
    && mkdir -p /usr/local/share/jupyter/kernels \
    && fix-permissions /usr/local/share/jupyter/kernels

# Now make a bin directory for installing the ihaskell executable on
# the PATH. This /opt/bin is referenced by the stack non-project-specific
# config.
RUN    mkdir -p /opt/bin \
    && fix-permissions /opt/bin
ENV PATH ${PATH}:/opt/bin

# Specify a git branch for IHaskell (can be branch or tag).
# The resolver for all stack builds will be chosen from
# the IHaskell/stack.yaml in this commit.
# https://github.com/gibiansky/IHaskell/commits/master
# IHaskell 2022-12-19
ARG IHASKELL_COMMIT=1c22a874ac0c8ed019229f4a0cd5a0bfda017357

# Specify a git branch for hvega
# https://github.com/DougBurke/hvega/commits/main
# hvega 2022-06-16
# hvega-0.12.0.3
# ihaskell-hvega-0.5.0.3
ARG HVEGA_COMMIT=2b453c230294b889564339853de02b0c1829a081

# Clone IHaskell and install ghc from the IHaskell resolver
RUN    cd /opt \
    && curl -L "https://github.com/gibiansky/IHaskell/tarball/$IHASKELL_COMMIT" | tar xzf - \
    && mv *IHaskell* IHaskell \
    && curl -L "https://github.com/DougBurke/hvega/tarball/$HVEGA_COMMIT" | tar xzf - \
    && mv *hvega* hvega \
    && fix-permissions /opt/IHaskell \
    && fix-permissions $STACK_ROOT \
    && fix-permissions /opt/hvega \
    && stack setup \
    && fix-permissions $STACK_ROOT \
    && rm /opt/stack/programs/x86_64-linux/ghc*.tar.xz

# Build IHaskell
# Note:we are NOT in the /opt/IHaskell directory here, we are installing ihaskell via the paths given in /opt/stack/global-project/stack.yaml
RUN stack build $STACK_ARGS ihaskell \
    && fix-permissions /opt/IHaskell \
    && fix-permissions $STACK_ROOT

# Install IHaskell.Display libraries https://github.com/gibiansky/IHaskell/tree/master/ihaskell-display
RUN stack build $STACK_ARGS ihaskell-aeson && \
    stack build $STACK_ARGS ihaskell-blaze && \
    stack build $STACK_ARGS ihaskell-charts && \
    stack build $STACK_ARGS ihaskell-diagrams && \
    stack build $STACK_ARGS ihaskell-gnuplot && \
    stack build $STACK_ARGS ihaskell-graphviz && \
    stack build $STACK_ARGS ihaskell-hatex && \
    stack build $STACK_ARGS ihaskell-juicypixels && \
    stack build $STACK_ARGS ihaskell-plot && \
    stack build $STACK_ARGS ihaskell-widgets && \
    stack build $STACK_ARGS hvega && \
    stack build $STACK_ARGS ihaskell-hvega && \
    fix-permissions $STACK_ROOT && \
    fix-permissions /opt/IHaskell && \
    fix-permissions /opt/hvega && \

# Bug workaround for https://github.com/IHaskell/ihaskell-notebook/issues/9
RUN mkdir -p /home/jovyan/.local/share/jupyter/runtime && \
    fix-permissions /home/jovyan/.local && \
    fix-permissions /home/jovyan/.local/share && \
    fix-permissions /home/jovyan/.local/share/jupyter && \
    fix-permissions /home/jovyan/.local/share/jupyter/runtime

# Install system-level ghc using the ghc which was installed by stack using the IHaskell resolver.
RUN mkdir -p /opt/ghc && \
    ln -s `stack path --compiler-bin` /opt/ghc/bin && \
    fix-permissions /opt/ghc
ENV PATH ${PATH}:/opt/ghc/bin

# Switch back to jovyan user
USER $NB_UID

RUN conda install --quiet --yes 'ipywidgets=7.7.1' && \
    conda clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"
