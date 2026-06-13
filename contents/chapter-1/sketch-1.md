# Background 
## 1. Vision Models for TME
- Nuclei segmentation is an important first step of histopathological image analysis in Whole Slide Images (WSIs) [1] as it is required for many downstream processes [1]. 
- Deep learning (DL) has became a staple for performing nuclei segmentation [1, 2]
- one of the main difficulties that DL models face is clumped nuclei which results in overlapping instances. 

## 2. Instance Segmentation is the popular method
- Instance Segmentation models are used due to their ability to classify each class objects as a unique instance [3]
- Mask-based instance segmentation is the preferred method to use [4]
- Cons:
  - Heavy: relying on 2-stage architecture or post-processing [5]
  - exclucivity: each pixel can belong to only one segment [6]

## 3. Alternative method: Contour-based Instance Segmentation
- Pros:
  - Lighter: single-stage [5]
  - Geometric Prior: Contour representation can be given geometric information prior to training [7]
- Cons:
  - NMS dependent: adds overhead and may remove close instances [8], ViT based removes dependency but require more computation cost. 

## 4. Proposed Solution
- This research proposes a deep learning model that bridges the gap, End-to-End + FCN + Geometry-Aware  
- **The Model**: A star-convex based, Fully Convolutional Network (FCN).
- **Key Advantages**:
  - *Geometry-Aware*: Uses ray-prediction (star-convexity) to capture true geometric boundaries, enabling precise area and morphological measurements without classifying every pixel.
  - *Lightweight*: FCN architecture keeps computational overhead low.
  - *Truly End-to-End*: Completely eliminates the need for NMS or post-processing, allowing it to natively and accurately resolve dense, highly overlapping cellular scenarios and lowers overhead cost.

# Problem Statements
1. Touching or Overlapping nuclei introduce computational overhead, either by 2-stage architecture, post-processing, or ViT architecture
2. State-of-the-art methods is computationally expensive and not suitable for edge deployment 



[1] https://link.springer.com/content/pdf/10.1007/s12530-023-09491-3.pdf
[2] https://link.springer.com/article/10.1007/s11684-020-0782-9
[3] https://arxiv.org/pdf/2108.13904
[4] https://www.sciencedirect.com/science/article/pii/S1361841524002858?via%3Dihub
[5] DOI 10.1109/CVPR42600.2020.01024
[6] https://pmc.ncbi.nlm.nih.gov/articles/PMC7596871/
[7] https://arxiv.org/pdf/1909.13226
[8] https://arxiv.org/pdf/1806.03535
