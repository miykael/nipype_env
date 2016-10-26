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
FROM miykael/nipype_basic
MAINTAINER Michael Notter <michaelnotter@hotmail.com>

#------------------------------------------------------------------------------
# Basic Preparation and ENV variables
#------------------------------------------------------------------------------
USER root
ENV HOME /home
ENV SOFT /soft
ENV BASHRC $HOME/.bashrc
ENV BASICUSER basicuser

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
