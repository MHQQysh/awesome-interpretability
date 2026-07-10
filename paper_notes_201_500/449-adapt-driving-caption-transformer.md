# 449. ADAPT: Action-aware Driving Caption Transformer

- **Authors:** Bu Jin, Xinyu Liu, Yupeng Zheng, Pengfei Li, Hao Zhao, Tong Zhang, Yuhang Zheng, Guyue Zhou, Jingjing Liu
- **Venue / year:** ICRA 2023
- **Paper:** https://arxiv.org/abs/2302.00673
- **Tags:** `autonomous-driving` `explanation` `multitask-learning` `video-captioning`

## Introduction
端到端自动驾驶的决策通常只输出 steering/speed 等控制信号，乘客和工程师看不到车辆为什么加速、刹车或转向。注意力图或 cost volume 对普通用户不够可读，ADAPT 的目标是同时输出 action narration（“车在加速”）和 reasoning（“因为信号灯变绿”），让自然语言解释与控制预测共享同一个视频表征。

## Method / Framework
ADAPT 使用 Video Swin 编码第一视角车载视频，接两个 head：DCG 用 vision-language Transformer 自回归生成 action 与 reason 两段文本，CSP 用 motion Transformer 预测 speed/course 等控制信号。总损失是 caption loss + control-signal MSE；动作控制提供语义监督，reasoning 则描述影响行动的环境因素。

训练中把视频 token、文本 token 和 segment embedding 送入共享 encoder，action/reason 各自用最多 15 个 WordPiece；视频从每段均匀采样 T 帧。BDD-X 约 7,000 视频、29,000 behavior-annotation pairs，每条带控制信号、动作描述和原因。

## Baselines / Comparisons
自动指标是 BLEU-4、CIDEr、METEOR、ROUGE-L；基线包括 S2VT、S2VT++、SAA 和 WAA。人工评测分别判断 narration 是否符合行动、reasoning 是否正确、完整 action+reason 是否同时正确；消融比较只做 caption 的 Single、加入控制信号作为输入但不 joint-predict 的 Single+、不同 speed/course supervision、句子顺序/attention mask 和帧数。

## Experiments / Findings
ADAPT 在 narration 上 B4/C/M=34.6/247.5/30.6，在 reasoning 上 11.4/102.6/15.2，优于 WAA 的 32.3/215.8/29.2 与 7.3/69.5/12.2；CIDEr 相对 WAA 提升 31.7（narration）和 33.1（reasoning）。人工准确率为 narration 90.0%、reasoning 90.3%、full sentence 82.7%，reasoning 明显优于 SAA 62.4%/WAA 66.0%。

joint action-aware training 是主要增益：Single 的 narration/reasoning CIDEr 为 238.9/89.7，Single+ 为 248.3/97.2，ADAPT 为 247.5/102.6；速度或 course 任一信号去掉都会下降，例如去掉 speed 后 narration CIDEr 降 29.3、reasoning 降 14.0。帧数从 2 到 32 影响性能与成本，32 帧最好但推理成本约 797 分钟/对应实验设置。

## Ablation / Error Analysis
控制信号与视频共享表征的多任务约束优于把控制 token 直接塞进单 caption 模型，说明控制监督不仅提供额外输入，更像 regularizer。action/reasoning 的生成顺序和 causal mask 会改变两段之间的信息泄漏；reasoning 过度依赖动作文本时可能变成事后描述，而非独立环境证据。

失败原因包括视频帧采样遗漏关键灯光/车辆、BDD-X 标注的原因不完整、控制信号与文字并非严格同步，以及自动 caption metric 对真正“为什么”解释的判断很弱。人工 full-sentence 只有 82.7%，也说明两段各自正确不保证组合解释完整。

## Limitations
BDD-X 是单一驾驶域和固定 action/control vocabulary，无法证明解释在未见城市、天气、驾驶策略或真实安全决策中可靠。自然语言 reason 仍是生成式输出，联合训练并不保证因果 faithful；视频 Swin、transformer 和控制传感器的误差会共同传递到解释。

## 两句话总结
ADAPT 用共享视频表示联合预测驾驶控制信号与 action/reasoning 文本，让自动驾驶决策具备用户可读的自然语言解释。BDD-X 实验显示控制监督显著提升尤其是 reasoning 的 CIDEr 和人工正确率，但解释是否真正是控制器的因果依据仍需反事实驾驶测试。

## Evidence note
已下载并逐段读取 arXiv PDF，核对了 BDD-X 数据、Video Swin+DCG/CSP、S2VT/SAA/WAA 对照、Table I/II、joint-training/control-signal/frame 消融和训练成本。
