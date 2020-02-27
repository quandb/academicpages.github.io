---
title: 'A Transfer Learning Approach for Sentiment Classification and Production Deployment'
date: 2020-02-27
permalink: /posts/2020/02/ulmfit_for_sentiment/
tags:
  - Machine Learning
  - Natural Language Processing
  - Transfer Learning
  - Production
---

Universal Language Model Fine-tuning (ULMFiT)[^1] is an effective transfer learning approach for solving variety tasks in Natural Language Processing.
The authors shed some light on the three common tasks of text classification: sentiment analysis, question classification, and topic classification.
Especially, their work achieves state-of-the-art results on six text classification tasks.

## Outline:
* Underlying ideas of ULMFiT
* Its performance on out domain corpus
* Production deployment

## Underlying ideas of ULMFiT
1. ULMFiT enabled robust inductive transfer learning approach for any NLP tasks with the same 3 layers architecture
2. ULMFiT pre-train a LM on large general corpus, and then fine tunes it on target task using novel technique. The model is universal in the sense that meet those practical criteria:
    ⋅⋅* It works across tasks varying in document size, number, label type
    ⋅⋅* Use a single architecture and training process
    ⋅⋅* It requires no custom feature engineering or preprocessing
    ⋅⋅* It does not require additional in-domain or label document
3. The famous `AWD-LSTM`  LM was used for ULMFiT with various tuned dropout parameters.
4. ULMFiT consists of three following steps:
    ⋅⋅* General domain LM pretraining (most expensive task, train once)
    ⋅⋅* Target task LM fine-tuning
    ⋅⋅* Target task classifier fine-tuning
5. The second (Target task LM fine-tuning) step: converges faster as it only need to adapt to the target data, and allows to train a robust LM even for small corpus. The proposed `discriminative fine-tuning` and `slangted triangular learning rate` (STLR) for fine-tuning the LM
    ⋅⋅* Discriminative fine-tuning: Different layer captures different type of information is the motivation behind the discriminative fine-tuning. Instead of using the same learning rate for all layers of the model, discriminative fine-tuning allows to tune different learning rate for different layer, for context of SGD.
    ⋅⋅* STLR modifies the triangular learning rate with a short increase and a long decay period, which found key for good performance. Which is first linearly increases and then linearly decay its according to the predefined update schedule.
6. Beside `discriminative fine-tuning` and STLR, they proposed `gradual unfreezing` for fine-tuning the classifier.
Gradual Unfreezing the model at the last layer as it contains least general knowledge. They first unfreeze the last layer and then fine-tune all the un-frozen layers for each epoch. Then unfreeze the next lower layer and repeat. Until they fine-tune all layers convergence at the last iteration.


## Its performance on out domain corpus

## Production deployment


[^1]: Jeremy Howard and Sebastian Ruder, [Universal Language Model Fine-tuning for Text Classification](https://arxiv.org/abs/1801.06146), 2018