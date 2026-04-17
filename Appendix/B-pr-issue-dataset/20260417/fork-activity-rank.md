# Fork 活跃度与生态衍生榜

采集时间: 2026-04-17

## 数据源

1. `GET /repos/openclaw/openclaw/forks?sort=newest&per_page=100` × 3 页 = 最近 push 的 300 个 fork
2. `GET /search/repositories?q=openclaw+fork:only+stars:>=3` = 所有 fork 中 star ≥ 3 的，共 240 条
3. `GET /search/repositories?q=openclaw+in:name` = 名字带 openclaw 的衍生项目（含原创，排序取 top 20）

## 发现总结

- 最近 push 的 300 个 fork 里**只有 2 个**拿到过 star（`alanestevanmendes-afk/openclaw` 和 `Larryzpl123/openclaw`，各 1 star），说明绝大多数 fork 是**一次性临时实验**或教程克隆，不是派生维护。
- 但以 star ≥ 3 为阈值后，共 240 个"有生命体征"的 fork，头部几个已经是独立产品或中文化工程。
- 派生头部呈现**中国化**和**端侧化**两条主线：`openclaw-cn`（中文社区版，4695 ★）、`EdgeClaw`（边缘/端云协同，1192 ★）、`OpenClawAndroid`（Android 三合一，255 ★）。
- 除 fork 外，**衍生生态（非 fork 原创仓库）**比 fork 更活跃：`openclaw/clawhub`（官方 skill 目录 8k ★）、`VoltAgent/awesome-openclaw-skills`（社区聚合 46k ★）、`Gen-Verse/OpenClaw-RL`（RL 训练 5k ★）。

## Top 20 有实质 star 的 fork（按 star 倒序）

