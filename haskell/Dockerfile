FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Extra arguments to `stack build`. Used to build --fast, see Makefile.
ARG STACK_ARGS=
ARG STACK_BINDIST="stack-2.11.1-linux-x86_64"
# Example IHaskell notebooks will be collected in this directory.
ARG EXAMPLES_PATH=/home/$NB_USER/ihaskell_examples

USER root
# The global snapshot package database will be here in the STACK_ROOT.
ENV STACK_ROOT=/opt/stack
RUN mkdir -p /etc/stack && \
    mkdir -p $STACK_ROOT && \
    mkdir -p $STACK_ROOT/global-project && \
    fix-permissions $STACK_ROOT && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}" && \
# Install system dependencies
    apt-get update && apt-get install -yq --no-install-recommends python3-pip libtinfo-dev libzmq3-dev libcairo2-dev libpango1.0-dev \
    libmagic-dev libblas-dev liblapack-dev libffi-dev libgmp-dev gnupg netbase graphviz gnuplot-nox \
    g++ gcc libc6-dev libffi-dev libgmp-dev make xz-utils zlib1g-dev less && \
    apt-get clean && rm -rf /var/lib/apt/lists/*
# Stack global non-project-specific config stack.config.yaml
# Stack global project stack.yaml
COPY stack.config.yaml /etc/stack/config.yaml
COPY global-hints.yaml /etc/stack/global-hints-cache.yaml
COPY stack-2.11.1-linux-x86_64.tar.gz /tmp/stack-2.11.1-linux-x86_64.tar.gz
COPY global-project.stack.yaml $STACK_ROOT/global-project/stack.yaml
# Stack Linux (generic) Manual download
RUN fix-permissions /etc/stack && \
    cd /tmp  && \
    tar zxf ${STACK_BINDIST}.tar.gz  && \
    cp ${STACK_BINDIST}/stack /usr/bin/stack  && \
    rm -rf ${STACK_BINDIST}.tar.gz ${STACK_BINDIST}  && \
    chown --recursive $NB_UID:users $STACK_ROOT/global-project && \
    fix-permissions $STACK_ROOT/global-project  && \
# fix-permissions for /usr/local/share/jupyter so that we can install the IHaskell kernel there.
    mkdir -p /usr/local/share/jupyter  && \
    fix-permissions /usr/local/share/jupyter  && \
    mkdir -p /usr/local/share/jupyter/kernels  && \
    fix-permissions /usr/local/share/jupyter/kernels && \
# Now make a bin directory for installing the ihaskell executable on the PATH. This /opt/bin is referenced by the stack non-project-specific config.
    mkdir -p /opt/bin  && \
    fix-permissions /opt/bin
ENV PATH ${PATH}:/opt/bin

# Clone IHaskell and install ghc from the IHaskell resolver
RUN cd /opt  && \
    curl -L "https://eoelab.org:1031/mirrored-sources/IHaskell/-/archive/master/IHaskell-master.tar.gz" | tar xzf -  && \
    mv *IHaskell* IHaskell  && \
    curl -L "https://eoelab.org:1031/mirrored-sources/hvega/-/archive/main/hvega-main.tar.gz" | tar xzf -  && \
    mv *hvega* hvega  && \
    fix-permissions /opt/IHaskell  && \
    fix-permissions $STACK_ROOT  && \
    fix-permissions /opt/hvega  && \
    stack setup  && \
    fix-permissions $STACK_ROOT  && \
    rm /opt/stack/programs/x86_64-linux/ghc*.tar.xz && \
# Build IHaskell
# Note that we are NOT in the /opt/IHaskell directory here, we are installing ihaskell via the paths given in /opt/stack/global-project/stack.yaml
    stack build $STACK_ARGS ihaskell  && \
    fix-permissions /opt/IHaskell  && \
    fix-permissions $STACK_ROOT  && \
# Install IHaskell.Display libraries
    stack build $STACK_ARGS ihaskell-aeson  && \
    stack build $STACK_ARGS ihaskell-blaze  && \
    stack build $STACK_ARGS ihaskell-charts  && \
    stack build $STACK_ARGS ihaskell-diagrams  && \
    stack build $STACK_ARGS ihaskell-gnuplot  && \
    stack build $STACK_ARGS ihaskell-graphviz  && \
    stack build $STACK_ARGS ihaskell-hatex  && \
    stack build $STACK_ARGS ihaskell-juicypixels  && \
#   stack build $STACK_ARGS ihaskell-magic  && \
    stack build $STACK_ARGS ihaskell-plot  && \
#   stack build $STACK_ARGS ihaskell-rlangqq  && \
#   stack build $STACK_ARGS ihaskell-static-canvas  && \
    stack build $STACK_ARGS ihaskell-widgets  && \
    stack build $STACK_ARGS hvega  && \
    stack build $STACK_ARGS ihaskell-hvega  && \
    fix-permissions $STACK_ROOT  && \
# Fix for https://github.com/IHaskell/ihaskell-notebook/issues/14#issuecomment-636334824
    fix-permissions /opt/IHaskell  && \
    fix-permissions /opt/hvega && \
# Bug workaround for https://github.com/IHaskell/ihaskell-notebook/issues/9
    mkdir -p /home/jovyan/.local/share/jupyter/runtime  && \
    fix-permissions /home/jovyan/.local  && \
    fix-permissions /home/jovyan/.local/share  && \
    fix-permissions /home/jovyan/.local/share/jupyter  && \
    fix-permissions /home/jovyan/.local/share/jupyter/runtime && \
# Install system-level ghc using the ghc which was installed by stack.using the IHaskell resolver.
    mkdir -p /opt/ghc && ln -s `stack path --compiler-bin` /opt/ghc/bin  && \
    fix-permissions /opt/ghc
ENV PATH ${PATH}:/opt/ghc/bin
# Switch back to jovyan user
USER $NB_UID

RUN stack exec ihaskell -- install --stack --prefix=/usr/local && \
    mamba install --quiet --yes 'ipywidgets=7.7.1' && \
    mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}" && \
# Collect all the IHaskell example notebooks in EXAMPLES_PATH.
    mkdir -p $EXAMPLES_PATH  && \
    cd $EXAMPLES_PATH  && \
    mkdir -p ihaskell  && \
    cp --recursive /opt/IHaskell/notebooks/* ihaskell/  && \
    mkdir -p ihaskell-juicypixels  && \
    cp /opt/IHaskell/ihaskell-display/ihaskell-juicypixels/*.ipynb ihaskell-juicypixels/  && \
    mkdir -p ihaskell-charts  && \
    cp /opt/IHaskell/ihaskell-display/ihaskell-charts/*.ipynb ihaskell-charts/  && \
    mkdir -p ihaskell-diagrams  && \
    cp /opt/IHaskell/ihaskell-display/ihaskell-diagrams/*.ipynb ihaskell-diagrams/  && \
    mkdir -p ihaskell-gnuplot  && \
    cp /opt/IHaskell/ihaskell-display/ihaskell-gnuplot/*.ipynb ihaskell-gnuplot/  && \
    mkdir -p ihaskell-widgets  && \
    cp --recursive /opt/IHaskell/ihaskell-display/ihaskell-widgets/Examples/* ihaskell-widgets/  && \
    mkdir -p ihaskell-hvega  && \
    cp /opt/hvega/notebooks/*.ipynb ihaskell-hvega/  && \
    cp /opt/hvega/notebooks/*.tsv ihaskell-hvega/  && \
    mkdir -p ihaskell-plot  && \
    cp /opt/IHaskell/ihaskell-display/ihaskell-plot/PlotExample.ipynb ihaskell-plot/  && \
    fix-permissions $EXAMPLES_PATH


