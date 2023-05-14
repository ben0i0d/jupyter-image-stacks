FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:python-g

LABEL maintainer="eoelab <eoelab@eoelab.org>"
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends build-essential cm-super dvipng ffmpeg && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

USER ${NB_UID}

# Install Python 3 packages
RUN mamba install --quiet --yes 'altair' 'beautifulsoup4' 'bokeh' 'bottleneck' 'cloudpickle' 'conda-forge::blas=*=openblas' \
    'cython' 'dask' 'dill' 'h5py' 'ipympl' 'ipywidgets' 'matplotlib-base' 'numba' 'numexpr' 'pandas' 'xlrd' \
    'patsy' 'protobuf' 'pytables' 'scikit-image' 'scikit-learn' 'scipy' 'seaborn' 'sqlalchemy' 'statsmodels' 'sympy' 'widgetsnbextension' && \
    mamba clean --all -f -y 

# Import matplotlib the first time to build the font cache.
ENV XDG_CACHE_HOME="/home/${NB_USER}/.cache/"

RUN MPLBACKEND=Agg python -c "import matplotlib.pyplot" && \
    fix-permissions "/home/${NB_USER}"
