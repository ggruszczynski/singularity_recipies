# Singularity recipies

[![https://www.singularity-hub.org/static/img/hosted-singularity--hub-%23e32929.svg](https://www.singularity-hub.org/static/img/hosted-singularity--hub-%23e32929.svg)](https://singularity-hub.org/collections/4746)

Official Documentation:

<https://singularityhub.github.io/singularityhub-docs/docs/getting-started/recipes>

## DIY

How to run:

```.sh

# to build
sudo singularity build image.sif recipe.def

# to run 
singularity shell --cleanenv lolcow_latest.sif  # Without the --cleanenv flag, the environment on the host system will be present within the container at run time.
singularity exec lolcow_latest.sif cowsay moo
singularity run lolcow_latest.sif

# to download
singularity pull shub://ggruszczynski/singularity_recipies
singularity run singularity_recipies_latest.sif

# to build from docker hub
singularity pull docker://openfoam/openfoam7-paraview56
singularity shell --cleanenv openfoam7-paraview56_latest.sif
singularity exec --cleanenv cat /etc/os-release

# to build from docker file
# https://www.nas.nasa.gov/hecc/support/kb/converting-docker-images-to-singularity-for-use-on-pleiades_643.html

$ sudo docker build -t ood-rstudio-bio.4.1.2 - < Dockerfile.4.1.2

$ docker images
REPOSITORY                   TAG       IMAGE ID       CREATED          SIZE
ood-rstudio-bio.4.1.2        latest    9ab18b041cba   27 minutes ago   7.05GB

$ docker save 9ab18b041cba -o ood_rstudio_bio_docker_412.tar

$ singularity build --sandbox lolcow docker-archive://lolcow.tar
```

### OpenFoam notes

OF fundation: vX versioning + third party
OF org: vYYMM versioning

## MPI notes

https://singularity.lbl.gov/faq
Why do we call ‘mpirun’ from outside the container (rather than inside)?
With Singularity, the MPI usage model is to call ‘mpirun’ from outside the container, and reference the container from your ‘mpirun’ command. Usage would look like this:

$ mpirun -np 20 singularity exec container.img /path/to/contained_mpi_prog
By calling ‘mpirun’ outside the container, we solve several very complicated work-flow aspects. For example, if ‘mpirun’ is called from within the container it must have a method for spawning processes on remote nodes. Historically ssh is used for this which means that there must be an sshd running within the container on the remote nodes, and this sshd process must not conflict with the sshd running on that host! It is also possible for the resource manager to launch the job and (in Open MPI’s case) the Orted processes on the remote system, but that then requires resource manager modification and container awareness.

In the end, we do not gain anything by calling ‘mpirun’ from within the container except for increasing the complexity levels and possibly losing out on some added performance benefits (e.g. if a container wasn’t built with the proper OFED as the host).

See the Singularity on HPC page for more details.
