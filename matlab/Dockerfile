# Copyright 2019 - 2023 The MathWorks, Inc.
# This Dockerfile allows you to build a Docker® image with MATLAB® installed using the MATLAB Package 
# Manager. Use the optional build arguments to customize the version of MATLAB, list of products to 
# install, and the location at which to install MATLAB.

# Specify license server information using the format: port@hostname 
ARG LICENSE_SERVER

FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="Pd.ch <cpd233@outlook.com>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

WORKDIR /tmp

COPY dep.txt /tmp/dep.txt

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
    ln -s /lib64/ld-linux-x86-64.so.2 /lib64/ld-lsb-x86-64.so.3 && \
    apt-get clean && rm -rf /var/lib/apt/lists/* 

# Run mpm to install MATLAB in the target location and delete the mpm installation afterwards.
# If mpm fails to install successfully, then print the logfile in the terminal, otherwise clean up.
RUN wget -q https://www.mathworks.com/mpm/glnxa64/mpm && chmod +x mpm && \
    ./mpm install \
    --release=r2023b \
    --destination=/opt/matlab/r2023b \
    --products MATLAB || (echo "MPM Installation Failure. See below for more information:" && cat /tmp/mathworks_root.log && false) && \
    rm -f mpm /tmp/mathworks_root.log && \
    && ln -s /opt/matlab/r2023b/bin/matlab /usr/local/bin/matlab

# Alternatively, you can put a license file into the container. Enter the details of the license server in this file and uncomment the following line.
COPY license.lic /opt/matlab/r2023b/licenses/

COPY libmwlmgrimpl.dll /opt/matlab/r2023b/bin/matlab_startup_plugins/lmgrimpl/

# install matlab kernel

ENV MATLAB_EXECUTABLE=/opt/matlab/r2023b/bin/matlab

RUN pip install pymatbridge && \
    pip install matlab_kernel && \
    python -m matlab_kernel install && \
    cd /opt/matlab/r2023b/extern/engines/python && python setup.py install

# The following environment variables allow MathWorks to understand how this MathWorks 
# product (MATLAB Dockerfile) is being used. This information helps us make MATLAB even better. 
# Your content, and information about the content within your files, is not shared with MathWorks. 
# To opt out of this service, delete the environment variables defined in the following line. 
# To learn more, see the Help Make MATLAB Even Better section in the accompanying README: 
# https://github.com/mathworks-ref-arch/matlab-dockerfile#help-make-matlab-even-better
ENV MW_DDUX_FORCE_ENABLE=true MW_CONTEXT_TAGS=MATLAB:DOCKERFILE:V1

USER ${NB_UID}
WORKDIR "${HOME}"