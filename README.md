# Track pedestrian trajectories for space usage planning
<img src="https://github.com/ronaldchwu/pedestrian-trajectory-tracker/blob/main/assets/aws-solution-architecture.png" width="1000" height="320"/>
![]('assets/aws-solution-architecture.png')

## Overview

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
