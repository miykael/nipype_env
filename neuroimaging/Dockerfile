FROM ubuntu:xenial

MAINTAINER Michael Notter <michaelnotter@hotmail.com>


#------------------------------------------------------------------------------
# Basic Preparation and ENV variables
#------------------------------------------------------------------------------
USER root
RUN ln -snf /bin/bash /bin/sh
ARG DEBIAN_FRONTEND=noninteractive

ENV HOME /work
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
                           dcm2niix \
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
# Install FSL and ANFI
#------------------------------------------------------------------------------
USER root
RUN apt-get install -y -qq fsl afni
ENV FSLDIR /usr/share/fsl/5.0
ENV FSLOUTPUTTYPE NIFTI_GZ
ENV FSLMULTIFILEQUIT TRUE
ENV POSSUMDIR /usr/share/fsl/5.0
ENV LD_LIBRARY_PATH /usr/lib/fsl/5.0:$LD_LIBRARY_PATH
ENV FSLTCLSH /usr/bin/tclsh
ENV FSLWISH /usr/bin/wis
ENV PATH /usr/lib/fsl/5.0:$PATH
ENV PATH /usr/lib/afni/bin:$PATH
RUN export PATH FSLDIR FSLMULTIFILEQUIT FSLOUTPUTTYPE


#------------------------------------------------------------------------------
# Install FreeSurfer
#------------------------------------------------------------------------------
USER root
RUN wget ftp://surfer.nmr.mgh.harvard.edu/pub/dist/freesurfer/5.3.0/freesurfer-Linux-centos6_x86_64-stable-pub-v5.3.0.tar.gz && \
    /bin/bash -c "tar -xvzf freesurfer-Linux-centos6_x86_64-stable-pub-v5.3.0.tar.gz \
                      -C /usr/local \
                      --exclude='freesurfer/average/mult-comp-cor' \
                      --exclude='freesurfer/subjects/bert' \
                      --exclude='freesurfer/subjects/cvs_avg35' \
                      --exclude='freesurfer/subjects/cvs_avg35_inMNI152' \
                      --exclude='freesurfer/subjects/fsaverage3' \
                      --exclude='freesurfer/subjects/fsaverage4' \
                      --exclude='freesurfer/subjects/fsaverage5' \
                      --exclude='freesurfer/subjects/fsaverage6' \
                      --exclude='freesurfer/subjects/fsaverage_sym' \
                      --exclude='freesurfer/subjects/lh.EC_average' \
                      --exclude='freesurfer/subjects/rh.EC_average' \
                      --exclude='freesurfer/subjects/V1_average' \
                      --exclude='freesurfer/trctrain' \
                      --exclude='freesurfer/average/3T18yoSchwartzReactN32*' \
                      --exclude='freesurfer/average/711-2*' \
                      --exclude='freesurfer/average/RB_all_*'" && \
    rm freesurfer-Linux-centos6_x86_64-stable-pub-v5.3.0.tar.gz && \
    /bin/bash -c 'echo "michaelnotter@hotmail.com\n14685\n *C4oZU2irTfPc" > /usr/local/freesurfer/.license'

ENV OS Linux
ENV MINC_BIN_DIR /usr/local/freesurfer/mni/bin
ENV MINC_LIB_DIR /usr/local/freesurfer/mni/lib
ENV MNI_DATAPATH /usr/local/freesurfer/mni/data
ENV FMRI_ANALYSIS_DIR /usr/local/freesurfer/fsfast
ENV PERL5LIB /usr/local/freesurfer/mni/lib/perl5/5.8.5
ENV MNI_PERL5LIB /usr/local/freesurfer/mni/lib/perl5/5.8.5
ENV FREESURFER_HOME /usr/local/freesurfer
ENV SUBJECTS_DIR /usr/local/freesurfer/subjects
ENV MNI_DIR /usr/local/freesurfer/mni
ENV FSF_OUTPUT_FORMAT nii.gz
ENV FSFAST_HOME /usr/local/freesurfer/fsfast
RUN export FREESURFER_HOME SUBJECTS_DIR MNI_DIR FSF_OUTPUT_FORMAT FSFAST_HOME FREESURFER_HOME MINC_BIN_DIR FMRI_ANALYSIS_DIR


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
# Install VTK (http://www.vtk.org)
#------------------------------------------------------------------------------
USER root
RUN ln -s /usr/lib/x86_64-linux-gnu/libgsl.so /usr/lib/libgsl.so.0
###   RUN apt-get -y build-dep vtk6 && \
###       update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5   40 --slave /usr/bin/g++ g++ /usr/bin/g++-5 && \
###       update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.9 60 --slave /usr/bin/g++ g++ /usr/bin/g++-4.9
WORKDIR $SOFT
ENV N_CPUS 4
ENV VTK_VERSION v6.3.0
ENV VTK_GIT https://gitlab.kitware.com/vtk/vtk.git
RUN mkdir /soft/vtk
RUN cd /soft/vtk
RUN git clone $VTK_GIT -b $VTK_VERSION /soft/vtk/VTK
RUN mkdir /soft/vtk/build
RUN cd /soft/vtk/build
RUN cmake -DCMAKE_BUILD_TYPE=Release \
          -DPYTHON_EXECUTABLE=/usr/bin/python2.7 \
          -DPYTHON_INCLUDE_DIR=/usr/include/python2.7 \
          -DPYTHON_INCLUDE_DIR=/usr/include/x86_64-linux-gnu/python2.7m \
          -DPYTHON_LIBRARY=/usr/lib/x86_64-linux-gnu/libpython2.7m.so.1 \
          /soft/vtk/VTK
