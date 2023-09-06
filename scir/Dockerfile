FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="Pd.ch <cpd233@outlook.com>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root
# R pre-requisites
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends gfortran gcc fonts-dejavu && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

USER ${NB_UID}
# R packages including IRKernel which gets installed globally.
# base
RUN mamba install --yes 'r-base' 'r-irkernel' && mamba clean --all -f -y && \
    # Interactivity and visualization
    R -e "install.packages(c('r-shiny','r-htmlwidgets','r-htmltools','r-crayon'))" && \
    # Machine learning and data mining
    R -e "install.packages(c('r-caret','r-e1071','r-forecast','r-randomforest','r-tidymodels'))" && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"