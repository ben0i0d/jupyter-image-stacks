# Copyright 2023 The MathWorks, Inc.
FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="Pd.ch <cpd233@outlook.com>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

COPY dep.txt /tmp/dep.txt

USER root


# base deps,copy by mathworks-ref-arch/container-images
RUN apt-get update && apt-get install --no-install-recommends -y `cat /tmp/dep.txt` && rm /tmp/dep.txt && \
# using Simulink code generation capabilities, compile mex files with gcc, g++, or gfortran.
    apt-get install -y gcc g++ gfortran && \
# enable running a program that makes use of MATLAB's Engine API for C and Fortran
    apt-get install -y csh  && \
# enable the playing of media files (mp3, mp4, etc.) from within MATLAB.
    apt-get install --no-install-recommends -y libgstreamer1.0-0 gstreamer1.0-tools gstreamer1.0-libav gstreamer1.0-plugins-base gstreamer1.0-plugins-good \
        gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly && \
# resolve any license manager issues
    ln -s /lib64/ld-linux-x86-64.so.2 /lib64/ld-lsb-x86-64.so.3
    apt-get clean && rm -rf /var/lib/apt/lists/* 

USER ${NB_UID}
WORKDIR "${HOME}"