# 25 源码层设计问题

## 本章目的

汇总 Part I-III 每章末尾的"仍存在的问题和缺陷"，与第 24 章的 issue 聚类交叉验证，按"设计级 / 工程级 / UX 级"三档严重度分级，形成 **OpenClaw 源码层已知问题全图**。

## 一、严重度评级方法

| 等级 | 定义 |
|---|---|
| 🔴 高 | 设计层面，修复需改模型 / 协议；影响多 session / 安全 / 合规 |
| 🟠 中 | 工程层面，修复需改代码结构；影响稳定性 / 性能 / 可维护性 |
| 🟡 低 | UX / DX 层面；不影响正确性，影响新人上手 / 可观测性 |

## 二、全图（按章来源）

### 2.1 Part I：架构与哲学

| 问题 | 来源章节 | 严重度 | 备注 |
|---|---|---|---|
| Gateway 单点 WS，无横向扩展 | 02 | 🔴 | 个人场景尚可；进入团队/企业即成瓶颈 |
| main/non-main session 边界曾存在 RCE（已修） | 03 | 🔴 | CVE-2026-25253，需要制度化验证 |
| Context Engine 缺"来源追溯" | 04 | 🟠 | 用户无法看"这段 context 从何处来" |
| Dreaming / Compaction 过度精简记忆 | 04 | 🟠 | issue #T3 |
| extension default-deny 不够细粒度 | 05 | 🟠 | per-tool 授权仍宽 |
| skill marketplace 反作弊浅 | 06 | 🔴 | ClawHavoc 后仍主要靠 VirusTotal |

### 2.2 Part II：执行链路

| 问题 | 来源章节 | 严重度 | 备注 |
|---|---|---|---|
| 启动路径的 respawn 机制隐晦 | 07 | 🟡 | 维护者学习曲线高 |
| CLI 命令树分散 3+ 目录 | 08 | 🟡 | 新 contributor 找不到 |
| routing rule DSL 弱（无组合条件） | 09 | 🟠 | 企业路由能力受限 |
| hooks 顺序无拓扑解析 | 09 | 🟠 | 多 plugin 干扰会飘 |
| auto-reply 策略对用户不透明 | 09 | 🟡 | 常见"新用户以为不工作" |
| A2UI 组件变更需要三端同步发版 | 10 | 🟠 | 发布节奏风险 |
| Node WS 断线无离线消息队列 | 10 | 🟠 | 手机 node 场景常见 |
| 语音延迟抖动 / failover 不够自动 | 11 | 🟠 | 中文 TTS 弱 |
| 异步 media job 失败无标准通知 | 12 | 🟠 | Runway 超时等 |
| link-understanding cache 非全局共享 | 12 | 🟡 | 成本浪费 |
| pairing UX 粗糙，新用户流程长 | 13 | 🟠 | onboarding 失败主因 |
| sandbox 冷启动秒级延迟 | 13 | 🟡 | 首次体验差 |
| tool policy 部分老 extension 未迁移 | 13 | 🔴 | 默认 `any` 灰色地带 |

### 2.3 Part III：通道 / 模型 / 端

| 问题 | 来源章节 | 严重度 | 备注 |
|---|---|---|---|
| channel health 缺统一监控 | 14 | 🟠 | webhook 掉线难以察觉 |
| 多通道同一用户身份合并弱 | 14 | 🟠 | 多 device 场景错乱 |
| provider 治理无版本冻结 | 15 | 🟠 | 新 model silent break |
| cost 展示 / 配额仪表盘缺失 | 15 | 🟠 | 钱包风险 |
| embedding provider 分散 | 15 | 🟡 | memory 多后端各自算 |
| gateway provider 循环风险保护不足 | 15 | 🟠 | OpenRouter → Anthropic → OpenRouter |
| 微信 / 钉钉 / 企业微信官方空白 | 16 | 🔴 | 最大 RoI 缺口 |
| 国产模型 tool_call 兼容参差 | 16 | 🟠 | 需 adapter 层归一 |
| 合规"仅境内"总开关缺 | 16 | 🔴 | 企业合规需求 |
| docs 中文本地化弱 | 16 | 🟡 | 入门曲线陡 |
| Sherpa-ONNX 中文 TTS 质量短板 | 16 | 🟠 | voice 核心体验 |
| skill 无 semver 版本治理 | 17 | 🟠 | 名字覆盖发布风险 |
| skill reputation 体系粗糙 | 17 | 🟠 | ClawHub 安全层浅 |
| macOS App 老系统兼容性 | 18 | 🟡 | < macOS 13 退化 |
| 远端 Gateway 状态一致性抖动 | 18 | 🟡 | 断网重连 |
| iOS 能力上限低 | 19 | 🔴 | 系统约束，难解 |
| Android 厂商杀后台 | 19 | 🔴 | 生态级问题 |
| capability discoverability 弱 | 19 | 🟡 | 用户不知"能做什么" |

