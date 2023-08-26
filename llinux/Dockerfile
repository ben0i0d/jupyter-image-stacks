FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="Pd.ch <cpd233@outlook.com>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

RUN apt-get update --yes && \
    apt-get --yes install \
        dbus-x11 xfce4 xfce4-panel xfce4-session xfce4-settings xorg xubuntu-icon-theme tigervnc-standalone-server tigervnc-xorg-extension && \ 
    apt-get clean && rm -rf /var/lib/apt/lists/*  && \
    fix-permissions "/home/${NB_USER}" && \
    fix-permissions "${CONDA_DIR}"

USER $NB_USER

RUN mamba install -y websockify && \
    pip install jupyter-remote-desktop-proxy  && \
    pip cache purge && mamba clean --all -f -y 
