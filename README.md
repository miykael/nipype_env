# Docker Nipype Environment

This Docker image provides a basic Nipype environment, with possible AFNI, FSL, FreeSurfer and SPM12 dependencies. The corresponding github repo can be found here: [github.com/miykael/nipype_env](https://github.com/miykael/nipype_env).

# Level0 - Basic environment with working Python & Nipype

For a Nipype environment with dipy, matplotlib, nibabel, nipy, numpy, pandas, scipy, seaborn and more use the docker image under [miykael/nipype_level0](https://hub.docker.com/r/miykael/nipype_level0/).

# Level1 - Neuroimaging environment with working FSL & AFNI

For a neuroimaging environment with Nipype, FSL and AFNI, use the docker image under [miykael/nipype_level1](https://hub.docker.com/r/miykael/nipype_level1/).

# Level2 - Neuroimaging environment with working FSL, AFNI & FreeSurfer

For a neuroimaging environment with Nipype, FSL, AFNI and FreeSurfer, use the docker image under [miykael/nipype_level2](https://hub.docker.com/r/miykael/nipype_level2/).

# Level3 - Neuroimaging environment with working FSL, AFNI, FreeSurfer & SPM12

For a neuroimaging environment with Nipype, FSL, AFNI, FreeSurfer and SPM12, use the docker image under [miykael/nipype_level3](https://hub.docker.com/r/miykael/nipype_level3/).

# Level4 - Neuroimaging environment with working FSL & SPM12

For a neuroimaging environment with Nipype, FSL and SPM12, use the docker image under [miykael/nipype_level4](https://hub.docker.com/r/miykael/nipype_level4/).

# License

This Dockerfile is based on the dockerfiles 'crn_base' and 'crn_nipype' from the Poldrack Lab ([github.com/poldracklab/crn_base](https://github.com/poldracklab/crn_base)), the dockerfiles from neurovault ([github.com/NeuroVault/NeuroVault](https://github.com/NeuroVault/NeuroVault)), the dockerfile biss2016-notebook from Oscar Esteban from [hub.docker.com/u/oesteban](https://hub.docker.com/u/oesteban), the dockerfile under ([github.com/BIDS-Apps/dockerfile-templates](https://github.com/BIDS-Apps/dockerfile-templates)) and the dockerfile under ([github.com/Neurita/neuro_docker](https://github.com/Neurita/neuro_docker)).

The jupyter notebook foundation is based on [jupyter/docker-stacks](https://github.com/jupyter/docker-stacks)'s base-, minimal- and scipy-notebook.

This means that the same copyrights apply to this Dockerfile, as they do for the above mentioned dockerfiles. For more information see: [github.com/miykael/nipype_env](https://github.com/miykael/nipype_env).
