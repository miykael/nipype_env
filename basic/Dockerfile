# This Dockerfile is based on the dockerfiles 'crn_base' and 'crn_nipype' from
# the Poldrack Lab (https://github.com/poldracklab/crn_base), the dockerfiles
# from neurovault (https://github.com/NeuroVault/NeuroVault), the dockerfile
# biss2016-notebook from Oscar Esteban under https://hub.docker.com/u/oesteban,
# the dockerfile under https://github.com/BIDS-Apps/dockerfile-templates and
# the dockerfile under https://github.com/Neurita/neuro_docker.
#
# This means that the same copyrights apply to this Dockerfile, as they do for
# the above mentioned dockerfiles. For more information see:
# https://github.com/miykael/nipype_env
FROM ubuntu:xenial
MAINTAINER Michael Notter <michaelnotter@hotmail.com>

#------------------------------------------------------------------------------
# Basic Preparation and ENV variables
#------------------------------------------------------------------------------
USER root
RUN ln -snf /bin/bash /bin/sh
ARG DEBIAN_FRONTEND=noninteractive

ENV HOME /home
ENV SOFT /soft
ENV BASHRC $HOME/.bashrc
ENV BASICUSER basicuser
ENV BASICUSER_UID 1000
RUN useradd -m -d $HOME -s /bin/bash -N -u $BASICUSER_UID $BASICUSER && \
    chown -R $BASICUSER $HOME

#------------------------------------------------------------------------------
# Update and install ubuntu dependencies
#------------------------------------------------------------------------------
USER root
RUN apt-get update -qq && \
    apt-get install -y -qq bzip2 \
                           cmake \
                           curl \
                           dicomnifti \
                           fusefat \
                           g++-4.9 \
                           gcc \
                           gcc-4.9 \
                           gfortran-4.9 \
                           git \
                           htop \
                           libatlas-base-dev \
                           libatlas-dev \
                           libblas-common \
                           libblas-dev \
                           libblas3 \
                           libfreetype6-dev \
                           libgomp1 \
                           libjpeg-dev \
                           libjpeg62 \
                           liblapack-dev \
                           libnewmat10ldbl \
                           libnifti2 \
                           libopenblas-dev \
                           libpng12-dev \
                           libqhull-dev \
                           libxft-dev \
                           libxml2-dev \
                           libxslt1-dev \
                           libyaml-dev \
                           tcsh \
                           unzip \
                           wget \
                           xorg \
                           zip

#------------------------------------------------------------------------------
# Install Neurodebian
#------------------------------------------------------------------------------
USER root
ENV NEURODEBIAN_URL http://neuro.debian.net/lists/xenial.de-md.full
RUN curl -sSL $NEURODEBIAN_URL | tee /etc/apt/sources.list.d/neurodebian.sources.list && \
    apt-key adv --recv-keys --keyserver hkp://pgp.mit.edu:80 0xA5D32F012649A5A9 && \
    apt-get update -qq

#------------------------------------------------------------------------------
# Install miniconda
#------------------------------------------------------------------------------
USER $BASICUSER
RUN curl -sSLO https://repo.continuum.io/miniconda/Miniconda-latest-Linux-x86_64.sh && \
    /bin/bash Miniconda-latest-Linux-x86_64.sh -b -p /usr/local/miniconda && \
    rm Miniconda-latest-Linux-x86_64.sh
ENV PATH /usr/local/miniconda/bin:$PATH
RUN export PATH

#------------------------------------------------------------------------------
# Update and install conda dependencies
#------------------------------------------------------------------------------
USER $BASICUSER
RUN conda update python conda --yes --quiet && \
    conda config --add channels conda-forge && \
    conda install --yes --quiet cython \
                                dipy \
                                future \
                                graphviz \
                                h5py \
                                ipython \
                                jupyter \
                                matplotlib \
                                networkx \
                                nibabel \
                                nipype \
                                nose \
                                numpy \
                                pandas \
                                pip \
                                pydotplus \
                                python-dateutil \
                                statsmodels \
                                scikit-image \
                                scikit-learn \
                                scipy \
                                setuptools \
                                seaborn \
                                simplejson \
                                traits \
                                xvfbwrapper && \
    python -c "from matplotlib import font_manager" && \
    conda clean -ay

#------------------------------------------------------------------------------
# Update and install pip dependencies
#------------------------------------------------------------------------------
USER $BASICUSER
RUN pip install --upgrade --quiet pip && \
    pip install --upgrade --quiet nipy \
                                  rdflib \
                --ignore-installed && \
    rm -rf ~/.cache/pip

#------------------------------------------------------------------------------
# Install Nipype
#------------------------------------------------------------------------------
USER $BASICUSER
RUN conda config --add channels conda-forge && \
    conda install --yes --quiet nipype

#------------------------------------------------------------------------------
# Clear apt cache and other empty folders
#------------------------------------------------------------------------------
USER root
RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get dist-upgrade -y && \
    apt-get autoremove -y && \
    apt-get autoclean -y && \
    apt-get remove -y && \
    apt-get clean -y
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /boot /media /mnt /srv /opt

#------------------------------------------------------------------------------
# Source to BASHRC and load configurations
#------------------------------------------------------------------------------
USER $BASICUSER
RUN source $BASHRC
USER root
RUN ldconfig
WORKDIR $HOME

CMD ["/bin/bash"]
