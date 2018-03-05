# Singularity Image for Knet

[![https://www.singularity-hub.org/static/img/hosted-singularity--hub-%23e32929.svg](https://www.singularity-hub.org/static/img/hosted-singularity--hub-%23e32929.svg)](https://singularity-hub.org/collections/686)

### USAGE

* Pull the base image
```
singularity pull --name knet.simg shub://KnetML/singularity-images:latest
```

* This image is not complete due to the cuda requirements. You should create an overlay image 
to complete the setup.
```
singularity image.create --size 2048 overlay.simg
```
* If you need more space, you can expand this image later
```
singularity image.expand overlay.simg
```

* Connect to the container with the overlay image. Don't forget to bind your cuda and cudnn paths.
```
singularity shell --bind <your cuda path>:/usr/local/cuda,<your cudnn path>:/opt/cudnn --overlay overlay.simg --nv Knet.simg
```

* Build Knet
```
julia -e 'Pkg.build("Knet")'
```

* Now you can work under the _/workdir_ folder

### Usage on the kuacc cluster

* Load necessary modules
```
module load cuda/9.0
module load cudnn/7.0.4/cuda-9.0
module load singularity
```

* Set SINGULARITY_CACHEDIR as your scratch
```
export SINGULARITY_CACHEDIR=/scratch/users/username
```

* On a compute node with gpu
```
singularity shell --bind /usr/local/cuda-9.0/:/usr/local/cuda,/kuacc/apps/cudnn/v7.0.4_CUDA_9.0:/opt/cudnn --overlay overlay.img --nv RG.simg
```

### DEPENDENCIES
* Singularity
* Cuda drivers and libraries (optional)
* Cudnn library (optinal)
