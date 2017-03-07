# This Dockerfile is based on the dockerfile 'fmriprep' from the Poldrack
# Lab (https://github.com/poldracklab/fmriprep). The jupyter notebook foundation
# is based on jupyter/docker-stacks's base-notebook.
#
# This means that the same copyrights apply to this Dockerfile, as they do for
# the above mentioned dockerfiles. For more information see:
# https://github.com/miykael/nipype_env

FROM jupyter/base-notebook
MAINTAINER Michael Notter <michaelnotter@hotmail.com>

#---------------------------------------------
# Update OS dependencies and setup neurodebian
#---------------------------------------------
USER root
RUN apt-get update && \
    apt-get install -yq --no-install-recommends bzip2 \
                                                ca-certificates \
                                                curl \
                                                git \
                                                tree \
                                                unzip \
                                                wget \
                                                xvfb \
                                                zip
ENV NEURODEBIAN_URL http://neuro.debian.net/lists/jessie.de-md.full
RUN curl -sSL $NEURODEBIAN_URL | sudo tee /etc/apt/sources.list.d/neurodebian.sources.list && \
    apt-key adv --recv-keys --keyserver hkp://pgp.mit.edu:80 0xA5D32F012649A5A9 && \
    apt-get update -qq

#----------------------------------------------------
# Update and install conda dependencies for python2.7
#----------------------------------------------------
USER $NB_USER

# Make sure that necessary packages are installed
RUN conda create -yq -n python2 python=2.7 ipython \
                                           cython \
                                           dipy \
                                           graphviz \
                                           joblib \
                                           jupyter \
                                           matplotlib \
                                           nb_conda \
                                           nb_conda_kernels \
                                           nilearn \
                                           notebook \
                                           pandas \
                                           pip \
                                           scikit-image \
                                           seaborn && \
    conda clean -tipsy

# Make sure that Python2 is loaded before Python3
ENV PATH=/opt/conda/envs/python2/bin:$PATH

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

#---------------------------------------------
# Install graphviz and update pip dependencies
#---------------------------------------------
USER root
RUN apt-get install -yq --no-install-recommends graphviz
USER $NB_USER
RUN pip install --upgrade --quiet pip && \
    pip install --upgrade --quiet nipy \
                                  rdflib \
                                  pprocess \
                --ignore-installed && \
    rm -rf ~/.cache/pip

#------------------------------------------
# Install newest version of Nipype (github)
#------------------------------------------
USER root
RUN apt-get install -y gcc
RUN pip install -e git+https://github.com/nipy/nipype#egg=nipype

#----------------------------------------
# Clear apt cache and other empty folders
#----------------------------------------
USER root
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /boot /media /mnt /srv
