FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/headwater:stable

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root 
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends \
    fonts-liberation pandoc git nano-tiny tzdata unzip vim-tiny openssh-client less texlive-xetex \
    texlive-fonts-recommended texlive-plain-generic xclip run-one build-essential cm-super dvipng ffmpeg && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN update-alternatives --install /usr/bin/nano nano /bin/nano-tiny 10

USER ${NB_UID}

COPY --chown=${NB_UID}:${NB_GID} Rprofile.site /opt/conda/lib/R/etc/

WORKDIR /tmp
RUN conda install --yes 'notebook' 'jupyterhub' 'jupyterlab' 'altair' 'beautifulsoup4' 'bokeh' 'bottleneck' 'cloudpickle' 'openblas' 'cython' \
    'dask' 'dill' 'h5py' 'ipympl' 'ipywidgets' 'jupyter_server>=2.0.0' 'matplotlib-base' 'numba' 'numexpr' 'openpyxl' 'pandas' 'patsy' 'protobuf' 'pytables' \
    'scikit-image' 'scikit-learn' 'scipy' 'seaborn' 'sqlalchemy' 'statsmodels' 'sympy' 'widgetsnbextension' 'xlrd' && \
    jupyter notebook --generate-config && \
    conda clean --all -f -y && \
    npm cache clean --force && \
    jupyter lab clean && \
    rm -rf "/home/${NB_USER}/.cache/yarn" && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

WORKDIR /tmp
RUN git clone https://github.com/PAIR-code/facets.git && \
    jupyter nbextension install facets/facets-dist/ --sys-prefix && \
    rm -rf /tmp/facets && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

# Import matplotlib the first time to build the font cache.
ENV XDG_CACHE_HOME="/home/${NB_USER}/.cache/"

RUN MPLBACKEND=Agg python -c "import matplotlib.pyplot" && \
    fix-permissions "/home/${NB_USER}"


ENV JUPYTER_PORT=8888
EXPOSE $JUPYTER_PORT

# Configure container startup
CMD ["start-notebook.sh"]

# Copy local files as late as possible to avoid cache busting
COPY start-notebook.sh start-singleuser.sh /usr/local/bin/
# Currently need to have both jupyter_notebook_config and jupyter_server_config to support classic and lab
COPY jupyter_server_config.py docker_healthcheck.py /etc/jupyter/

USER root

RUN sed -re "s/c.ServerApp/c.NotebookApp/g" \
    /etc/jupyter/jupyter_server_config.py > /etc/jupyter/jupyter_notebook_config.py && \
    fix-permissions /etc/jupyter/

# This healtcheck works well for `lab`, `notebook`, `nbclassic`, `server` and `retro` jupyter commands

HEALTHCHECK --interval=5s --timeout=3s --start-period=5s --retries=3 \
    CMD /etc/jupyter/docker_healthcheck.py || exit 1

# Switch back to jovyan to avoid accidental container runs as root
USER ${NB_UID}

WORKDIR "${HOME}"
