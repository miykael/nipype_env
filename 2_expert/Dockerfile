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
# https://github.com/miykael/nipype_env

FROM miykael/nipype_advanced
MAINTAINER Michael Notter <michaelnotter@hotmail.com>

#---------------------------
# Install FreeSurfer (v.6.0)
#---------------------------
USER root
RUN wget -q ftp://surfer.nmr.mgh.harvard.edu/pub/dist/freesurfer/6.0.0/freesurfer-Linux-centos6_x86_64-stable-pub-v6.0.0.tar.gz
RUN /bin/bash -c "tar -xvzf freesurfer-Linux-centos6_x86_64-stable-pub-v6.0.0.tar.gz \
                      -C /usr/local \
                      --exclude='freesurfer/subjects/bert' \
                      --exclude='freesurfer/subjects/cvs_avg35' \
                      --exclude='freesurfer/subjects/cvs_avg35_inMNI152' \
                      --exclude='freesurfer/subjects/fsaverage3' \
                      --exclude='freesurfer/subjects/fsaverage4' \
                      --exclude='freesurfer/subjects/fsaverage5' \
                      --exclude='freesurfer/subjects/fsaverage6' \
                      --exclude='freesurfer/subjects/fsaverage_sym' \
                      --exclude='freesurfer/subjects/V1_average' \
                      --exclude='freesurfer/average/mult-comp-cor' \
                      --exclude='freesurfer/average/3T18yoSchwartzReactN32*' \
                      --exclude='freesurfer/average/711-2*' \
                      --exclude='freesurfer/average/RB_all_*' \
                      --exclude='freesurfer/average/RLB700_atlas_*' \
                      --exclude='freesurfer/average/SVIP_*'"
RUN rm freesurfer-Linux-centos6_x86_64-stable-pub-v6.0.0.tar.gz
RUN /bin/bash -c 'echo "michaelnotter@hotmail.com\n14685\n *C4oZU2irTfPc\n FSPrz8qCCW5a6" > /usr/local/freesurfer/.license'

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
