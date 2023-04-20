FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-g
LABEL maintainer="eoelab <eoelab@eoelab.org>"


SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

# Install all OS dependencies for fully functional notebook server
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends tzdata openssh-client less fonts-liberation pandoc run-one && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Create alternative for nano -> nano-tiny
RUN update-alternatives --install /usr/bin/nano nano /bin/nano-tiny 10
# Switch back to jovyan to avoid accidental container runs as root
USER ${NB_UID}