## 三、高严重度问题的再分组

把所有 🔴 放在一起：

1. **Gateway 横向扩展**（02）—— 架构级限制
2. **main/non-main 安全边界**（03, 13）—— 需要制度化回归测试
3. **skill marketplace 反作弊**（06, 17）—— 行为级检测仍未完善
4. **tool policy 老 extension 未全迁移**（13）—— 存量代码遗留
5. **微信 / 钉钉 / 企业微信官方空白**（16）—— 中国市场最大缺口
6. **合规"仅境内"总开关缺**（16）—— 企业合规刚需
7. **iOS 能力上限**（19）—— 平台级限制
8. **Android 厂商杀后台**（19）—— 生态级限制

→ 共 8 条"设计级"问题，其中 5 条是"必须改"（社区可改），3 条是"平台约束"（需对外协作）。

## 四、高中严重度问题与 Issue 量交叉验证

把第 24 章的 Top 10 抱怨与源码问题对照：

| 抱怨（issue 侧） | 对应源码问题 |
|---|---|
| T1 Agent 回错人 | pairing UX（13）+ auto-reply 不透明（09）|
| T2 failover 造成行为飘 | provider 治理无版本冻结（15） |
| T3 Memory 遗忘 / 重复 | Dreaming/Compaction 过度精简（04） |
| T4 CPU/mem 高 | Node WS 无离线队列（10）+ Sherpa CPU（11） |
| T5 webhook 掉线 | channel health 缺监控（14） |
| T6 CI 失败 | 多矩阵基础设施问题（工程级） |
| T7 Canvas 渲染错位 | A2UI 三端发版脱钩（10） |
| T8 Android 被杀 | 厂商生态约束（19） |
| T9 onboarding 难 | pairing UX（13）+ doctor 不够智能 |
| T10 docs 过期 | docs 中文本地化（16）+ maintainer 不足 |

→ 10 条 issue 痛点中 **8 条** 在本章有源码层对应。映射度高意味着用户反馈真实反映设计缺陷。

## 五、推演优先级（为第 26 章铺垫）

按"严重度 × 社区可改 × RoI"三维打分：

| 问题 | 严重度 | 可改性 | RoI | 排序 |
|---|---|---|---|---|
| 微信/钉钉/企业微信官方化 | 🔴 | 高 | 极高 | **P0** |
| tool policy 全量迁移 + 老 extension 补声明 | 🔴 | 高 | 高 | **P0** |
| skill reputation + 行为检测 | 🔴 | 中 | 高 | **P1** |
| Gateway 可观测（channel health / cost / provider） | 🟠 | 高 | 高 | **P1** |
| Memory 质量（compaction 策略 + 来源可见） | 🟠 | 中 | 高 | **P1** |
| A2UI 版本治理 / fallback 契约 | 🟠 | 高 | 中 | P2 |
| voice 延迟 + 中文 TTS | 🟠 | 中 | 高 | P2 |
| docs 中文本地化 | 🟡 | 高 | 高 | P2 |
| mobile node 厂商白名单 guide | 🟠 | 低（依赖厂商） | 中 | P3 |
| iOS 能力深化（Shortcut bridge） | 🔴 | 低 | 中 | P3 |

P0-P3 的具体 roadmap 建议见第 26 章。

## 下一章预告

第二十六章基于本章的问题清单，给出未来 3-6 月的六条重点优化方向，每条配源码证据、修复路径、成功指标。
