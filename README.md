# OpenClaw 源码深度研究

> 对 [openclaw/openclaw](https://github.com/openclaw/openclaw) @ v2026.4.15 的系统性源码阅读与生态调研。
>
> 采集基线日期：**2026-04-17**；数据覆盖：2026-02-01 以来约 23,014 条 commit、1,200 条 merged PR、420 条 issue、300 个最新 fork + 240 个 star≥3 的 fork + 20 个衍生仓库。

## 快速导航

- **想一次性看懂**：从 [总纲 — OpenClaw 技术主线分析](./总纲-OpenClaw技术主线分析.md) 开始
- **想了解外界怎么看**：读 [全网调研 — 社区认知地图](./全网调研-社区认知地图.md)
- **关心架构设计**：Part I
- **关心执行链路**：Part II
- **关心通道与模型**：Part III
- **关心生态演进**：Part IV（直接回答"社区关注什么"）
- **关心缺陷与未来**：Part V（直接回答"优化方向"）
- **需要原始数据**：[Appendix](./Appendix/)

## 全部章节

### 总纲与调研

- [总纲 — OpenClaw 技术主线分析](./总纲-OpenClaw技术主线分析.md)
- [全网调研 — 社区认知地图](./全网调研-社区认知地图.md)

### Part I Architecture and Philosophy — 架构与设计哲学

1. [01 项目定位与 Molty 愿景](./Part%20I%20Architecture%20and%20Philosophy/01-vision-and-positioning.md)
2. [02 Gateway 控制面总览](./Part%20I%20Architecture%20and%20Philosophy/02-gateway-control-plane.md)
3. [03 Agent 与 Session 模型](./Part%20I%20Architecture%20and%20Philosophy/03-agent-and-session-model.md)
4. [04 Context Engine 与记忆体系](./Part%20I%20Architecture%20and%20Philosophy/04-context-engine-and-memory.md)
5. [05 插件与扩展机制](./Part%20I%20Architecture%20and%20Philosophy/05-plugin-extension-system.md)
6. [06 Skill 体系与 ClawHub](./Part%20I%20Architecture%20and%20Philosophy/06-skill-system-and-clawhub.md)

### Part II Source Execution — 源码执行链路

7. [07 启动与进程模型](./Part%20II%20Source%20Execution/07-bootstrap-and-process.md)
8. [08 CLI 与命令体系](./Part%20II%20Source%20Execution/08-cli-and-commands.md)
9. [09 路由 Hooks 与 Auto-reply](./Part%20II%20Source%20Execution/09-routing-hooks-autoreply.md)
10. [10 Tools Canvas 与 Nodes](./Part%20II%20Source%20Execution/10-tools-canvas-nodes.md)
11. [11 实时语音与转写](./Part%20II%20Source%20Execution/11-realtime-voice-transcription.md)
12. [12 多媒体生成与理解](./Part%20II%20Source%20Execution/12-media-generation-understanding.md)
13. [13 安全 沙箱与配对](./Part%20II%20Source%20Execution/13-security-sandbox-pairing.md)

### Part III Channels Extensions Apps — 通道 / 模型 / 客户端

14. [14 Channels 抽象与 DM 策略](./Part%20III%20Channels%20Extensions%20Apps/14-channels-and-dm-policy.md)
15. [15 模型提供方接入全景](./Part%20III%20Channels%20Extensions%20Apps/15-model-providers-landscape.md)
16. [16 中国区生态适配](./Part%20III%20Channels%20Extensions%20Apps/16-china-ecosystem-adaptation.md)
17. [17 Skills 实战剖析](./Part%20III%20Channels%20Extensions%20Apps/17-skills-deep-dive.md)
18. [18 macOS 菜单栏 App](./Part%20III%20Channels%20Extensions%20Apps/18-macos-menubar-app.md)
19. [19 iOS 与 Android 节点](./Part%20III%20Channels%20Extensions%20Apps/19-mobile-nodes-ios-android.md)

### Part IV Variants and PR Evolution — 变种生态与演进（用户问 2）

20. [20 活跃 Fork 与变种生态](./Part%20IV%20Variants%20and%20PR%20Evolution/20-active-forks-survey.md)
21. [21 同类 AI 助手横向对比](./Part%20IV%20Variants%20and%20PR%20Evolution/21-peer-projects-comparison.md)
22. [22 二月至今 PR 演进全景](./Part%20IV%20Variants%20and%20PR%20Evolution/22-pr-evolution-since-feb.md)
23. [23 社区关注的能力增强](./Part%20IV%20Variants%20and%20PR%20Evolution/23-what-community-wants.md)

### Part V Issues and Roadmap — 缺陷分析与优化建议（用户问 3）

24. [24 Issues 与抱怨聚类](./Part%20V%20Issues%20and%20Roadmap/24-issues-clustering.md)
25. [25 源码层设计问题](./Part%20V%20Issues%20and%20Roadmap/25-source-level-design-flaws.md)
26. [26 重点优化方向建议](./Part%20V%20Issues%20and%20Roadmap/26-roadmap-recommendations.md)

### Appendix 附录与原始数据集

- [Appendix 目录说明](./Appendix/README.md)
- 原始数据集：[Appendix/B-pr-issue-dataset/20260417](./Appendix/B-pr-issue-dataset/20260417/)

## 研究方法

本研究遵循 `source-deep-research` 7 阶段工作流：

1. 通读源码，产出 27 章骨架（[scripts/chapters.yaml](./scripts/chapters.yaml)，构建后随 scripts/ 一起不发布）
2. 全网调研（2026-02 ~ 2026-04 中英文资料），写两篇总纲
3. Part IV/V 专属数据采集：PR、Issue、Fork、Commits 四路并行
4. 27 章按"七维框架"（外部视角 → 本质 → 痛点 → 方案 → 实现 → 易错 → 竞品 → 缺陷）写作
5. 交叉 review（源码引用存在性、Mermaid 白底板、导航一致性）
6. 系统化整理（本 README、Appendix 索引、命名规范）
7. 仓库构建与发布

## 致谢

- 研究对象：[openclaw/openclaw](https://github.com/openclaw/openclaw) 及全体 contributor（特别是 Peter Steinberger、Vincent Koc、Takhoffman、mbelinky、gumadeiras 等）
- 生态数据：GitHub REST API + 社区 awesome 列表 + 中英文技术媒体
- 相关研究对照：本仓库同目录 `hermes-agent-study` 与 `claude-code-source-analysis`

## 许可

本研究为独立分析，所有源码引用遵循原项目 License。研究文本本身采用 CC BY-SA 4.0。
