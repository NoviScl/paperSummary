# Brief summary of interesting papers that I've read 

### Contents

* [1. Adversarial NLP](#1-adversarial-nlp)
* [2. Model Efficiency](#2-model-efficiency)
* [3. New Models](#3-new-models)
* [4. Interpretability and Analysis](#4-interpreability-and-analysis)
* [5. Machine Reading Comprehension](#5-machine-reading-comprehension)
* [6. Practical Tricks](#6-practical-tricks)
* [7. Other Tasks](#7-other-tasks)

## 1. Adversarial NLP





## 2. Model Efficiency

1. **BERT Loses Patience: Fast and Robust Inference with Early Exit**. arXiv 2020. [[pdf](https://arxiv.org/abs/2006.04152)]

During inference, instead of setting a threshold for early exit, the models exits when the predictions of each layer's classifier remain the same for a certain number of layers. Apart from inference speedup, it also achieves better robustness with ensemble effects (as it uses multiple layers' classifiers).

2. **Poly-encoders: Transformer Architectures and Pre-training Strategies for Fast and Accurate Multi-sentence Scoring**. ICLR 2020. [[pdf](https://arxiv.org/abs/1905.01969)]

For retrieval tasks with many candidates, cross-encoder (concat query + candidate and feed to transformer for every pair) is slow, bi-encoder (encoder and cache query and each candidate separately, use dot product as score) performs worse. Poly-encoder first encodes query and candidate separately which allows for caching of candidates. Then, it adds additional multi-head attention over context representations as well as context-candidate attention to get the final context embedding. The final score for each candidate is simply the dot product of final context embedding and that candidate embedding (aggregated over raw candidate encodings). This method is much faster than cross-encoder and also performs better than simple bi-encoder.

## 3. New Models 

1. **STRUCTBERT: INCORPORATING LANGUAGE STRUCTURES INTO PRE-TRAINING FOR DEEP LANGUAGE UNDERSTANDING**. ICLR 2020. [[pdf](https://openreview.net/pdf?id=BJgQ4lSFPH)]

It extends BERT with two additional training objectives: 1) Randomly shuffle trigrams of unmasked tokens, and train the model to restore the correct word order (i.e. predict the original unshuffled token). This is trained together with MLM objective. 2) Upgrade the NSP objective to a three-way classification task for the relationship of the sentence pairs: next sentence, previous sentence, or a random sentence from a different document (Each type sampled 1/3 training examples).  
Adding these two simple objectives significantly improves the performance on downstream tasks compared to corresponding baselines.

## 4. Interpretability and Analysis

## 5. Machine Reading Comprehension




## 6. Practical Tricks

## 7. Other Tasks





