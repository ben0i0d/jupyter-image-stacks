FROM eoelab.org:1032/mirrored-sources/mirror-image-stacks/ubuntu:22.04

LABEL maintainer="eoelab <eoelab@eoelab.org>"

ARG NB_USER="jovyan"
ARG NB_UID="1000"
ARG NB_GID="100"

EXPOSE 8888

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

COPY cert/eoelab.org.crt /usr/local/share/ca-certificates/eoelab.org.crt

USER root
ENV DEBIAN_FRONTEND noninteractive 
RUN sed -i 's@//.*archive.ubuntu.com@//mirrors.ustc.edu.cn@g' /etc/apt/sources.list && \
    sed -i 's/security.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list && \   
    apt-get update --yes && apt-get upgrade --yes && \
    apt-get install --yes --no-install-recommends sudo ca-certificates locales tini wget curl git git-lfs vim bzip2 zip unzip xclip p7zip-full unrar fonts-wqy-zenhei && \
    update-ca-certificates && \
    apt-get clean && rm -rf /var/lib/apt/lists/* && \
    echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && locale-gen

# Configure environment
ENV CONDA_DIR=/opt/conda \
    SHELL=/bin/bash \
    NB_USER="${NB_USER}" \
    NB_UID=${NB_UID} \
    NB_GID=${NB_GID} \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    LANGUAGE=en_US.UTF-8
ENV PATH="${CONDA_DIR}/bin:${PATH}" \
    HOME="/home/${NB_USER}"
# Copy a script that we will use to correct permissions after running certain commands
COPY conf/fix-permissions /usr/local/bin/fix-permissions
RUN chmod a+rx /usr/local/bin/fix-permissions && \
    sed -i 's/^#force_color_prompt=yes/force_color_prompt=yes/' /etc/skel/.bashrc && \
    echo 'eval "$(command conda shell.bash hook 2> /dev/null)"' >> /etc/skel/.bashrc && \
    echo "auth requisite pam_deny.so" >> /etc/pam.d/su && \
    sed -i.bak -e 's/^%admin/#%admin/' /etc/sudoers && \
    sed -i.bak -e 's/^%sudo/#%sudo/' /etc/sudoers && \
    useradd -l -m -s /bin/bash -N -u "${NB_UID}" "${NB_USER}" && \
    mkdir -p "${CONDA_DIR}" && \
    chown "${NB_USER}:${NB_GID}" "${CONDA_DIR}" && \
    chmod g+w /etc/passwd && \
    fix-permissions "/home/${NB_USER}" && \
    fix-permissions "${CONDA_DIR}"


USER ${NB_UID}

# Setup work directory for backward-compatibility
RUN mkdir "/home/${NB_USER}/work" && fix-permissions "/home/${NB_USER}"

COPY --chown="${NB_UID}:${NB_GID}" conf/initial-condarc "${CONDA_DIR}/.condarc"

WORKDIR /tmp
#install jupyter python mamba extension
#change sources
RUN wget -qO /tmp/micromamba.tar.bz2 "https://micromamba.snakepit.net/api/micromamba/linux-64/latest" && \
    tar -xvjf /tmp/micromamba.tar.bz2 --strip-components=1 bin/micromamba && rm /tmp/micromamba.tar.bz2 && \
    ./micromamba install --root-prefix="${CONDA_DIR}" --prefix="${CONDA_DIR}" --yes 'mamba' "python=3.10" 'jupyter_core' && \
    rm micromamba && \
    mamba list python | grep '^python ' | tr -s ' ' | cut -d ' ' -f 1,2 >> "${CONDA_DIR}/conda-meta/pinned" && \
    mamba install --yes 'notebook' 'jupyterhub' 'jupyterlab' && \
    jupyter notebook --generate-config && \
    pip config set global.index-url https://mirrors.bfsu.edu.cn/pypi/web/simple && \
    npm config set registry https://registry.npmmirror.com/ && \
    pip install jupyterlab_widgets jupyterlab-drawio jupyterlab-language-pack-zh-CN jupyterlab_tabnine && \
    pip cache purge && \
    npm cache clean --force && \
    jupyter lab clean && \
    rm -rf "/home/${NB_USER}/.cache/yarn" && \
    mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}" 

COPY --chown=${NB_UID}:${NB_GID} conf/Rprofile.site /opt/conda/lib/R/etc/
COPY start-sh/start-notebook.sh start-sh/start-singleuser.sh /usr/local/bin/
COPY conf/jupyter_server_config.py conf/docker_healthcheck.py /etc/jupyter/

# Fix permissions on /etc/jupyter as root
USER root
# Legacy for Jupyter Notebook Server, see: [#1205](https://github.com/jupyter/docker-stacks/issues/1205)
RUN sed -re "s/c.ServerApp/c.NotebookApp/g" /etc/jupyter/jupyter_server_config.py > /etc/jupyter/jupyter_notebook_config.py && \
    fix-permissions /etc/jupyter/
# HEALTHCHECK documentation: https://docs.docker.com/engine/reference/builder/#healthcheck
HEALTHCHECK --interval=5s --timeout=3s --start-period=5s --retries=3 CMD /etc/jupyter/docker_healthcheck.py || exit 1
# Configure container startup
ENTRYPOINT ["tini", "-g", "--"]
CMD ["start.sh"]

# Copy local files as late as possible to avoid cache busting
COPY start-sh/start.sh /usr/local/bin/

# Switch back to jovyan to avoid accidental container runs as root
USER ${NB_UID}

WORKDIR "${HOME}"
