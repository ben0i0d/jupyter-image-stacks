FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:matlab-c-minimal

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

COPY conf/mpm_input_r2023b.txt /opt/mpm_input_r2023b.txt

RUN apt-get update && \
    mpm install --inputfile /opt/mpm_input_r2023b.txt && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

USER ${NB_UID}
