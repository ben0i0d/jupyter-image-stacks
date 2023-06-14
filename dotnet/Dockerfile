FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root
# Install system dependencies
RUN mkdir -p /opt/dotnet/tools && \
    apt-get update && apt-get install -y --no-install-recommends dotnet-sdk-7.0 && \
    apt-get clean && rm -rf /var/lib/apt/lists/* && \
    dotnet tool install Microsoft.dotnet-interactive --tool-path /opt/dotnet/tools
RUN dotnet interactive jupyter install && \
    fix-permissions /opt/dotnet && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

USER $NB_USER
