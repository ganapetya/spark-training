# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
#ARG REGISTRY=quay.io
#ARG OWNER=jupyter
#ARG BASE_CONTAINER=$REGISTRY/$OWNER/pyspark-notebook
FROM peter/pyspark-notebook

LABEL maintainer="Jupyter Project <jupyter@googlegroups.com>"

# Fix: https://github.com/hadolint/hadolint/wiki/DL4006
# Fix: https://github.com/koalaman/shellcheck/wiki/SC3014
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

# RSpark config
ENV R_LIBS_USER="${SPARK_HOME}/R/lib"
RUN fix-permissions "${R_LIBS_USER}"

# R pre-requisites
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends \
    fonts-dejavu \
    gfortran \
    gcc && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

USER ${NB_UID}

# R packages including IRKernel which gets installed globally.
RUN mamba install --yes 'r-base' 
RUN mamba install --yes     'r-ggplot2' 
RUN mamba install --yes     'r-irkernel' 
RUN mamba install --yes     'r-rcurl' 
RUN mamba install --yes     'r-sparklyr'
RUN mamba clean --all -f -y 
RUN fix-permissions "${CONDA_DIR}" 
RUN fix-permissions "/home/${NB_USER}"

