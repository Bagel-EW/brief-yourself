# Interview Protocol

## Shared Conversation Principles

- 从经历、选择和行为开始，再提炼标签。
- 每轮围绕一个中心问题展开；通常一次问 1 个主问题，必要时附 1 个澄清问题。
- 单题原则用于降低当轮负担，不允许演变成没有上限的连续追问。开始时说明预计用时与总回合。
- 先复述所理解的具体内容，再追问缺口，不要连续发问卷。
- 不把流畅回答当作真实性证据，也不把犹豫当作缺点。
- 用户可以跳过任何问题、暂停、删除材料或限制某一领域。
- 每个阶段都允许回看与修正，不等到最后才让用户审核。
- 默认在 3 个有效用户回复后先交付 Calibration Card；用户反馈疲劳或询问剩余题数时立即综合，不要求先答完当前问题。
- 遵守 Depth 的硬上限：Quick 最多 5 个 prompts / 2–3 个用户回合 / 5–10 分钟，Standard 最多 8 / 3–4 / 15–25 分钟，Deep 最多 12 / 4–5 / 30–45 分钟。
- 当前 Session 达到上限后先交付阶段结果；继续探索必须建立新的 Session Contract。

## Stage 0: Session Preview

在第一个探索问题前读取 `session-contract.md`，完整展示本次目标、Depth、Evidence、Pace、覆盖与排除维度、最多 prompts、预计时长、用户回合、来源、交付、保存边界、退出方式和 Question Map。保存边界分别说明是否归档到 Personal Store、下游任务是否可以另存。推荐默认组合，等待用户接受或自然语言调整；接受前不进入探索。用户可以选择 Guided chat、Batch 或 Voice-friendly，不必被迫逐题回答。

## Quick Brief

适合求职等任务前置，目标是在约 5–10 分钟内形成最低可用 Context。

覆盖五项：

1. 用户现在要完成什么，以及为什么重要；
2. 与任务有关的关键经历和可确认事实；
3. 用户认为自己的优势、困难和边界；
4. 用户希望如何被理解，以及最不希望被误解什么；
5. 当前现实约束和不可牺牲项。

结束时生成 `depth: quick` 的临时 Task Context，明确未覆盖领域。Quick Brief 默认不直接改写长期画像；任务结束后，只有可复用的认识才进入待审核 Domain Patch。不要因为是快速版而自动填充性格结论。

Quick Brief 最多使用 5 个 prompts 和 2–3 个用户回合；达到任一上限时直接综合。

## Standard Brief

Standard 是默认 Depth，目标是在 15–25 分钟、3–4 个用户回合、最多 8 个 prompts 内形成可校准的 Domain 候选。

1. 用一组 prompts 确认当前用途、关键经历与现实约束；
2. 从具体经历提炼价值动机、工作方式、协作边界与公开表达；
3. 默认在第 3 个有效用户回复后先交付 Calibration Card；
4. 只在会实质改变结论时追加一次关键校准；
5. 交付 Human Brief、候选 Domain Claim、反例与 unknowns，长期写回仍需单独批准。

不要为了覆盖全部 Dimension Map 自动升级为 Deep。Standard 达到当前用途的最低充分画像后立即结束。

## Deep Brief

Deep Brief 在 30–45 分钟、4–5 个用户回合、最多 12 个 prompts 内完成。下列阶段可以合并进同一回合，不能把阶段名称当作额外回合，也不能包含无限数量的后续单题。详细预算和停止条件见 `interaction-budget.md`。

### Stage 1: Orientation And Source Map

明确用户为何现在想认识自己、希望哪些任务将来使用这份 Context、哪些领域不希望涉及。列出可用来源与权限。

### Stage 2: Grounding In Lived Experience

选择 2–4 个真实片段：重要决定、投入时刻、挫折、冲突、被认可或被误解的经历。追问当时做了什么、为什么、结果如何，而不只问感受标签。

### Stage 3: Patterns Across Contexts

比较现实生活、工作、公开表达和独处时的差异。寻找重复模式、触发条件、反例与变化轨迹。

### Stage 4: Tensions And External Evidence

把用户自述与历史行为、他人反馈或项目记录并列。用探索性语言提出矛盾：说明依据、承认可能误读，让用户解释或反驳。

### Stage 5: Synthesis And Calibration

分批呈现候选 Claim、重要张力和未知项。让用户确认、改写、拒绝或标记未解决。新认识默认先成为对应 Domain 的候选 Claim；只有跨场景、相对稳定、非 `inference` 且经用户确认的少量认识，才可提议进入 Core。最后生成 Human-readable Brief 与 Structured Context。

不要把覆盖所有人生领域当作完成标准。当前用途已经拥有最低充分画像时结束，把未覆盖领域放入 `unknowns`。

## History-assisted Evidence

History-assisted 是 Evidence modifier，不是第四种 Depth。仅在明确授权后执行，并继承已选 Quick、Standard 或 Deep 的全部预算：

1. 说明能够读取哪些会话、文件或项目记录；
2. 让用户选择范围、时间段和排除项；
3. 围绕当前缺口按需检索，先提取具体行为证据和反复出现的主题，不整批导入全部历史；
4. 把模式写成 `observation`，解释写成 `inference`；
5. 提供来源引用或可识别的证据摘要；
6. 请用户补充背景，尤其关注 Agent 看不到的线下生活与内在感受；
7. 经校准后先生成待审核 Patch，再由用户决定是否合并到对应层级。

不要把沉默、缺少记录、表达频率或 Agent 使用熟练度直接解释为人格特质。

如果按需检索已经回答某个 prompt，不要再向用户重复提问；把节省下来的预算用于减少负担，而不是扩展探索范围。

## Update Mode

先读取现有版本并询问近期发生了什么变化。重点检查：

- 新事实或新经历；
- 已失效的目标、约束和状态；
- 原有 Claim 的新证据或反例；
- 支线任务返回的待审核 Patch；
- 长期未复核但影响较大的认识。

按 Core、Domain 和待审核 Patch 分层检查。更新时提高版本号，保留必要的变更理由；优先挑战、退休或 supersede 旧 Claim，不用新版本抹掉用户仍想保留的历史阶段。

## Closing Language

结束时使用校准性表达，例如：

> 这是一份基于当前材料、截至目前的工作画像。它保留了仍不确定和正在变化的部分。你可以随时修正、限制使用范围或删除其中任何一项。
