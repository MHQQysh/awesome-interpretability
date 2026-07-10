# 368. PowerInfer: Fast Large Language Model Serving with a Consumer-grade GPU

- **Authors:** Yixin Song, Zeyu Mi, Haotong Xie, Haibo Chen
- **Venue / year:** ACM SOSP 2024
- **Paper:** https://arxiv.org/abs/2312.12456
- **Tags:** `llm-serving` `activation-sparsity` `neuron-locality` `gpu-cpu-offload`

## Introduction
Running large LLMs locally improves privacy, customization, and cost, but a consumer GPU cannot hold hundreds of billions of parameters and PCIe transfers make layer-wise CPU/GPU offloading slow. PowerInfer observes that neuron activation follows a skewed power law: a small set of hot neurons is repeatedly used, while cold neurons depend on the current input.

The paper is interpretability-adjacent rather than a rationale method: activation locality provides a concrete behavioral explanation for why sparse, hybrid serving can work. The system uses this observation to decide what belongs on the GPU and what can stay on the CPU.

## Method / Framework
PowerInfer profiles activation frequency offline, places hot neurons on the GPU and cold neurons on the CPU, predicts input-dependent activations online, and computes only selected neurons with neuron-aware sparse operators. Predictor size is adapted to layer sparsity/skewness. An integer-linear-programming placement policy considers activation frequency, inter-layer communication, GPU memory, and CPU/GPU bandwidth.

The implementation extends llama.cpp with about 4,200 lines of C++/CUDA and adds an offline profiler/solver. It supports OPT 7B-175B, Llama2 7B-70B, Falcon-40B, and consumer GPUs such as RTX 4090 and RTX 2080Ti. Table 1 reports 97% MLP sparsity for OPT-30B, 43% for Llama2-13B, and 53% for Yi-34B.

## Baselines / Comparisons
The main baselines are llama.cpp and SpecInfer, with comparisons to vLLM/A100 for server-grade hardware. Ablation baselines progressively add predictors/operators, hybrid execution, and neuron placement policy. Sparse-kernel comparisons include PyTorch sparse operators and PIT on GPU.

The paper also compares quantized and non-quantized models, PC-High and PC-Low hardware, different input lengths, batch sizes, ReLU versus SwiGLU models, and model accuracy on MMLU, TruthfulQA, and related tasks.

## Experiments / Findings
On a single RTX 4090, PowerInfer averages 13.20 tokens/s for quantized models and 8.32 tokens/s for non-quantized models, reaching 29.08 and 16.06 peak values in the reported settings. It improves over llama.cpp by up to 8.00x for quantized and 11.69x for non-quantized serving; OPT-30B reaches 82% of an A100's generation rate. On PC-Low the average speedup is about 4.71x, despite only 11GB GPU memory.

For long inputs, speedup is smaller during prefill because many neurons are active, but generation still benefits from hybrid sparsity. For batches up to 32, PowerInfer remains faster, with the reported batch-32 speedup around 4.38x. Accuracy loss is negligible across model sizes/tasks, and the predictor overhead is less than 10% of total inference time.

## Ablation / Error Analysis
The stepwise performance breakdown shows predictors plus neuron-aware operators yield 1.87x/3.32x speedups for Bamboo-7B, while adding the hybrid engine and placement policy increases gains to 2.60x/7.80x in the reported settings. This isolates the value of sparse kernels from the value of choosing where neurons run.

Failure regimes are also clear: lower activation sparsity in SiLU/SwiGLU models gives only about 1.47-1.7x speedup, long prompt prefill reduces gains, and predictor mistakes can cause small accuracy fluctuations. PowerInfer's effectiveness is therefore tied to sparsity, memory bandwidth, and batch size rather than being an unconditional serving improvement.

## Limitations
The system requires offline profiling and a solver policy, and CPU compute can become the bottleneck for cold neurons. It is optimized for batch-one/local workloads, not necessarily data-center throughput. The method relies on predictable natural activation sparsity, so dense or low-sparsity architectures obtain smaller benefits, and skipped neurons remain a potential source of accuracy drift.

## 两句话总结
PowerInfer 利用 LLM 推理中少量 hot neurons 高频激活的局部性，把 hot neurons 放到 GPU、cold neurons 放到 CPU，并用在线预测器和 neuron-aware sparse operator 避免整层搬运。它在单张 RTX 4090 上大幅超过 llama.cpp 且几乎不损失准确率，但长 prompt、低稀疏模型和 CPU 冷神经元会明显削弱收益。

## Evidence note
本笔记依据 SOSP 2024/arXiv v2 全文逐节核对；速度、稀疏率和准确率结论按正文摘要、表 1、表 7/8 记录。
