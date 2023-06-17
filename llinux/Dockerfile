FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends dbus-x11 libgl1-mesa-glx xfce4 xfce4-panel xfce4-session xfce4-settings xorg xubuntu-icon-theme websockify && \
    pip install jupyter-remote-desktop-proxy && \
    pip cache purge && \
    apt-get clean && rm -rf /var/lib/apt/lists/*  && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}" 

USER ${NB_UID}


