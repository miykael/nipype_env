# This Dockerfile is based on the dockerfiles from 'crn_base' and 'crn_nipype'
# from the Poldrack Lab (https://github.com/poldracklab/crn_base), the
# dockerfiles from neurovault (https://github.com/NeuroVault/NeuroVault) and
# the dockerfile biss2016-notebook from Oscar Esteban under
# https://hub.docker.com/u/oesteban.
# The jupyter notebook foundation is based on jupyter/docker-stacks's
# base-, minimal- and scipy-notebook.
#
# This means that the same copyrights apply to this Dockerfile, as they do for
# the above mentioned dockerfiles. For more information see:
# https://github.com/miykael/nipype_tutorial

FROM jupyter/base-notebook
MAINTAINER Michael Notter <michaelnotter@hotmail.com>

#---------------------------------
# Update & install OS dependencies
#---------------------------------
USER root
RUN apt-get update && \
    apt-get install -yq apt-utils \
                        build-essential \
                        bzip2 \
                        cmake \
                        curl \
                        dicomnifti \
                        emacs \
                        fusefat \
                        g++-4.9 \
                        gcc \
                        gcc-4.9 \
                        gfortran-4.9 \
                        git \
                        htop \
                        inkscape \
                        jed \
                        libatlas-base-dev \
                        libav-tools \
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
                        libsm6 \
                        libxft-dev \
                        libxml2-dev \
                        libxrender1 \
                        libxslt1-dev \
                        libyaml-dev \
                        pandoc \
                        python-dev \
                        tcsh \
                        tree \
                        unzip \
                        vim \
                        wget \
                        xorg \
                        xvfb \
                        zip && \
    apt-get clean


#--------------------
# Install Neurodebian
#--------------------
USER root
ENV NEURODEBIAN_URL http://neuro.debian.net/lists/jessie.de-md.full
RUN curl -sSL $NEURODEBIAN_URL | sudo tee /etc/apt/sources.list.d/neurodebian.sources.list && \
    apt-key adv --recv-keys --keyserver hkp://pgp.mit.edu:80 0xA5D32F012649A5A9 && \
    apt-get update -qq


#-----------------------------------------------------------------------
# Update and install conda dependencies for python2.7 (including nipype)
#-----------------------------------------------------------------------
USER $NB_USER
RUN conda create --yes -n python2 python=2.7 anaconda

# Set Python2 as condas default environment
ENV PATH /opt/conda/envs/python2/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

# Make sure that necessary packages are installed
RUN conda install -n python2 -qy beautifulsoup4 \
                                 bokeh \
                                 cloudpickle \
                                 cython \
                                 dill \
                                 dipy \
                                 graphviz \
                                 h5py \
                                 hdf5 \
                                 ipython \
                                 ipywidgets \
                                 jupyter \
                                 matplotlib \
                                 networkx \
                                 nibabel \
                                 nipype \
                                 nose \
                                 numba \
                                 numexpr \
                                 numpy \
                                 pandas \
                                 patsy \
                                 pip \
                                 pydicom \
                                 python-dateutil \
                                 pyzmq \
                                 scikit-image \
                                 scikit-learn \
                                 scipy \
                                 seaborn \
                                 setuptools \
                                 sqlalchemy \
                                 statsmodels \
                                 vincent \
                                 xlrd && \
    conda clean -tipsy

# Import matplotlib the first time to build the font cache.
ENV XDG_CACHE_HOME /home/$NB_USER/.cache/
RUN MPLBACKEND=Agg $CONDA_DIR/envs/python2/bin/python -c "import matplotlib.pyplot"

# Activate ipywidgets extension in the environment that runs the notebook server
RUN jupyter nbextension enable --py widgetsnbextension --sys-prefix

# Install Python 2 kernel spec globally to avoid permission problems when NB_UID
# switching at runtime and to allow the notebook server running out of the root
# environment to find it. Also, activate the python2 environment upon kernel
# launch.
USER root
RUN pip install kernda --no-cache && \
    $CONDA_DIR/envs/python2/bin/python -m ipykernel install && \
    kernda -o -y /usr/local/share/jupyter/kernels/python2/kernel.json && \
    pip uninstall kernda -y


#------------------------------------
# Update and install pip dependencies
#------------------------------------
USER $NB_USER
RUN pip install --upgrade --quiet pip && \
    pip install --upgrade --quiet nipy \
                                  rdflib \
                --ignore-installed && \
    rm -rf ~/.cache/pip


#----------------------------------------
# Clear apt cache and other empty folders
#----------------------------------------
USER root
RUN apt-get autoremove -y && \
    apt-get autoclean -y && \
    apt-get remove -y && \
    apt-get clean -y
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /boot /media /mnt /srv
