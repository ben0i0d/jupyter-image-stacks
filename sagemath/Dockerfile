ARG SAGE_VERSION=9.0
ARG SAGE_PYTHON_VERSION=3.7
FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/headwater:stable

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends fonts-liberation pandoc run-one git nano-tiny tzdata unzip vim-tiny openssh-client less \
    texlive-xetex texlive-fonts-recommended texlive-plain-generic xclip dvipng ffmpeg imagemagick texlive tk tk-dev jq && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

USER ${NB_UID}

WORKDIR /tmp
RUN conda install --yes 'notebook' 'jupyterhub' 'jupyterlab' 'widgetsnbextension' sage=$SAGE_VERSION python=$SAGE_PYTHON_VERSION && \
    jupyter notebook --generate-config && \
    conda clean --all -f -y && \
    npm cache clean --force && \
    jupyter lab clean && \
    rm -rf "/home/${NB_USER}/.cache/yarn" && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

# Install sagemath kernel and extensions using conda run:
#   Create jupyter directories if they are missing
#   Add environmental variables to sage kernal using jq
RUN echo ' \
        from sage.repl.ipython_kernel.install import SageKernelSpec; \
        SageKernelSpec.update(prefix=os.environ["CONDA_DIR"]); \
    ' | conda run sage && \
    echo ' \
        cat $SAGE_ROOT/etc/conda/activate.d/sage-activate.sh | \
            grep -Po '"'"'(?<=^export )[A-Z_]+(?=)'"'"' | \
            jq --raw-input '"'"'.'"'"' | jq -s '"'"'.'"'"' | \
            jq --argfile kernel $SAGE_LOCAL/share/jupyter/kernels/sagemath/kernel.json \
            '"'"'. | map(. as $k | env | .[$k] as $v | {($k):$v}) | add as $vars | $kernel | .env= $vars'"'"' > \
            $CONDA_DIR/share/jupyter/kernels/sagemath/kernel.json \
    ' | conda run sh && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

# Install sage's python kernel
RUN echo ' \
        ls /opt/conda/envs/sage/share/jupyter/kernels/ | \
            grep -Po '"'"'python\d'"'"' | \
            xargs -I % sh -c '"'"' \
                cd $SAGE_LOCAL/share/jupyter/kernels/% && \
                cat kernel.json | \
                    jq '"'"'"'"'"'"'"'"' . | .display_name = .display_name + " (sage)" '"'"'"'"'"'"'"'"' > \
                    kernel.json.modified && \
                mv -f kernel.json.modified kernel.json && \
                ln  -s $SAGE_LOCAL/share/jupyter/kernels/% $CONDA_DIR/share/jupyter/kernels/%_sage \
            '"'"' \
    ' | conda run sh && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

COPY --chown=${NB_UID}:${NB_GID} Rprofile.site /opt/conda/lib/R/etc/
ENV JUPYTER_PORT=8888
EXPOSE $JUPYTER_PORT

# Configure container startup
CMD ["start-notebook.sh"]

# Copy local files as late as possible to avoid cache busting
COPY start-notebook.sh start-singleuser.sh /usr/local/bin/
# Currently need to have both jupyter_notebook_config and jupyter_server_config to support classic and lab
COPY jupyter_server_config.py docker_healthcheck.py /etc/jupyter/

# Fix permissions on /etc/jupyter as root
USER root

# Legacy for Jupyter Notebook Server, see: [#1205](https://github.com/jupyter/docker-stacks/issues/1205)
RUN sed -re "s/c.ServerApp/c.NotebookApp/g" \
    /etc/jupyter/jupyter_server_config.py > /etc/jupyter/jupyter_notebook_config.py && \
    fix-permissions /etc/jupyter/

# HEALTHCHECK documentation: https://docs.docker.com/engine/reference/builder/#healthcheck
# This healtcheck works well for `lab`, `notebook`, `nbclassic`, `server` and `retro` jupyter commands
# https://github.com/jupyter/docker-stacks/issues/915#issuecomment-1068528799
HEALTHCHECK --interval=5s --timeout=3s --start-period=5s --retries=3 \
    CMD /etc/jupyter/docker_healthcheck.py || exit 1

RUN update-alternatives --install /usr/bin/nano nano /bin/nano-tiny 10

# Switch back to jovyan to avoid accidental container runs as root
USER ${NB_UID}

WORKDIR "${HOME}"



