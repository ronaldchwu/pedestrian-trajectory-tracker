# Track pedestrian trajectories for space usage planning
<img src="https://github.com/ronaldchwu/pedestrian-trajectory-tracker/blob/main/assets/aws-solution-architecture.png" width="1200">

## Overview
Understanding how people walk around in a space is useful. Knowing how customers explore a retail stores reveals the most or least visited area, so that the store owner can improve store layouts and staff placement. Knowing how pedestrian walk through public spaces helps identify points of congestion and possible barrier of evacuation. Security camera videos provide useful data for such analysis. Computer vision AI made it possible to simply analyze videos and extract pedestrian trajectories, without the need to attach any tracking device to people. Video analytics is projected to increase in market size from $1.1 billion in 2018 to $4.5 billion in 2025 [report by Tractica](https://omdia.tech.informa.com/OM011985/Video-Analytics).  With both increased market demands and advances in object-tracking AI algorithms, more and more machine learning solution providers are offering pedestrian tracking services to meet clients' needs.

Interested in how such AI solution is developed, I decided to use open-source software and cloud-computing platforms to build a pedestrian tracker service from scratch. I aim at deploying a easily maintainable and accessible service on AWS cloud. It allows me to minimize running costs, while making use of AWS's MLOps functionality to experiment with various computer vision algorithms.

This service allows users to simply upload a video to a cloud storage (AWS S3), and then receive 1) an annotated video with people in tracking boxes and 2) the detailed trajectory of each person.  The trajectories can be projected to 2D floor plan for detailed spatial flow analyses. All the underlying analyses are automatically triggered by the video upload, and are processed using the serverless AWS Fargate. Data scientists and developers can experiment with different versions of computer vision models and pre- and post-processing scripts, save them as model checkpoints (in S3) and Docker Image (in AWS ECR), and easily deploy them on the Fargate service.

-- note 27-Mar-2021: The multi-object tracking models are developed and tested. Trajectory analysis is the next step.

## Methods
* Two-step online method: YOLOv3 + SORT
* One-shot deep learning method: FairMOT
[ifzhang/FairMOT](https://github.com/ifzhang/FairMOT)
> [**FairMOT: On the Fairness of Detection and Re-Identification in Multiple Object Tracking**](http://arxiv.org/abs/2004.01888),            
> Yifu Zhang, Chunyu Wang, Xinggang Wang, Wenjun Zeng, Wenyu Liu,        
> *arXiv technical report ([arXiv 2004.01888](http://arxiv.org/abs/2004.01888))*

## Performance
### Two-step online method: YOLOv3 + SORT
<img src="assets/shopping-mall2-SORT-results-largefont.gif" width="600"/> 

### One-shot deep learning method: FairMOT
<img src="assets/shopping-mall2-results-FairMOT-ct03dt03-largefont.gif" width="600"/> 

## Next to implement
* Project pedestrian trajectories onto 2D floor plans.
* Post-process trajectories (e.g. fix inaccurate re-id over frames)

## Acknowledgement
