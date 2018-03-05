Bootstrap: docker
From: ubuntu:latest

%environment
   
   LANG=C.UTF-8
   LC_ALL=C.UTF-8
   export LANG LC_ALL
   export LD_LIBRARY_PATH=/usr/lib:/usr/lib64:/usr/local/cuda/lib64:/usr/local/cuda/lib:/opt/cudnn/lib64:$LD_LIBRARY_PATH
   export PATH=/opt/julia-0.6/bin:/usr/local/cuda/bin:$PATH
   export JULIA_PKGDIR=/workdir/.julia

%runscript
exec jupyter notebook --notebook-dir=/workdir/notebooks --ip='*' --port=8888 --no-browser

%post
 
   echo "Here we are installing software and other dependencies for the container!"
   apt-get update
   apt-get install -y \
    build-essential \
    libzmq3-dev \
    pkg-config \
    python \
    python-dev \
    python-pip \
    git \
    vim \
    libxml2 \
    wget \
    curl \
    unzip \
    cmake \

    pip install jupyter
    
    mkdir -p /opt/julia-0.6.2-dev && \
    curl -s -L https://julialang-s3.julialang.org/bin/linux/x64/0.6/julia-0.6.2-linux-x86_64.tar.gz | tar -C /opt/julia-0.6.2-dev -x -z --strip-components=1 -f -
    ln -fs /opt/julia-0.6.2-dev /opt/julia-0.6
    
    export JULIA_PKGDIR=/workdir/.julia
    
    /opt/julia-0.6/bin/julia -e 'Pkg.init()'
    /opt/julia-0.6/bin/julia -e 'Pkg.add("Knet")'
    /opt/julia-0.6/bin/julia -e 'Pkg.add("JLD")'
    /opt/julia-0.6/bin/julia -e 'Pkg.add("ArgParse")'
    /opt/julia-0.6/bin/julia -e 'Pkg.add("PyCall")'
    /opt/julia-0.6/bin/julia -e 'Pkg.add("Images")'
    /opt/julia-0.6/bin/julia -e 'Pkg.add("IJulia")'
    /opt/julia-0.6/bin/julia -e 'Pkg.build("IJulia")'

    rm -rf /workdir/.julia/.cache
    rm -rf /workdir/.julia/lib

    mkdir -p /workdir/notebooks

    chmod -R 777 /workdir

    mkdir -p /opt/cudnn
    mkdir -p /usr/local/cuda