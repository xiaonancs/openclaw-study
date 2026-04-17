# Appendix A 章节配置与源码路径

本研究的章节骨架来自 `scripts/chapters.yaml`（该文件随 `.gitignore` 一同不进入发布仓）。为保证**可复现、可审阅**，本附录把核心字段以 Markdown 形式发布。

- 源码基线：openclaw/openclaw @ v2026.4.15（commit c56b56e）
- 采集日期：2026-04-17
- 章节总量：27 章（含第 27 章结语）+ 2 篇总纲 + 5 个 Appendix 条目

## Part I Architecture and Philosophy

| id | 标题 | 一句话定位 | 主要源码路径 |
|---|---|---|---|
| 01 | 项目定位与 Molty 愿景 | 从 Warelay→Clawdbot→Moltbot→OpenClaw 的演化路径与 local-first 原则 | VISION.md / README.md / docs/concepts/architecture.md |
| 02 | Gateway 控制面总览 | 单一控制面：协议、端口、WebSocket、会话分发、健康检查 | src/gateway / src/daemon / src/bootstrap / src/entry.ts / docs/gateway/* |
| 03 | Agent 与 Session 模型 | 多 Agent 路由、per-agent session 隔离、main vs non-main | src/agents / src/sessions / src/chat / docs/concepts/* |
| 04 | Context Engine 与记忆体系 | active-memory / honcho / lancedb / wiki 多 backend + dreaming / compaction | src/context-engine / src/memory-host-sdk / extensions/memory-* |
| 05 | 插件与扩展机制 | Plugin SDK / package contract / activation boundary / 106 extension 加载 | src/plugins / packages/plugin-sdk / packages/plugin-package-contract |
| 06 | Skill 体系与 ClawHub | bundled/managed/workspace 三层 + SKILL.md 契约 + ClawHavoc 后加固 | skills/ / extensions/kilocode / docs/concepts/agent-workspace.md |

## Part II Source Execution

| id | 标题 | 一句话定位 | 主要源码路径 |
|---|---|---|---|
| 07 | 启动与进程模型 | entry→bootstrap→daemon→gateway 启动链 / respawn / launchd / Docker | src/entry*.ts / src/bootstrap / src/process / src/daemon / Dockerfile* |
| 08 | CLI 与命令体系 | 顶层 CLI / onboard / interactive TUI / chat-command 分派 | src/cli / src/commands / src/interactive / src/tui / src/wizard |
| 09 | 路由 Hooks 与 Auto-reply | inbound 路由、hooks 双向钩子、auto-reply 策略、thread-ownership | src/routing / src/hooks / src/auto-reply / src/flows / extensions/thread-ownership |
| 10 | Tools Canvas 与 Nodes | Tools 执行 / A2UI Canvas / Nodes 配对 / browser-lifecycle-cleanup | src/canvas-host / src/node-host / src/browser-lifecycle-cleanup.ts / vendor/a2ui |
| 11 | 实时语音与转写 | Voice Wake / Talk Mode / Deepgram / ElevenLabs / Sherpa-ONNX | src/realtime-voice / src/realtime-transcription / src/tts / extensions/speech-core |
| 12 | 多媒体生成与理解 | image/video/music 三条生成链 + media / link understanding | src/image-generation / src/media* / src/music-generation / src/link-understanding |
| 13 | 安全 沙箱与配对 | DM pairing / Docker sandbox 三层 / secrets / tool policy / CVE 修复 | src/security / src/pairing / src/secrets / Dockerfile.sandbox* / SECURITY.md |

## Part III Channels Extensions Apps

| id | 标题 | 一句话定位 | 主要源码路径 |
|---|---|---|---|
| 14 | Channels 抽象与 DM 策略 | channel 接口 / pairing 三档 / 12+ 主流 messenger | src/channels / extensions/whatsapp / telegram / slack / discord / … |
| 15 | 模型提供方接入全景 | 50+ provider / model-failover / copilot-proxy / vercel-ai-gateway | extensions/openai / anthropic / google / openrouter / litellm / … |
| 16 | 中国区生态适配 | feishu / qqbot / line / zalo + deepseek / qwen / kimi / qianfan / volcengine | extensions/feishu / qqbot / deepseek / moonshot / qwen / volcengine / … |
| 17 | Skills 实战剖析 | coding-agent / clawhub / canvas / voice-call / gh-issues / obsidian / trello | skills/* |
| 18 | macOS 菜单栏 App | 菜单栏 / PTT overlay / WebChat / SSH 远端 Gateway | apps/macos / apps/shared / Swabble |
| 19 | iOS 与 Android 节点 | pairing 协议 / camera-photos-sms / shell / Android 后台策略 | apps/ios / apps/android / apps/shared / src/pairing |

## Part IV Variants and PR Evolution（用户问 2）

| id | 标题 | 一句话定位 | 数据来源 |
|---|---|---|---|
| 20 | 活跃 Fork 与变种生态 | 300 最近 fork + 240 star≥3 fork + 20 衍生原创，四条主线 | Appendix/B forks-* |
| 21 | 同类 AI 助手横向对比 | 8 个同类项目能力矩阵 + 象限图 | 本仓库前 19 章 + hermes-agent-study + claude-code-source-analysis |
| 22 | 二月至今 PR 演进全景 | 1200 合并 PR 按主题 / 月度 / 作者 / 路径聚类 | Appendix/B prs-* + path-frequency |
| 23 | 社区关注的能力增强 | 综合 20-22 + issue 交叉，5 条主线 + 次级主题 | Appendix/B analysis-summary.json |

## Part V Issues and Roadmap（用户问 3）

| id | 标题 | 一句话定位 | 数据来源 |
|---|---|---|---|
| 24 | Issues 与抱怨聚类 | 420 issue 按主题 × label × 月度，Top 10 痛点 | Appendix/B issues-* |
| 25 | 源码层设计问题 | Part I-III 每章缺陷汇总，按严重度分级，交叉验证 issue | 本仓库前 19 章 |
| 26 | 重点优化方向建议 | 六条 R1-R6，每条配源码证据 / 修复路径 / 成功指标 | 第 24-25 章综合 |
| 27 | 结语 全景与判断 | 四个总体判断 + 三个预测 + 两个未解难题 | 全 26 章综合 |

## Appendix 条目

- **A 章节配置与源码路径**（本文件）
- **B PR / Issue / Fork / Commit 数据集**：`Appendix/B-pr-issue-dataset/20260417/`
- **C Fork 活跃度榜**：`Appendix/B-pr-issue-dataset/20260417/fork-activity-rank.md`
- **D 封面与阅读路径**：`Appendix/D 封面与阅读路径.md`
- **E 源码归档**：不随仓库分发；通过 `git clone https://github.com/openclaw/openclaw @ v2026.4.15` 获取原始代码

## 写作框架（七维）

非数据驱动章节一律走七维结构：

1. 本章外部视角
2. 本质是什么
3. 核心问题和痛点
4. 解决思路与方案（含 Mermaid）
5. 实现细节关键点
6. 易错点和注意事项
7. 竞品对比 / 仍存在的问题

数据驱动章节（Part IV / V）走 "数据源 → 指标 → 交叉分析 → 结论"。

## 如何复现本研究

1. `git clone` OpenClaw 到 `openclaw-repo/`，`git fetch --unshallow`
2. 跑 `scripts/collect-github-data.sh <YYYYMMDD>` 采集（需要 gh auth 可走 5000/hr 鉴权模式）
3. 跑 `scripts/analyze-data.py <YYYYMMDD>` 生成 analysis-summary.json
4. 按 Part I-V 顺序阅读并按七维框架写作
5. 参考 [第 26 章 Roadmap](../Part%20V%20Issues%20and%20Roadmap/26%20%E9%87%8D%E7%82%B9%E4%BC%98%E5%8C%96%E6%96%B9%E5%90%91%E5%BB%BA%E8%AE%AE.md) 比对最新演进
