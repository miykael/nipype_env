# This Dockerfile is based on 'miykael/nipype_env', for more information see:
# https://hub.docker.com/r/miykael/nipype_env/
#
# This Dockerfile adds a stripped-down version of FreeSurfer to the environment

FROM miykael/nipype_env
MAINTAINER Michael Notter <michaelnotter@hotmail.com>

# Switch to root user for installation
USER root

# Download and install FreeSurfer
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

# Set-up FreeSurfer environment
RUN echo '#!/bin/bash' > /etc/profile.d/freesurfer_env.sh && \
    echo 'export FREESURFER_HOME=/usr/local/freesurfer' >> /etc/profile.d/freesurfer_env.sh && \
    echo 'export SUBJECTS_DIR=/usr/local/freesurfer/subjects' >> /etc/profile.d/freesurfer_env.sh && \
    echo 'export MNI_DIR=/usr/local/freesurfer/mni' >> /etc/profile.d/freesurfer_env.sh && \
    echo 'export FSF_OUTPUT_FORMAT=nii.gz' >> /etc/profile.d/freesurfer_env.sh && \
    echo 'export FSFAST_HOME=/usr/local/freesurfer/fsfast' >> /etc/profile.d/freesurfer_env.sh && \
    echo 'source $FREESURFER_HOME/FreeSurferEnv.sh' >> /etc/profile.d/freesurfer_env.sh && \
    echo 'source /etc/profile.d/freesurfer_env.sh' >> /etc/bash.bashrc

ENV SHELL /bin/bash