RUN make -j $N_CPUS
RUN make install


#------------------------------------------------------------------------------
# Install ANTs
#------------------------------------------------------------------------------
USER root
ENV ANTS_VERSION v2.1.0
ENV ANTS_GIT https://github.com/stnava/ANTs.git
WORKDIR $SOFT
RUN mkdir $SOFT/ants
RUN cd $SOFT/ants
RUN git clone $ANTS_GIT -b $ANTS_VERSION $SOFT/ants/ANTs
RUN mkdir $SOFT/ants/build
RUN cd $SOFT/ants/build
RUN cmake -DCMAKE_BUILD_TYPE=Release \
          -DUSE_VTK=ON \
          -DUSE_SYSTEM_VTK=ON \
          -DVTK_DIR=$SOFT/vtk/build \
          $SOFT/ants/ANTs && \
RUN make -j $N_CPUS
RUN echo "export ANTSPATH=${SOFT}/ants/build/bin" >> $BASHRC
RUN echo 'addpath $ANTSPATH' >> $BASHRC


#------------------------------------------------------------------------------
# Install MATLAB MCR
#------------------------------------------------------------------------------
USER root
WORKDIR $SOFT
ENV MATLAB_VERSION R2016b
RUN mkdir /soft/mcr_install && \
    mkdir /soft/mcr
RUN wget -P /soft/mcr_install http://www.mathworks.com/supportfiles/downloads/${MATLAB_VERSION}/deployment_files/${MATLAB_VERSION}/installers/glnxa64/MCR_${MATLAB_VERSION}_glnxa64_installer.zip
RUN unzip -q /soft/mcr_install/MCR_${MATLAB_VERSION}_glnxa64_installer.zip -d /soft/mcr_install && \
    /soft/mcr_install/install -destinationFolder /soft/mcr -agreeToLicense yes -mode silent && \
    rm -rf /soft/mcr_install

# Configure environment
ENV MCR_VERSION v91
ENV LD_LIBRARY_PATH /soft/mcr/${MCR_VERSION}/runtime/glnxa64:/soft/mcr/${MCR_VERSION}/bin/glnxa64:/soft/mcr/${MCR_VERSION}/sys/os/glnxa64:/soft/mcr/${MCR_VERSION}/sys/opengl/lib/glnxa64:$LD_LIBRARY_PATH
ENV MCR_INHIBIT_CTF_LOCK 1
RUN export LD_LIBRARY_PATH


#------------------------------------------------------------------------------
# Install SPM Standalone
#------------------------------------------------------------------------------
USER root
ENV SPM_VERSION 12
ENV SPM_REVISION r6906
ENV SPM_DIR /soft/spm${SPM_VERSION}
ENV SPM_EXEC ${SPM_DIR}/spm${SPM_VERSION}
RUN wget -P /soft http://www.fil.ion.ucl.ac.uk/spm/download/restricted/bids/spm${SPM_VERSION}_${SPM_REVISION}_Linux_${MATLAB_VERSION}.zip
RUN unzip -q /soft/spm${SPM_VERSION}_${SPM_REVISION}_Linux_${MATLAB_VERSION}.zip -d /soft && \
    rm -f /soft/spm${SPM_VERSION}_${SPM_REVISION}_Linux_${MATLAB_VERSION}.zip
RUN echo "export SPM_DIR=$SOFT/spm12" >> $BASHRC && \
    echo "export SPMMCRCMD='$SPM_DIR/run_spm12.sh $MCR_DIR script'" >> $BASHRC && \
    echo "export FORCE_SPMMCR=1" >> $BASHRC


#------------------------------------------------------------------------------
# Clear apt cache and other empty folders
#------------------------------------------------------------------------------
USER root
RUN apt-get clean remove -y && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm -rf /boot /media /mnt /opt /srv


#------------------------------------------------------------------------------
# Source to BASHRC and load configurations
#------------------------------------------------------------------------------
USER $BASICUSER
RUN source $BASHRC
USER root
RUN ldconfig
WORKDIR $HOME

CMD ["/bin/bash"]
