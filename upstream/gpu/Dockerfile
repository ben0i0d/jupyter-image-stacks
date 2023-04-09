FROM nvidia/cuda:11.6.2-cudnn8-runtime-ubuntu20.04
LABEL maintainer="eoelab <eoelab@eoelab.org>"
USER root
RUN sed -i 's@//.*archive.ubuntu.com@//mirrors.ustc.edu.cn@g' /etc/apt/sources.list && \
    sed -i 's/security.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
