FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="Pd.ch <cpd233@outlook.com>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root
# R pre-requisites
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends fonts-dejavu unixodbc unixodbc-dev r-cran-rodbc gfortran gcc && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}" && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

USER ${NB_UID}
# R packages including IRKernel which gets installed globally.
# variableinspector
RUN mamba install --yes 'r-base' 'r-caret' 'r-crayon' 'r-devtools' 'r-e1071' 'r-forecast' 'r-hexbin' 'r-htmltools' 'r-htmlwidgets' 'r-irkernel' \
    'r-nycflights13' 'r-randomforest' 'r-rcurl' 'r-rmarkdown' 'r-rodbc' 'r-rsqlite' 'r-shiny' 'r-tidymodels' 'r-tidyverse' 'unixodbc' && \
    mamba clean --all -f -y && \
    pip install lckr-jupyterlab-variableinspector && \
    pip cache purge && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"
