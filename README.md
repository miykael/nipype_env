# Important Note!

There's a great new software, called [Neurodocker](https://github.com/kaczmarj/neurodocker) that can replace all different levels in this repo. For this reason, I will stop working on this repo and recommend everybody to use [Neurodocker](https://github.com/kaczmarj/neurodocker) instead.

Using [Neurodocker](https://github.com/kaczmarj/neurodocker), allows you to create your own Dockerfile with your own preferences. And you can always be sure to have the most up-to-date version of all softwares.


# Old Readme Content

## Docker Nipype Environment

This Docker image provides a basic Nipype environment (with Python 3), with possible FSL, AFNI, FreeSurfer, ANTs and SPM12 dependencies. The corresponding github repo can be found here: [github.com/miykael/nipype_env](https://github.com/miykael/nipype_env).

## Level0 - Python 3 & Nipype

For a Nipype environment with MRIQC, fMRIprep, dipy, matplotlib, nibabel, nilearn, nipy, numpy, pandas, scipy, seaborn and more use the docker image under [miykael/nipype_level0](https://hub.docker.com/r/miykael/nipype_level0/).

## Level1 - FSL & AFNI

For a neuroimaging environment with Nipype, FSL and AFNI, use the docker image under [miykael/nipype_level1](https://hub.docker.com/r/miykael/nipype_level1/).

## Level2 - FSL, AFNI & SPM12

For a neuroimaging environment with Nipype, FSL, AFNI, R and SPM12, use the docker image under [miykael/nipype_level2](https://hub.docker.com/r/miykael/nipype_level2/).

## Level3 - FSL, AFNI, SPM12 & ANTs

For a neuroimaging environment with Nipype, FSL, AFNI, SPM12 and ANTs, use the docker image under [miykael/nipype_level3](https://hub.docker.com/r/miykael/nipype_level3/).

## Level4 - FSL, AFNI, SPM12, ANTs & FreeSurfer

For a neuroimaging environment with Nipype, FSL, AFNI, SPM12, ANTs and FreeSurfer, use the docker image under [miykael/nipype_level4](https://hub.docker.com/r/miykael/nipype_level4/).

## Level5 - FSL, AFNI, SPM12 & FreeSurfer

For a neuroimaging environment with Nipype, FSL, AFNI, SPM12 and FreeSurfer, use the docker image under [miykael/nipype_level5](https://hub.docker.com/r/miykael/nipype_level5/).

## License

This Dockerfile is based on the dockerfile 'fmriprep' from the Poldrack Lab ([github.com/poldracklab/fmriprep](https://github.com/poldracklab/fmriprep)). The jupyter notebook foundation is based on [jupyter/docker-stacks](https://github.com/jupyter/docker-stacks)'s base-notebook.

This means that the same copyrights apply to this Dockerfile, as they do for the above mentioned dockerfiles. For more information see: [github.com/miykael/nipype_env](https://github.com/miykael/nipype_env).
