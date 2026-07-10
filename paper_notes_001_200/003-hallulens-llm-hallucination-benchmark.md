# 003. HalluLens: LLM Hallucination Benchmark

## 3. HalluLens: LLM Hallucination Benchmark

- **作者 / 发表**：Yejin Bang, Ziwei Ji, Alan Schelten, Anthony Hartshorn, Tara Fowler, Cheng Zhang, Nicola Cancedda, Pascale Fung；ACL 2025
- **论文链接**：[ACL Anthology](https://aclanthology.org/2025.acl-long.1176/)
- **方向**：幻觉诊断、基准测试、外部/内部幻觉分类
- **要解决的问题**：现有工作经常把 hallucination、factuality 和知识错误混在一起，导致不同论文使用的定义、类别和评价方式不一致；尤其缺少专门评估 extrinsic hallucination 的统一基准。
- **怎么解决**：论文提出 HalluLens，先给出区分 intrinsic hallucination 与 extrinsic hallucination 的分类框架，再构造三个面向 extrinsic hallucination 的任务。测试集采用动态生成策略，降低静态数据泄漏并增强评测稳健性，同时发布可复用代码库。
- **摘要概括**：HalluLens 将幻觉从宽泛的“事实性不好”中拆出来，强调生成内容偏离训练知识或用户输入的外部幻觉。它通过统一 taxonomy、动态测试集和多任务设置，为比较不同模型的幻觉行为提供更一致的实验入口。
- **阅读要点**：这类基准论文是可解释性研究的测量基础。没有稳定的行为定义和反泄漏评测，就很难判断一个内部解释是否真的对应幻觉成因，而不是只对应某个数据集的表面模式。
