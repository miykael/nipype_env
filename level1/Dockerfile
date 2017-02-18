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

FROM miykael/nipype_level0
MAINTAINER Michael Notter <michaelnotter@hotmail.com>

#---------------------
# Install FSL and ANFI
#---------------------
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

#----------------------------------------
# Clear apt cache and other empty folders
#----------------------------------------
USER root
RUN apt-get autoremove -y && \
    apt-get autoclean -y && \
    apt-get remove -y && \
    apt-get clean -y
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /boot /media /mnt /srv
