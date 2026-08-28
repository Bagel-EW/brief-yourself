# Session Contract

## Purpose

在提出第一个自我探索问题之前，让用户清楚知道：会探索什么、最多问多少、预计多久、使用哪些资料、最后得到什么，以及如何暂停或跳过。

不要用“我们先聊聊”启动一个没有边界的访谈。

当用户意图已经足够明确时，第一条访谈回复就展示完整 Session Contract。只允许在用户接受前询问 Contract 选择或资料授权，不得提前索取个人经历、价值判断或其他探索内容。

## Three Choices

### 1. Depth

| Depth | 预计用时 | 最多 prompts | 用户回合 | 默认覆盖 |
|---|---:|---:|---:|---|
| Quick | 5–10 分钟 | 5 | 2–3 | 当前目标、关键经历、优势/困难、边界 |
| Standard | 15–25 分钟 | 8 | 3–4 | 再加入价值动机、工作方式、协作与公开表达 |
| Deep | 30–45 分钟 | 12 | 4–5 | 再加入跨场景差异、反例、变化轨迹与未知项 |

默认推荐 `Standard`。Deep 不代表一次穷尽人生；达到当前用途的最低充分画像后仍应结束。

### 2. Evidence

- **Conversation-only**：只使用本次回答。
- **Materials-assisted**：读取用户主动提供的简历、项目或笔记。
- **History-assisted**：经授权按需检索历史对话与项目记录，再让用户纠错。

Evidence 是取证方式，不是另一种深度。History-assisted 应减少直接提问，不能在读取大量历史后再重复问一遍。

History-assisted 必须继承所选 Depth 的最多 prompts、用户回合和预计时长，不能拥有独立预算或延长当前 Session。

### 3. Pace

- **Guided chat**：每回合一个主题，显示 `Round N / Total`。
- **Batch**：先展示完整 Question Map，用户一次回答多个 prompts。
- **Voice-friendly**：用户自由口述，Agent 自行映射维度，只追影响结论的缺口。

Agent 应推荐一个默认组合，用户可以直接接受或用自然语言调整，不要求填写复杂表单。

## Counting Rules

- `prompt` 指需要用户提供新信息或作出判断的一个独立请求；同一句中的多个独立问题分别计数。
- `用户回合` 指用户接受 Session Contract 后的一次回复及其后的 Agent 综合；Contract 的接受或调整不计入探索回合。
- Agent 的复述、Calibration Card 和最终交付不计入 prompt，但其中夹带的新问题必须计数。
- 三项预算任一达到上限，或触发停止条件时，立即停止新增问题并交付阶段结果。

## Dimension Map

1. 当前状态与使用目的；
2. 重要经历、选择与转折；
3. 价值观与动机；
4. 能力模式、工作方式与摩擦；
5. 协作、关系与交换边界；
6. 现实、职业、公开与任务中的不同自我；
7. 张力、反例、限制与不可牺牲项；
8. 变化轨迹、未知项与下一次复核。

不要机械覆盖八项。根据 Depth 和用途标明本次包含与不包含的维度。

## User-facing Preview

开始前用短卡片说明：

```text
本次目标：<用途>
建议方式：Standard + History-assisted + Voice-friendly
预计投入：15–25 分钟，3–4 个用户回合，最多 8 个 prompts
本次覆盖：当前状态、关键经历、价值动机、工作方式、协作边界、公开表达
暂不覆盖：亲密关系、健康等与用途无关的领域
资料范围：<已授权来源>
交付：Calibration Card → 用户修正 → 候选 Personal Context
保存边界：<是否归档到 Personal Store；下游任务是否可以另存；长期写回默认关闭>
控制权：可以跳过、暂停、缩短；停止时仍会获得阶段性结果
```

随后等待用户接受或调整。用户只说“开始”时，采用推荐组合，不再要求二次选择。在收到接受前，不进入 Question Map 的第一个探索问题。

## Question Map

进入访谈时展示与所选 Depth 相符的本轮计划，而不是逐个隐藏问题。以下是 Standard 示例：

```text
Round 1 / 3：目标与关键经历（3 prompts，可任选 2 个）
Round 2 / 3：价值、工作方式与协作边界（3 prompts）
Round 3 / 3：Calibration Card，只做修改与确认
```

Deep 模式最多再增加一个反例回合和一个最终批准回合。

Quick 使用 2–3 个用户回合、最多 5 个 prompts；Standard 使用 3–4 个用户回合、最多 8 个 prompts；Deep 使用 4–5 个用户回合、最多 12 个 prompts。Question Map 必须落在对应上限内。

## Controls

用户可以随时说：

- “跳过这个维度”；
- “直接给阶段结果”；
- “缩短为 Quick”；
- “暂停并保存”；
- “不要保存这一段”。

任何元反馈都优先于访谈计划。用户无需解释为什么不想继续回答。

如果用户在当前 Session 结束后希望继续，先交付本轮阶段结果，再展示一份新的 Session Contract，说明新增目标、范围和预算。不要通过“再问最后一个问题”连续延长原 Session。
