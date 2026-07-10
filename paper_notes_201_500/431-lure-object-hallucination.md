# 431. Analyzing and Mitigating Object Hallucination in Large Vision-Language Models

- **Authors:** Yiyang Zhou, Chenhang Cui, Jaehong Yoon, Linjun Zhang, Zicheng Zhang, and others
- **Venue / year:** ICLR 2024
- **Paper:** https://arxiv.org/abs/2310.00754
- **Tags:** `LVLM` `object-hallucination` `post-hoc-correction` `CHAIR` `LURE`

## Introduction
本文把 LVLM 的幻觉具体化为 object hallucination：生成描述中出现图像不存在的物体，或遗漏关键物体/属性。作者不把问题只归因于“模型看不清”，而是先问幻觉在自回归生成中为什么会系统性出现：训练数据中的物体共现偏差会把不存在的物体带入答案；高不确定性物体更容易被错误预测；错误还会随生成位置向后累积。因此论文的核心目标不是重新训练一个更强的视觉编码器，而是做一个可以接在任意 LVLM 后面的轻量级 post-hoc revisor。

## Method / Framework
方法 LURE（LVLM hallcUination REvisor）类似去噪自编码器。训练时先用 COCO/LLaVA-150k 的正确描述作为目标，再用 GPT-3.5 构造更接近真实错误的 hallucinatory description：加入可能与场景物体共现的虚假物体，并把高熵物体、句子后部物体替换为 `[IDK]`。然后微调一个 LVLM revisor，把被污染的描述恢复为正确描述。

推理时，原 LVLM 先生成描述；LURE 读取候选名词的 token entropy，并根据位置阈值把高不确定性或靠后的物体 mask 成 `[IDK]`，再让 revisor 结合图像重新生成。这个设计把“哪些词需要重新检查”显式编码进输入，而不是无条件重写整段文本，因而可以与 MiniGPT-4、LLaVA、MMGPT、LLaMA-Adapter、mPLUG-Owl 和 InstructBLIP 等不同 LVLM 组合。

## Baselines / Comparisons
实验在 MSCOCO 2014 的 5,000 张测试图像上进行，比较 Original、Teacher（用 BLIP-2 短描述提供上下文）、CoT（先列物体再描述）、Greedy Decoding、GPT-Ensemble 和 GPT-Teacher。指标包括 CHAIR-I/CHAIR-S、BLEU、CLIP score，以及人工和 GPT-3.5 排名；模型覆盖六个开源 LVLM，而不是只在一个 backbone 上展示效果。

## Experiments / Findings
LURE 在六个 LVLM 上都降低了物体幻觉，并在自动 CHAIR、GPT 评测和人工评测中超过上述强基线。论文的解释是，Teacher/CoT/GPT-Teacher 仍然会发生错误传播：如果短描述或物体清单错了，后续长描述会沿着错误继续展开；LURE 则直接针对高风险物体做重审。作者还在 MME、其他图像数据和不同 revisor backbone 上检查泛化，结论是方法不依赖单一 LVLM。

## Ablation / Error Analysis
消融分别去掉共现物体、uncertainty mask 和 position mask，结果显示三类信号共同训练出的 revisor 最好；只用普通随机/通用噪声不能同样逼真地模拟 LVLM 的错误。作者也比较不同 revisor backbone，说明 LURE 对 backbone 有一定敏感性但整体趋势稳定。错误分析集中在三类原因：物体检测/名词匹配不完整、entropy 不能完美代表视觉错误，以及后处理可能删掉本来正确但低概率的物体。

## Limitations
训练数据中的幻觉由 GPT-3.5 合成，真实错误分布可能更复杂；`[IDK]` 的 noun matching 也依赖词法识别。CHAIR 只覆盖 80 个 COCO 物体，不能完整评价细粒度属性、关系和开放词汇；revisor 需要额外模型推理和微调成本，且对长描述、复杂关系和视觉证据不足的场景仍可能改错。

## 两句话总结
LURE 先用共现、生成不确定性和位置三种统计规律制造逼真的幻觉输入，再训练一个 revisor 在推理时重建更可信的视觉描述。它的关键贡献是把 LVLM 幻觉从“整段答案是否正确”变成“哪些物体需要被重新核验”，并在多个开源 LVLM 上用 CHAIR、GPT 和人工评测验证了这一后处理路线。

## Evidence note
已逐段读取本地 PDF，核对了三因素分析、LURE 训练/推理算法、COCO 与 LLaVA-150k 设置、六个 LVLM、基线和人工/GPT/CHAIR 评测，以及模块与 backbone 分析。
