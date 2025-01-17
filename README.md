# Deep-Worm-Tracker

This repository contains the code to Deep-Worm-Tracker a realtime Deep Learning based *C. elegans* worm tracker. It is a combination of Deep object detection (YOLO) and tracking (Strong SORT) models.

## Features
* A large scale annotated dataset containing 3000 worm images is made available for training the object detection model.
* A first of its kind *C. elegans* re-id dataset containing 32 worm identities is used for training the tracking model.
* Annotated images account for background variability like worm trails, eggs, change in magnification, dust particles and marker prints used for demacrating quadrants in actual chemotaxis assays.
* Training time reduced to just 9 to 26 min based on network dimensions.
* Fast inference speed of 8 to 15 ms based on the YOLO object detection model.
* Added functionality to segment and skeletonize tracked worms.
* Individual worm trajectories are also highlighted. 

## Steps to Installation

It is reccomended to create separate conda environments for installing individual packages for the object detection (training YOLO model), tracking (training the torchreid model) and combined final model (for getting final track results). The steps would look something like:

1. Clone the repository:

   `git clone https://github.com/knaticat/Deep-Worm-Tracker.git`

2. Setup environments:
    ```bash
       #for running the Deep-Worm-Tracker using pretrained model weights (Quick start)
       cd Deep-Worm-Tracker
       conda create -n yolostrong
       pip install -r requirements.txt
       
       #for training yolo model
       cd yolov5
       conda create -n yolo
       pip install -r requirements.txt
       
       #for training torchreid model
       cd strong_sort
       conda create -n torchreid
       pip install -r requirements.txt
    ```
3. Running the tracker after cloning the Deep-Worm-Tracker repository:

    ```bash
       $ python3 track.py --source <can be video, webcam, image file> --yolo-weights <path to weights file stored in worm_object_weights> 
       --strong-sort-weights <one of mobilenetv2_x1_0_worm.pt, mobilenetv2_x1_4_worm.pt, osnet_ain_x0_5_worm.pt, osnet_x0_5_worm.pt, osnet_x0_25_worm.pt>
       --img <network dimension> --do-segment --do-skeleton --show-track --show-id-black --show-vid --save-vid
    ```
## Training YOLO model

The step by step instructions for training a YOLO model with a new dataset can be found in [YOLO training](https://github.com/ultralytics/yolov5/wiki/Train-Custom-Data)&nbsp;.
The worm detection dataset is available at [worm-data](https://drive.google.com/drive/folders/1PM4Rvrz-V6p-xqAEWsz66tAKu4W5x8Mc?usp=sharing) which can be extracted and training resumed.
A combination of [DarkMark](https://github.com/stephanecharette/DarkMark)&nbsp; and [Roboflow](https://roboflow.com/annotate) were used for annotating the dataset images. It's reccomended to use Roboflow for future annotations on newer image data for the user friendly interface. 

## Training Strong SORT reid model

We have made available a [re-id dataset](https://drive.google.com/drive/folders/13ZfVVmoCg2Z58oicaYwotK1johqDDZTM?usp=sharing)&nbsp; for *C. elegans* that contains 32 worm identities and is structured similar to the [MARS dataset](http://zheng-lab.cecs.anu.edu.au/Project/project_mars.html)&nbsp;.
We use [torchreid](https://github.com/KaiyangZhou/deep-person-reid)&nbsp; library for training different reid models.
Pretrained checkpoint files for different reid models can be found [here](https://drive.google.com/drive/folders/15L6CCpVGf7p4nXK5BqHpw9WzxxX5nxLc?usp=sharing)&nbsp;.
The available pretrained models are: mobilenetv2_x1_0_worm.pt, mobilenetv2_x1_4_worm.pt, osnet_ain_x0_5_worm.pt, osnet_x0_5_worm.pt, osnet_x0_25_worm.pt. 
These model weight files are automatically downloaded by the script. More details on training Strong SORT model can be found in the strong_sort folder.

## YOLOv4-DeepSORT training

The entire dataset for training the YOLOv4-DeepSORT model remain the same. The individual code and configuration files used in this study can be found [here](https://drive.google.com/drive/folders/1SAdn5v7Kwy9swjJjvkXtolRVicTG4j4A?usp=sharing)&nbsp;.
## Acknowledgements

This work was possible due to the contributions from:
* [Yolov5-StrongSORT_OSNet](https://github.com/mikel-brostrom/Yolov5_StrongSORT_OSNet)&nbsp; by mikel-brostrom 
* [YOLOv5](https://github.com/ultralytics/yolov5)&nbsp; by ultralytics
* [Deep-Person-Reid](https://github.com/KaiyangZhou/deep-person-reid)&nbsp; by KaiyangZhou
* [YOLOv4](https://github.com/AlexeyAB/darknet)&nbsp; by AlexeyAB
* [DeepSORT](https://github.com/nwojke/deep_sort)&nbsp; by nwojke
* [YOLOv4-DeepSORT](https://github.com/theAIGuysCode/yolov4-deepsort)&nbsp; by theAIGuysCode
* [DarkMark](https://github.com/stephanecharette/DarkMark)&nbsp; by stephanecharette

## Cite the work

```latex
@article{BANERJEE2023106024,
title = {Deep-worm-tracker: Deep learning methods for accurate detection and tracking for behavioral studies in C. elegans},
journal = {Applied Animal Behaviour Science},
volume = {266},
pages = {106024},
year = {2023},
issn = {0168-1591},
doi = {https://doi.org/10.1016/j.applanim.2023.106024},
url = {https://www.sciencedirect.com/science/article/pii/S016815912300196X},
author = {Shoubhik Chandan Banerjee and Khursheed Ahmad Khan and Rati Sharma},
keywords = {, Real time tracking, Deep learning, Tracker},
abstract = {Accurate detection and tracking of model organisms such as C. elegans worms remains a fundamental task in behavioral studies. Traditional Machine Learning and Computer Vision methods produce poor detection results and suffer from repeated ID switches during tracking under occlusions and noisy backgrounds. Considering this, we propose Deep-Worm-Tracker, an end-to-end Deep Learning (DL) model, which is a combination of You Only Look Once (YOLOv5) object detection model and Strong Simple Online Real Time Tracking (Strong SORT) tracking backbone that is highly accurate and provides tracking results in real-time inference speeds. Present literature has few solutions to track animals under occlusions and even fewer publicly available large-scale animal re-ID datasets. Thus, we also provide a worm re-ID dataset to minimize worm ID switches, which, to the best of our knowledge, is the first of its kind for C. elegans. We are able to track worms at a mean Average Precision (mAP@0.5) >98% within just 9 min of training time with inference speeds of 9–15 ms for worm detection and on average 27 ms for worm tracking. Our tracking results show that Deep-Worm-Tracker is well suited for ethological studies involving C. elegans.}
}
```
