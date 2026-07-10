# 145. Enabling Large Language Models to Generate Text with Citations

- **Authors:** Tianyu Gao, Howard Yen, Jiatong Yu, Danqi Chen
- **Venue:** EMNLP 2023
- **Paper:** https://aclanthology.org/2023.emnlp-main.398
- **Tags:** Citation, Retrieval, Factuality, Benchmark, ALCE

## Introduction

LLM 的回答流畅但容易 hallucinate，用户没有 supporting evidence 就很难核验。商业生成式搜索系统已经提供引用，却依赖闭源检索和人工评估，难以复现。论文提出 ALCE（Automatic LLMs’ Citation Evaluation），把“检索证据、生成答案、为每条陈述附引用”作为可复现的端到端任务。

## Method / Framework

ALCE 覆盖 ASQA、QAMPARI、ELI5 三个数据集，检索 corpus 分别为 Wikipedia 或 Sphere。系统给问题检索 passages，再生成带 [1][2] 引用的答案。评价分三维：

- fluency，用 MAUVE；
- correctness，用 answer / claim recall 等 NLI 派生指标；
- citation quality，用 citation recall（每条陈述至少有支持证据）和 citation precision（引用是否相关且不冗余）。

论文还比较 VANILLA、SUMM / SNIPPET、INTERACT、INLINE SEARCH、RERANK、POST CITE 和 CLOSED BOOK。SUMM / SNIPPET 用 ChatGPT 压缩 passage 以适应 context；INTERACT 允许模型主动检查全文；RERANK 从多次回答中按 citation recall 选最优。

## Baselines / Comparisons

模型包括 ChatGPT、GPT-4、LLaMA、Alpaca、Vicuna、LLaMA-2-Chat，且和 closed-book、商业或既有 retrieval pipeline 对比。数据覆盖 ambiguous factoid、list question 和 why/how/what 长答案，指标明确区分“答案看起来正确”和“每个 claim 真被引用支持”。

## Experiments / Findings

- ELI5 上即使最好系统也有约 50% 的回答缺少完整 citation support，说明当前引用生成远未达到可靠搜索助手标准。
- ASQA 上 ChatGPT VANILLA（5 passages）citation recall / precision 为 73.6 / 72.5；加入 rerank 后提高到 84.8 / 81.6。GPT-4 5 passages 为 68.5 / 75.6，20 passages 为 73.0 / 76.5。
- VANILLA 这种简单“把检索 passages 放进 prompt”已经接近最强 prompting；SUMM / SNIPPET 通常提高 correctness，但压缩损失会降低 citation quality。
- INLINE SEARCH 没有改善表现，模型不会稳定地在生成过程中选择何时搜索；INTERACT 也没有带来稳定收益，说明当前 LLM 还不擅长多步工具使用。
- 更多 context 对 ChatGPT 不一定有帮助，但 GPT-4 受益；top-k 检索质量和 context window 是主要瓶颈。LLaMA-2-Chat 比基础 LLaMA 更能利用引用格式。

## Ablation / Error Analysis

检索器、passage 数量、summary / snippet 压缩、interactive check、reranking 和 post-hoc cite 是主要消融。POST CITE 可以提高引用覆盖，却可能把答案和证据的生成时对应关系断开；top-1 passage 自己作为答案能取得高 citation 分数，但 fluency 差，使用前两句又因 coverage 低而 correctness 差，说明 benchmark 对 shortcut 有防护。

## Limitations

ALCE 的 correctness 主要依赖 TRUE NLI 和 GPT-3 生成 sub-claims，自动指标仍可能与人类判断错位。检索 corpus、context window 和模型版本会影响可复现性；citation precision 的“冗余引用”规则也不等同于所有用户偏好。论文没有解决开放网页的时效性、来源可信度和跨段综合事实。

## 两句话总结

ALCE 把带引用的 LLM 生成变成同时考察流畅性、事实正确性和引用质量的端到端 benchmark。实验显示检索质量和上下文长度比复杂 prompt 更关键，而当前模型在完整、逐句可验证的 citation support 上仍有很大缺口。
