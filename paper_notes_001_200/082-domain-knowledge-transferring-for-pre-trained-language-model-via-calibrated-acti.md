# 082. Domain Knowledge Transferring for Pre-trained Language Model via Calibrated Activation Boundary Distillation

> 人工精读笔记：DoKTra。重点核对了知识转移框架、任务基线、表 2--7 和组件消融。

## 0. 论文信息

- **作者**：Dongha Choi, HongSeok Choi, Hyunju Lee
- **主题**：领域知识转移、知识蒸馏、激活边界、模型校准
- **来源**：[ACL 2022](https://aclanthology.org/2022.acl-long.116)
- **代码**：[DoKTra](https://github.com/DMCB-GIST/DoKTra)
- **任务**：生物医学、临床和金融文本分类

## 1. Introduction：要解决什么问题

领域 PLM（如 BioBERT、FinBERT）通常靠大量领域语料继续预训练获得优势，但这种流程昂贵，而且新领域模型出现后，已有通用模型或下游 student 需要重新预训练。普通知识蒸馏主要匹配 teacher/student 的输出概率或 hidden representation，容易把 teacher 的过度自信和不可靠概率一并传给 student。

作者提出的问题是：能否利用一个领域 teacher 的知识，把一个规模更小或更通用的 student 快速迁移到领域任务，同时避免额外领域预训练？核心想法是先校准 teacher 的置信度，再蒸馏 teacher hidden representation 中的激活边界，而不是只蒸馏 logits。

## 2. Method / Framework：怎么解决

### 2.1 Calibrated Teacher Training（CTT）

teacher 先在领域/任务数据上 fine-tune。随后加入 confidence penalty / entropy regularization，使输出分布不至于过度尖锐。直观上，CTT 让 teacher 的监督信号“可蒸馏”：正确类仍应占优，但非正确类的概率不再被不合理地压到接近零。

### 2.2 Activation Boundary Distillation（ABD）

对 teacher 和 student 的 [CLS] hidden representation，作者关注每个维度激活的正负边界。激活指示函数把 x>0 视为 active、否则 inactive；distillation loss 迫使 student 复现 teacher 的 activation pattern。相比 L2/KL 直接拟合数值，ABD 只传递决策边界附近的结构，理论上更能保留任务判别形状且不要求 student 完全复制 teacher 的尺度。

### 2.3 训练流程

student 先 fine-tune，再进行 calibrated teacher training 和 activation boundary distillation；损失在 distillation 与 refinement 之间通过 gamma 切换/平衡。最终 student 用 task classification loss 加上边界迁移目标训练。框架的关键不是单一 loss，而是“先校准 teacher，再迁移 activation boundary”这一组合。

## 3. Baseline / 对比基线

- BERT-ft、ALBERT-ft、RoBERTa-ft：只对 student 做任务 fine-tuning，检验蒸馏是否真的带来收益。
- BioBERT-ft、RoBERTa-PM-ft：领域/更强预训练模型作为 teacher 或直接下游模型，提供领域知识上限和现有强基线。
- KLD：标准 KL-divergence output distillation，检查 ABD 是否优于只拟合输出分布。
- TAPT：Task-Adaptive Pre-Training，代表“继续用任务数据预训练”的常见知识适配方案。
- DoKTra-CTT、完整 DoKTra：分别移除 calibrated teacher training 或 activation-boundary 组件，构成机制对照。

这里的重点不是引入一个更大的 teacher，而是比较“直接 fine-tune、输出蒸馏、任务自适应预训练、激活边界蒸馏”四条知识迁移路线。

## 4. Experiments / Findings：结果如何读

### 4.1 五个生物医学/临床任务

数据包括 ChemProt、GAD、DDI、i2b2 和 HoC，指标主要是 micro-F1/F1。表 2 显示 DoKTra 在 ALBERT 和 RoBERTa student 上平均性能稳定提升：ALBERT-DoKTra 平均 F1 约 79.02，RoBERTa-DoKTra 约 80.53，并报告相对 student 规模的性能/参数效率提升。

表 3 的比较结论是：DoKTra 在五个任务总体优于普通 fine-tuned student，并在多个任务达到或超过 BioBERT、RoBERTa-PM 等领域基线；但 RoBERTa-PM 在 i2b2 上仍有优势，说明更强的领域预训练并没有被完全替代。

### 4.2 与 TAPT 的比较

TAPT 需要把任务数据再次用于预训练，论文报告在其设置下 TAPT 训练约 7 小时，而 DoKTra 约 1 小时完成。两者都是 task-specific，但 DoKTra 直接利用已有 teacher，并通过校准和 ABD 迁移知识；因此收益不仅是 F1，也包括适配成本。

### 4.3 CTT/ABD 消融

ChemProt 消融表对比 teacher-ft、+KLD、+CTT+KLD 和 +CTT+ABD。teacher fine-tune 约 76.20 F1；加入 KLD 只有小幅变化，CTT+KLD 约 76.87，而完整 CTT+ABD 约 77.42。结论是：校准本身能改善蒸馏信号，但 ABD 是进一步增益的主要来源；仅仅把输出概率蒸给 student 不足以复现 teacher 的判别结构。

### 4.4 金融领域迁移

作者用 FinBERT 作为 financial teacher，在 Financial PhraseBank 与 FinTextSen 上测试 ALBERT/RoBERTa student。表 7 中 ALBERT-DoKTra 和 RoBERTa-DoKTra 的结果超过 FinBERT-ft teacher，说明方法不只适用于 biomedical domain，也能把领域知识迁移到通用 student。

## 5. Ablation / 机制解释

主要消融有两类：去掉 CTT，检验性能是否只是来自 teacher；把 ABD 换成 KLD，检验激活边界是否比概率匹配更有用。另有与 TAPT 的成本/性能对照。结果支持如下因果链：CTT 让 teacher 的 supervision 更可靠；ABD 让 student 学到 activation pattern，而非被 teacher 的 probability scale 限制；二者组合才稳定超过普通 fine-tuning 和 KLD。

## 6. Limitations / 局限与复现注意

- DoKTra 需要领域 teacher，不能在没有任何领域模型或领域标签数据时凭空产生知识。
- 方法按任务训练，作者也承认 task-specific；换领域/换任务通常要重新蒸馏。
- 激活边界只在选定层和 [CLS] 表征上约束，未证明能完整迁移生成式或 token-level 能力。
- 论文数据规模和模型规模有限，financial 结果虽有迁移性，但不等同于通用领域适配保证。

## 7. 两句话总结

论文要解决的是昂贵的领域继续预训练和不可靠 teacher 概率蒸馏问题。DoKTra 先用置信度正则校准 teacher，再把 teacher 的 hidden activation boundary 蒸馏给 student，消融显示 ABD 比单纯 KLD 更关键，并在生物医学、临床和金融任务上以较低适配成本取得稳定收益。
