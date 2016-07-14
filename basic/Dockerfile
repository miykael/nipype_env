# This Dockerfile is based on the dockerfiles 'crn_base' and 'crn_nipype' from
# the Poldrack Lab (https://github.com/poldracklab/crn_base), the dockerfiles
# from neurovault (https://github.com/NeuroVault/NeuroVault) and the dockerfile
# biss2016-notebook from Oscar Esteban under https://hub.docker.com/u/oesteban.
#
# This means that the same copyrights apply to this Dockerfile, as they do for
# the above mentioned dockerfiles. For more information see:
# https://github.com/miykael/nipype_env

FROM ubuntu:15.04
MAINTAINER Michael Notter <michaelnotter@hotmail.com>

# Switch to root user for installation
USER root

# Preparations
RUN ln -snf /bin/bash /bin/sh
ARG DEBIAN_FRONTEND=noninteractive

# Update packages and install the minimal set of tools
RUN apt-get update && \
    apt-get install -y --no-install-recommends wget curl git xvfb bzip2 unzip tcsh \
                                               apt-utils \
                                               build-essential \
                                               gfortran \
                                               graphviz \
                                               liblapack-dev \
                                               libblas-dev \
                                               libatlas-dev \
                                               libatlas-base-dev \
                                               libblas3 \
                                               libblas-common \
                                               libopenblas-dev \
                                               libxml2-dev \
                                               libxslt1-dev \
                                               libfreetype6-dev \
                                               libpng12-dev \
                                               libqhull-dev \
                                               libxft-dev \
                                               libjpeg-dev \
                                               libyaml-dev

# Enable neurodebian
RUN curl -sSL http://neuro.debian.net/lists/vivid.de-md.full | tee /etc/apt/sources.list.d/neurodebian.sources.list && \
    curl -sSL http://neuro.debian.net/lists/vivid.us-tn.full >> /etc/apt/sources.list.d/neurodebian.sources.list && \
    apt-key adv --recv-keys --keyserver hkp://pgp.mit.edu:80 0xA5D32F012649A5A9 && \
    apt-get update

# Install miniconda
RUN curl -sSLOk https://repo.continuum.io/miniconda/Miniconda-latest-Linux-x86_64.sh && \
    /bin/bash Miniconda-latest-Linux-x86_64.sh -b -p /usr/local/miniconda && \
    rm Miniconda-latest-Linux-x86_64.sh && \
    echo '#!/bin/bash' > /etc/profile.d/conda_env.sh && \
    echo 'export PATH=/usr/local/miniconda/bin:$PATH' >> /etc/profile.d/conda_env.sh && \
    echo 'source /etc/profile.d/conda_env.sh' >> /etc/bash.bashrc

ENV PATH /usr/local/miniconda/bin:$PATH

# Install dependencies in conda
RUN conda install --yes cython \
                        h5py \
                        ipython \
                        jupyter \
                        matplotlib \
                        networkx \
                        nose \
                        numpy \
                        pandas \
                        scikit-image \
                        scikit-learn \
                        scipy \
                        seaborn && \
    python -c "from matplotlib import font_manager" && \
    conda clean -a

# Install dependencies in pip
RUN pip install --upgrade pip && \
    pip install --upgrade dipy \
                          nibabel \
                          nipy \
                          pydotplus \
                          rdflib \
                          xvfbwrapper

# Install graphviz and pygraphviz
RUN apt-get install -y graphviz libgraphviz-dev && \
    pip install --upgrade pygraphviz graphviz

# Install nipype
RUN pip install -e git+https://github.com/nipy/nipype#egg=nipype

# Clear apt cache and other empty folders
RUN apt-get clean remove -y && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm -rf /boot /media /mnt /opt /srv

ENV SHELL /bin/bash
