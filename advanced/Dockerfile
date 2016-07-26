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
RUN apt-get update -qq && \
    apt-get install -y -qq fsl afni

# Set-up FSL and AFNI environment
ENV FSLDIR /usr/share/fsl/5.0
ENV FSLOUTPUTTYPE NIFTI_GZ
ENV FSLMULTIFILEQUIT TRUE
ENV LD_LIBRARY_PATH /usr/lib/fsl/5.0
ENV PATH /usr/lib/fsl/5.0:$PATH
ENV PATH /usr/lib/afni/bin:$PATH
RUN export PATH FSLDIR FSLMULTIFILEQUIT FSLOUTPUTTYPE LD_LIBRARY_PATH

# Clear apt cache and other empty folders
RUN apt-get clean remove -y && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

CMD ["/bin/bash"]
