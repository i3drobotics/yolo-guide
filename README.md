# YOLOv5 example
Example repository for running machine learning using YOLOv5 on PyTorch

## Environment
Python v3.5+

## Data structure
Data is not provided in this repository but the following data structure is expected:
```
.
├── data
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
train: train/images
val: valid/images

nc: 2
names: ['cat','dog']
```

## Setup
Clone YOLOv5 github repository
```
git clone https://github.com/ultralytics/yolov5
```
Install requirements (python3)
```
cd yolov5
python -m pip install -qr requirements.txt
```
Test pytorch is setup
```
import torch
from IPython.display import Image, clear_output  # to display images

clear_output()
print(f"Setup complete. Using torch {torch.__version__} ({torch.cuda.get_device_properties(0).name if torch.cuda.is_available() else 'CPU'})")
```