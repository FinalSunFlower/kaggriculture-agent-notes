# Kaggriculture Agent 工作笔记

**语言：** [English](README.md) | [简体中文](README.zh-CN.md)

本仓库记录一个 Kaggle Kaggriculture agent 的开发、评估与打包过程。仓库包含一份工作笔记、一个完整的最终 agent，以及适用的许可证和归属说明。笔记不局限于本次比赛，也总结了研究长时域、部分可观测、多智能体经济仿真的一套实用方法。

## 快速概览

- **任务：** 双人农场与市场经营模拟
- **agent 文件：** [`main.py`](main.py)，对应 Kaggle 提交 `56720623`
- **工作笔记：** [`WORKING_NOTE.md`](WORKING_NOTE.md) | [`WORKING_NOTE.zh-CN.md`](WORKING_NOTE.zh-CN.md)
- **提交状态：** Complete
- **记录的 public score：** 1662.0
- **记录的 private score：** 4279.6
- **分数快照日期：** 2026-10-19
- **本地验证环境：** Python 3.12 与 `kaggle-environments` 1.32.7
- **许可证：** Apache-2.0，见 [`LICENSE`](LICENSE) 和 `main.py` 中的 notices

这些分数是有日期的竞赛平台快照，不是对任意对手的通用性能估计。工作笔记说明了评估范围和限制。

## 仓库内容

| 路径 | 说明 |
| --- | --- |
| [`main.py`](main.py) | 自包含的单文件最终提交 agent |
| [`WORKING_NOTE.md`](WORKING_NOTE.md) | 英文版任务概览、研究过程、评估记录、经验和复现信息 |
| [`WORKING_NOTE.zh-CN.md`](WORKING_NOTE.zh-CN.md) | 与英文版结构对应的简体中文工作笔记 |
| [`LICENSE`](LICENSE) | Apache License 2.0 正文 |
| [`.gitattributes`](.gitattributes) | Git 文本文件处理规则 |

仓库不包含临时实验、回放文件、账号日志、凭证或数据集。

## 已记录的验证

最终提交源码按原始 Python 字节检查，无 UTF-8 BOM；在 Python 3.12 下编译成功；使用 `kaggle-environments` 1.32.7 完整运行 720 步后，双方状态均为 `DONE`。这些检查说明在所测试环境中可以完成基本打包和完整回合运行，但不代表对未见对手池的胜率。

## 从哪里开始

阅读 [`WORKING_NOTE.zh-CN.md`](WORKING_NOTE.zh-CN.md) 或 [`WORKING_NOTE.md`](WORKING_NOTE.md)，了解任务模型、最终增量改动的动机、证据及其边界，以及可迁移到类似仿真任务的工作流程。代码保留了上游归属和许可证 notices；再分发或修改时请继续保留这些说明。
