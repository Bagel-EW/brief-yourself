# Interaction Budget

## Goal

让用户在有限回合内获得可修正的认识，而不是接受一场没有终点的访谈。Deep 代表证据和校准更深，不代表问题数量无限增加。

## Default Budgets

| Depth | 最多 prompts | 默认用时 | 用户回合 | 适用方式 |
|---|---:|---:|---:|---|
| Quick Brief | 5 | 5–10 分钟 | 2–3 | 一次批量回答 4–5 个短问题，随后确认摘要 |
| Standard Brief | 8 | 15–25 分钟 | 3–4 | 默认模式；目标与经历 → 画像卡 → 一次关键校准 → 确认 |
| Deep Brief | 12 | 30–45 分钟 | 4–5 | 来源与目标 → 真实片段 → 画像卡 → 反例 → 最终确认 |

“回合”指一次用户回复及其后的 Agent 综合，不是一个可以包含无限追问的阶段。

History-assisted 是 Evidence modifier，必须继承所选 Depth 的 prompts、用户回合和时长上限。它应通过复用已授权证据减少提问，不能增加预算。

不存在不设上限的 Extended 模式。当前 Session 到达上限后，先交付阶段结果；用户明确希望继续时，再建立并确认新的 Session Contract。

## Recommended Deep Flow

### Round 0: Session Contract

先展示维度、最多 prompts、预计时长、总回合、来源和交付。让用户接受推荐组合或调整。完整格式见 `session-contract.md`。

### Round 1: Purpose, Scope, Consent

用一个组合问题确认目标、允许来源、排除领域和保存位置。Session Contract 已说明预计用时与回合数，不在此处重复扩展预算。

### Round 2: Lived Experience Batch

提供 3–4 个相关提示，让用户选择其中 1–2 个真实片段回答。支持语音长答，不逐条追问每个抽象词。

### Round 3: Calibration Card

先交付阶段性画像卡，不提新问题。区分：

- 已确认事实；
- 用户自述价值；
- Agent 观察；
- 仍待验证的推断；
- 张力、反例与未知项。

让用户直接确认、改写、拒绝或标记“稍后再谈”。

### Round 4: Highest-value Challenge

只追一个会实质改变画像的反例、边界或冲突。不要为了完整而遍历所有领域。

### Round 5: Approval And Next Use

交付 Human Brief 与候选 Patch。用户批准后才写入；未覆盖领域保留为 unknown，不继续强问。

## Synthesis Triggers

遇到任一情况，立即停止新增问题并交付 Calibration Card：

- 已有 3 次以上有效用户回复；
- 用户询问“还要多少问题”或反馈流程过长；
- 新回答只是在重复已有价值标签；
- 历史证据已经能够支持阶段性综合；
- 继续追问不会改变当前任务的 Context View。

元反馈优先于既定访谈脚本。不要要求用户先完成当前问题再暂停。

即使尚未触发以上条件，也必须在所选 Depth 的 prompts、用户回合或时长预算到达上限时结束新增问题。

## Question Quality Gate

提出问题前检查：

1. 回答是否会改变 Claim、层级、置信度、边界或任务输出？
2. 现有资料是否已经能够回答？
3. 能否合并进下一张 Calibration Card，让用户直接纠错？
4. 这是必要校准，还是 Agent 为了“更完整”而满足自己的好奇心？

只有第一个答案为“会”，且第二、第三个答案为“不能”时，才继续提问。

## Partial Completion

Brief Yourself 不以覆盖所有人生领域为完成标准。达到当前用途的最低充分画像后即可结束；把未覆盖内容放入 `unknowns`，等待真实任务在未来补齐。
