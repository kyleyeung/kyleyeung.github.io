---
title: 'Show and Tell: Lessons learned from the 2015 MSCOCO Image Captioning Challenge'
date: 2017-01-12 20:12:12
tags: 
- Image Caption
- Paper Notes
categories: Deep Learning
mathjax: true
---

## Main contribution
1. An end-to-end system for image caption
2. The model combines state-of-art sub-networks for vision and language models, yielding significantly better performance compared to state-of-the-art approaches
  - On the Pascal dataset, NIC yielded a BLEU score of 59, to be compared to the current state-of-the-art of 25, while human performance reaches 69
  - On Flickr30k, we improve from 56 to 66, and on SBU, from 19 to 28
3. We describe the lessons learned from participating in the first MSCOCO competition, which helped us to improve our initial model and place first in automatic metrics, and first (tied with another team) in human evaluation.
<!-- more -->

## Differences from related work
### from Mao et al.
*Explain images with multimodal recurrent neural networks*
*Deep captioning with multimodal recurrent neural networks (m-rnn)*
1. We use a more powerful RNN model
2. We provide the visual input to the RNN model directly
3. We have substantially better results on the established benchmarks

### from Kiros et al.
*Unifying visual-semantic embeddings with multimodal neural language models*
1. They use two separate pathways (one for images, one for text) to define a joint embedding
2. Even though they can generate text, their approach is highly tuned for ranking

## Model
$$\theta^*=\arg\max_ {\theta}\sum_{(I,S)}logp(S|I;\theta)$$
$\theta$ are the parameters of our model, $I$ is an image, and $S$ its correct transcription

Since $S$ represents a sentence and its length is unbounded, so the chain rule is applied:
$$logp(S|I)=\sum_{t=0}^{N}logp(S_t|I,S_0,\cdots,S_{t-1})$$
A RNN is used to model $$p(S_t|I,S_0,\cdots,S_{t-1} )$$:
$$h_{t+1}=f(h_t,x_t)$$
Here, two **crucial design choices** are to be made:
1. the form of $f$ :**LSTM**
2. how are the images and words fed as inputs $x_t$:
    - images -> CNN (BatchNorm)
    - words -> embedding from *Efficient estimation of word representations in vector space*

### LSTM-based Sentence Generator
(Introduction for LSTM ommitted)

#### Training
$$x_{-1}=CNN(I)$$
$$x_t=W_eS_t,t\in{ \{0 \dots N-1\} }$$
$$p_{t+1} = LSTM(x_t),t\in{ \{0 \dots N-1\} }$$
$S_t$ is a one-hot vector to the size of the dictionary, representing each word.
$S_0$ is a special start word and $S_N$ is a special stop word.
The image $I$ only input **once** at $t=-1$.Why **once**? 
>We empirically verified that feeding the image at each time step as an extra input yields inferior results, as the network can explicitly exploit noise in the image and overfits more easily.


**Loss Function**:
$$L(I,S)=-\sum_{t=1}^Nlogp_t(S_t)$$
The above loss is minimized w.r.t. all the parameters of the LSTM, the top layer of the image embedder CNN and word embeddings $W_e$.

#### Inference
Two approches can be used  to generate a sentence given a image:
1. **Sampling**
Sample the first word according to $p_1$, then continue sampling until the end token of some maximum length.
2. **BeamSearch**
Iteratively consider the set of the $k$ best sentences up to time $t$ as candidates to generate sentences of size $t+1$, and keep only the resulting best $k$ of them.
Why it's better?
This better approximates $S=\arg\max_ {S^\prime}p(S^\prime|I)$

## Experiments
### Evaluation 
1. **Human raters**
  - Ask the graders to evaluate each generated sentence with a scale from 1 to 4.
  - Each image was rated by 2 workers.
  - In case of disagreement we simply average the scores and record the average as the score
2. **BLEU**
  - A form of precision of word n-grams between generated and reference sentences
  - Correlates well with human evaluations
3. **Perplexity**
  - The geometric mean of the inverse probability for each predicted word
  - Not reported because BLEU is always preferred.
4. **CIDER**
  - It measures consistency between n-gram occurrences in generated and reference sentences, where this consistency is weighted by n-gram saliency and rarity.
5. **METEOR abd ROUGE**
6. **reacall@K**

### Datasets

| Dataset Name | train | valid | test|
| :-------- | :-------:| :------: | :------:|
| Pascal VOC 2008 |-| - |   1000  |
|Flickr8k|6000|1000|1000|
|Flickr30k|28000|1000|1000|
|MSCOCO|82783|40504|40775|
|SBU|1M|-|-|
>With the exception of SBU, each image has been annotated by labelers with 5 sentences that are relatively visual and unbiased. SBU consists of descriptions given by image owners when they uploaded them to Flickr. As such they are not guaranteed to be visual or unbiased and thus this dataset has more noise.


