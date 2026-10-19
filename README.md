# Kaggriculture Agent Working Notes

**Language:** [English](README.md) | [简体中文](README.zh-CN.md)

This repository documents the development, evaluation, and packaging of one agent for Kaggle's Kaggriculture competition. It contains a working note, one complete submitted agent, and the applicable license and attribution material. The note is intended to be useful beyond this competition: it describes a practical method for studying long-horizon, partially observable, multi-agent economic simulations.

## At a glance

- **Task:** Two-player farming and market simulation
- **Agent artifact:** [`main.py`](main.py), submitted as Kaggle submission `56720623`
- **Working notes:** [`WORKING_NOTE.md`](WORKING_NOTE.md) | [`WORKING_NOTE.zh-CN.md`](WORKING_NOTE.zh-CN.md)
- **Submission status:** Complete
- **Recorded public score:** 1662.0
- **Recorded private score:** 4279.6
- **Score snapshot date:** 2026-10-19
- **Runtime used for local validation:** Python 3.12 and `kaggle-environments` 1.32.7
- **License:** Apache-2.0; see [`LICENSE`](LICENSE) and the notices in `main.py`

Scores are reported as a dated platform snapshot, not as a general estimate of performance against arbitrary opponents. The working note explains the evaluation scope and limitations.

## Repository contents

| Path | Description |
| --- | --- |
| [`main.py`](main.py) | Self-contained, single-file submitted agent |
| [`WORKING_NOTE.md`](WORKING_NOTE.md) | English task overview, research process, evaluation record, lessons, and reproducibility notes |
| [`WORKING_NOTE.zh-CN.md`](WORKING_NOTE.zh-CN.md) | 简体中文工作笔记，与英文版结构对应 |
| [`LICENSE`](LICENSE) | Apache License 2.0 text |
| [`.gitattributes`](.gitattributes) | Git text-file handling |

Temporary experiments, replay files, account logs, credentials, and datasets are not included.

## Validation recorded

The submitted source was checked as raw Python source with no UTF-8 byte-order mark. It compiled under Python 3.12, and a complete 720-step run using `kaggle-environments` 1.32.7 finished with both agents in the `DONE` state. These checks establish basic packaging and full-episode execution for the tested setup; they do not establish a win rate against an unseen opponent pool.

## Start here

Read [`WORKING_NOTE.md`](WORKING_NOTE.md) or [`WORKING_NOTE.zh-CN.md`](WORKING_NOTE.zh-CN.md) for the task model, the reasoning behind the final incremental change, the evidence and its limits, and a reusable workflow for similar simulation problems. The code retains upstream attribution and license notices; preserve them when redistributing or modifying it.
