---
layout: post
title: Fine-tuning T5 LLM To Balance Textual Dataset
date: 2023-12-06 00:00:00 +0300
description: This post will show how can you can fine-tune T5 model and utilize it to balance your textual dataset. # Add post description (optional)
img: fine-tune-llm.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Applied Data Science, LLM, T5, Imbalanced Dataset]
---
# Balancing Textual Datasets with T5 LLM

Imbalanced datasets can pose a significant challenge in the field of data science, especially when working with textual data. Addressing this issue is crucial to ensure the robustness and fairness of machine learning models. In this blog post, we'll explore various techniques for balancing textual datasets, focusing on the popular T5 (Text-To-Text Transfer Transformer) model. Traditional approaches like undersampling and oversampling have been widely employed to mitigate class imbalances, but we'll delve into a more advanced and nuanced method – leveraging a generative model. By employing T5, we can generate synthetic samples, enriching the dataset and enhancing the model's ability to handle diverse text inputs. Join us as we navigate through the intricacies of balancing a dataset derived from HTML home pages of various websites, classified based on their LinkedIn page information.

Anything explained in this post can be found in the github repository: [dvirla/Balancing_Textual_Datasets](https://github.com/dvirla/Balancing_Textual_Datasets)

<div style="text-align:center;">
  <img src="../assets/img/textual-data-snippet.jpeg" alt="data snippet" width="800"/>
  <p style="color:grey;">A Snippet of The Dataset</p>
</div>


## The Imbalanced Dataset: Navigating Class Disparities in Web Content

Our dataset is a collection of HTML home pages extracted from diverse websites, each classified into specific industries ranging from "construction" and "health, wellness, and fitness" to "wireless" and beyond. The challenge at hand lies in the stark imbalances among these classes, where some industries boast thousands of occurrences, while others are represented by less than 1000 instances. Such class imbalances can severely impact the effectiveness of machine learning models, as they tend to favor the majority classes, potentially neglecting the nuances of the minority ones. In our exploration of balancing textual datasets using T5, we aim to address these disparities and enhance the model's ability to generalize across various industries, ensuring a more equitable representation in the learning process.

<div style="text-align:center;">
  <img src="../assets/img/textual-data-industries.png" alt="imbalanced classes" width="800"/>
  <p style="color:grey;">Imbalanced Classes</p>
</div>

The methods that were tested in order to deal with the imbalance are:
1. Undersampling - Randomly selecting records from every industry we want to undersample and removing all others (up to a certain percentile).
2. Oversampling - Randomly selecting records (with replacements) from the smaller classes.
3. Generative model - Training a T5 model for every rare class in the dataset. We will use those models to generate new records for every class using different inputs which  are based on real texts from the class.


## Preprocessing: Crafting a Refined Foundation for Textual Analysis

To ensure the quality and uniformity of our textual dataset, we undertook a meticulous preprocessing procedure. This multi-stage process aimed to rectify issues arising from the scraping of HTML content and create a standardized foundation for subsequent analysis. Initial steps involved the separation of concatenated words, such as transforming "RetailBusiness" into "Retail Business," addressing anomalies likely introduced during scraping. Subsequently, we cleansed the text by eliminating characters beyond English letters, numbers, and a select set of punctuation marks (['(', ')', '-', '.', ':', ',', '?', '&']). Multiple spaces were reduced to a single space, and consecutive dots were streamlined to a singular occurrence. Tags lingering from HTML were removed, and the first sentence, along with the last two sentences, was excised to eliminate potential artifacts like copyrights or dropdown menus. Additionally, language prediction was employed to retain only English texts. Finally, the dataset underwent a transformation into a format compatible with the fasttext text classification library, paving the way for a more streamlined and effective analysis.


## Adapting the Dataset for T5 Fine-Tuning: Unleashing the Power of Fill-in-the-Blank Tasks

To harness the capabilities of the [T5 model by Hugging Face](https://huggingface.co/docs/transformers/model_doc/t5) for our specific dataset, a crucial step involved adapting the given data into a fill-in-the-blank task, aligning with the original T5 pre-training objective. This transformation starts by converting our dataset into a format conducive to the fill-in-the-blank paradigm. Subsequently, armed with the adapted data, we initiated the fine-tuning process on the pre-trained T5, leveraging the `t5-base` pre-trained model weights. The fine-tuning operation, aimed at customizing the model to the nuances of our imbalanced dataset. Notably, for each of the five least frequent classes, a distinct model was fine-tuned, exclusively trained on the texts from that industry. This deliberate approach allowed each model to grasp the unique stylistic nuances of its respective industry, ensuring that the generated text would closely mimic real-world records and enhance the model's performance on the minority classes.

<div style="text-align:center;">
  <img src="../assets/img/unsupervised-fine-tune-objective.jpeg" alt="Fine-tune paradigm" width="600"/>
  <p style="color:grey;">Fill-in-the-blank Paradigm</p>
</div>


## Unleashing T5 for Text Generation: Crafting Diverse Narratives with a Purposeful Approach

Text generation, a pivotal aspect of our project, was accomplished using the 'summarize' T5 task. To ensure the creation of rich and diverse generated text, imbued with different contexts reflective of various industries, we devised a meticulous input generation strategy. The input structure was standardized as "summarize: `<industry name>`. `<input>`," with the `<input>` component dynamically crafted through a multi-step process. Leveraging two distinct files, one housing selected keywords per industry and the other comprising randomly chosen sentences per industry, we curated each `<input>` in one of two ways:
1. With a probability of 0.5, an input based on two randomly chosen keywords was generated.
2. With an equal probability of 0.5, an input grounded in a single random sentence was crafted.

This diverse input pool was then tokenized using the T5 Tokenizer. \
The text generation itself was orchestrated through a 4-branch Beam Search, meticulously designed to enforce the constraint that no 2-gram would appear more than once in the generated text. This thoughtful and purposeful approach aimed at producing coherent and contextually varied text outputs aligned with the distinct characteristics of each industry.

## Metrics for Method Evaluation: Unveiling the Impact on Rare Industries

To discern the effectiveness of each method applied to our imbalanced dataset, a comprehensive set of metrics was employed, focusing particularly on the performance in rare industries. The evaluation criteria included:
1. F1-Micro - considers the overall model performance across all classes.
2. F1-Macro - providing an equal-weighted assessment of each class.
3. F1-Weighted - accounts for class imbalances by assigning weights proportional to class frequencies.
4. Recall per industry - offering insights into the model's ability to correctly identify instances within each specific industry.

These metrics collectively provided a nuanced understanding of the impact of various techniques on the challenging task of mitigating class imbalances, offering a holistic view of the model's performance, especially in the context of the less frequently represented industries.

<div style="text-align:center;">
  <img src="../assets/img/recall-per-industry-t5-fine-tune.jpeg" alt="Recall per industry (class)" width="800"/>
  <p style="color:grey;">Recall Per Industry (class) For Each Method</p>
</div>

<div style="text-align:center;">
  <img src="../assets/img/micro-f1-t5-fine-tune.png" alt="average f1-micro and f1-macro" width="600"/>
  <p style="color:grey;">Average F1-Micro & F1-Macro</p>
</div>

## Conclusion: Unveiling the Power of Text Generation in Dataset Augmentation

In conclusion, our exploration into balancing imbalanced datasets using T5 has yielded insightful results, particularly in the context of rare industries. The text generation method, deployed as an oversampling technique, demonstrated notable advantages in terms of recall, showcasing its efficacy in capturing instances from underrepresented classes. While the oversampling method showcased competitive performance in terms of F1-Micro and F1-Macro metrics, the text generation approach exhibited a slight edge. Notably, the text generation method not only proved to be a valuable oversampling alternative but also holds promise in fostering a more robust classifier. By exposing the model to a diverse range of examples, it has the potential to learn nuanced patterns across various industries, contributing to improved generalization. As we navigate the complex landscape of imbalanced datasets, the text generation method emerges as a versatile and potent tool for enhancing the performance and resilience of our classifiers.
