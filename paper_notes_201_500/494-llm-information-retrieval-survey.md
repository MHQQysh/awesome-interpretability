# 494. Large Language Models for Information Retrieval: A Survey

- **Authors:** Yutao Zhu, Huaying Yuan, Shuting Wang, Jiongnan Liu, Wenhan Liu, Chenlong Deng, Haonan Chen, Zheng Liu, Zhicheng Dou, Ji-Rong Wen
- **Venue / year:** arXiv 2023, updated version v5
- **Paper:** [arXiv:2308.07107](https://arxiv.org/abs/2308.07107)
- **Tags:** `survey` `information-retrieval` `query-rewriting` `reranking` `search-agent`

## Introduction
Information retrieval is moving from term matching and neural relevance modeling toward systems in which an LLM rewrites queries, retrieves or reranks documents, reads evidence, and acts as a search agent. The survey addresses how to combine traditional fast retrieval with LLM semantic understanding without losing interpretability, efficiency, factuality, or controllability. It focuses on text retrieval where query-document relevance is scored, rather than treating every generation task as IR.

## Method / Framework
The survey decomposes an LLM-enhanced IR system into query rewriter, retriever, reranker, reader, and agent layers. For query rewriting it organizes prompting, supervised fine-tuning, and interactive/feedback-driven methods. For retrieval it distinguishes dense encoders, generative retrievers, LLM-based embedders, knowledge distillation, and prompt-based URL/document generation. For reranking it separates supervised encoder/encoder-decoder/decoder models from unsupervised pointwise, listwise, and pairwise prompting, data augmentation, and reasoning-intensive methods.

The reader layer covers passive readers such as RAG and REALM as well as active readers that retrieve, verify, and revise evidence. The search-agent section extends the pipeline into planning, tool use, multi-step retrieval, multi-agent coordination, web interaction, and research/code agents. Evaluation is organized around retrieval metrics such as precision, recall, MRR, MAP, and nDCG, plus answer accuracy, faithfulness, citation/source validity, and human preference for generated responses.

## Baselines / Comparisons
Historical baselines include Boolean and term-based retrieval, vector-space models, language models, BM25, dense neural retrievers, and conventional index-retrieve-rank pipelines. The survey compares LLM methods such as RepLLaMA, TART, DSI, LLM-URL, monoT5, RankT5, RankLLaMA, RankGPT, PRP, and many prompt/data-augmentation variants. Generative retrieval is contrasted with indexed retrieval: it removes a separate index by storing document identifiers in model parameters, but must handle corpus scale, valid DocID decoding, and updates.

## Experiments / Findings
This is a literature survey, so its tables summarize methods and properties rather than reporting a new common benchmark. A recurring finding is that LLMs can add semantic understanding and instruction-following to every IR stage, but an LLM is not automatically a fast retriever: model size and inference latency are often in tension with first-stage recall requirements. Generative retrievers benefit from larger models and synthetic queries, yet scaling to very large document collections remains difficult.

The reranking synthesis shows a progression from supervised mono encoders to listwise/pairwise LLM prompting and reasoning-aware ranking. Reader and agent work increases answer usefulness by combining retrieval with summarization, verification, and planning, but it also transfers error sources from ranking to query formulation, evidence selection, generation, and tool execution. The survey emphasizes that answer faithfulness or citation presence must not be confused with relevance ranking alone.

## Ablation / Error Analysis
The survey's main ablations are the repeated comparisons of component choices: prompt versus fine-tuning, dense versus generative retrieval, pointwise/listwise/pairwise reranking, passive versus active reading, and single-step versus agentic search. It repeatedly identifies API cost, long inference time, domain mismatch, synthetic-query mismatch, context overload, position bias, inconsistent relevance judgments, citation/source bias, hallucination, and evaluation leakage as failure modes. A retrieved passage can still bias or confuse the generator, and a fluent answer can remain unsupported.

## Limitations
The field changes faster than a survey can stabilize: the current local text is an updated v5, while many methods and conclusions evolve after the original 2023 release. Tables organize published evidence but do not constitute a controlled meta-analysis, and method names can hide different datasets, backbones, retrieval depths, and judge protocols. Runtime, licensing, privacy, and index freshness are often underreported in academic comparisons.

## Two-sentence summary
The survey presents LLM-enhanced IR as a modular trajectory from query rewriting and retrieval through reranking, reading, and autonomous search agents. Its central lesson is that LLMs improve semantic and reasoning interfaces but introduce cost, latency, bias, hallucination, and evidence-faithfulness problems that require component-level evaluation.

## Evidence note
Read the full local arXiv text, including the query-rewriting, retriever, reranker, reader, search-agent, evaluation, bias, and future-direction sections and comparison tables.

