---
layout: default
title: DSC 180A Methodology Assignment 4
---

# DSC 180A Methodology Assignment 4

**Name:** Kenji Gunawan

**UCSD email:** kegunawan@ucsd.edu

**Section:** Quantization of ML Models

**Mentor:** Arya Mazumdar

**1. What is the most interesting topic covered in your domain this quarter?**

The most interesting topic to me is how different quantization strategies lead to different error rates. In particular, [KIVI's finding that keys benefit from per-channel quantization while values benefit from per-token quantization](https://proceedings.mlr.press/v235/liu24bz.html) shows why we need to consider the original distribution instead of treating all cached tensors the same way. I also find it interesting that lower reconstruction error does not necessarily translate into lower attention error. This suggests that preserving individual values accurately is not enough: we also need to consider how those values interact with the queries and contribute to the attention output. It makes me interested in evaluating quantization in the context of the full attention computation, rather than relying on reconstruction error alone.

**2. Describe a potential investigation you would like to pursue for your Quarter 2 Project.**

For my Quarter 2 Project, I would like to investigate how quantizing the KV cache affects inference quality, especially whether differences in reconstruction and attention error predict differences in actual task performance. I would compare an unquantized KV-cache baseline with several bit widths and quantization strategies while keeping the model weights and evaluation settings fixed. To keep the project manageable, I would begin with one model and two types of tasks: question answering and long-context retrieval. I would measure task accuracy alongside KV-cache memory usage, and vary context length to see when quality begins to deteriorate. I would also examine examples where a method has low reconstruction error but produces an incorrect answer. My goal would be to identify which measurements best predict quality loss and which quantization settings offer a useful balance between memory savings and reliable inference.

**3. What is a potential change you’d make to the approach taken in your current Quarter 1 Project?**

A change I would make to our Quarter 1 approach is to evaluate models from additional families, such as DeepSeek and Llama, instead of focusing only on Qwen. This would help us determine whether our observations generalize or depend on the particular model we tested. I would start with small checkpoints that fit our available hardware and compare each quantized model against its own unquantized baseline. I would use the same evaluation examples, context lengths, and quantization settings wherever the implementations support them. I would also document differences in model size and attention architecture, since those could affect the comparison. This broader evaluation would make our conclusions more convincing and help us identify cases where a quantization strategy needs to be adapted to a particular model.

**4. What other techniques would you be interested in using in your project?**

I would be interested in trying mixed-precision KV-cache quantization, where more sensitive layers or attention heads receive more bits while less sensitive ones use fewer. I would first measure sensitivity by quantizing one component at a time and observing the change in attention output and task accuracy. Another technique I would explore is keeping unusually large values in higher precision while quantizing the remaining values more aggressively. I would also consider selecting quantization settings using attention-output error on a small calibration set, then evaluating those settings on separate held-out examples. I would compare these approaches under a similar total cache-memory budget, including any extra storage they require, to determine whether they use the available memory more effectively than a uniform bit-width baseline.
