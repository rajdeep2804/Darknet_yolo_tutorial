# Darknet_yolo_tutorial

# Training Framework

A generalised training framework consisting of all kinds of models like Yolo, RCNN etc. with default model configurations.

## Description
This repo acts like a training guide for yolo object detection model got custom data.

## Testing CUDA,
To test that CUDA works, go to the CUDA demo suite directory:
`cd /usr/local/cuda/extras/demo_suite/`
./deviceQuery

## Executing  darknet
Download the yolov3 weights in Darknet dir:
`wget https://pjreddie.com/media/files/yolov3.weights`

Make Sure in `Makefile` 'gpu == 1'.
From darknet dir run `make`

For Testing Darknet:
./darknet detect cfg/yolov3.cfg yolov3.weights data/dog.jpg

###  Custom Training,
Copy "yolov3.cfg" file from cfg to custom_data/cfg dir, and rename to `yolov3-custom.cfg`

## Making changes in yolov3-custom.cfg file:

The maximum number of iterations for which our network should be trained is set with the param `max_batches=4000`. Also update `steps=3200,3600` which is 80%, 90% of max_batches, you can set value based on your training requirements.

classes param in the yolo layers to based on number of classes you are workning with like for one or `2 class` at `line numbers: 610, 696, 783`.

Similarly we will need to update the filters param based on the classes count `filters=(classes + 5) * 3`. 
For a single class we should set `filters=18` at `line numbers: 603, 689, 776`.

## Updating custom_data dir,
### Updating "custom.names" file : Mention all class name,
### Updating "detector.names" file : 
`classes=1`
train=custom_data/train.txt //Path to text file of images path for training.
valid=custom_data/test.txt // Path to text file of images path for testing.
names=custom_data/custom.names //Path to the class names
backup=backup/ //path to save weights

`Test.txt` need to store the path of each image used for testing
`Train.txt`  need to store the path of each image used for training

## Command to initialise training, 
```bash
./darknet detector detector train custom_data/detector.data custom_data/cfg/yolov3-custom.cfg yolov3.weights
```
## Evaluating your training,
```bash 
./darknet detector test custom_data/detector.data custom_data/cfg/yolov3-custom.cfg backup/yolov3_final.weights -ext_output -out eval.json < eval.txt
./darknet detector map data/obj.data custom_data/cfg/yolov3-custom.cfg backup/yolov3_final.weights
./darknet detector recall data/obj.data custom_data/cfg/yolov3-custom.cfg backup/yolov3_final.weights
```
#note eval.json will store all the output bounding box for each input image path stored in the eval.txt, //eval.txt will be prepared exactly like `test.txt/train.txt`