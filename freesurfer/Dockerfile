# This Dockerfile is based on the dockerfiles 'crn_base' and 'crn_nipype' from
# the Poldrack Lab (https://github.com/poldracklab/crn_base), the dockerfiles
# from neurovault (https://github.com/NeuroVault/NeuroVault) and the dockerfile
# biss2016-notebook from Oscar Esteban under https://hub.docker.com/u/oesteban.
#
# This means that the same copyrights apply to this Dockerfile, as they do for
# the above mentioned dockerfiles. For more information see:
# https://github.com/miykael/nipype_env

FROM miykael/nipype_basic
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
ENV FREESURFER_HOME /usr/local/freesurfer
ENV SUBJECTS_DIR /usr/local/freesurfer/subjects
ENV MNI_DIR /usr/local/freesurfer/mni
ENV FSF_OUTPUT_FORMAT nii.gz
ENV FSFAST_HOME /usr/local/freesurfer/fsfast
ENV FREESURFER_HOME FreeSurferEnv.sh
RUN export FREESURFER_HOME SUBJECTS_DIR MNI_DIR FSF_OUTPUT_FORMAT FSFAST_HOME FREESURFER_HOME 

CMD ["/bin/bash"]