| fork | star | 语言 | 最近 push | 方向 |
|------|------|------|-----------|------|
| [jiulingyun/openclaw-cn](https://github.com/jiulingyun/openclaw-cn) | 4695 | TypeScript | 2026-04-12 | 中文社区版，内置钉钉/企业微信/飞书/QQ/微信，国内网络优化 |
| [DenchHQ/DenchClaw](https://github.com/DenchHQ/DenchClaw) | 1524 | TypeScript | 2026-04-16 | Fully Managed OpenClaw 框架，CRM 自动化 |
| [OpenBMB/EdgeClaw](https://github.com/OpenBMB/EdgeClaw) | 1192 | TypeScript | 2026-04-15 | Edge-Cloud 协同个人 AI 助手 |
| [MaoTouHU/OpenClawChinese](https://github.com/MaoTouHU/OpenClawChinese) | 347 | JavaScript | 2026-04-17 | 汉化版，Claude/ChatGPT LLM 接入 |
| [AtomicBot-ai/atomicbot](https://github.com/AtomicBot-ai/atomicbot) | 283 | TypeScript | 2026-04-17 | 最快启动的 OpenClaw 发行 |
| [Tornadopp/openclaw-learn](https://github.com/Tornadopp/openclaw-learn) | 271 | None | 2026-02-15 | 中文教程（涵盖安装/配置/实战/避坑） |
| [pudge0313/openclaw-](https://github.com/pudge0313/openclaw-) | 259 | None | 2026-02-15 | 同类教程镜像 |
| [OpenClawAndroid/openclaw-android-assistant](https://github.com/OpenClawAndroid/openclaw-android-assistant) | 255 | TypeScript | 2026-04-17 | AnyClaw = OpenClaw + Codex + Claude Code 在 Android 上三合一 |
| [RainbowRain9/openclaw-china](https://github.com/RainbowRain9/openclaw-china) | 116 | TypeScript | 2026-03-29 | 中国插件：飞书/钉钉/QQ/企业微信 |
| [luolin-ai/openclawWeComzh](https://github.com/luolin-ai/openclawWeComzh) | 110 | TypeScript | 2026-03-16 | 企业微信落地（含 AEON 认知观察协议实现） |
| [0xSojalSec/mimiclaw](https://github.com/0xSojalSec/mimiclaw) | 99 | None | 2026-02-10 | $5 芯片上跑 OpenClaw（No OS/Linux/Node/Mac 版） |
| [rollysys/agents-radar](https://github.com/rollysys/agents-radar) | 88 | TypeScript | 2026-04-17 | 追踪 Claude Code / Codex / Gemini CLI / OpenClaw 生态变化 |
| [sunkencity999/localclaw](https://github.com/sunkencity999/localclaw) | 80 | TypeScript | 2026-02-21 | 针对小型本地开源模型优化的 OpenClaw |
| [panyw5/opencode](https://github.com/panyw5/opencode) | 79 | TypeScript | 2026-04-17 | 开源编码代理（借鉴 OpenClaw 架构） |
| [N1nEmAn/edict-2.0](https://github.com/N1nEmAn/edict-2.0) | 73 | Python | 2026-03-06 | 三省六部制 Multi-Agent 编排（9 个专精 agent） |
| [CrayBotAGI/OpenCray](https://github.com/CrayBotAGI/OpenCray) | 72 | TypeScript | 2026-03-11 | 小龙虾机器人：国产生态 + 国产大模型 |
| [mberman84/awesome-openclaw-usecases](https://github.com/mberman84/awesome-openclaw-usecases) | 70 | None | 2026-02-09 | 用例社区集合 |
| [iwhalo/openclaw-](https://github.com/iwhalo/openclaw-) | 57 | None | 2026-02-15 | 教程镜像 |
| [bighamx/openclaw-android-node-apk](https://github.com/bighamx/openclaw-android-node-apk) | 56 | TypeScript | 2026-04-17 | Android Node 伴随 APK 自动发布 |
| [ppcvote/openclaw-claude-proxy](https://github.com/ppcvote/openclaw-claude-proxy) | 54 | JavaScript | 2026-03-21 | Opus 4.6 Telegram AI 助手（Claude Max + OpenClaw，一键配置） |

## 衍生生态（非 fork 但围绕 OpenClaw）Top 10

| 仓库 | star | 语言 | 近 push | 类型 |
|------|------|------|---------|------|
| [VoltAgent/awesome-openclaw-skills](https://github.com/VoltAgent/awesome-openclaw-skills) | 46374 | - | 2026-04-04 | skill 社区聚合（5400+） |
| [hesamsheikh/awesome-openclaw-usecases](https://github.com/hesamsheikh/awesome-openclaw-usecases) | 29684 | - | 2026-03-24 | 用例社区 |
| [openclaw/clawhub](https://github.com/openclaw/clawhub) | 8066 | TypeScript | 2026-04-17 | 官方 skill 目录 |
| [Gen-Verse/OpenClaw-RL](https://github.com/Gen-Verse/OpenClaw-RL) | 5010 | Python | 2026-04-16 | RL 训练任意 agent（"talk-to-train"） |
| [linuxhsj/openclaw-zero-token](https://github.com/linuxhsj/openclaw-zero-token) | 4328 | TypeScript | 2026-04-17 | 无 API token 接入主流模型 |
| [openclaw/skills](https://github.com/openclaw/skills) | 4167 | Python | 2026-04-17 | 官方 skill 归档 |
| [xianyu110/awesome-openclaw-tutorial](https://github.com/xianyu110/awesome-openclaw-tutorial) | 4150 | Shell | 2026-04-16 | 中文零基础教程 |
| [clawdbot-ai/awesome-openclaw-skills-zh](https://github.com/clawdbot-ai/awesome-openclaw-skills-zh) | 3928 | - | 2026-03-30 | 中文官方 skill 库 |
| [BytePioneer-AI/openclaw-china](https://github.com/BytePioneer-AI/openclaw-china) | 3822 | TypeScript | 2026-04-15 | 中国插件（飞书/钉钉/QQ/企业微信/微信） |
| [TianyiDataScience/openclaw-control-center](https://github.com/TianyiDataScience/openclaw-control-center) | 3818 | TypeScript | 2026-04-13 | 可视化控制中心（黑箱→可观测） |

## 重点关注方向聚类

基于以上头部 fork/派生的描述提取，**社区正在补齐的能力**：

1. **中国生态接入**（#1/#9/#11/#16 及多个"中国"冠名仓库）：钉钉、企业微信、飞书、QQ、微信、国内网络代理
2. **端侧与嵌入式**（#3/#11/#8/#19）：边缘协同、$5 芯片、Android 独立助手
3. **可观测与管理面**（#2/#15/#20 及 TianyiDataScience control-center）：从"黑箱"走向"控制面板 + CRM"
4. **国产模型对接**（#14/#16）：零 token、本地开源模型、DeepSeek/Kimi/Qwen
5. **Agent 编排与 RL**（`Gen-Verse/OpenClaw-RL`、`N1nEmAn/edict-2.0`）：多 agent 编排 + 可训练
6. **教程与 awesome 列表**（#6/#7/#17 等，多个 4k+ star 教程）：中文社区的学习路径稀缺，教程本身成独立生态

## 结论

openclaw 的派生生态并非围绕"fork 改代码"展开，而是：

- **官方主线仓库接收绝大部分 PR**（23k commits 仅 3 人高产：Peter/Vincent/Tak）
- **社区贡献集中在 skill/教程/用例**（awesome 列表 star 反而高于代码 fork）
- **fork 里真正活跃的是中文化与中国生态补齐**——这是官方仓库没覆盖或覆盖不足的部分
