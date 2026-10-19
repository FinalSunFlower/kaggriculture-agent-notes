# Kaggriculture Agent Research Notes

一份关于 Kaggriculture 竞赛 agent 的研究归档。这里记录可验证的方法、一个最终提交版本和它的证据边界，不是冠军复盘，也不声称解决了所有强对手。

Kaggriculture 是一个双人农场经营模拟：每个 agent 在 30 天、每天 24 个回合中安排工人、作物、牲畜和交易；双方共同影响动态市场，最终按赛季经营结果进行竞赛评价。它不是提示注入或工具攻击赛。

本仓库参考了 [jed-aas-benchmark-notes](https://github.com/FinalSunFlower/jed-aas-benchmark-notes) 的归档方式：公开一份工作笔记和一个完整代码版本，不把临时实验、回放和日志混成结果。

**English index:** Research notes for a Kaggle Kaggriculture farming-simulation agent, covering multi-agent simulation, dynamic markets, replay-based evaluation, risk-aware strategy iteration, and submission validation. The archive contains one submitted Python agent and a Chinese working note.

**Keywords:** Kaggriculture, Kaggle competition, game-playing agent, multi-agent simulation, dynamic market, replay analysis, empirical evaluation, agent robustness.

## 结果快照

Kaggle CLI 在 2026-10-19 显示，最终提交 56720623 状态为 COMPLETE，public score 为 1662.0，private score 为 4279.6。竞赛列表显示账户 rank 1695 / 10148。这些是平台返回的赛果快照；本文不据此推断奖牌、策略普适性或对其他版本的因果优势。

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
