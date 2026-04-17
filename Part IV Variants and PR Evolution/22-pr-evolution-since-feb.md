# 22 二月至今 PR 演进全景

## 数据源

- [analysis-summary.json](../Appendix/B-pr-issue-dataset/20260417/analysis-summary.json)：1200 条已合并 PR（2026-02-01 ~ 2026-04-17）
- [authors-top50.txt](../Appendix/B-pr-issue-dataset/20260417/authors-top50.txt)、[path-frequency-top40.txt](../Appendix/B-pr-issue-dataset/20260417/path-frequency-top40.txt)
- [commits-by-month.txt](../Appendix/B-pr-issue-dataset/20260417/commits-by-month.txt)：月度 commit 聚合
- [commit-theme-by-month.csv](../Appendix/B-pr-issue-dataset/20260417/commit-theme-by-month.csv)、[commit-themes-summary.json](../Appendix/B-pr-issue-dataset/20260417/commit-themes-summary.json)

## 一、体量与节奏

### 1.1 commit 月度分布

| 月份 | commit 数 |
|---|---|
| 2026-01 | 32 |
| 2026-02 | 6985 |
| 2026-03 | 8710 |
| 2026-04（17 日止）| 7287 |
| 合计 | **~23014** |

1 月还是安静期，2 月爆发（见第 23 章）。3 月保持峰值，4 月在 17 天已达 7287——接近月度新高。

### 1.2 PR 月度分布（按 pr_by_month_theme 汇总）

- **2026-02**：561 条合入
- **2026-03**：42 条合入（因为 3 月数据窗口偏窄，多条仍 open）
- **2026-04**：872 条合入

4 月的活跃度集中，说明 CVE 后修复 + 通道扩展 + 模型接入同时推进。

## 二、按主题的总占比（1200 条）

| 主题 | PR 数 | 占比 |
|---|---|---|
| channels-messaging | 250 | 20.8% |
| uncategorized | 227 | 18.9% |
| models-providers | 172 | 14.3% |
| memory-context | 145 | 12.1% |
| security-sandbox | 139 | 11.6% |
| agent-session | 128 | 10.7% |
| ci-build-infra | 102 | 8.5% |
| docs-i18n | 91 | 7.6% |
| browser-tools | 57 | 4.8% |
| canvas-ui-web | 55 | 4.6% |
| image-video-media | 52 | 4.3% |
| onboarding-setup | 48 | 4.0% |
| cron-schedule | 46 | 3.8% |
| voice-audio | 43 | 3.6% |
| apps-mobile | 27 | 2.3% |
| skills-hub | 26 | 2.2% |
| webhooks-integrations | 7 | 0.6% |

**三大重点：**channel / model / memory + security。这四块占 PR 近 60%。

## 三、按 Label 的 channel 细分

从 [pr_labels_top](../Appendix/B-pr-issue-dataset/20260417/analysis-summary.json)：

| label | PR 数 |
|---|---|
| channel: telegram | 78 |
| channel: discord | 54 |
| channel: msteams | 41 |
| channel: matrix | 38 |
| channel: whatsapp-web | 36 |
| channel: slack | 35 |
| channel: feishu | 23 |
| channel: bluebubbles | 20 |
| channel: imessage | 17 |
| channel: mattermost | 13 |
| channel: qqbot | 13 |
| channel: voice-call | 12 |

**观察**：Telegram 依然领先，但 MS Teams / Matrix 增速最快——明显的企业向倾斜。

## 四、按源码路径（commit 层）

[path-frequency-top40.txt](../Appendix/B-pr-issue-dataset/20260417/path-frequency-top40.txt) top 10：

| 路径 | commit 数 |
|---|---|
| src/agents | 14536 |
| src/gateway | 5576 |
| src/commands | 5458 |
| src/auto-reply | 5042 |
| src/infra | 4910 |
| src/plugins | 4750 |
| CHANGELOG.md | 4687 |
| src/config | 3832 |
| ui/src | 3690 |
| src/plugin-sdk | 3404 |

**结论**：`src/agents` 是最活跃的一块——agent loop 改造、session 模型、prompt 调优的主战场。其次是 Gateway、commands、auto-reply、plugin 几个核心设施。

## 五、Top 作者（前 20 名）

（来自 [authors-top50.txt](../Appendix/B-pr-issue-dataset/20260417/authors-top50.txt)）

| 作者 | PR 数 |
|---|---|
| vincentkoc | 61 |
| mbelinky | 43 |
| gumadeiras | 39 |
| eleqtrizit | 33 |
| obviyus | 30 |
| Takhoffman | 29 |
| hxy91819 | 28 |
| pgondhi987 | 24 |
| mcaxtr | 23 |
| neeravmakwana | 23 |
| sebslight / ngutman | 20 |
| sudie-codes | 19 |
| quotentiroler / jalehman / openperf | 17 |
| tyler6204 / 100yenadmin | 15 |
| vignesh07 | 14 |
| steipete | 13 |

**观察**：

- Peter Steinberger（`steipete`）作为 founder 合入量中等，主要做方向把控而不亲自堆 PR
- vincentkoc、mbelinky、gumadeiras、eleqtrizit 四人是高产外部 contributor
- `hxy91819`（28 PR）可能是中文生态活跃的一条线索

## 六、按月 × 主题交叉（2026-02 vs 2026-04）

| 主题 | Feb | Apr | 增长 |
|---|---|---|---|
| channels-messaging | 89 | 158 | +77% |
| security-sandbox | 41 | 96 | +134% |
| models-providers | 39 | 129 | +231% |
| memory-context | 36 | 104 | +189% |
| agent-session | 43 | 76 | +77% |
| canvas-ui-web | 19 | 34 | +79% |
| image-video-media | 11 | 37 | +236% |
| voice-audio | 8 | 31 | +288% |
| cron-schedule | 12 | 32 | +167% |
| browser-tools | 18 | 39 | +117% |

**最猛增长的三条**：voice-audio / image-video-media / models-providers。结合 CVE 时间线，4 月已从"修洞"转回"加能力"。

## 七、Top 50 PR 主题（定性归纳）

根据 `prs-merged-p1..p8.json` 抽样，top PR 集中在：

1. **channel coverage**：MS Teams v2 adapter、Matrix E2EE、WhatsApp Web 多设备支持
2. **provider expansion**：DeepSeek V4 tool_call、ZhipuAI 新模型、GPT 5.4 stream
3. **security fixes**：tool policy schema、pairing strict fallback、sandbox 镜像升级
4. **memory**：memory-wiki 本地 vector、LanceDB upsert、Dreaming 定时优化
5. **voice**：barge-in 稳定性、SIP 兼容、Sherpa-ONNX 中文声音加入
6. **canvas**：form validation、chart fallback、UI 组件扩展
7. **onboarding**：doctor 命令扩展、wizard 多语言、setup 时效优化

## 八、生态节奏的三条解读

1. **事件驱动型爆发**：1 月安静—2 月爆发—3 月持续—4 月转向加能力。此节奏与 CVE-2026-25253 公开时间（3 月）吻合
2. **核心贡献者稀疏**：99% PR 由 50 位作者完成；高产 3 位（Peter、Vincent、Tak）占比尤其高
3. **去中心化 contributor 长尾**：next-50 名作者每人 2-5 条 PR，覆盖 channel 和 model provider 各 角落——生态"宽但浅"

## 下一章预告

第二十三章把 fork 分析与 PR 演进综合起来，归纳**社区关注的能力增强**，形成对 OpenClaw 未来 3-6 月重点方向的自上而下结论。
