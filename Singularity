Bootstrap: docker
From: nvidia/cuda:8.0-cudnn6-devel-ubuntu16.04

%post
    apt-get update && apt-get -y install locales
    locale-gen en_US.UTF-8
    apt-get install -y git wget python3-dev python3-pip
    apt-get clean
    mkdir /om
    mkdir /om2


    NV_DRIVER_VERSION=375.20      # <---- EDIT: CHANGE THIS FOR YOUR SYSTEM
    NV_DRIVER_FILE=NVIDIA-Linux-x86_64-${NV_DRIVER_VERSION}.run

    working_dir=$(pwd)
    # download and run NIH NVIDIA driver installer
    wget http://us.download.nvidia.com/XFree86/Linux-x86_64/${NV_DRIVER_VERSION}/NVIDIA-Linux-x86_64-${NV_DRIVER_VERSION}.run

    echo "Unpacking NVIDIA driver into container..."
    cd /usr/local/
    sh ${working_dir}/${NV_DRIVER_FILE} -x
    rm ${working_dir}/${NV_DRIVER_FILE}
    mv NVIDIA-Linux-x86_64-${NV_DRIVER_VERSION} NVIDIA-Linux-x86_64
    cd NVIDIA-Linux-x86_64/
    for n in *.$NV_DRIVER_VERSION; do
        ln -v -s $n ${n%.375.20}   # <---- EDIT: CHANGE THIS IF DRIVER VERSION
    done
    ln -v -s libnvidia-ml.so.$NV_DRIVER_VERSION libnvidia-ml.so.1
    ln -v -s libcuda.so.$NV_DRIVER_VERSION libcuda.so.1
    cd $working_dir

    echo "Adding NVIDIA PATHs to /environment..."
    NV_DRIVER_PATH=/usr/local/NVIDIA-Linux-x86_64
    echo "

LD_LIBRARY_PATH=/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64:$NV_DRIVER_PATH:\$LD_LIBRARY_PATH
PATH=$NV_DRIVER_PATH:\$PATH
export PATH LD_LIBRARY_PATH

" >> /environment

    apt-get install -y libcupti-dev
    pip3 install --upgrade pip
    pip3 install tensorflow-gpu
    pip3 install keras
