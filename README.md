# Kaggriculture Agent 工作笔记

一份关于 Kaggriculture 竞赛 agent 的工作笔记，记录问题分析、策略迭代、验证结果和仍待解决的限制，并附一个完整代码版本。

Kaggriculture 是一个双人农场经营模拟：每个 agent 在 30 天、每天 24 个回合中安排工人、作物、牲畜和交易；双方共同影响动态市场，最终按赛季经营结果进行竞赛评价。它不是提示注入或工具攻击赛。

**English index:** Research notes for a Kaggle Kaggriculture farming-simulation agent, covering multi-agent simulation, dynamic markets, replay-based evaluation, risk-aware strategy iteration, and submission validation. The archive contains one submitted Python agent and a Chinese working note.

**Keywords:** Kaggriculture, Kaggle competition, game-playing agent, multi-agent simulation, dynamic market, replay analysis, empirical evaluation, agent robustness.

## 结果快照

Kaggle CLI 在 2026-10-19 显示，提交 56720623 状态为 COMPLETE，public score 为 1662.0，private score 为 4279.6。这是平台返回的赛果快照，不单独说明策略对不同对手或环境都适用，也不能归因某一代码改动带来的全部变化。

最终代码来自这一提交，文件 SHA-256 为 4bb6df2f8bb0e15c1c8e6666dca9102d47c6180c7f6c9407f6b1dd979a1ea5d3。它是对公开策略代码的组合与增量修改，并非完全从零编写；原始代码中的 Apache-2.0 许可和上游出处说明均保留。

## 文件

| 文件 | 用途 |
|---|---|
| [main.py](main.py) | 一个完整、可直接作为 Kaggle 单文件 agent 的提交版本 |
| [WORKING_NOTE.md](WORKING_NOTE.md) | 竞赛说明、研究过程、可复用经验、验证记录与局限 |
| [LICENSE](LICENSE) | Apache License 2.0 |

没有收录候选版本、数据集、回放、账号日志或 Kaggle 凭证。

## 验证

源码以原始字节通过 Python 3.12 编译检查，无 UTF-8 BOM。使用 kaggle-environments 1.32.7 与 starter 完整运行 720 步，双方状态均为 DONE；提交记录也载明 Kaggle 验证回合双方完成。该冒烟测试只证明接口和整局运行正常，不证明对未知对手的胜率。

## 许可与出处

main.py 内保留了 Apache-2.0 文本及其继承代码的归属说明。主要公开来源包括 Kaggle 上的 Kaggriculture public state-router 与 Shop Router 工作；原文件中的详细 notices 是代码来源和变更边界的依据。LICENSE 提供许可证全文。
