FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="Pd.ch <cpd233@outlook.com>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]


USER root

ENV SCILAB_EXECUTABLE='scilab-cli'

COPY turbovnc_2.2.6_amd64.deb /tmp/turbovnc_2.2.6_amd64.deb

#install desktop+scilab+extension
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends dbus-x11 firefox xfce4 xfce4-panel xfce4-session xfce4-settings xorg xubuntu-icon-theme \
        scilab scilab-* && \
    fix-permissions "/home/${NB_USER}" && \ 
    # Install TurboVNC (https://github.com/TurboVNC/turbovnc)
    apt-get install -y -q /tmp/turbovnc_2.2.6_amd64.deb  && \
    apt-get remove -y -q light-locker  && \
    rm /tmp/turbovnc_2.2.6_amd64.deb  && \
    ln -s /opt/TurboVNC/bin/* /usr/local/bin/ && \
    apt-get clean && rm -rf /var/lib/apt/lists/* 

USER ${NB_UID}

RUN mamba install -y websockify && \
    pip install scilab_kernel jupyter-remote-desktop-proxy && \
    mamba clean --all -f -y && \
    pip cache purge
    