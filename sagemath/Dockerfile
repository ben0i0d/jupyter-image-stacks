FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:base-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

# Sage pre-requisites and jq for manipulating json
RUN apt-get update && \
    apt-get install -y --no-install-recommends dvipng ffmpeg imagemagick texlive tk tk-dev jq && \
    rm -rf /var/lib/apt/lists/*

USER $NB_UID

# Install Sage conda environsage
RUN conda install --quiet --yes -c conda-forge widgetsnbextension sage && \
    conda clean --all -f -y && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

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
