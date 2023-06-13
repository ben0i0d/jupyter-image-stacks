FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root
# Install system dependencies
RUN wget https://packages.microsoft.com/config/debian/11/packages-microsoft-prod.deb -O packages-microsoft-prod.deb && \
    dpkg -i packages-microsoft-prod.deb && \
    rm packages-microsoft-prod.deb && \
    apt-get update && apt-get install -yq --no-install-recommends dotnet-sdk-7.0 aspnetcore-runtime-7.0 && \
    apt-get clean && rm -rf /var/lib/apt/lists/* && \
    dotnet tool install --global Microsoft.dotnet-interactive && \
    dotnet interactive jupyter install && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

USER $NB_USER
