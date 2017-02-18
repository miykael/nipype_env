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

FROM miykael/nipype_level2
MAINTAINER Michael Notter <michaelnotter@hotmail.com>

#-------------------
# Install MATLAB MCR
#-------------------
USER root
RUN mkdir -p /opt
ENV MATLAB_VERSION R2016b
RUN mkdir /opt/mcr_install && \
    mkdir /opt/mcr
RUN wget -q -P /opt/mcr_install http://www.mathworks.com/supportfiles/downloads/${MATLAB_VERSION}/deployment_files/${MATLAB_VERSION}/installers/glnxa64/MCR_${MATLAB_VERSION}_glnxa64_installer.zip
RUN unzip -q /opt/mcr_install/MCR_${MATLAB_VERSION}_glnxa64_installer.zip -d /opt/mcr_install && \
    /opt/mcr_install/install -destinationFolder /opt/mcr -agreeToLicense yes -mode silent && \
    rm -rf /opt/mcr_install && \
    rm /opt/mcr/v91/sys/os/glnxa64/libstdc++.so.6 && \
    ln /usr/lib/x86_64-linux-gnu/libstdc++.so.6.0.21 /opt/mcr/v91/sys/os/glnxa64/libstdc++.so.6

# Configure environment
ENV MCR_VERSION v91
ENV LD_LIBRARY_PATH /opt/mcr/${MCR_VERSION}/runtime/glnxa64:/opt/mcr/${MCR_VERSION}/bin/glnxa64:/opt/mcr/${MCR_VERSION}/sys/os/glnxa64:/opt/mcr/${MCR_VERSION}/sys/opengl/lib/glnxa64:$LD_LIBRARY_PATH
ENV MCR_INHIBIT_CTF_LOCK 1
RUN export LD_LIBRARY_PATH


#--------------------------------------
# Install SPM Standalone
#--------------------------------------
USER root
ENV SPM_VERSION 12
ENV SPM_REVISION r6906
ENV SPM_DIR /opt/spm${SPM_VERSION}
ENV SPM_EXEC ${SPM_DIR}/spm${SPM_VERSION}
RUN wget -P /opt http://www.fil.ion.ucl.ac.uk/spm/download/restricted/bids/spm${SPM_VERSION}_${SPM_REVISION}_Linux_${MATLAB_VERSION}.zip
RUN unzip -q /opt/spm${SPM_VERSION}_${SPM_REVISION}_Linux_${MATLAB_VERSION}.zip -d /opt && \
    rm -f /opt/spm${SPM_VERSION}_${SPM_REVISION}_Linux_${MATLAB_VERSION}.zip
RUN echo "export SPM_DIR=/opt/spm12" >> $HOME/.bashrc && \
    echo "export SPMMCRCMD='$SPM_DIR/run_spm12.sh /opt/mcr/v91/ script'" >> $HOME/.bashrc && \
    echo "export FORCE_SPMMCR=1" >> $HOME/.bashrc


#-----------------------------------------
# Source to BASHRC and load configurations
#-----------------------------------------
USER $NB_USER
RUN source $HOME/.bashrc
USER root
RUN ldconfig

CMD ["/bin/bash"]
