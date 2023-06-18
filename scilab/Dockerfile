FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root
# Set GUI environment variables
ENV SCREEN_RESOLUTION 1280x768
ENV SCILAB_EXECUTABLE="scilab-adv-cli"
# Set display
ENV DISPLAY :0

COPY turbovnc_2.2.6_amd64.deb /tmp/turbovnc_2.2.6_amd64.deb 

#install novnc+desktop
RUN apt-get -y -q update && \ 
    apt-get -y -q upgrade && \ 
    apt-get -y -q install dbus-x11 firefox xfce4 xfce4-panel xfce4-session xfce4-settings xorg xubuntu-icon-theme && \ 
    fix-permissions "/home/${NB_USER}" && \ 
# Install TurboVNC (https://github.com/TurboVNC/turbovnc)
    apt-get install -y -q /tmp/turbovnc_2.2.6_amd64.deb  && \
    apt-get remove -y -q light-locker  && \
    rm /tmp/turbovnc_2.2.6_amd64.deb  && \
    ln -s /opt/TurboVNC/bin/* /usr/local/bin/  && \
# Install scilab + extension
    apt-get update --yes && \
    apt-get install --yes scilab scilab-* && \
    apt-get clean && rm -rf /var/lib/apt/lists/* 

USER ${NB_UID}

RUN mamba install -y websockify && \
    pip install jupyter-remote-desktop-proxy scilab_kernel && \
    mamba clean --all -f -y && \
    pip cache purge
