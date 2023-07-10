---
layout: post
title: "Question generation and its evaluation"
date: 2023-07-07  
author: Forrest Sheng Bao/鮑盛
---

# General info
* The input of a question generator can contain the answer (e.g., a span in the input document) or not. The former is answer-answer while the latter is answer-agnostic, or without-answer-supervision. The answer-agnostic case is similar to summarization. 
* Shallow vs. Deep QG: Shallow, also called low cognitive demanding, if a question's answer can be found in one sentence and/or is explicitly given. Recently, the focus shifts to multi-hop QG, where the answer can only be obtained by inferencing on multiple sentences. Thus, the questions are considered high cognitive demanding (HCD) or deep.
* The output of a question generator can be sentences or multiple choices. In the multiple choice case, a key is generate good distractors. 

# Question generation papers 

## Low cognitive demanding QG
1. [Unsupervised multiple-choice question generation
for out-of-domain Q&A fine-tuning, ACL-2022](https://aclanthology.org/2022.acl-short.83.pdf). Input: a topic word and a supporting document. Approach: Use jsRealB text realizer from the constituent parsing tree. Distractors are highest ranking candidates except the correct answer. Dataset: SciQ. 
2. [Learning to Generate Questions by Learning
What not to Generate, CGC-QG, WWW-2019](https://arxiv.org/pdf/1902.10418.pdf). **Answer-aware**. Approach: Similar to extractive summarization that predicts whether each token is a "clue" word that should be used to construct the question. To predict the "clueness", a GCN is applied on the parsing tree. 


## High cognitive demanding QG
1. Capturing Greater Context for Question Generation, AAAI-2020. Approach: Non-transformer attention networks between document and answer. Dataset: Squad, MS MARCO, and NewsQA. Evaluation: ROUGE, BLEU and METEOR, and human evaluation on naturalness (grammar) and difficulty. 
2. [Educational Question Generation of Children Storybooks via Question
Type Distribution Learning and Event-Centric Summarization, ACL-2022](https://aclanthology.org/2022.acl-long.348.pdf). **Answer-agonostic**  Approach: BERT to predict a question type, BART to generate the summary from document and question type, and finally BART to generate the question from the summary.  Dataset: FairyTaleQA. Evaluation: ROUGE-L and BERTScore, and human evaluation on four aspects: question type, validity, readability, and child appropriateness. 
3. [CQG: A Simple and Effective Controlled Generation Framework for
Multi-hop Question Generation, ACL-2022](https://aclanthology.org/2022.acl-long.475.pdf). **Answer-aware.** Approach: extracting entities using Graph Attention Network (GAT) and then predicting a flag to each token and finally use the cross attention between input and the flag sequence to generate the question. Dataset: HotpotQA. Evaluation: ROUGE-L, METEOR, and human evaluation on fluency, relevance, and complexity. 
4. [A Feasibility Study of Answer-Agnostic Question Generation for
Education, ACL-2022](https://aclanthology.org/2022.findings-acl.151.pdf). **Answer-agnositic.** This paper finds that QG can be better if the input is not the original document but the summary, human-written or machine-generated. Evaluation: human evaluation. Refs: This paper discusses the difference between answer-agnostic and answer-aware QG.
5. [Question Generation for Reading Comprehension Assessment by Modeling
How and What to Ask, ACL-2022](https://aclanthology.org/2022.findings-acl.168.pdf) Approach: "Thus, in how to ask (HTA), we train (fine-tune) a model
on large QG datasets, and then, we further train
the model to teach the model what to ask (WTA)." **mentioned education**
6. [Semantic Graphs for Generating Deep Questions, ACL-2020](https://aclanthology.org/2020.acl-main.135.pdf). **Answer-aware**.  Approach: First build a semantic graph from the input document (using semantic role lableing, SRL, and dependency parsing), then jointly attend the graph and the input document, and finally decode the question. Evaluation: ROUGE, BLEU, and METEOR, and human evaluation on fluency, relevance, and complexity. Dataset: hotpotQA.
7. [Exploring Question-Specific Rewards for Generating Deep Questions, COLING-2020](https://aclanthology.org/2020.coling-main.228.pdf) **Answer-agnostic**. Approach: Using reward functions to train question generator. The reward covers three aspectss, fluency, relevance, and answerbility. For fluency, the reward is perplexity. For revelance, a BERT model is finetuned to discirminate correct and negative answers. 

# Datasets
* [FairyTaleQA, ACL-22, for educational QA](https://aclanthology.org/2022.acl-long.54.pdf)
* HotPotQA, requires reasoning over multiple Wikipedia pages. 
* SQUAD, 2016, the classic

# The evaluation of question generation 