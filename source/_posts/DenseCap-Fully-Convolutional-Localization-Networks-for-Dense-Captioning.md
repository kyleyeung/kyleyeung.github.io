---
title: 'DenseCap: Fully Convolutional Localization Networks for Dense Captioning'
date: 2017-02-28 22:14:16
tags: 
- Image Caption
- Paper Notes
categories: Deep Learning
mathjax: true
---
## Main Contribution
1. Introduced dense captioning task
2. Proposed Fully Convolutional Localization Network
3. Evaluation on Visual  Genome dataset
![Alt text](.\images\1487395538664.png)
<!--more-->

## Related Work
### Object Detection
- CNN
- R-CNN : each region of interest was processed independently
- Fast R-CNN : processing all regions with only single forward pass of the CNN
- **Faster R-CNN**:a region proposal network (RPN) that regresses from anchors to regions of interest

Difference from Faster R-CNN: 

>However, they adopt a 4-step optimization process, while our approach does not require training pipelines. Additionally, we replace their RoI pooling mechanism with a differentiable, spatial soft attention mechanism. In particular, this change allows us to backpropagate through the region proposal network and train the whole model jointly.

### Image Caption
- **Deep visual-semantic alignments for generating image descriptions**

Difference:
>...but they do not tackle the joint task of detection of description in one model. Our model is end-to-end and designed in such way that the prediction for each region is a function of the global   image context, which we show also ultimately leads to stronger performance.

## Model
### Overview
To design an architecture that jointly localizes regions of interest and then describes each with natural language.
### Model Atchitecture
#### CNN
VGG-16, remove the final pooling layer
#### Fully Convolutional Localization Layer
Based on that of Faster R-CNN with a few improvements.