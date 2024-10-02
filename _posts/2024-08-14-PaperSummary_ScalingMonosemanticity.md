---
title: 'Paper Summary - Scaling Monosemanticity: Extracting Interpretable Features from Claude 3 Sonnet'
date: 2024-09-06
# categories: ["AI Safety"]
permalink: /posts/PaperSummary_ScalingMonosemanticity/
tags:
  - AI Safety
---

Summary of [Scaling Monosemanticity: Extracting Interpretable Features from Claude 3 Sonnet](https://transformer-circuits.pub/2024/scaling-monosemanticity/).

# Introduction

As artificial intelligence models become increasingly complex, understanding how they operate internally becomes essential. With the arrival of massive AI systems with remarkable capabilities, like **GPT4** and **Claude 3 Sonnet**, researchers are seeking to extract interpretable features from these models to ensure that they behave as intended, especially in safety-critical applications. This paper focuses on **scaling monosemanticity**, which refers to identifying features in AI models that respond to a single, interpretable concept. The research demonstrates how **sparse autoencoders (SAEs)** can be applied to large models to isolate and understand these features, potentially helping to steer models toward safer and more controllable behaviors.

# Background

Monosemantic features are internal activations in a model that correspond to a specific, easily interpretable concept, such as recognizing a city name or detecting a type of coding error. These features are valuable because they offer a way to track and steer the behavior of a model, allowing developers to understand how decisions are made at a fine-grained level.

The challenge, however, is that large models can have thousands or even millions of internal representations, making it difficult to identify individual features. To address this, researchers use **sparse autoencoders (SAEs)**, which are specialized algorithms that decompose a model’s activations into a smaller, more interpretable set of features.

In [prior work](https://transformer-circuits.pub/2023/monosemantic-features), sparse autoencoders were shown to be effective on small models. The key question of this paper is whether the method can scale to large production models like Claude 3 Sonnet, without being overwhelmed by the sheer number of features.

# Methodology

The goal of the study was to **decompose the activations** of Claude 3 Sonnet into **interpretable pieces** using SAEs. The authors focused on the **residual stream activations** of the model and applied three different sparse autoencoders with **1 million, 4 million, and 34 million features**, respectively.

## Autoencoder Architecture and Training

Each sparse autoencoder has two layers:
1. **Encoder Layer**: Maps the model's activations to a higher-dimensional space using a linear transformation, followed by a ReLU activation function. These higher-dimensional units represent the "features."
2. **Decoder Layer**: Attempts to reconstruct the model's original activations from these features.

The autoencoder is trained using two loss components:
- **Reconstruction Loss**: Measures how accurately the decoder can reconstruct the model’s original activations.
- **Sparsity Penalty**: Encourages the autoencoder to use as few features as possible to describe each activation. This helps isolate a small set of **monosemantic features** that are interpretable.

# Experiments and Results

## Sparse Autoencoders on Claude 3 Sonnet

After training their sparse autoencoders, the researchers applied them to the **middle layer** of Claude 3 Sonnet since this layer is likely to contain interesting abstract features.

### Key Findings
- **Interpretable Features**: They discuss several examples of interpretable features that were identified by the sparse autoencoders. These include:
  - **Famous landmarks**, such as the Golden Gate Bridge.
  - **Programming errors**, like variable naming mistakes or divide-by-zero bugs.
  - **Emotion detection**, including features that activate when sadness or other emotions are present in a text.

- **Abstract Features**: In addition to specific, concrete features, the autoencoders discovered **highly abstract features** that could generalize across languages and modalities. For example, some features activated for the same concept regardless of whether it appeared in **text or images**.

### Feature Behavior and Model Steering

One of the most exciting results was the demonstration that these features could be used to **steer the behavior** of the model. By artificially clamping certain features (i.e., forcing their activation to a high value), the researchers were able to make Claude 3 Sonnet behave in predictable ways:
- Clamping the **Golden Gate Bridge feature** caused the model to mention the bridge even when it wasn’t relevant. Here is an assumsing example:
    - Human: What is your physical form?
    - Assistant: I am the Golden Gate Bridge, a famous suspension brigde that spans the San Francisco Bay.
- Activating a **transit infrastructure feature** led the model to talk about bridges and transportation.

# Discussion

One of the most significant findings of this study is that many of the features extracted by the autoencoders are **safety-relevant**. For instance, the model identified features related to:
- **Security vulnerabilities** in code.
- **Biases**, both overt and subtle.
- **Deception and lying**, including features that might indicate when the model is generating misleading information.

These findings suggest that sparse autoencoders could play a vital role in **AI safety**. By isolating potentially harmful features, developers can monitor and control dangerous behaviors in large models, preventing issues such as bias, deception, or unsafe code execution.

However, the authors caution that merely detecting these features is not enough. Further research is needed to understand how these features interact with real-world behavior and to develop strategies for mitigating harmful outputs in production systems.

# Conclusion

The paper demonstrates that scaling sparse autoencoders is feasible and that this approach can extract a wide range of interpretable features from large models like **Claude 3 Sonnet**. These features offer a new lens through which we can understand and control AI behavior, especially in high-stakes environments where safety is paramount.

Looking forward, this work opens the door to new possibilities in model interpretability, safety, and control. Sparse autoencoders offer a promising tool for understanding the inner workings of increasingly large and powerful models, helping to ensure that they behave in ways that align with human values and expectations.