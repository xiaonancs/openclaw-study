# 20 活跃 Fork 与变种生态

## 数据源

- `GET /repos/openclaw/openclaw/forks?sort=newest&per_page=100` × 3 页 = 最近 push 的 **300 个** fork（见 [Appendix/B/forks-p1..p3.json](../Appendix/B-pr-issue-dataset/20260417/forks-p1.json)）
- `GET /search/repositories?q=openclaw+fork:only+stars:>=3` = star ≥ 3 的 fork **240 条**（[forks-starred.json](../Appendix/B-pr-issue-dataset/20260417/forks-starred.json)）
- `GET /search/repositories?q=openclaw+in:name` = 名字带 openclaw 的衍生仓库 top 20（[related-named.json](../Appendix/B-pr-issue-dataset/20260417/related-named.json)）
- 汇总分析：[fork-activity-rank.md](../Appendix/B-pr-issue-dataset/20260417/fork-activity-rank.md)

采集时间 2026-04-17，基线 openclaw @ v2026.4.15。

## 一、fork 生态的"两个背离"

### 背离 1：近期活跃 fork ≠ 有实质价值 fork

最近 push 的 300 个 fork 里只有 **2 个** 拿到过 star（各 1 star）。其余几乎都是：

- 教程跟做 / 课堂练习
- 自动化工具偶尔产生的镜像
- 企业内网 mirror

结论：**fork 数（73.1k）不是衡量生态健康的指标**，真正的衍生活力要看"star ≥ 3 且近 3 个月有 push"的交集。

### 背离 2：fork 活力 < 非 fork 原创项目的活力

头部衍生项目里最有影响力的几个都不是 fork：

| 仓库 | star | 类型 |
|---|---|---|
| [VoltAgent/awesome-openclaw-skills](https://github.com/VoltAgent/awesome-openclaw-skills) | 46374 | skill 社区聚合 |
| [hesamsheikh/awesome-openclaw-usecases](https://github.com/hesamsheikh/awesome-openclaw-usecases) | 29684 | 用例社区 |
| [openclaw/clawhub](https://github.com/openclaw/clawhub) | 8066 | 官方 skill 目录 |
| [Gen-Verse/OpenClaw-RL](https://github.com/Gen-Verse/OpenClaw-RL) | 5010 | RL 训练 |
| [xianyu110/awesome-openclaw-tutorial](https://github.com/xianyu110/awesome-openclaw-tutorial) | 4150 | 中文零基础教程 |

fork 里最高只有 `jiulingyun/openclaw-cn`（4695 ★）。这说明 **OpenClaw 的生态是"外接扩展"而非"派生分叉"**。

## 二、Top 20 star fork 的方向聚类

基于 [fork-activity-rank.md](../Appendix/B-pr-issue-dataset/20260417/fork-activity-rank.md) 的 top 20：

| 方向 | 代表仓库 | fork 数量（top 20 内） |
|---|---|---|
| 中文 / 中国生态 | openclaw-cn、RainbowRain9/openclaw-china、luolin-ai/openclawWeComzh、OpenClawChinese、CrayBotAGI/OpenCray | 5 |
| 端侧 / 嵌入式 / 移动 | OpenBMB/EdgeClaw、OpenClawAndroid、mimiclaw、bighamx/openclaw-android-node-apk | 4 |
| 教程 / 文档 | Tornadopp/openclaw-learn、pudge0313/openclaw-、iwhalo/openclaw- | 3 |
| 企业 / 可观测 / CRM | DenchHQ/DenchClaw | 1 |
| 本地开源模型 | sunkencity999/localclaw | 1 |
| 多 agent 编排 | N1nEmAn/edict-2.0 | 1 |
| 辅助工具 | rollysys/agents-radar、awesome-openclaw-usecases、ppcvote/openclaw-claude-proxy、panyw5/opencode | 4 |

**核心结论**：fork 头部生态有**三大主线 + 一条教程线**：

1. **中国化**（含通道 + 模型）
2. **端侧化**（Android / 嵌入式 / 边缘）
3. **企业 / 可观测化**
4. 教程内容型 fork（非代码）

## 三、每条主线的空缺与价值

### 3.1 中国化主线（5 个头部 fork）

- **空缺**：官方仓库不支持企业微信 / 钉钉 / 微信；国产模型 tool_call 兼容性参差；中文 TTS 短板；docs/ 无中文
- **价值**：4000+ ★ 的 openclaw-cn 说明真实需求巨大；官方吸收节奏慢，让 fork 成为事实标准

### 3.2 端侧化主线

- **空缺**：官方 Android App 能力有上限（见第 19 章）；iOS 受系统约束；本地小模型链路不成熟
- **价值**：OpenBMB/EdgeClaw 等把 OpenClaw 向物联网/家庭边缘推；agents-radar 则让社区追踪生态变化

### 3.3 企业 / 可观测化主线

- **空缺**：OpenClaw 原生 UI 偏工程；"Control Center"、"CRM 自动化"类需求在官方缺席
- **价值**：DenchHQ/DenchClaw（1524 ★）和 TianyiDataScience/openclaw-control-center（3818 ★）填了这块

### 3.4 教程 / awesome 列表

- **空缺**：官方 docs/ 英文为主，入门曲线陡
- **价值**：中文教程多为 Shell / Markdown 仓库，低门槛高 star，体现学习需求的"堰塞湖"

## 四、fork 活跃度的时间分布

从 [fork-activity-rank.md](../Appendix/B-pr-issue-dataset/20260417/fork-activity-rank.md) 提取：

- top 20 里 push 在 2026-04 的有 12 个
- 2026-03 的 4 个
- 2026-02 的 4 个

**即 60% 头部 fork 仍在活跃维护**，说明生态不是"冲榜式短期热度"，而是有稳定贡献者。

## 五、与官方 PR 的对照

（更完整数据见第 22 章）Top PR 作者：vincentkoc 61、mbelinky 43、gumadeiras 39、eleqtrizit 33、obviyus 30、Takhoffman 29……主要集中在海外；中文社区的贡献更多通过 fork 而非直接 PR。

**对比结论**：官方仓库的 PR 活力来自"北美 + 欧洲"社区；中国 / 企业化 / 端侧化的活力来自 fork。两路没有对冲，而是互补。

## 六、对 OpenClaw 的启示

1. **企业微信 / 钉钉 / 微信不做就是把占比 4000+ ★ 的需求拱手让给 fork**
2. **非 fork 原创生态比 fork 更重要**：clawhub / RL / awesome 列表，值得列为官方 roadmap
3. **Control Center / 可观测化**是官方未触达的高需求领域
4. **中文文档 i18n** 是 ROI 最高的投入之一

## 下一章预告

第二十一章把 OpenClaw 与其他 AI 助手（Claude Code / Cursor / Codex / Continue / Aider / Opencode / Kilocode）放在同一个能力矩阵里做横向比较。
