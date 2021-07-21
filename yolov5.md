# YOLOv5 guide
Guide for running machine learning using YOLOv5

## Environment
Python v3.5+  
Bash terminal (Use [git bash](https://git-scm.com/downloads) on Windows)

## Data structure
Data is not provided in this repository but the following data structure is expected:
```
.
├── dataset
│   ├── train           # training data
│   │   ├── images      # training images (.jpg)
│   │   ├── labels      # object labels for images (.txt)
│   ├── test            # test data
│   │   ├── images      # testing images (.jpg)
│   │   ├── labels      # object labels for images (.txt)
│   ├── valid           # validation data
│   │   ├── images      # validation images (.jpg)
│   │   ├── labels      # object labels for images (.txt)
│   ├── data.yaml       # YOLOv5 data configuration file
```
```
Labels should be in standard YOLO v5 format  
(<object index> <x> <y> <width> <height>) 
e.g. (8 0.49278846153846156 0.5420673076923077 0.7716346153846154 0.6778846153846154)
```
'data.yaml' should contain standard YOLOv5 configuration data e.g.
```
train: ../dataset/train/images
val: ../dataset/valid/images

nc: 2
names: ['cat','dog']
```

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
gshell download TrainedModels/Sharps/YOLOv5/data/dataset/dataset.zip
```
### Extract data
Extract data to current directory
```
mkdir dataset
unzip dataset.zip -d dataset
```
Replace the path to your data in the extracted data.yaml file
```
sed -i 's#train: /content/train/images#train: ../dataset/train/images#g' dataset/data.yaml
sed -i 's#val: /content/valid/images#val: ../dataset/valid/images#g' dataset/data.yaml
```

## Setup
Clone YOLOv5 github repository
```
git clone https://github.com/ultralytics/yolov5
```
Install torch CUDA (optional)
```
python -m pip install torch==1.9.0+cu111 torchvision==0.10.0+cu111 -f https://download.pytorch.org/whl/torch_stable.html
```
Install requirements (python3)
```
cd yolov5
python -m pip install -r requirements.txt
```
On some linux machine the following library may also be needed to fix  
*libGL.so.1: cannot open shared object file: No such file or directory*
```
sudo apt install libgl1-mesa-glx
```

## Train
Start training using YOLOv5
```
python train.py --img 640 --batch 2 --epochs 200 \
    --data ../dataset/data.yaml \
    --cfg models/yolov5s.yaml \
    --project ../runs/train \
    --weights ' '
```
The resulting weights will be in runs/train/exp/weights/

## Test
Test trained model
```
python test.py --iou 0.65 --half \
  --data ../dataset/data.yaml \
  --weights ../runs/train/exp/weights/best.pt
```

## Inference
Run inference on trained model. Results are saved to runs/detect
Replace 'path/to/source/' with chosen source.
```
python detect.py \ 
  --weights ../runs/train/exp/weights/best.pt \
  --img 640 \
  --source path/to/source/
  --exist-ok
```
source can be a wide range of types
```
--source 0 # webcam
         file.jpg #image
         file.mp4 #video
         path/ #directory
         path/*.jpg #glob
         'https://youtu.be/dQw4w9WgXcQ' #YouTube video
         'rtsp://example.com/media.mp4' #RTSP, RTMP, HTTP stream
```
