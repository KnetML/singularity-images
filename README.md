# Singularity Image for Knet

[![https://www.singularity-hub.org/static/img/hosted-singularity--hub-%23e32929.svg](https://www.singularity-hub.org/static/img/hosted-singularity--hub-%23e32929.svg)](https://singularity-hub.org/collections/686)

### USAGE

* Pull the base image
```
singularity pull --name knet.simg shub://KnetML/singularity-images:latest
```

* Not to loose your changes, you should create an overlay image.
```
singularity image.create --size 2048 overlay.simg
```
* If you need more space, you can expand this image later
```
singularity image.expand overlay.simg
```

* Connect to the container with the overlay image.
```
singularity shell --overlay overlay.simg Knet.simg
```

* If you want to enable the gpu usage, you can bind your cuda and cudnn paths.
```
singularity shell --bind <your cuda path>:/usr/local/cuda,<your cudnn path>:/opt/cudnn --overlay overlay.simg --nv Knet.simg
```

* Build Knet
```
julia -e 'Pkg.build("Knet")'
```

* Now you can work under the _/workdir_ folder

### Running the image
You can also run the image directly. It starts an ipython notebook server. You can connect to this server on localhost:8888.
```
singularity run --bind <your cuda path>:/usr/local/cuda,<your cudnn path>:/opt/cudnn --overlay overlay.simg --nv Knet.simg
```

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
singularity shell --bind /usr/local/cuda-9.0/:/usr/local/cuda,/kuacc/apps/cudnn/v7.0.4_CUDA_9.0:/opt/cudnn --overlay overlay.img --nv Knet.simg
```

### DEPENDENCIES
* [Singularity](singularity.lbl.gov)
* Cuda drivers and libraries (optional)
* Cudnn library (optinal)
