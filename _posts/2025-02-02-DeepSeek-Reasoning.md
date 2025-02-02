---
title: How DeepSeeks learns reasoning capability
layout: post
---

It first learns simple rules exactly right, then complex combinatorial thinking process.

1. Pattern matching in a memory-economical way ("attention latent vector"). A pattern is a short piece of text, for example "United Kingdom" as in "Lorem ipsum dolor sit amet United Kingdom consectetur adipiscing elit".

2. Learn the basic rules exactly right ("Finetuning"), like 2 + 3 = 5.

3. Optimise the thinking process towards the correct one for more complex problems ("GRPO Policy Optimisation"). It generates many candidate thinking processes, and gives the correct one higher score.

It does the Finetuning and Policy Optimisation twice over.

![DeepSeek-R1 training](/assets/2025-DeepSeek-R1/deepseek-training.png)


### References
DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning [https://arxiv.org/abs/2501.12948](https://arxiv.org/abs/2501.12948)