- **Pascal dataset** :customary used for testing only after a system has been trained on different data. 
- **SBU**:1000 images were hold out for testing and train on the rest. 
- **MSCOCO**: 4K random images reserved from the MSCOCO validation set as test, called COCO-4k, and are used to report results.

### Results
#### Training Details
How dataset size affects generalization?
>We believe that, even with the results we obtained which are quite good, the advantage of our method versus most current human-engineered approaches will only increase in the next few years as training set sizes will grow.

How to deal with overfitting?
1. Initialize the weights of the CNN component to a pretrained model (e.g., on ImageNet) -> helped quite a lot
2. Initializing word embedding $W_e$ from a large news corpus -> no significant gains were observed
3. Model level overfitting-avoiding techniques
  - Dropout -> a few BLEU points improvement
  - Ensembling models -> a few BLEU points improvement
  - Trading off number of hidden units versus depth

How to train the weights?
- All sets of weights
- Stochastic gradient descent
- Fixed learning rate
- No momentum
- All weights were randomly initialized except for the CNN weights

#### Generation Results
>We do think it is more meaningful to report BLEU-4, which is the standard in machine translation moving forward.

#### Transfer Learning, Data Size and Label Quality
- Flickr30k to Flickr8k :the results obtained are 4 BLEU points better. **Why?** More training data added.
- MSCOCO to Flicker8k:All the BLEU scores degrade by 10 points. **Why?** There are likely more differences in vocabulary and a larger mismatch.
- MSCOCO to PASCAL: BLEU-1 59
- Flicker30k to PASCAL: BLEU-1 53
- MSCOCO to SBU: BLEU-1 degrades from 28. **Why?** SBU has weak labeling (i.e., the labels were captions and not human generated descriptions), the task is much harder with a much larger and noisier vocabulary.

#### Generation Diversity Discussion
>If we take the best candidate, the sentence is present in the training set 80% of the times. 

**Why?** The amount of training data is quite small, so it is relatively easy for the model to pick “exemplar” sentences and use them to generate descriptions. 

>If we instead analyze the top 15 generated sentences, about half of the times we see a completely novel description, but still with a similar BLEU score, indicating that they are of enough quality, yet they provide a healthy diversity.

#### Ranking Results
>NIC is doing surprisingly well on both ranking tasks.

#### Human Evaluation
>BLEU is not a perfect metric, as it does not capture well the difference between NIC and human descriptions assessed by raters.

#### Analysis of Embeddings
Some of the relationships learned by the model will help the vision component:
>Indeed, having “horse”, “pony”, and “donkey” close to each other will encourage the CNN to extract features that are relevant to horse-looking animals.

## The MSCOCO Image Captioning Challenge
### Metrics
**CIDER**:All the automatic metrics to correlate with each other quite strongly

### Improvements Over CVPR15 Model
#### Image Model Improvement
GoogleLeNet -> *Batch Normalization*

#### Image Model Fine Tuning
For training:
- To avoid overfitting, initialized the image convolutional network with a pretrained model.
- Fix its parameters and only train the LSTM part of the model on the MSCOCO training set.

For fine tuning:
- Fine tuning the image model must be carried after the LSTM parameters have settled on a good language model. **Why?** Jointly training both, the noise in the initial gradients coming from the LSTM into the image model corrupted the CNN and would never recover.
- Train for about 500K steps (freezing the CNN parameters), and then switch to jointly train the model for an additional 100K steps.
- Improvements achieved by this was 1 BLEU-4 point. **Why?**  
>More importantly, this change allowed the model to transfer information from the image to the language which was likely not possible due to the insufficient coverage of the ImageNet label space. For instance, after the change we found many examples where we predict the right colors, e.g. “A blue and yellow train ...”. It is plausible that the toplayer CNN activations are overtrained on ImageNet-specific classes and could throw away interesting features (such as color), thus the caption generation model may not output words corresponding to those features, without fine tuning the image model.

#### Scheduled Sampling
**A curriculum learning strategy**
- Gently change the training process from a fully guided scheme using the true previous word, towards a less guided scheme which mostly uses the model generated word instead.
- It improved up to 1.5 BLEU-4 points

#### Ensembling
- an ensemble of 5 models trained with Scheduled Sampling and 10 models trained with finetuning the image model. 
- further improved the results by 1.5 BLEU-4 points. **Why?** To be discovered.

#### Beam Size Reduction
- The best beam size turned out to be small:**3**. **Why?**
>Hence, if the model was well trained and the likelihood was aligned with human judgement, increasing the beam size should always yield better sentences. The fact that we obtained the best performance with a relatively small beam size is an indication that **either the model has overfitted** or **the objective function used to train it (likelihood) is not aligned with human judgement**.

- By **reducing** the beam sizem, the novelty of generated sentences **increased**. **Why?**
> This hypothesis supports the fact that the model has overfitted to the training set.

- Reduced beam size technique as another way to regularize (by adding some noise to the inference process).

- The single change improved CIDER score the most, yielding more than 2 BLEU-4 points improvement.

## Conclusion
