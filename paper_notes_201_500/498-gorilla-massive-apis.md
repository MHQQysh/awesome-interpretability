# 498. Gorilla: Large Language Model Connected with Massive APIs

- **Authors:** Shishir G. Patil, Tianjun Zhang, Xin Wang, Joseph E. Gonzalez
- **Venue / year:** arXiv 2023 (arXiv:2305.15334)
- **Paper:** [arXiv:2305.15334](https://arxiv.org/abs/2305.15334)
- **Tags:** `tool-use` `API-calling` `retrieval` `hallucination`

## Introduction
LLMs can write plausible code but often call a nonexistent API, choose the wrong library, or supply invalid arguments. This problem becomes harder when the available tool set is large, overlapping, and changes after training; putting every API description into the prompt is impossible. Gorilla asks whether a small open model can learn reliable API invocation and adapt to changing documentation through retrieval.

## Method / Framework
APIBench is built from 1,645 API calls: 94 Torch Hub calls, 626 TensorFlow Hub v2 calls, and 925 selected HuggingFace models. The authors convert model cards into structured records containing domain, framework, functionality, API name/call, arguments, requirements, examples, performance, and description. GPT-4-style self-instruction generates ten natural-language user requests per API from a small set of manually prepared examples, yielding 16,450 instruction/API pairs.

Gorilla is a retrieve-aware fine-tuned LLaMA-7B model. In zero-shot mode it sees only the user instruction; in retrieval mode BM25, GPT-Index/text-davinci-003, or an oracle retriever appends API documentation. The evaluation parses the generated code and uses AST subtree matching: a call is correct when the target API call appears as the right functional subtree, while hallucination means the generated call is not a subtree of any API in the database. A second task adds constraints such as minimum ImageNet accuracy or parameter count.

## Baselines / Comparisons
Baselines are LLaMA-7B, GPT-3.5, GPT-4, and Claude under zero-shot, BM25, GPT-Index, and oracle retrieval. The study also compares Gorilla trained without retrieval to Gorilla trained with a ground-truth retriever, then evaluates these models with imperfect retrievers. This isolates model fine-tuning from retrieval quality and distinguishes choosing the right API from writing the right call.

## Experiments / Findings
In zero-shot API-call accuracy, Gorilla scores 59.13/71.68/83.79 on TorchHub/HuggingFace/TensorFlow Hub, versus GPT-4's 38.70/19.80/18.20 and GPT-3.5's 48.38/16.81/41.75. Gorilla's hallucination rates are 6.98/10.95/5.40, substantially below GPT-4's 36.55/37.16/78.65 in the same three settings. The paper reports Gorilla's best zero-shot result as 20.43 percentage points above GPT-4 overall and 10.75 above GPT-3.5 in its aggregate comparison.

With oracle retrieval, Gorilla reaches 67.20% TorchHub, 91.26% HuggingFace, and 94.16% TensorFlow Hub accuracy. Non-oracle retrieval can hurt: adding BM25 at test time to a model not trained with retrieval drops accuracy by 21.50 points on TorchHub and 47.57 on HuggingFace in the reported comparison. Training with a ground-truth retriever improves TorchHub by 12.37 points and HuggingFace by 23.46 points over training without retrieval, but GPT-Index and BM25 still leave a large gap to oracle.

Constraint-aware accuracy is lower for every model. Gorilla is strongest without a retriever at 47.88%, while with BM25/GPT-Index/oracle it reaches 30.28/26.76/67.60%; GPT-3.5 reaches 43.66/33.80/33.09/69.01. Thus the model can reason over API-card constraints, but retrieval quality remains a bottleneck.

## Ablation / Error Analysis
The retrieval ablation shows that retrieval is not automatically beneficial: irrelevant documentation can misguide the generator. The API-change experiment shows the benefit of retrieve-aware training because the model can follow updated documentation rather than memorized calls. Error analysis separates hallucinated tools from selecting the wrong existing API; GPT-4 and GPT-3.5 often hallucinate nonexistent model names or local paths, while Gorilla greatly reduces this failure.

## Limitations
APIBench focuses on linear ML API calls and AST-level functional correctness, not real execution, security, side effects, or multi-step tool plans. Dataset instructions are synthetic and model-card quality is uneven. Gorilla's results depend on the retriever, API documentation freshness, and the LLaMA-7B domain, so the paper does not establish general reliability for arbitrary web APIs or high-risk actions.

## Two-sentence summary
Gorilla combines self-instruction fine-tuning with API-document retrieval so a LLaMA-7B model can choose and invoke tools with fewer hallucinations. It beats GPT-4 on the paper's APIBench AST metric, but the retrieval ablations show that a bad retriever can erase the gain and that constraint-aware, real-world tool use remains difficult.

## Evidence note
Read the full arXiv text, including APIBench construction, AST metric, zero-shot/retriever tables, retriever-aware training ablation, changing-document experiment, constraint table, and limitations/social-impact discussion.

