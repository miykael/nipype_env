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
FROM miykael/nipype_expert
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
# Install MATLAB MCR
#------------------------------------------------------------------------------
USER root
WORKDIR $SOFT
ENV MATLAB_VERSION R2016b
RUN mkdir $SOFT/mcr_install && \
    mkdir $SOFT/mcr
RUN wget -P $SOFT/mcr_install http://www.mathworks.com/supportfiles/downloads/${MATLAB_VERSION}/deployment_files/${MATLAB_VERSION}/installers/glnxa64/MCR_${MATLAB_VERSION}_glnxa64_installer.zip
RUN unzip -q $SOFT/mcr_install/MCR_${MATLAB_VERSION}_glnxa64_installer.zip -d $SOFT/mcr_install && \
    $SOFT/mcr_install/install -destinationFolder $SOFT/mcr -agreeToLicense yes -mode silent && \
    rm -rf $SOFT/mcr_install && \
    rm /soft/mcr/v91/sys/os/glnxa64/libstdc++.so.6 && \
    ln /usr/lib/x86_64-linux-gnu/libstdc++.so.6.0.21 /soft/mcr/v91/sys/os/glnxa64/libstdc++.so.6

# Configure environment
ENV MCR_VERSION v91
ENV LD_LIBRARY_PATH $SOFT/mcr/${MCR_VERSION}/runtime/glnxa64:$SOFT/mcr/${MCR_VERSION}/bin/glnxa64:$SOFT/mcr/${MCR_VERSION}/sys/os/glnxa64:$SOFT/mcr/${MCR_VERSION}/sys/opengl/lib/glnxa64:$LD_LIBRARY_PATH
ENV MCR_INHIBIT_CTF_LOCK 1
RUN export LD_LIBRARY_PATH

#------------------------------------------------------------------------------
# Install SPM Standalone
#------------------------------------------------------------------------------
USER root
WORKDIR $SOFT
ENV SPM_VERSION 12
ENV SPM_REVISION r6906
ENV SPM_DIR $SOFT/spm${SPM_VERSION}
ENV SPM_EXEC ${SPM_DIR}/spm${SPM_VERSION}
RUN wget -P $SOFT http://www.fil.ion.ucl.ac.uk/spm/download/restricted/bids/spm${SPM_VERSION}_${SPM_REVISION}_Linux_${MATLAB_VERSION}.zip
RUN unzip -q $SOFT/spm${SPM_VERSION}_${SPM_REVISION}_Linux_${MATLAB_VERSION}.zip -d $SOFT && \
    rm -f $SOFT/spm${SPM_VERSION}_${SPM_REVISION}_Linux_${MATLAB_VERSION}.zip
RUN echo "export SPM_DIR=$SOFT/spm12" >> $BASHRC && \
    echo "export SPMMCRCMD='$SPM_DIR/run_spm12.sh $SOFT/mcr/v91/ script'" >> $BASHRC && \
    echo "export FORCE_SPMMCR=1" >> $BASHRC

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
