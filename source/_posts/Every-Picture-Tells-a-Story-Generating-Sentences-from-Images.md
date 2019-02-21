---
title: 'Every Picture Tells a Story:Generating Sentences from Images'
date: 2017-01-16 19:31:17
tags: 
- Image Caption
- Paper Notes
categories: Deep Learning
mathjax: true
---

## Main Contribution
1. A dataset to study the problem of generating short descriptive sentences from images
2. A novel representation intermediate between images and sentences
3. A novel, discriminative approach that produces very good results at sentence annotation
4. Methods to use distributional semantics to cope with out of vocabulary words for illustration
5. A quantitative evaluation of sentence generation at a useful scale
<!-- more -->

## Approach
- Assume a space of Meanings that comes between the space of Sentences and the space of Images
- Map each to the meaning space
- Compare the results

### Mapping Image to Meaning
Representation of meaning: **a triplet of <object, action, scene>**
**Why?**
> This triplet provides a holistic idea about what the image (resp. sentence) is about and what is most important.For the image, this is the part that people would talk about first; for the sentence, this is the structure that should be preserved in the tightest summary.

**How to construct the triplet?** 
Each slot in the meaning representation can take a value from a set of discrete values.The edges correspond to the binary relationships between nodes.
![Alt text|middle](\images\1484534108821.png)
Objects: 23 nouns
Actions: 15 values
Scenes: 29 values

**How to do inference?**
> Having provided the potentials of the MRF, we use a greedy method to do inference. Inference involves finding the best selection of the discrete sets of values given the unary and binary potentials.

### Image Potentials
#### Node Potentials
**Image features**
- Felzenszwalb et al. detector responses
- Hoiem et al. classification responses
- Gist-based scene classification responses

**Node features**
Node features are built by fitting a linear SVM to predict each of the nodes independently on the image features.
> This is a number-of-nodes-dimensional vector and each element in this vector provides a score for a node given the image.

(TLDR)Similar images are expected to have similar meanings, so a set of note potentials are computed as a combination of node features :
>- by matching image features, we obtain the k-nearest neighbours in the training set to the test image, then compute the average of the node features over those neighbours, *computed from the image side*. By doing so, we have a representation of what the node features are for similar images. 
>- by matching image features, we obtain the k-nearest neighbours in the training set to the test image, then compute the average of the node features over those neighbours, *computed from the sentence side.* By doing so, we have a representation of what the sentence representation are for images that look like our image.
>- by matching those node features derived from classifiers and detectors (above), we obtain the k-nearest neighbours in the training set to the test image, then compute the average of the node features over those neighbours, *computed from the image side*. By doing so, we have a representation of what the node features are for images that produce similar classifier and detector outputs.
>- by matching those node features derived from classifiers and detectors (above), we obtain the k-nearest neighbours in the training set to the test image, then compute the average of the node features over those neighbours, *computed from the sentence side*. By doing so, we have a representation of what the sentence representation does for images that produce similar classifier and detector outputs.


#### Edge Potentials
**Two problems**
1. Introducing a parameter for each edge results in unmanageable number of parameters
2. Estimates of the parameters for the majority of edges would be noisy

**Solution**
1. Control the number of parameters
2. Do smoothing

**How to limit the number of parameters?**
Use four different estimates for edges:
- The normalized frequency of the word A in our corpus, $f(A)$
- The normalized frequency of the word B in our corpus, $f(B)$
- The normalized frequency of (A and B) at the same time, $f(A, B)$
- $f(A, B)\over f(A)f(B)$

#### Sequence Potentials

**How to represent a sentence?**
Curran & Clark parser --> dependency parse --> (object, action) pairs

**How to measure similarity?**
Lin Similarity Measure

**How to discover co-occurring actions?**
Action Co-occurrence Score

**Node potentials**
>1. First we compute the similarity of each object, scene, and action extracted from each sentence. This gives us the the first estimates for the potentials over the nodes. We call this the sentence node feature.
>2. For each sentence, we also compute the average of sentence node features for other four sentences describing the same images in the train set.
>3. We compute the average of k nearest neighbors in the sentence node features space for a given sentence. We consider this as our third estimate for nodes.
>4. We also compute the average of the image node features for images corresponding to the nearest neighbors in the item above.
>5. The average of the sentence node features of reference sentences for the nearest neighbors in the item 3 is considered as our fifth estimate for nodes.
>6. We also include the sentence node feature for the reference sentence.

**Edge Potentials**
Identical to those of images.

#### Learning
2 mappings to be learned:
1. image space --> meaning space, using image potentials
2. sentence space --> meaning space, using sentence potentials

**Structure learning problem**
$$\min_{\omega} {\lambda\over2}\lVert\omega\rVert^2+{1\over n}\sum _{i\in examples}\xi _i$$
subject to
$$\omega\Phi(x_i,y_i)+\xi_i\ge\max_{y\in meaning\ space }\omega\Phi(x_i,y)+L(y_i,y)\ \forall i\in examples$$
$$\xi_i\ge 0 \ \forall i \in examples$$

## Evaluation
### Dataset
Built from PASCAL 2008 images with Amazon Mechanical Turk

### Inference
