---
title: Paper Excerpts in Image Caption
date: 2017-02-27 21:36:50
tags: 
- Image Caption
- Paper Notes
categories: Deep Learning
mathjax: true
---
This article intends to become a collection of brief summaries of papers in recent years (2014-), in the area of deep image captioning.
<!-- more -->

## 2014
---
### Deep Fragment Embeddings for Bidirectional Image Sentence Mapping
arXiv:1406.5679
#### Model
A model for bidirectional retrieval of images and sentences through a multi-modal embedding of visual and natural language data. 
The model works on a finer level and embeds **fragments of images** (objects) and **fragments of sentences** (typed dependency tree relations) into a common space.
New **fragment alignment objective** that learns to directly associate these fragments across modalities.
#### Representation
Image: RCNN detections -> Image Fragments
Sentence: Dependency relations -> Sentence fragments
#### Experiment
Sentence/Image Retrieval

### Multimodal Neural Language Models
ICML 2014
*This paper is outdated, no need for further research.*
####Model
Multimodal Log-Bilinear Models

### Explaining Images with Multimodal Recurrent Neutal Networks
arXiv:1410.1090
#### Model
**m-RNN model** 
Objective: the probability distribution of generating a word given previous words and the image
Image features are input at **each** word generation
#### Representation
Image features are extracted by a CNN, and then fed to m-RNN to generate the sentence.
#### Experiment
IAPR TC-12
Flickr 8K
Flickr 30K


### Unifying Visual-Semantic Embeddings with Multimodal Neural Language Models
arXiv:1411.2539
#### Model
encoder-decoder models
For the encoder, we learn a joint image-sentence embedding where sentences are encoded using long short-term memory (LSTM) recurrent neural networks [1]. Image features from a deep convolutional network are projected into the embedding space of the LSTM hidden states.
For decoding, we introduce a new neural language model called the structure-content neural language model (SC-NLM).
Loss:pairwise ranking loss
#### Experiment
Image annotation/search: better than m—RNN

### Learning a Recurrent Visual Representation for Image Caption Generation
arXiv:1411.5654 (and CVPR 2015)
#### Model
Use an RNN to learn **bi-directional** mapping between images and descriptions.
An **Recurrent Visual Memory** that automatically learns to remember long-term visual concepts to aid in both sentence generation and visual feature reconstruction.
#### Representation
A bi-directional representation capable of generating both novel descriptions from images and visual epresentations from descriptions.
As a word is generated or read the visual representation is updated to reflect the new information contained in the word.
#### Experiment
PASCAL 1K
Flickr 8k & 30K
MS COCO
Caption: Perplexity BLEU METEOR
Sentence/Image Retrieval



## 2015
---
### Long-term Recurrent Convolutional Networks for Visual Recognition and Description
CVPR 2015
#### Model
LRCN(Longterm Recurrent Convulutional Network)
Different from CNN-RNN model, it used a CNN and a stack of 4 LSTMs for image caption. 
Image features are provided as input to the sequential modal at **each** timestep.
#### Experiment
Flickr 30k
MS COCO 2014
better than m-RNN model （B1 58~67）

### From Captions to Visual Concepts and Back
CVPR 2015
#### Model
Pipeline: 
detect words -> generate sentences -> re-rank sentences
1. Use weakly-supervised learning to create detectors for a set of words commonly found in image captions
2. Train a maximum entropy (ME) LM from a set of training image descriptions
3. Re-rank a set of high-likelihood sentences by a linear weighting of sentence features
#### Experiment
MSCOCO

### Image Specificity
#### Contribution
Introduced the notion of image specificity.
Presented two mechanisms to measure specificity given multiple descriptions of an image: an automated measure and a measure that relies on human judgement.
#### Insight
The rationale is the following: a specific image should be ranked high only if the query description matches the reference description of that image well, because we know that sentences that describe this image tend to be very similar. For ambiguous images, on the other hand, even mediocre similarities between query and reference descriptions may be good enough.
#### Experiment
Image Search
## 2016
---
### DenseCap: Fully Convolutional Localization Networks for Dense Captioning
CVPR 2016

## 2017
---
### MAT: A Multimodal Attentive Translator for Image Captioning
arXiv:1702.05658

#### Model
a **sequence-to-sequence RNN model**
The input image is represented as **a sequence of detected objects** which feeds as the source sequence of the RNN model.

#### Representation
extract the objects features in the image and arrange them in a order using convolutional neural networks. 
a sequential attention layer is introduced to selectively attend to the objects that are related to generate corresponding words in the sentences. 
#### Experiment
surpasses the state-of-the-art methods in all metrics 
a CIDEr of 1.029 (c5) and 1.064 (c40) on MSCOCO evaluation server