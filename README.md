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

1. **Combating Adversarial Misspellings with Robust Word Recognition**. ACL 2019. [[pdf](https://www.aclweb.org/anthology/P19-1561.pdf)]

To combat adversarial spelling mistakes, the authors propose placing a word recognition model in front of the downstream classifier. The word recognition model is based on semi-character based RNN (ScRNN) - one hot representations of first and last character in the word + bag-word-characters representations of internal characters. To handle rare and unseen words that the word recognition model predicts UNK, they introduce Backoff strategies: 1) Pass-through: directly pass the (possibly misspelled) word as is; 2) Backoff to neutral word: backing off to a neutral word like ‘a’, which has a similar distribution across classes; 3) Backoff to background model: fall back upon a more generic word recognition model trained upon a larger, less-specialized corpus whenever the foreground word recognition model predicts UNK. The word recognition model is trained to reconstruct the original word from char-attacked inputs. This method improves model robustness significantly under character-level attacks, compared to simple adversarial training baseline.



## 2. Model Efficiency

1. **BERT Loses Patience: Fast and Robust Inference with Early Exit**. arXiv 2020. [[pdf](https://arxiv.org/abs/2006.04152)]

During inference, instead of setting a threshold for early exit, the models exits when the predictions of each layer's classifier remain the same for a certain number of layers. Apart from inference speedup, it also achieves better robustness with ensemble effects (as it uses multiple layers' classifiers).

2. **Poly-encoders: Transformer Architectures and Pre-training Strategies for Fast and Accurate Multi-sentence Scoring**. ICLR 2020. [[pdf](https://arxiv.org/abs/1905.01969)]

For retrieval tasks with many candidates, cross-encoder (concat query + candidate and feed to transformer for every pair) is slow, bi-encoder (encoder and cache query and each candidate separately, use dot product as score) performs worse. Poly-encoder first encodes query and candidate separately which allows for caching of candidates. Then, it adds additional multi-head attention over context representations as well as context-candidate attention to get the final context embedding. The final score for each candidate is simply the dot product of final context embedding and that candidate embedding (aggregated over raw candidate encodings). This method is much faster than cross-encoder and also performs better than simple bi-encoder.

## 3. New Models 

1. **STRUCTBERT: INCORPORATING LANGUAGE STRUCTURES INTO PRE-TRAINING FOR DEEP LANGUAGE UNDERSTANDING**. ICLR 2020. [[pdf](https://openreview.net/pdf?id=BJgQ4lSFPH)]

It extends BERT with two additional training objectives: 1) Randomly shuffle trigrams of unmasked tokens, and train the model to restore the correct word order (i.e. predict the original unshuffled token). This is trained together with MLM objective. 2) Upgrade the NSP objective to a three-way classification task for the relationship of the sentence pairs: next sentence, previous sentence, or a random sentence from a different document (Each type sampled 1/3 training examples).  
Adding these two simple objectives significantly improves the performance on downstream tasks compared to corresponding baselines.


## 4. Interpretability, Analysis, Bias

1. **Improving Transformer Models by Reordering their Sublayers**. ACL 2020. [[pdf](https://arxiv.org/pdf/1911.03864.pdf)]

In the vanilla Transformer, each layer consists of a self-attention sublayer followed by a feedforward sublayer. The authors analyse the effect of reordering the sublayers by generating random transformer models, varying the number of each type of sublayer, and their ordering, while keeping the number of parameters constant. They find that models with more self-attention toward the bottom and more feedforward sublayers toward the top tend to perform better in general (on LM). Based on this, they propose sandwich transformer: `k` self-attention layers at the bottom, `(n-k)` self-attention + feedforward pairs in the middle, followed by `k` feedforward layers at the top. This model achieves better performance on LM than vanilla transformer, but no significant gains on translation. 

2. **"You are grounded!": Latent Name Artifacts in Pre-trained Language Models**. arXiv 2020. [[pdf](https://arxiv.org/abs/2004.03012)]

The authors used a set of experiments to demonstrate the artifacts associated with names in pretrained language models: 1) Prompt the PLM with given names, the most likely next word predicted by the PLM is the last name of a prominent named entity (e.g., Donald -> Trump); 2) Simple classifiers can easily recover masked given names of frequently discussed people in the media, as compared to other people; 3) Sentiment classifiers given strong negative sentiments to many names of people discussed in the media. Furthermore, they found that name swapping significantly drops model performance on template-based synthetic QA test sets.



## 5. Machine Reading Comprehension




## 6. Practical Tricks

1. **Revisiting Few-sample BERT Fine-tuning**. arXiv 2020. [[pdf](https://arxiv.org/abs/2006.05987)]

Three useful tricks to improve BERT finetuning performance: 1) Adding back the bias-correction steps in BERTAdam optimizer improves stability especially for small datsets (and reduces degenerate runs). 2) Re-initializing the top K (K to be tuned) layers (including pooler layers) speeds up training and improves performance. 3) Training for more iterations (> 3 epochs) improves performance :)


2. **ReZero is All You Need: Fast Convergence at Large Depth**. arXiv 2020. [[pdf](https://arxiv.org/abs/2003.04887)]

ReZero is a new technique for training deep neural networks (Transformers, ResNet, etc.), where we add a residual connection for each layer: x<sub>i+1</sub> = x<sub>i</sub> + &alpha;F(x<sub>i</sub>). The trainable parameter &alpha; is initialized as 0 at the beginning so it's just performing the identity operation, but it will dynamically evolve to suitable values during initial stages of training. For the original Transformers, each sublayer is added via a residual connection and then normalized by LayerNorm. We can replace this with ReZero connection. This achieves significant speedup for convergence. 


## 7. Other Tasks





