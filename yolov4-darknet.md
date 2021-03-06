# YOLOv4 darknet guide
Guide for running machine learning using YOLOv4 on darknet

## Environment
Python v3.5+
### Windows
Bash terminal (e.g. [git bash](https://git-scm.com/downloads))  
CMake 3.18 or higher ([download](https://cmake.org/download/))
Visual studio 2017 or higher ([download](https://www.visualstudio.com/downloads/))
vcpkg ([download](https://github.com/Microsoft/vcpkg))

## Data structure
Data is not provided in this repository but the following data structure is expected:
```
.
├── dataset
│   ├── train           # images (.jpg) and labels (.txt) used to train model
│   ├── test            # images (.jpg) and labels (.txt) used to test model
│   ├── valid           # images (.jpg) and labels (.txt) used to train model
```
```
Labels should be in standard YOLO v5 format  
(<object index> <x> <y> <width> <height>) 
e.g. (8 0.49278846153846156 0.5420673076923077 0.7716346153846154 0.6778846153846154)
```
'train','test', and 'valid' folders should also contain a '_darknet.labels' file with a human readable list of the labeled objects e.g.
```
cat
dog
rat
bat
ball
```
index of objects in file should match the object index in the label files

## Download data
If you are using private data hosted on the i3drml account use the following commands to download the dataset.zip for the data you are using to train.  

### Setup gshell
This only works in linux, if you are using windows you will need to download the data manually.  

Install gshell
```
python -m pip install gshell
```
Login to the i3drml google account
```
gshell init 
```
Download the dataset.zip file from the drive
```
gshell download path/to/dataset.zip
```
e.g.
```
gshell download TrainedModels/Sharps/darknet/data/dataset/dataset.zip
```
### Extract data
Extract data to current directory
```
mkdir dataset
unzip dataset.zip -d dataset
```

## Setup
### Darknet
Download darknet from repository
```bash
git clone https://github.com/AlexeyAB/darknet.git
```
Download the newly released yolov4 ConvNet weights
```bash
cd darknet/
curl -L https://github.com/AlexeyAB/darknet/releases/download/darknet_yolo_v3_optimal/yolov4.conv.137 --output yolov4.conv.137
```
Install opencv  
Make sure OPENCV_DIR environment variable is set. See [opencv-windows.md](opencv-windows.md) for details or following the official guide from OpenCV [here](https://docs.opencv.org/4.5.2/d3/d52/tutorial_windows_install.html). 
```
sudo apt install libopencv-dev
```
Install CUDA (optional)  
For CUDA support you will need to install CUDA and cuDNN. See [cuda-windows.md](cuda-windows.md) for details or following the official guide from NVIDIA for CUDA toolkit [here](https://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html) and CUDNN [here](https://docs.nvidia.com/deeplearning/cudnn/install-guide/index.html).

#### Build darknet from makefile (linux)
You may need to adjust this for different CUDA hardware  
```
cd darknet/
sed -i 's/OPENCV=0/OPENCV=1/g' Makefile
sed -i 's/GPU=0/GPU=1/g' Makefile
sed -i 's/CUDNN=0/CUDNN=1/g' Makefile
sed -i "s/ARCH= -gencode arch=compute_60,code=sm_60/ARCH= ${ARCH_VALUE}/g" Makefile
make
```
If you are using a computer without CUDA hardware you can turn off GPU support using the following commands instead:
```
sed -i 's/OPENCV=0/OPENCV=1/g' Makefile
sed -i 's/GPU=1/GPU=0/g' Makefile
sed -i 's/CUDNN=1/CUDNN=0/g' Makefile
make
```

#### Build darknet using vcpkg (windows)
1. Install or update Visual Studio to at least version 2017, making sure to have it fully patched (run again the installer if not sure to automatically update to latest version). If you need to install from scratch, download VS from here: [Visual Studio Community](http://visualstudio.com)

3. Install `git` and `cmake`. Make sure they are on the Path at least for the current account

4. Install [vcpkg](https://github.com/Microsoft/vcpkg) and try to install a test library to make sure everything is working, for example `vcpkg install opengl`

5. Define an environment variables, `VCPKG_ROOT`, pointing to the install path of `vcpkg`

6. Define another environment variable, with name `VCPKG_DEFAULT_TRIPLET` and value `x64-windows`

7. Open Powershell and type these commands:

```PowerShell
PS \>                  cd $env:VCPKG_ROOT
PS Code\vcpkg>         .\vcpkg install pthreads opencv[ffmpeg] #replace with opencv[cuda,ffmpeg] in case you want to use cuda-accelerated openCV
```

> If you have the following issue:
> ```
> CMake Error at scripts/cmake/vcpkg_execute_required_process.cmake:105 (message):
> Command failed: ninja -v
> ```
> Check the logs at 'vcpkg\buildtrees\opencv4\config-x64-windows-out.log' for details on the error. This may happen due to a missing download for NVidia optical flow. 
> ```
> Downloads are not permitted during configure. Please pre-download the file "C:/vcpkg/downloads/opencv-cache/nvidia_optical_flow/a73cd48b18dcc0cc8933b30796074191-edb50da3cf849840d680249aa6dbef248ebce2ca.zip":
> vcpkg_download_distfile(OCV_DOWNLOAD
>     URLS "https://github.com/NVIDIA/NVIDIAOpticalFlowSDK/archive/edb50da3cf849840d680249aa6dbef248ebce2ca.zip"
>     FILENAME "opencv-cache/nvidia_optical_flow/a73cd48b18dcc0cc8933b30796074191-edb50da3cf849840d680249aa6dbef248ebce2ca.zip"
>     SHA512 0
> )
> ```
> You will need to download the file manually from [here](https://github.com/NVIDIA/NVIDIAOpticalFlowSDK/archive/edb50da3cf849840d680249aa6dbef248ebce2ca.zip) and place it in the 'vcpkg/downloads/opencv-cache/nvidia_optical_flow/' folder and rename the zip a73cd48b18dcc0cc8933b30796074191-edb50da3cf849840d680249aa6dbef248ebce2ca.zip.

8.  Open Powershell, go to the `darknet` folder and build with the command `.\build.ps1`. If you want to use Visual Studio, you will find two custom solutions created for you by CMake after the build, one in `build_win_debug` and the other in `build_win_release`, containing all the appropriate config flags for your system.

### Configure Data
Create folder to store weights
```
mkdir weights
```
Copy training files into folders for use in darknet
```bash
cp dataset/train/_darknet.labels darknet/data/obj.names
mkdir darknet/data/obj
cp dataset/train/*.jpg darknet/data/obj/
cp dataset/valid/*.jpg darknet/data/obj/

cp dataset/train/*.txt darknet/data/obj/
cp dataset/valid/*.txt darknet/data/obj/
```
Create darknet data files (expects dataset to be in 'dataset' folder in the repository folder root directory e.g. path/to/yolo-guide/dataset)
```
python scripts/create-yolov4-data.py
```

## Train
```
cd darknet
./darknet detector train data/obj.data cfg/custom-yolov4-detector.cfg yolov4.conv.137 -dont_show -map
```
