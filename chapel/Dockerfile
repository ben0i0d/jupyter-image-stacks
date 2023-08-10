FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]


WORKDIR /opt

ENV PATH="${PATH}:/home/linuxbrew/.linuxbrew/bin/" \
    CHPL_LIB_PIC=shared \
    HOMEBREW_BREW_GIT_REMOTE="https://mirrors.ustc.edu.cn/brew.git" \
    HOMEBREW_CORE_GIT_REMOTE="https://mirrors.ustc.edu.cn/homebrew-core.git" \
    OMEBREW_BOTTLE_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles" \
    HOMEBREW_API_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles/api" 

USER root

RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends build-essential && \
    apt-get clean && rm -rf /var/lib/apt/lists/*  

USER ${NB_UID}

RUN NONINTERACTIVE=1 /bin/bash -c "$(curl -fsSL https://mirrors.ustc.edu.cn/misc/brew-install.sh)" && \
    brew update && brew install chapel  && \
    pip install --no-cache-dir jupyter-kernel-chapel  

WORKDIR "${HOME}"





