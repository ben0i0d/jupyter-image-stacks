FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="Pd.ch <cpd233@outlook.com>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root
# R pre-requisites
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends r-base && \
    apt-get clean && rm -rf /var/lib/apt/lists/* && \
    fix-permissions "/home/${NB_USER}" && \
    fix-permissions "${CONDA_DIR}"

USER ${NB_UID}
# R packages including IRKernel which gets installed globally.
# base
RUN R -e "install.packages('IRkernel')" && \
    R -e "IRkernel::installspec(user = FALSE)" && \
    # Interactivity and visualization
    R -e "install.packages('shiny','htmlwidgets','htmltools','crayon')" && \
    # Machine learning and data mining
    R -e "install.packages('caret','e1071','forecast','randomforest','tidymodels')" && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"