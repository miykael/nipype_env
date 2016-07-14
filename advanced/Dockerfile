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

# Install FSL and AFNI
RUN apt-get update && \
    apt-get install -y fsl afni

# Set-up environment
RUN echo '#!/bin/bash' > /etc/profile.d/neuro_env.sh && \
    echo 'export FSLDIR=/usr/share/fsl' >> /etc/profile.d/neuro_env.sh && \
    echo 'export PATH=$FSLDIR/5.0/bin:$PATH' >> /etc/profile.d/neuro_env.sh && \
    echo 'source $FSLDIR/5.0/etc/fslconf/fsl.sh' >> /etc/profile.d/neuro_env.sh && \
    echo 'export PATH=/usr/lib/afni/bin:$PATH' >> /etc/profile.d/neuro_env.sh && \
    echo 'source /etc/profile.d/neuro_env.sh' >> /etc/bash.bashrc

# Clear apt cache and other empty folders
RUN apt-get clean remove -y && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENV SHELL /bin/bash
