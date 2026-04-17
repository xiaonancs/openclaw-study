# 03 Agent 与 Session 模型

## 本章外部视角

[Enrico Piovano 的架构 Deep Dive](https://enricopiovano.com/blog/openclaw-architecture-deep-dive) 把 OpenClaw 的 agent 概括为 "Pi Agent Core" 的 Think-Act-Observe 循环；[Towards AI 的 Sunil Rao](https://pub.towardsai.net/openclaw-personal-ai-assistant-that-actually-does-your-work-538588507155) 强调 "one agent per persona" 而不是 "one agent per task"。但两篇都没讲清楚 agent 和 session 的关系——这正是本章要补的盲点，资料来源于 [docs/concepts/agent-loop.md](../../openclaw-repo/docs/concepts/agent-loop.md)、[docs/concepts/session.md](../../openclaw-repo/docs/concepts/session.md)、[src/agents/](../../openclaw-repo/src/agents) 的 1312 个 TS 文件、[src/sessions/](../../openclaw-repo/src/sessions) 的核心代码。

## 一、本质是什么

OpenClaw 里有两个被混用但结构不同的概念：

- **Agent**：一个"人设"（persona）。有名字、soul 描述、关联的 skills、默认 model、默认 channel。
- **Session**：一条具体的对话。每次在 IM 里和 agent 说话都对应一个 session；iOS Node 的 dictation 也是一个 session；CLI 的 `openclaw chat` 还是一个 session。

**一个 agent 拥有多个 session**，session 是 agent 的"容器"。会话记忆、工具使用历史、中断状态都绑定在 session 上，不绑定在 agent 上。

[docs/concepts/agent-loop.md:5-11](../../openclaw-repo/docs/concepts/agent-loop.md) 给出了精确定义：

> An agentic loop is the full "real" run of an agent: intake → context assembly → model inference → tool execution → streaming replies → persistence. … In OpenClaw, a loop is a single, serialized run per session.

"per session"三个字是理解整个系统的关键。沙箱、限流、main/non-main 权限、记忆加载、事件流都在 session 尺度上工作。

## 二、核心问题和痛点

把 agent 和 session 分离要解决五个真实的工程问题：

1. **并发消息**：同一个 agent 被多个 channel 同时 @，不能让两个 loop 互相污染 context
2. **一致性**：一条长任务（比如 coding-agent 改代码）中途不能被另一条消息打断
3. **权限分层**：`main` session（用户自己 DM）能执行敏感命令，`non-main` session（群聊/回复来自陌生人）必须沙箱
4. **跨端同步**：macOS、iOS、WebChat 都看的是同一个"对话"，session 必须跨客户端共享
5. **记忆归属**：记忆属于 agent（长期），也属于 session（短期）——两者如何合并

## 三、解决思路与方案

OpenClaw 的答法是"**agent 定义人设 + session 承载运行时 + loop 串行化每个 session 的执行**"。

<div style="background: #ffffff !important; background-color: #ffffff !important; padding: 16px; border-radius: 8px; margin: 16px 0;" bgcolor="#ffffff">

```mermaid
%%{init: {'theme': 'neutral', 'themeVariables': {'background': '#ffffff', 'primaryColor': '#f5f5f5', 'primaryTextColor': '#000000', 'primaryBorderColor': '#333333', 'lineColor': '#444444', 'textColor': '#000000', 'mainBkg': '#f5f5f5', 'nodeBorder': '#333333', 'clusterBkg': '#fafafa', 'clusterBorder': '#888888', 'edgeLabelBackground': '#ffffff', 'actorBkg': '#f5f5f5', 'actorBorder': '#333333', 'actorTextColor': '#000000', 'actorLineColor': '#444444', 'signalColor': '#444444', 'signalTextColor': '#000000', 'noteBkgColor': '#f0f0f0', 'noteTextColor': '#000000', 'noteBorderColor': '#888888'}}}%%
flowchart TD
    subgraph AgentDefinition
        SoulMD[SOUL.md]
        AgentsMD[AGENTS.md]
        ToolsMD[TOOLS.md]
        SkillsDecl[skills[]]
        DefaultModel[default model]
    end

    subgraph SessionRuntime
        SKey["sessionKey<br/>main / group / dm / custom"]
        LoopQ[loop queue]
        CtxBuild[context assembly]
        EmbeddedPi[runEmbeddedPiAgent]
        Stream[stream events]
    end

    subgraph MemoryLayer
        LongTerm["MEMORY.md<br/>(agent)"]
        Daily["memory/YYYY-MM-DD.md"]
        ShortTerm["session transcript"]
    end

    AgentDefinition -.load at start.-> CtxBuild
    SKey --> LoopQ
    LoopQ --> EmbeddedPi
    EmbeddedPi --> Stream
    LongTerm --> CtxBuild
    Daily --> CtxBuild
    ShortTerm --> CtxBuild
    CtxBuild --> EmbeddedPi
```

</div>

[docs/concepts/agent-loop.md:21-40](../../openclaw-repo/docs/concepts/agent-loop.md) 用四步描述了 loop 的骨架：

1. `agent` RPC validates params, resolves session, persists session metadata, returns `{ runId, acceptedAt }` **immediately**（异步确认 + 后台 loop）
2. `agentCommand` 加载 skills snapshot、resolve model、调用 `runEmbeddedPiAgent`
3. `runEmbeddedPiAgent` 内部**串行化**执行（per-session queue + global queue）
4. `subscribeEmbeddedPiSession` 把底层事件桥接成 `agent` stream

"immediately 返回 runId"是关键设计——client 不必在 WS 上等 loop 完成，而是通过订阅 `stream: "assistant"` 增量接收 token。

## 四、实现细节关键点

### 4.1 session key 决定 main/non-main

[docs/gateway/sandboxing.md 模式说明](../../openclaw-repo/docs/gateway/sandboxing.md) 明确：

> `"non-main"` is based on `session.mainKey` (default `"main"`), not agent id. Group/channel sessions use their own keys, so they count as non-main and will be sandboxed.

也就是说 `sessionKey == "main"` 是一等公民，其他所有 session 都是二等公民。群聊、来自陌生人的 DM、`openclaw chat --session=test` 生成的临时 session 都被视为 non-main，默认进沙箱。这是 OpenClaw 身份优先安全模型的具体体现（对应 [Part II Ch13](../Part%20II%20Source%20Execution/13-security-sandbox-pairing.md)）。

### 4.2 skills snapshot 在 loop 开头冻结

`agentCommand` 在 loop 开始前**冻结 skills snapshot**——即便 ClawHub 在 loop 执行过程中推了 skill 更新，本次 loop 依然使用起跑时的版本。这是为了防止"中途换 skill 导致语义漂移"。对应源码路径（[src/agents/agent-command.ts](../../openclaw-repo/src/agents/agent-command.ts)）。

### 4.3 anthropic/openai payload 有单独 policy

[src/agents/anthropic-payload-policy.ts](../../openclaw-repo/src/agents/anthropic-payload-policy.ts)、[src/agents/anthropic-transport-stream.ts](../../openclaw-repo/src/agents/anthropic-transport-stream.ts)、[src/agents/anthropic-vertex-stream.ts](../../openclaw-repo/src/agents/anthropic-vertex-stream.ts) 表明 agent 层专门为 Anthropic 系（含 Vertex）写了 payload 组装 + transport 的策略文件。类似文件 OpenAI、Google、xAI、DeepSeek、Qwen 都有一套。这暗示 agent 对"模型 provider 异质性"的处理是通过**per-provider policy 模块**做的，而不是靠统一抽象。代价是代码量大，收益是每个 provider 的边界条件能深度定制。

### 4.4 ACP spawn parent stream

[src/agents/acp-spawn.ts](../../openclaw-repo/src/agents/acp-spawn.ts) 和 [src/agents/acp-spawn-parent-stream.ts](../../openclaw-repo/src/agents/acp-spawn-parent-stream.ts) 实现 **Agent Communication Protocol**（ACP）——agent 可以 spawn 另一个 agent 作为子 agent，父子之间通过 stream 交换。这对应 [docs/concepts/multi-agent.md](../../openclaw-repo/docs/concepts/multi-agent.md) 中 "supervisor / worker" 模式。注意这是 2026-03 才正式定稿的协议（见 [全网调研](../全网调研-社区认知地图.md) 盲区部分 ACP 一项）。

### 4.5 session pruning 与 compaction 对偶

[docs/concepts/session-pruning.md](../../openclaw-repo/docs/concepts/session-pruning.md) 与 [docs/concepts/compaction.md](../../openclaw-repo/docs/concepts/compaction.md) 是两个相关但不同的机制：

- **pruning**：删除 session transcript 里的老消息（减小上下文）
- **compaction**：把老消息压成摘要，写回到 session（保留语义）

默认 compaction 先行、pruning 兜底。[docs/concepts/memory.md](../../openclaw-repo/docs/concepts/memory.md) 提到"Before compaction summarizes conversations, OpenClaw runs an automatic memory flush to save important context to memory files" —— flush 把重要事实写到 `MEMORY.md`，compaction 才能安全地丢弃原文。

### 4.6 session-tool 把 session 本身变成工具

[docs/concepts/session-tool.md](../../openclaw-repo/docs/concepts/session-tool.md) 描述一个有趣设计：agent 可以调用 `session_*` 工具来开新 session、切 session、查 session 列表。也就是 agent 自己能编排自己的 sessions，这是 OpenClaw 区别于一般 agent 框架的关键点——session 不是隐式容器，而是一等资源。

## 五、易错点和注意事项

1. **sessionKey 必须稳定**：相同 channel + 相同用户必须映射到相同 sessionKey。否则记忆不会跨消息延续。`src/gateway/session-store-key.ts` 是 key 生成算法，不要手动绕过
2. **"main" 的定义**：不是 agent 名叫 main，也不是 session id 等于 "main"，而是 `session.mainKey == "main"` —— 通过 `mainKey` 字段显式声明
3. **runId 和 sessionId 是两级**：一个 session 可以有多次 run（每次发消息都是新 run），runId 是具体执行实例，sessionId 是持久容器
4. **skills snapshot 在哪一刻冻结决定版本**：如果用户在 loop 进行中 `openclaw skills update`，本次 loop 不受影响；下次才生效
5. **并发**：同一 session 发两条消息，第二条会排队；两个不同 session 可以并发
6. **ACP 尚不稳定**：父 agent spawn 子 agent 时的异常传递、取消传递还有边界，[CHANGELOG](../../openclaw-repo/CHANGELOG.md) 里 2026-03 / 04 有多次 acp 相关 fix

## 六、竞品对比

| 维度 | OpenClaw | Claude Code | Codex | hermes-agent |
|------|----------|-------------|-------|--------------|
| 多 session | 支持、一等资源 | 单 session per 进程 | 多 session、web-managed | 多 session |
| main/non-main | 身份分级 | 无分级 | user-scoped | 无显式分级 |
| agent persona | SOUL.md 明文 | system prompt 内嵌 | 内置 system | 类似 SOUL |
| multi-agent | ACP + supervisor | 无 | 无 | 有（自有协议） |
| 会话记忆 | MD + vector | 无持久化 | 无 | 自管理 |

## 七、仍存在的问题和缺陷

1. **loop 排队是全局的**：per-session + global 两层队列，意味着一个 session 的慢任务会拖延别的 session 的 loop 起跑——有测试里看到的 `agent-command.live-model-switch.test.ts` 处理相关场景，但实际用户侧仍报告"消息积压"（[Issue #8731](https://github.com/openclaw/openclaw/issues/8731)）
2. **session 粒度的记忆是隐式的**：用户不知道哪些片段会进入下次 context。需要 "memory inspector" 之类工具——目前只有概念级文档
3. **ACP 协议仍在演化**：2026-03 才定稿，社区实战样本少（相关 skill 只有 2 个）
4. **agent persona 的版本化缺失**：SOUL.md 没有 schema 版本，修改后无法 rollback；出事故时难以复盘

## 下一章预告

第四章进入 **Context Engine 与记忆体系**，把"context 到底怎么装出来"这一步具象化——MD 文件、vector 索引、dreaming/compaction 如何在 agent loop 的 `intake → context assembly` 这一步协作。
