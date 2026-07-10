# 085. A Causal Lens for Evaluating Faithfulness Metrics

> 人工精读笔记：EMNLP 2025。重点是评价“faithfulness metric 本身是否可靠”，而不是再提出一个只在单一任务上有效的解释分数。

## 0. 论文信息

- **作者**：Kerem Zaman, Shashank Srivastava
- **来源**：[EMNLP 2025](https://aclanthology.org/2025.emnlp-main.1496)
- **代码**：[CausalDiagnosticity](https://github.com/KeremZaman/CausalDiagnosticity)
- **任务**：fact-checking、analogy completion、object counting、multi-hop reasoning

## 1. Introduction：要解决什么问题

LLM 的自然语言解释可以很有说服力，但不同 faithfulness metric 往往被分开评估，缺少一个有 ground truth 的 testbed。若 metric 不能稳定地区分 faithful explanation 和 unfaithful explanation，就不能仅凭它的数值声称模型解释可信。

作者把问题重新定义为 **diagnosticity**：给定一对解释，metric 是否更偏好真正 faithful 的那一个？关键困难是怎样得到“有因果依据的 faithful/unfaithful pair”，而不是用随意 filler 或人工改写制造标签。

## 2. Method / Framework：怎么解决

### 2.1 Causal Diagnosticity

先对模型做知识编辑，把同一输入下模型的内部事实或答案改变；然后生成与原模型机制一致的 faithful explanation，以及仍指向旧事实/错误理由的 unfaithful explanation。因为 explanation pair 来自模型编辑前后的受控差异，评估 metric 时有更明确的 causal ordering。

对每个 pair，若 metric 分数 F1>F2 且 F1 对应 faithful explanation，则记为 diagnostic；反之为错误。框架支持 binary metric，也支持连续分数并在不同 margin/噪声设置下比较。

### 2.2 四类任务

从简单到复杂覆盖 fact-checking、analogy completion、object counting 和 multi-hop reasoning。这样可以区分 metric 是真的识别解释忠实性，还是只对某种短文本、某种答案格式有效。

## 3. Baseline / 对比方法

- **Simulatability**：把解释提供给模型或用户，观察是否能复现答案，测的是可模拟性。
- **Corruption-based CoT metrics**：扰动/删除 CoT 后检查答案变化，代表常见一致性测试。
- **CC-SHAP**：比较答案和解释的 Shapley attribution convergence。
- **Filler Tokens**：加入与任务无关的 filler，测试 metric 是否把真正有用的解释与无信息文本区分开；论文发现它在总体上表现最好。
- **Binary vs Continuous**：不是模型 baseline，而是评分形式的对照；连续分数通常比阈值化标签更有 diagnosticity，但也更容易受到噪声和模型选择影响。

## 4. Experiments / Findings：结果如何读

### 4.1 Metric 排名依赖任务和模型

不同任务上最 diagnostic 的 metric 并不固定。某个 metric 在 fact-checking 上有效，不代表它能识别 object counting 或 multi-hop explanation；同一 metric 换模型后也可能改变排序。因此“在一个 benchmark 上分数最高”不能推出它是普适 faithfulness 指标。

### 4.2 Filler Tokens 总体最稳

论文的总体结论是 Filler Tokens 在跨任务、跨模型比较中最可靠。它的优势不是复杂，而是测试目标清楚：若解释只添加无因果作用的文本，好的 faithfulness metric 不应把这些 filler 当作真正理由。

### 4.3 连续指标优于 binary，但更脆弱

连续 score 能保留解释重要性差异，通常比把解释直接判为 faithful/unfaithful 的 binary metric 更 diagnostic；但连续指标会放大小的 attribution noise、生成随机性和模型选择差异。作者因此主张未来 metric 既要连续，也要具有跨模型/任务的稳定性。

### 4.4 任务复杂度影响结论

在 fact-checking、analogy 等较短任务中，解释与答案的因果关系更容易被表面一致性近似；object counting 和 multi-hop 更容易出现解释文本正确但内部路径不一致的情况。诊断框架的价值在于把这些失败暴露出来，而不是把所有任务平均成一个数字。

## 5. Ablation / 机制解释

- **解释生成方法对照**：post-hoc explanation 与 CoT 都纳入，防止 metric 只适配一种解释形式。
- **任务/模型交叉评估**：检验 metric 是否泛化，而不是在单一 backbone 上调参。
- **连续/二元 scoring**：分离“评分信息量”与“评分稳定性”。
- **知识编辑构造 pair**：以编辑前后因果事实为锚点，替代完全启发式的错误解释生成。

## 6. Limitations / 局限与复现注意

- 知识编辑并不总能只改变一个内部事实，编辑副作用会影响 faithful/unfaithful 标签的纯度。
- Diagnosticity 仍依赖解释生成器、编辑算法和模型本身；ground truth 是操作化的，不是完整证明。
- 四个任务覆盖面有代表性但不全面，尤其没有覆盖长文本工具调用、视觉解释或真实用户决策。
- 连续指标的噪声敏感性意味着需要报告多次运行、置信区间和跨模型方差，而不能只报平均排名。

## 7. 两句话总结

论文要解决的是 faithfulness metric 也需要被严格验证，而不是默认任何解释分数都可信。它用知识编辑构造有因果依据的 faithful/unfaithful explanation pairs，提出 Causal Diagnosticity 对多任务、多模型指标做系统比较，发现 Filler Tokens 总体最稳、连续指标更有信息但更容易受噪声影响。
