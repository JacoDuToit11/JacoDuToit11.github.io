---
title: 'Paper Summary - GSM-Symbolic: Understanding the Limitations of Mathematical Reasoning in Large Language Models'
date: 2024-08-04
# categories: ["AI Safety"]
permalink: /posts/UnderstandingLimitationsOfMathematicalReasoningInLLMs/
tags:
  - AI Safety
---

Summary of [Understanding the Limitations of Mathematical Reasoning in Large Language Models](https://arxiv.org/pdf/2410.05229).

# Introduction
Recent advances in large language models (LLMs) have led to notable improvements in mathematical reasoning tasks, particularly as assessed by the GSM8K benchmark, designed for grade-school-level math. However, the authors of this paper suggest that current benchmarks may not capture genuine reasoning ability but rather reflect LLMs' ability for pattern-matching on training data. To address this, the authors introduce GSM-Symbolic, a new benchmark built on symbolic templates that allow for better control and variation in mathematical questions. The study explores the robustness and limitations of LLMs in mathematical reasoning, focusing on performance variance across question modifications and complexity levels.

# Background
LLMs have excelled in diverse tasks from natural language processing to question answering, raising interest in their logical reasoning abilities. However, current research questions if these models engage in actual reasoning or rely on probabilistic pattern matching. Studies suggest that token biases heavily influence LLM outputs, implying that small changes in question structure can impact results disproportionately. GSM8K, the most widely used benchmark, provides a single fixed question set, which limits its capacity to gauge the true reasoning depth of LLMs, prompting the need for a new benchmark that truly measures reasoning ability.

# GSM-Symbolic
GSM-Symbolic is derived from GSM8K but enhances it with symbolic templates that systematically adjust variables and structure while maintaining logical consistency. This method produces a vast range of question variants, allowing for deeper evaluation of LLMs’ reasoning capacity. In contrast to GSM8K, GSM-Symbolic can alter question parameters, providing a distribution of performance metrics rather than a single accuracy measure. 

# Experiments and Results
The authors conducted extensive evaluations with GSM-Symbolic across multiple state-of-the-art models, with findings as follows:

1. **Variability in Performance**: Models showed significant accuracy variance across GSM-Symbolic, with declines observed for all models when questions were varied slightly. This suggests a sensitivity to training data patterns rather than true reasoning.

2. **Sensitivity to Question Elements**: Models exhibited robustness to changes in proper names but were highly sensitive to numerical alterations. For example, simply adjusting numbers in a question led to performance drops, highlighting that models may not be reasoning mathematically but following pattern-based shortcuts.

3. **Impact of Complexity**: The study tested performance against varying question complexities by altering the number of clauses in each problem. Results showed that as complexity increased, model performance dropped. This further suggested that LLMs struggle with tasks that demand more logical structuring.

4. **The GSM-NoOp Dataset**: To test true understanding, the authors introduced GSM-NoOp, a variant that includes irrelevant information within questions. Despite the irrelevant clauses, most models incorrectly incorporated them into the reasoning process, leading to performance drops of up to 65%. This failure to ignore distractions highlights a lack of genuine logical reasoning ability.

# Conclusions
The study’s results emphasize the limitations of current LLMs in mathematical reasoning tasks. GSM-Symbolic reveals that these models are highly sensitive to input variations, dependent on pattern recognition rather than logical structuring. The findings suggest that without meaningful formal reasoning, LLMs may struggle in real-world applications requiring mathematical reasoning. Moving forward, the paper underscores the importance of developing new evaluation methods and more robust AI models capable of true logical reasoning.