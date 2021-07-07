# CUDA Windows installation

## Download
Download CUDA toolkit from [CUDA website](http://developer.nvidia.com/cuda-downloads)  
Download CUDNN from [CUDNN website](https://developer.nvidia.com/cudnn)  
NVIDIA developer program membership is required for CUDNN download but this is free. 

## Install
Install CUDA toolkit using donwloaded installer.  
Unzip the downloaded cuDNN zip file.
```
unzip cudnn-11.4-windows-x64-v8.2.2.26.zip
```
Copy the following files into the CUDA Toolkit directory.
> Copy bin\cudnn*.dll to C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\vx.x\bin.  
> Copy include\cudnn*.h to C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\vx.x\include.  
> Copy lib\x64\cudnn*.lib to C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\vx.x\lib\x64.  

Set the CUDA_PATH environment variable to the CUDA Toolkit directory e.g. C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\vx.x