# Tensorflow object detection api setup on UNT Talon3 HPC

This guide describes how to set tensorflow object detection api on UNT HPC

Ian Zurutuza - Sept 9, 2019


## Load tensorflow-gpu
`module load tensorflow/1.10.1-gpu`

now check that all necessary modules were loaded
`module list` 

should output:

    1) gcc/8.1.0   
    2) slurm/16.05.8   
    3) python/3.6.0   
    4) cuda/90/toolkit/9.0.176   
    5) cudnn/7.0/cuda90   
    6) tensorflow/1.10.1-gpu

## Clone this repository
`git clone https://github.com/ianzur/obj-detect.git`

## Set up virtual environment
Load the current packages from the system environment with `--system-site-packages`

`python3 -m venv --system-site-packages venvs/obj_detect`

> You shouldn't have to install any dependencies (lots of system packages are already installed)\
> Just incase I `pip freeze`d my virtual environment. Check for differences with:

`pip freeze | diff -s obj-detect/venv_requirements.txt -`


## Set up tensorflow object detection api
Clone tensorflow models: 
`git clone https://github.com/tensorflow/models.git`

Clone + make pycocotools (optional)
```
git clone https://github.com/cocodataset/cocoapi.git
cd cocoapi/PythonAPI
make
cp -r pycocotools <path/to/tensorflow>/models/research/
```

"Install" protoc
```
# From <<path/to/tensorflow>/models/research>
wget -O protobuf.zip https://github.com/google/protobuf/releases/download/v3.0.0/protoc-3.0.0-linux-x86_64.zip
unzip protobuf.zip
```

Then compile .proto files: 
`./bin/protoc object_detection/protos/*.proto --python_out=.`

Finally build and install setup scripts in directories "slim" and "research"\
Source your virtual environment first 
```
python setup.py build 
python setup.py install
```

:tada::tada::tada: **You did it!!** :tada::tada::tada:

## Test install 
If you are using tensorflow-gpu you must submit a job to test this (head nodes don't have access to the gpus)\
The `obj-detect/test_install/` has a sample job script to submit 


