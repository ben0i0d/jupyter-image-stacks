FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="Pd.ch <cpd233@outlook.com>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

COPY --chown=$NB_USER:$NB_USER jupyter_remote_desktop_proxy /opt/novnc/jupyter_remote_desktop_proxy
COPY --chown=$NB_USER:$NB_USER setup.py MANIFEST.in README.md LICENSE /opt/novnc/

USER root

RUN apt-get update --yes && \
    apt-get --yes --no-install-recommends install \
        dbus-x11 xfce4 xfce4-panel xfce4-session xfce4-settings xorg xubuntu-icon-theme tigervnc-standalone-server tigervnc-xorg-extension && \ 
    apt-get clean && rm -rf /var/lib/apt/lists/* 
    mamba install -y websockify && \
    cd /opt/novnc  && \
    pip install -e .  && \
    fix-permissions "/home/${NB_USER}" && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions /opt/novnc

USER $NB_USER


