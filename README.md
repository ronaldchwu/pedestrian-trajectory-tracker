# Track pedestrian trajectories for space usage planning
<img src="https://github.com/ronaldchwu/pedestrian-trajectory-tracker/blob/main/assets/aws-solution-architecture.png" width="1200">

## Overview
Understanding how people walk around in a space is useful. Knowing how customers explore a retail stores reveals the most or least visited area, so that the store owner can improve store layouts and staff placement. Knowing how pedestrian walk through public spaces helps identify points of congestion and possible barrier of evacuation. Security camera videos provide useful data for such analysis. Computer vision AI made it possible to simply analyze videos and extract pedestrian trajectories, without the need to attach any tracking device to people. Video analytics is projected to increase in market size from $1.1 billion in 2018 to $4.5 billion in 2025 ([report by Tractica](https://omdia.tech.informa.com/OM011985/Video-Analytics)).  With both increased market demands and advances in object-tracking AI algorithms, more and more machine learning solution providers are offering pedestrian tracking services to meet clients' needs.

Interested in how such AI solution is developed, I decided to use open-source software and cloud-computing platforms to build a pedestrian tracker service from scratch. I aim at deploying a easily maintainable and accessible service on AWS cloud. It allows me to minimize running costs, while making use of AWS's MLOps functionality to experiment with various computer vision algorithms.

This service allows users to simply upload a video to a cloud storage (AWS S3), and then receive 1) an annotated video with people in tracking boxes and 2) the detailed trajectory of each person.  The trajectories can be projected to 2D floor plan for detailed spatial flow analyses. All the underlying analyses are automatically triggered by the video upload, and are processed using the serverless AWS Fargate. Data scientists and developers can experiment with different versions of computer vision models and pre- and post-processing scripts, save them as model checkpoints (in S3) and Docker Image (in AWS ECR), and easily deploy them on the Fargate service.

*-- note 27-Mar-2021: The multi-object tracking models are developed and tested. Trajectory analysis is the next step.*

## Methods
Pedestrian tracking is a multi-object tracking (MOT) problem. It involves detecting people in video frames using deep learning models, and associating the positive detections to specific personID using some tracking algorithms. Therefore, to deliver good solutions, we need to select good combinations of deep learning model and tracking algorithm.

Here I experimented with one simple, baseline solution and one state-of-the-art (SOTA) solution. 

**A) Baseline solution:** ([YOLOv3-SORT.ipynb](YOLOv3-SORT.ipynb))
- Use classic object detection deep learning model (YOLOv3) to detect people in each video frame. This and other classic models are widely available on different frameworks (Tensorflow, PyTorch, mxnet) and can be easily imported and used. Here I use the [gluoncv implementation of the YOLOv3 model](https://cv.gluon.ai/build/examples_detection/demo_yolo.html#sphx-glr-build-examples-detection-demo-yolo-py).
- Use a Simple Online and Realtime Tracking (SORT) algorithm that identify people's trajectories based only on locations of the positive detection bounding boxes. This approach does not require learning about each person's appearance (e.g. color of cloth) and is easy to implement (with just one .py script, using implementation of ([abewley/sort](https://github.com/abewley/sort)) ).

**B) SOTA solution:** ([FairMOT.ipynb](FairMOT.ipynb))
- Use FairMOT, a deep learning model specifically designed for multi-object tracking. This deep neural network can simultaneously detect people and learn about their individual feature embeddings (person's appearance).
- Use a tracking algorithm that uses both locations and feature embeddings to associate positive detections to specific person ID.

The SOTA model has a much more complicated algorithm design. Fortunately, the authors of FairMOT provides open-source implemention scripts of both the detection and tracking tasks ([ifzhang/FairMOT](https://github.com/ifzhang/FairMOT/blob/master/src/track.py)). With a few customized script modification, the model can be run and deployed on AWS cloud environment.


## Performance
### Baseline solution: YOLOv3 + SORT
<img src="assets/shopping-mall2-SORT-results-largefont.gif" width="600"/> 

### SOTA solution: FairMOT
<img src="assets/shopping-mall2-results-FairMOT-ct03dt03-largefont.gif" width="600"/> 

## Next to implement
* Project pedestrian trajectories onto 2D floor plans.
* Post-process trajectories (e.g. fix inaccurate re-id over frames)

## Acknowledgements
Thanks to the developers of SORT and FairMOT for providing open-source implemention scripts. 
For details of FairMOT model, please refer to the original publication:
> [**FairMOT: On the Fairness of Detection and Re-Identification in Multiple Object Tracking**](http://arxiv.org/abs/2004.01888),            
> Yifu Zhang, Chunyu Wang, Xinggang Wang, Wenjun Zeng, Wenyu Liu,        
> *arXiv technical report ([arXiv 2004.01888](http://arxiv.org/abs/2004.01888))*
