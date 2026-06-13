# Datasets and Enviromental Setup
## Datasets 
### PanNuke
### MoNuSAC
### PUMA
### panopTILs

## Enviromental Setup
- Desktop
    - RTX 5070ti
- Edge device
    - jetson orin nano

# Methods
## Data Preprocessing
### Ingestion Pipeline
#### RayCast Representation
- Encoding and Decoding
#### Data Ingestion Design
### Transform Pipeline
#### Data Profile Registration
#### Stain Vector Estimation and Normalization
#### Image Size Standardization
### Data Augmentation Strategies
#### RayCast Permutations
#### Augmentation Operations


## Architecture Design
### Design Considerations
- contour-based inst seg -> use detection model as the base -> pwant FCN+single stage+lightweight+no post-hoc processing -> YOLO26 selected.

- YOLO26 is designed for sparse general object -> uses P3-5 scale -> nuclei is dense and small -> switch used scale to P2-4
    - why not P2-P5? -> heavier and redundant

- Nuclei are small -> regular downsampling will smooths out their limited features -> need downsampling method that preserve features -> DWT-based downsampling

- P2-P4 scale introduce fg:bg ratio imbalance. -> even with 100 inst/img, its still 100/5376 at 256x256px img -> separate cls head into binary and multiclass head for better separation of concern

- post-hoc processing free -> need other way to handle it -> inter-scale softmax before output decoding to supress lower conf output
### Proposed Architecture Components
- ResoConv (replace downsample)
- RayCastDetect
    - Inter-scale Competition
    - RayCast Refinement

## Training Strategy and Optimization
### Curriculum
- MuSGD Optimizer
- SGDR LR scheduler
- TAL scheduler (o2m dominant -> o2o dominant)
    - with added o2m head for training only
- o2o head topk annealing (3 -> 1)

### Assignment
- dual-assignment TAL
- Cost Matrix
    - o2m : $\text{cls}^\alpha \times \text{pIoU}^\beta \times e^{-d^2/2\sigma^2}$
    - o2o : $\text{cls}^\alpha \times e^{-d^2/2\sigma^2}$

### Loss Function
- Loss = lambda_cls * L_cls + lambda_xy * L_xy + lambda_rays * L_rays + lambda_piou * L_piou + lambda_smooth * L_smooth
    - o2m cls uses focal
    - o2o binary cls uses focal -> separates fg from bg
    - o2o multiclass cls uses BCE -> only sees fg

- L_xy -> huber
    - lambda = 500
- L_rays -> L1 log space on analytical rays (how the rays will look like if centroid is placed in c_x, c_y)
    - "L1 in log-space: $|\log r_{\text{pred}} - \log r_{\text{GT}}|$ where GT rays are computed analytically from the predicted centroid via Cramer's rule"
    - circular
    - lambda = 14
- L_piou -> IoU but the area calculation uses sum of sector area-> geometry supervision + adjacent rays interaction
    - "$-\log(\text{PolarIoU})$, where IoU is computed from sector-area decomposition $\sum_i \frac{1}{2} r_i r_{i+1} \sin(2\pi/n)$" — emphasizes why adjacent rays interact
    - circular
    - lambda = 3
- L_smooth -> L1 on 2nd-order discrete curvature
    - $$r_{i+1} - 2r_i + r_{i-1}$$
    - circular
    - lambda = 0->3 over 40% of max epoch


## Evaluation Procedure
insert procedure diagram
### Evaluation Metrics
- AJI
- bPQ
- mPQ
- F1 (also P and R)
- Predicted nuclei
- GFOPs
- Params
- Runtime
### Benchmarking
- against some SOTA models
- all trained and tested with pannuke
- fold1 train, fold2 val, fold3 test
- uses all metrics metioned
### Generalization Test
- testing on MoNuSAC and PUMA
- see how does the model trained on pannuke perform on other dataset
- only use AJI, bPQ, and F1 <- avoid dataset taxonomy clash
### Evaluation on Edge Device
- testing on jetson orin nano
- 2 runtime method: ONNX and TensorRT
- on pannuke
- uses all metrics except GFLOPs and Params

# Research Workflow
insert diagram
