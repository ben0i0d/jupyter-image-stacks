FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

ENV PATH="/home/linuxbrew/.linuxbrew/bin:/home/linuxbrew/.linuxbrew/sbin:${PATH}" \
    CHPL_LIB_PIC=shared \
    HOMEBREW_BREW_GIT_REMOTE="https://mirrors.ustc.edu.cn/brew.git" \
    HOMEBREW_CORE_GIT_REMOTE="https://mirrors.ustc.edu.cn/homebrew-core.git" \
    OMEBREW_BOTTLE_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles" \
    HOMEBREW_API_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles/api" 

# install homebrew
USER root

RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends gcc g++ m4 perl make mawk pkg-config cmake  \
    llvm-11-dev llvm-11 llvm-11-tools clang-11 libclang-11-dev libedit-dev && \
    apt-get clean && rm -rf /var/lib/apt/lists/*   

RUN brew update && brew install chapel  && \
    pip install --no-cache-dir jupyter-kernel-chapel  

USER ${NB_UID}
WORKDIR "${HOME}"




