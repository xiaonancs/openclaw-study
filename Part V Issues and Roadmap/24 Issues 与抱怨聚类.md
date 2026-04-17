# 24 Issues 与抱怨聚类

## 数据源

- [analysis-summary.json](../Appendix/B-pr-issue-dataset/20260417/analysis-summary.json)：issues 2026-01-01 以后 420 条
- [issues-p1..p3.json](../Appendix/B-pr-issue-dataset/20260417/issues-p1.json) + [issues-updated-p1..p3.json](../Appendix/B-pr-issue-dataset/20260417/issues-updated-p1.json)

采集时间 2026-04-17。

## 一、总体状态

| 指标 | 数值 |
|---|---|
| 总 issue 数（近 4 个月）| 420 |
| open | 278 |
| closed | 142 |
| closed 率 | 33.8% |

## 二、按主题的聚类

| 主题 | issue 数 | 占比 |
|---|---|---|
| agent-session | 176 | 41.9% |
| channels-messaging | 149 | 35.5% |
| models-providers | 140 | 33.3% |
| ci-build-infra | 126 | 30.0% |
| memory-context | 85 | 20.2% |
| onboarding-setup | 71 | 16.9% |
| canvas-ui-web | 68 | 16.2% |
| browser-tools | 65 | 15.5% |
| apps-mobile | 59 | 14.0% |
| security-sandbox | 49 | 11.7% |
| image-video-media | 38 | 9.0% |
| cron-schedule | 30 | 7.1% |
| skills-hub | 16 | 3.8% |
| voice-audio | 15 | 3.6% |
| webhooks-integrations | 12 | 2.9% |
| uncategorized | 9 | 2.1% |

（注：主题可多标签，总和 > 100%）

**Top 5 抱怨主题**：agent-session、channels-messaging、models-providers、ci-build-infra、memory-context。

## 三、按 Label

| label | count |
|---|---|
| stale | 150 |
| bug | 125 |
| regression | 56 |
| bug:behavior | 43 |
| enhancement | 11 |
| bug:crash | 8 |
| maintainer | 2 |

**观察**：

- stale 占 35.7%——issue triage 跟不上
- regression 占 13.3%——破坏性修改导致回归频发
- enhancement 只占 2.6%——用户更多是"坏了"不是"想要新功能"

## 四、月度趋势（issue 新增）

| 月份 | 总数 | agent-session | models-providers | channels-messaging |
|---|---|---|---|---|
| 2026-02 | 10 | - | 2 | - |
| 2026-03 | 162 | 74 | 50 | 76 |
| 2026-04 | 248 | 102 | 88 | 73 |

**结论**：issue 量 3 月暴涨（162 条）、4 月继续（248 条）。与 commit 峰值（第 22 章）对应——改的多，坏的就多；regression 占比升高印证。

## 五、十大痛点主题

从 issues-p* 抽样 + 关键词共现，识别出以下 **Top 10 抱怨**：

### T1. Agent 回错人 / 越权

- channel 内 agent 被非预期用户触发
- 未配对 DM 莫名其妙走进 agent loop（pairing 配置坑）
- 根因：pairing 策略文档不清 / settings 迁移默认值

### T2. model failover 造成行为不一致

- "同样的 prompt，一会儿 Claude 一会儿 DeepSeek，输出差太多"
- failover 无提示
- 根因：failover 对用户不透明

### T3. Memory 遗忘 / 重复

- "昨天说过今天问我还重复问"
- "Dreaming 把重要事件合并掉了"
- 根因：compaction 策略保守有余，保留不足

### T4. Agent 占用 CPU / 内存过高

- 实时语音常开时 Sherpa-ONNX CPU 满
- Playwright 进程残留
- 根因：生命周期清理不完整

### T5. channel webhook 掉线

- MS Teams / Slack webhook 经常要手动重启
- cloudflared tunnel 飘
- 根因：缺少统一 health + auto-restart

### T6. CI 失败阻塞本地 dev

- CI build-infra 占 126 issue
- pnpm / docker 版本漂移
- 根因：多 OS 多 Node 版本矩阵膨胀

### T7. Canvas 渲染错位 / 组件不支持

- 老版 macOS App 不支持新 A2UI 组件
- fallback 文本语序乱
- 根因：客户端与 canvas schema 发版脱钩

### T8. Android node 被系统杀

- 华为 / 小米 / OPPO 机型 30min 就被冻结
- 后台通知 WS 断线
- 根因：无法绕开厂商电源管理

### T9. 新用户 onboarding 太硬

- `openclaw setup` 成功率低，常卡在 Docker / pnpm / Node 版本
- wizard 对非英语用户不友好
- 根因：doctor 仍不够智能

### T10. 文档空洞或过期

- 配置字段 docs 没跟进代码
- i18n 中文教程零散
- 根因：docs-i18n 团队无官方成员

## 六、stale 比率说明什么

150 / 420 ≈ 35.7% 的 issue 被 stale 机器人关闭。这种高比率反映的不是"问题已解决"，而是：

- 非 maintainer 回复少
- 某些 issue 缺 repro，但作者不再跟进
- stale 机制过激（常见 30 天无活动）

建议 roadmap 里包含 **"缩短 stale 触发时间 + 人工 triage labeler"** 的一条。

## 七、分布对比：issue 痛点 vs PR 关注

| 领域 | issue 占比 | PR 占比 | gap 解读 |
|---|---|---|---|
| agent-session | 41.9% | 10.7% | **痛点大，修得少** |
| channels-messaging | 35.5% | 20.8% | 基本对应 |
| models-providers | 33.3% | 14.3% | **痛点大于投入** |
| ci-build-infra | 30% | 8.5% | **严重 gap** |
| apps-mobile | 14% | 2.3% | **gap** |

**结论**：**agent-session 逻辑问题、CI 基础设施、移动端稳定性** 是 issue 量 ≫ PR 量最显著的三个领域。官方投入与用户痛点存在错位。

## 下一章预告

第二十五章汇总 Part I-III 每章末尾的"仍存在的问题和缺陷"，形成源码层级的设计问题全图。
