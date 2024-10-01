---
title: 'Paper Summary: Improving Alignment and Robustness with Circuit Breakers'
date: 2024-08-06
# categories: ["AI Safety"]
permalink: /posts/PaperSummary_ImprovingAlignmentRobustnessCircuitBreakers/
tags:
  - AI Safety
  - Representation Engineering

---

In this post, I summarize [Improving Alignment and Robustness with Circuit Breakers](https://arxiv.org/abs/2406.04313).

# Introduction
As AI systems, especially large language models (LLMs), continue to advance, their potential for both benefit and harm grows. These models have demonstrated remarkable capabilities across a wide range of applications, but they are not without their flaws. One of the key issues is their vulnerability to adversarial attacks—attacks that can trick AI systems into generating harmful or unethical content.

Existing approaches such as refusal training and adversarial training aim to mitigate these risks by teaching the models to reject harmful prompts or to resist adversarial inputs. However, these techniques have limitations. Refusal training, while somewhat effective, can be bypassed by carefully crafted attacks. Adversarial training, on the other hand, can be resource-intensive and often fails to generalize beyond the specific attacks it was trained on. In other words, neither of these methods provides the level of robustness needed for real-world deployment of AI systems in safety-critical applications.

To address these shortcomings, the authors of this paper propose a novel method called **circuit breaking**. Inspired by techniques in representation engineering, circuit breakers operate by directly controlling the internal model representations that lead to harmful outputs. This approach promises to prevent harmful outputs in a more general and robust way, without sacrificing the model’s utility or requiring extensive retraining.

# Methodology
The main contribution of this paper is the introduction of **circuit breakers**, which prevent harmful outputs by intervening in the internal representations of the model. Instead of focusing on the input or output stages, this method "short-circuits" the harmful processes that occur within the model itself.

Circuit breakers work by altering the model’s **representation space**—the internal layers of neural networks where information is processed and transformed. The key idea is to reroute representations responsible for harmful outputs to neutral or incoherent states, preventing the model from completing harmful generations.

The method is implemented through a process called **Representation Rerouting (RR)**, which makes use of two main datasets:

1. **Circuit Breaker Set**: Consists of harmful examples that are used to trigger the circuit breaker mechanism.
2. **Retain Set**: Contains safe examples that should not trigger the circuit breaker. It ensures that the model retains its ability to generate benign and useful outputs.

The training process aims to remap harmful representations in the **Circuit Breaker Set** to safe or incoherent states using a combination of two loss functions: **rerouting loss** and **retain loss**. The **rerouting loss** alters the harmful representations by minimizing the cosine similarity between harmful representations and their corresponding rerouted versions. However, they use a ReLU function to push the rerouted representations away in an orthogonal direction (Cosine Similarity = 0) such that inputs which trigger these represenations produce irrelevant outputs. This ensures that harmful processes are effectively "short-circuited" without disrupting the model’s normal functionality. The **retain loss**, on the other hand, works by ensuring that the representations in the **Retain Set** are preserved, maintaining the model’s ability to generate useful outputs. 

# Experiments and Results

## Large Language Models
The authors applied their circuit-breaking technique to two widely used open-source models: **Mistral-7B** and **Llama-3-8B**. Both models were tested under a variety of adversarial conditions, including:

- **Gradient-based attacks**.
- **Representation-space attacks**, which directly target the internal structure of the model.
- **Prefilling attacks**, where harmful completions are "pre-filled" into the model’s output.

The results were highly promising:
- Circuit breakers **reduced the success rate of adversarial attacks by up to 90%** in some cases.
- The models retained their performance on standard benchmarks such as **MT Bench** and **MMLU**, demonstrating that the method did not compromise their overall capabilities.
- The **attack success rate (ASR)** dropped significantly across all attack types, including **manual**, **multilingual**, and **embedding attacks**, showing the generalizability of this method to various adversarial strategies.

## Multimodal Models
The authors also applied circuit breakers to **multimodal models**, which combine text and image inputs. Multimodal systems are particularly vulnerable to adversarial image-based attacks, where carefully designed images can cause models to generate harmful outputs.

In the experiment, the model was tested against adversarial image attacks using **Projected Gradient Descent (PGD)**. Circuit breakers reduced the harmful compliance rate by **84%**, while the model's performance on multimodal benchmarks like **MMMU** and **LLaVA-Wild** remained unaffected. This demonstrates that circuit breakers can be applied effectively to multimodal systems without sacrificing their utility.

## AI Agents
The authors also extended circuit breakers to AI agents—systems that can interact with external tools and perform tasks like browsing the web or making function calls. These agents pose additional risks because they can take harmful actions, such as sending malicious messages or executing harmful code.

To test this, the authors developed a **function-calling safety benchmark** and evaluated the agents under both normal and adversarial conditions. Circuit breakers reduced the rate of harmful actions by **84%**, even in settings where the agent was forced to make a function call. This suggests that circuit breakers could play a key role in controlling the behavior of AI agents in adversarial environments.

# Conclusion
The introduction of circuit breakers marks a significant step forward in improving AI alignment and robustness. Unlike traditional methods that focus on filtering inputs or supervising outputs, circuit breakers operate on the internal processes that lead to harmful outputs. This approach offers several key advantages:

- **Generalization**: Circuit breakers are attack-agnostic, meaning they can defend against a wide range of adversarial attacks, including those unseen during training.
- **Efficiency**: The method does not require extensive retraining or fine-tuning, making it more practical than adversarial training.
- **Scalability**: Circuit breakers can be applied to a variety of AI models, from large language models to multimodal systems and AI agents.

The results demonstrate that circuit breakers can significantly reduce the likelihood of harmful outputs while preserving the model’s performance on standard tasks. By addressing the root causes of harmful behaviors in AI systems, circuit breakers offer a promising new avenue for improving the safety and reliability of AI technologies.