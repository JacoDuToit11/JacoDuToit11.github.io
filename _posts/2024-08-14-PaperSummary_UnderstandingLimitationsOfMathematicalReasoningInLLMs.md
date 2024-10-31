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
LLMs have excelled in diverse tasks from natural language processing to question answering, raising interest in their logical reasoning abilities. However, current research questions whether these models engage in actual reasoning or rely on probabilistic pattern matching. Studies suggest that token biases heavily influence LLM outputs, implying that small changes in question structure can impact results disproportionately. GSM8K, the most widely used benchmark, provides a single fixed question set, which limits its capacity to gauge the true reasoning depth of LLMs, prompting the need for a new benchmark that truly measures reasoning ability.

# GSM-Symbolic
GSM-Symbolic is derived from GSM8K but enhances it with symbolic templates that systematically adjust variables and structure while maintaining logical consistency. This method produces a vast range of question variants, allowing for deeper evaluation of LLMs’ reasoning capacity. In contrast to GSM8K, GSM-Symbolic can alter question parameters, providing a distribution of performance metrics rather than a single accuracy measure. 

<!-- <img title="Template Creation Process" alt="Template Creation Process" src="/images/Posts/PaperSummary_GSM/TemplateCreation.png"> -->
<!-- [Template Creation Process](https:/images/Posts/PaperSummary_GSM/TemplateCreation.png) -->

# Experiments and Results
The authors conducted extensive evaluations with GSM-Symbolic across multiple state-of-the-art models, with findings as follows:

1. **Variability in Performance**: 
Using GSM-Symbolic, the authors discovered a striking degree of performance variance for all tested models, even with minor changes to the question phrasing or numbers. For instance, one model (Gemma2-9B) had a performance gap of over 12% between its best and worst instances on the same question. Another model, Phi-3.5-mini, showed a similar variance of about 15%. The results emphasize that the accuracy reported on a single instance of a question in GSM8K may not provide a reliable measure of a model’s true capabilities, as performance on different instances can vary widely.Therefore, the current evaluation frameworks like GSM8K may not offer an accurate view of LLMs' abilities. A single metric on a fixed dataset might inflate confidence in these models' reasoning skills, while GSM-Symbolic demonstrates the need for diverse question variants to measure reliability more comprehensively.

2. **Sensitivity to Question Elements**: 
The paper's second experiment examines how robust LLMs are when superficial question elements are altered. Specifically, the authors tested three scenarios:

- Changing only the proper names in a question (e.g., switching “Alice” to “Bob”),
- Modifying only the numerical values, and
- Altering both the names and numbers simultaneously.
The findings reveal that LLMs are generally resilient to changes in proper names, with only a slight decrease in accuracy. However, when numerical values are adjusted, the models’ performance declines notably. When both names and numbers are changed, the drop is even more pronounced.

This sensitivity is a worrying sign that these models may rely more on surface-level token associations rather than on a deep understanding of mathematical structure. If LLMs truly understood the problem, they would handle numerical adjustments as effectively as they do name changes. This experiment underscores that many LLMs lack true mathematical reasoning, leaning instead on pattern-matching strategies likely rooted in training data. The drop in performance across combined changes further reveals the models' fragility and challenges in managing even minor complexity increases.

3. **Impact of Complexity**: 
To assess how question difficulty impacts LLM performance, the authors created progressively challenging variants of GSM-Symbolic questions by adding or removing clauses. Each clause represents additional information, requiring more reasoning steps to reach the solution. These difficulty levels include:

- GSM-Symbolic-M1 (a simpler variant with one clause removed),
- GSM-Symbolic (the baseline),
- GSM-Symbolic-P1 (one additional clause), and
- GSM-Symbolic-P2 (two additional clauses).
Across all models, performance consistently deteriorated with increased question difficulty, both in terms of mean accuracy and performance variance. Even a single added clause led to a measurable drop, and the variance widened as complexity increased. This trend suggests that models aren’t equipped to handle the gradual buildup of reasoning steps in complex mathematical problems, with higher variance reflecting models’ inconsistent problem-solving abilities.

The results imply that these models do not perform formal reasoning but likely apply a form of probabilistic pattern-matching. If true reasoning were involved, one would expect performance to degrade at a linear rate as the complexity of questions increased. Instead, the performance distribution shows that complexity has an exponential impact, highlighting the need for models that can generalize through genuine logical reasoning rather than data-specific pattern recognition.

4. **The GSM-NoOp Dataset**: 
The fourth part of the results addresses a critical question: can LLMs accurately identify and ignore irrelevant information in mathematical problems? The GSM-NoOp benchmark was designed to test this, where questions from GSM-Symbolic are augmented with irrelevant yet seemingly pertinent details (No-Op information) that do not impact the solution. For instance, a problem might specify that five of the items involved were “smaller than average,” a detail that does not contribute to the problem's solution.

The results from GSM-NoOp are revealing: all tested models experienced catastrophic performance drops, up to 65%, when these irrelevant details were introduced. Rather than disregarding non-essential information, models often incorporate it into their calculations. This behavior implies that LLMs interpret each detail as necessary, likely due to their training process, which emphasizes correlations over the logical structure of a problem. The heavy reliance on token-level pattern matching instead of formal reasoning leads models to erroneously integrate any mentioned detail into their solution process.

To further probe this tendency, the authors designed two additional experiments:

NoOp-Symb: In this setup, models are primed with eight examples from GSM-Symbolic, each a variation of the same question but without irrelevant information. These examples guide the model through the logical steps needed to solve the problem. However, when presented with a test question containing No-Op details, models still struggled to ignore the irrelevant information, showing that priming them with relevant examples does not substantially improve performance.
NoOp-NoOp: This variant presents the model with eight different questions from GSM-NoOp, each containing irrelevant No-Op information. Ideally, this should help the model generalize that certain types of details can be disregarded. Yet, results show that even with multiple examples, models continue to process extraneous information as relevant.
These results reveal that current LLMs lack the capacity to perform nuanced information filtering, instead interpreting each token as essential. This finding is especially concerning for mathematical reasoning, where discerning relevant information is key to accurate problem-solving. The models’ failure in NoOp-Symb and NoOp-NoOp underscores the limitations of their “reasoning” abilities, highlighting the tendency to replicate training data patterns rather than grasp logical principles.

# Conclusions
The study’s results emphasize the limitations of current LLMs in mathematical reasoning tasks. GSM-Symbolic reveals that these models are highly sensitive to input variations, dependent on pattern recognition rather than logical structuring. The findings suggest that without meaningful formal reasoning, LLMs may struggle in real-world applications requiring mathematical reasoning. Moving forward, the paper underscores the importance of developing new evaluation methods and more robust AI models capable of true logical reasoning.