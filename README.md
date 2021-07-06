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
train: path/to/data/train/images
val: path/to/data/valid/images

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

clear_output()
print(f"Setup complete. Using torch {torch.__version__} ({torch.cuda.get_device_properties(0).name if torch.cuda.is_available() else 'CPU'})")
```

## Train
Start training using YOLOv5. Replace 'path/to/data/data.yaml' with the path to your data.yaml file. 
```
python train.py --img 640 --batch 2 --epochs 200 \
    --data path/to/data/data.yaml \
    --cfg models/yolov5s.yaml \
    --weights ' '
```

## Test
Test trained model. Replace 'path/to/data/data.yaml' with the path to your data.yaml file. 
```
python test.py --iou 0.65 --half \
  --data path/to/data/data.yaml \
  --weights runs/train/exp/weights/best.pt
```

## Inference
Run inference on trained model. Results are saved to runs/detect. Replace 'path/to/data/valid' with the path to data you want to run the model against.
Source can be a wide range of types
```
--source 0 # webcam
         file.jpg #image
         file.mp4 #video
         path/ #directory
         path/*.jpg #glob
         'https://youtu.be/dQw4w9WgXcQ' #YouTube video
         'rtsp://example.com/media.mp4' #RTSP, RTMP, HTTP stream
```
```
python detect.py --weights runs/train/exp/weights/best.pt \
  --img 640 \
  --source source/data
  --exist-ok
```