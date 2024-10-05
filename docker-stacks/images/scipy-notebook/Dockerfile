# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
#ARG REGISTRY=quay.io
#ARG OWNER=jupyter
#ARG BASE_CONTAINER=$REGISTRY/$OWNER/minimal-notebook
FROM  peter/minimal-notebook
LABEL maintainer="Jupyter Project <jupyter@googlegroups.com>"

# Fix: https://github.com/hadolint/hadolint/wiki/DL4006
# Fix: https://github.com/koalaman/shellcheck/wiki/SC3014
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends \
    # for cython: https://cython.readthedocs.io/en/latest/src/quickstart/install.html
    build-essential \
    # for latex labels
    cm-super \
    dvipng \
    # for matplotlib anim
    ffmpeg && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

USER ${NB_UID}

# Install Python 3 packages
RUN mamba install --yes 'altair' 

RUN mamba install --yes     'beautifulsoup4' 
RUN mamba install --yes     'bokeh' 
RUN mamba install --yes     'bottleneck' 
RUN mamba install --yes     'cloudpickle' 
RUN mamba install --yes    'conda-forge::blas=*=openblas' 
RUN mamba install --yes     'cython' 
RUN mamba install --yes     'dask' 
RUN mamba install --yes     'dill' 
RUN mamba install --yes     'h5py' 
RUN mamba install --yes     'ipympl' 
RUN mamba install --yes     'ipywidgets' 
RUN mamba install --yes     'jupyterlab-git' 
RUN mamba install --yes     'matplotlib-base' 
RUN mamba install --yes     'numba' 
RUN mamba install --yes     'numexpr' 
RUN mamba install --yes     'openpyxl' 
RUN mamba install --yes     'pandas' 
RUN mamba install --yes     'patsy' 
RUN mamba install --yes     'protobuf' 
RUN mamba install --yes     'pytables' 
RUN mamba install --yes     'scikit-image' 
RUN mamba install --yes     'scikit-learn' 
RUN mamba install --yes     'scipy' 
RUN mamba install --yes     'seaborn' 
RUN mamba install --yes     'sqlalchemy' 
RUN mamba install --yes     'statsmodels' 
RUN mamba install --yes     'sympy' 
RUN mamba install --yes     'widgetsnbextension' 
RUN mamba install --yes     'xlrd' 
RUN mamba clean --all -f -y 
RUN fix-permissions "${CONDA_DIR}" 
RUN fix-permissions "/home/${NB_USER}"

# Install facets package which does not have a `pip` or `conda-forge` package at the moment
WORKDIR /tmp
RUN git clone https://github.com/PAIR-code/facets && \
    jupyter nbclassic-extension install facets/facets-dist/ --sys-prefix && \
    rm -rf /tmp/facets && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

# Import matplotlib the first time to build the font cache
RUN MPLBACKEND=Agg python -c "import matplotlib.pyplot" && \
    fix-permissions "/home/${NB_USER}"

USER ${NB_UID}

WORKDIR "${HOME}"
