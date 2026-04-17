# Appendix 附录与数据集

## 目录

- [B-pr-issue-dataset/20260417/](./B-pr-issue-dataset/20260417/) — 原始数据集（采集时间 2026-04-17）

## B. PR / Issue / Fork / Commit 数据集

采集自 GitHub REST API（unauthenticated rate-limit 模式）+ 本地 `git log`。

### 数据文件

| 文件 | 说明 |
|---|---|
| `prs-merged-p1..p8.json` | 合并 PR, 按 `sort=updated&direction=desc` 分页，共 800 条 |
| `prs-early-p1..p4.json` | `sort=created&direction=asc` 分页，补覆盖 2026-02/03，共 400 条 |
| `issues-p1..p3.json` | 按 created desc 的 issue 分页 |
| `issues-updated-p1..p3.json` | 按 updated desc 的 issue 分页（补覆盖） |
| `forks-p1..p3.json` | `sort=newest` 最近 push 的 300 个 fork |
| `forks-starred.json` | star ≥ 3 的 fork 搜索结果 |
| `related-named.json` | 名字带 openclaw 的衍生仓库（含非 fork） |
| `contributors.json` | 前 100 贡献者 |
| `releases.json` | release 列表 |
| `repo-meta.json` | 仓库元数据 |
| `commits-since-feb.psv` | `git log` 导出的 commit 记录（PSV 格式） |
| `commits-by-month.txt` | 月度 commit 数 |
| `path-frequency-top40.txt` | 改动最频繁的 top40 路径 |
| `authors-top50.txt` | PR 作者 top50 |
| `commit-themes-*.json/.csv` | 按主题分类的 commit 聚合 |
| `analysis-summary.json` | 所有统计的汇总结果 |
| `fork-activity-rank.md` | fork 榜 Markdown 可读报告 |
| `collect.log` | 采集脚本日志 |

### 用途

- **Part IV 第 20、22 章**：基于这些数据做分布与演进分析
- **Part V 第 24 章**：基于 issue 做聚类与痛点识别
- **未来增量研究**：可以在 `B-pr-issue-dataset/<新日期>/` 下再做一次采集与对比

### 采集方法

见 [scripts/collect-github-data.sh](../scripts/collect-github-data.sh)（发布后与 scripts/ 一同 .gitignore 排除；如需复现请在正式仓库根目录跑）。

### 重跑建议

- 重跑命令：`bash scripts/collect-github-data.sh <YYYYMMDD>`
- 分析命令：`python3 scripts/analyze-data.py <YYYYMMDD>`
- 注意：未鉴权模式下 60 req/h，需自然分批；建议配 `GH_TOKEN` 走 5000 req/h。

## 附录承载的三类用途

1. **量化证据**：任何定量断言都应能追溯到本目录
2. **可复现性**：脚本 + 日期戳数据即可重跑
3. **时间切片**：同一目录下不同日期的采集，可做"演进对比"
