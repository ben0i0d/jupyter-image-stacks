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
    apt-get install --yes --no-install-recommends build-essential && \
    apt-get clean && rm -rf /var/lib/apt/lists/*   && \
    sed -i -E '/^session optional\s+pam_umask\.so$/ s/$/ umask=0022/' /etc/pam.d/common-session  && \
    sed -i -E '/^session optional\s+pam_umask\.so$/ s/$/ umask=0022/' /etc/pam.d/common-session-noninteractive  && \
    localedef -i en_US -f UTF-8 en_US.UTF-8  && \
    useradd -m -s /bin/bash linuxbrew  && \
    echo 'linuxbrew ALL=(ALL) NOPASSWD:ALL' >>/etc/sudoers  && \
    su - linuxbrew -c 'mkdir ~/.linuxbrew' && \
    git clone --depth=1 https://eoelab.org:1031/mirrored-sources/brew.git && \
    mv brew /home/linuxbrew/.linuxbrew/Homebrew  && \
    chown  /home/linuxbrew/.linuxbrew/Homebrew linuxbrew

USER linuxbrew

WORKDIR /home/linuxbrew

RUN mkdir -p .linuxbrew/bin .linuxbrew/etc .linuxbrew/include .linuxbrew/lib .linuxbrew/opt .linuxbrew/sbin \
    .linuxbrew/share .linuxbrew/var/homebrew/linked .linuxbrew/Cellar && \
    ln -s ../Homebrew/bin/brew .linuxbrew/bin/brew  && \
    git -C .linuxbrew/Homebrew remote set-url origin https://github.com/Homebrew/brew  && \
    git -C .linuxbrew/Homebrew fetch origin  && \
    HOMEBREW_NO_ANALYTICS=1 HOMEBREW_NO_AUTO_UPDATE=1 brew tap --force homebrew/core  && \
    brew install-bundler-gems  && \
    brew cleanup  && \
    { git -C .linuxbrew/Homebrew config --unset gc.auto; true; }  && \
    { git -C .linuxbrew/Homebrew config --unset homebrew.devcmdrun; true; }  && \
    rm -rf .cache  && \
    touch .linuxbrew/.homebrewdocker

RUN brew update && brew install chapel  && \
    pip install --no-cache-dir jupyter-kernel-chapel  

USER ${NB_UID}
WORKDIR "${HOME}"




