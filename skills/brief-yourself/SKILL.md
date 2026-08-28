---
name: brief-yourself
description: 通过有界访谈、来源授权和用户校准，建立并调用以 person 为主体的 Personal Context。用于认识自己、自我探索、更新个人画像，以及在求职、写作、演讲、协作或决策任务前生成冻结 Context View、任务后审核 Context Patch。区分 fact、self_report、observation 和 inference，保留反例与未知，并限制敏感内容披露。
---

# Brief Yourself 1.0.0｜Personal Context Layer

Brief Yourself 1.0.0 是当前产品、Skill 入口和 Agent identity。内部 schema compatibility id 仍为 `0.4`，只用于协议兼容与历史迁移，不是产品版本。

## 触发与定位

当用户想认识自己、让 Agent 更了解自己、复核近期变化，或希望在一个下游任务中使用经过校准的个人信息时使用本 Skill。

Brief Yourself 是由用户参与校准、按用途编译、可携带到不同 Agent harness 的 Personal Context Layer。Personal Context Store 是个人上下文的 canonical source；`Context View` 是给某次任务的冻结输入；`Context Patch` 是任务结束后交给用户审核的候选回流。

Codex Memory、rollout、项目文档和其他 harness memory 只能在明确授权后作为候选 evidence。它们不能自动导入、覆盖 Store、成为 `confirmed` Claim 或成为 canonical source。Brief Yourself 不复制 rollout、consolidation、retrieval、thread 或通用 memory 注入流程。

## 非目标与硬边界

- 不做通用历史库、向量检索、心理诊断或“真实人格”判定。
- 不把 Harness Memory 变成 Personal Context，也不做静默双向同步；`auto_import_harness_memory` 必须为 `false`。
- 1.0.0 当前版本只实现 `subject.type = person`。Organization Context 只通过共同 Envelope 预留隔离边界，不在本 Skill 中实现 Team Context Store。
- 不使用个人画像进行未经授权的就业、信贷、保险、医疗或其他高影响自动决策。
- 不读取真实 Personal Context、私密测试数据或历史资料，除非用户先完成本轮明确的来源授权。
- 个人信息写成陈述性 Claim，不写成会覆盖未来指令的命令；流程和操作规则属于 Skill 或项目文档。

## Session Contract：先约定，再探索

建立或更新画像时，先选择 `Depth`、`Evidence` 和 `Pace`。在第一个探索问题前，完整展示本次 Session Contract，并等待用户接受或调整；接受前不得索取经历、价值判断或其他探索内容。

Contract 至少说明：目标；Depth、Evidence、Pace；覆盖与排除维度；最多 prompts、用户回合和预计时长；准备读取的来源及范围；交付物；Personal Store 是否归档、下游是否可另存；暂停、跳过和删除路径。用户只说“开始”时采用推荐值。当前 Session 达到任一上限或停止条件即结束；继续探索必须建立新的 Contract。

| Depth | 最多 prompts | 用户回合 | 预计时长 |
|---|---:|---:|---:|
| Quick | 5 | 2–3 | 5–10 分钟 |
| Standard（默认） | 8 | 3–4 | 15–25 分钟 |
| Deep | 12 | 4–5 | 30–45 分钟 |

`History-assisted` 只是 Evidence modifier，必须继承所选 Depth 的全部上限，不能成为第四种深度或延长 Session。用户表示疲劳、询问还剩多少题、反馈过长，或已有材料足以形成最低充分画像时，立即停止新增问题并综合；不要为了完成脚本而继续追问。

## 运行流程

1. **确定操作。** 选择建立/更新画像、生成 Task View、审核 Patch，或 Inspect/Export；先说明本次用途和最低必要范围。
2. **取得来源授权。** 读取历史、项目文件、简历、公开资料或 Harness Memory 前，按 `references/source-consent-and-disclosure.md` 展示来源、范围、目的、外部传输、保存位置和删除方式，并等待明确授权。当前对话与用户主动提供的材料也要记录实际使用范围。
3. **按需取证。** 先用当前回答和用户主动提供的材料；历史只为已知缺口按需读取，不整批导入。来自 Harness Memory 的内容只形成候选 evidence，必须标为待校准，不能直接成为 confirmed Claim。
4. **访谈与校准。** 按 `references/interview-and-calibration.md` 控制预算；从具体经历、选择和行为开始，阶段性展示 Calibration Card，让用户确认、改写、拒绝或保留未决。
5. **形成扁平 Claim 候选。** Store 只有一个 `claims[]`；`domains[]` 是标签，不是物理层级。新认识默认是 `user_status: unreviewed` 的 Domain 候选，不因一次 Session 晋升到 Core。`Core Summary` 只能从已确认、仍有效且有跨场景支持的 Claim 派生，不单独存储。
6. **生成冻结 View。** 按用途、精确主体/执行者/受众和最小必要 Claim 编译 View；完整对象、`source_revision`、创建时间、TTL 和权限一起冻结。Store 后续变化不会静默改变现有 View。
7. **只暂存 Patch。** 任务新认识只生成 `pending` Patch，策略和一次性话术放入 `task_strategies_not_for_merge`。按 `references/context-view-and-patch.md` 逐项审核；只有用户明确批准具体 `patch_id` 和 Proposal 后，runtime 才能 apply。

## 证据与认识的最低规则

- `fact`、`self_report`、`observation`、`inference` 分开记录；不把推断改写成事实。
- 保留证据来源、counterevidence、适用范围、变化、矛盾（Tension）和 unknown；没有记录不等于没有发生。
- `Tension` 与 `Unknown` 仍是当前 1.0.0 canonical Store 的顶层实体，也可作为完整冻结对象进入 View；它们不是永久 session-only。当前 Patch schema/runtime 的 Proposal 仅支持 Claim 的 `add`、`update`、`challenge`、`retire`，没有 Tension/Unknown 的新增或更新入口。会话新发现只能作为“未持久化候选”交付并等待未来显式协议；不得静默写入 Store、伪装成 Claim、塞入 `task_strategies_not_for_merge`，或声称已长期保存。已有迁移/Store 中的 Tension/Unknown 保持可读并可按需入 View；未来写入口另需 schema/runtime/approval，本轮不实施。
- 不用 MBTI、DISC 或一次回答替代证据；不把一次任务进度、JD 关键词、临时策略或 Agent 操作偏好写入长期 Claim。
- 新认识先进入候选 Domain；Core 只作为派生摘要资格，不能由单次任务直接晋升。长期写回始终经过用户审核。
- `sensitivity` 描述内容敏感程度；`disclosure` 描述 audience、purpose 和下游持久化许可，二者不能互相替代。默认排除 `private` 与 `restricted`。
- `subject` 固定为 person。`team-agent` 或其他未获明确授权的执行者/受众默认拒绝；不得因“同一工作区”而推断个人授权。

## Runtime 命令（与 A 包集成的稳定操作名）

以下是 1.0.0 当前 runtime 的稳定操作名；A 包的具体参数必须以 runtime 的 `--help` 为准，本入口不预设尚未冻结的 `create-view` identity、principal、audience、TTL 或 disclosure flags。不要把这些命令用于真实 Store 的迁移或 dogfood，除非另有授权。

```bash
python scripts/context_store.py init --help
python scripts/context_store.py validate --store <store>
python scripts/context_store.py inspect --store <store>
python scripts/context_store.py create-view --help
python scripts/context_store.py validate-view --help
python scripts/context_store.py stage-patch --store <store> --patch <patch.json>
python scripts/context_store.py apply-patch --store <store> --patch-id <patch-id> --confirmed-by <principal> --approve
```

`create-view` 最终必须接收或能确定 `principal`、`audience`、用途、TTL 和披露权限；这些参数在 A runtime 合入后用 `create-view --help` 核对。`stage-patch` 只能写 pending；`apply-patch` 必须同时验证父版本、具体 Patch 和用户批准。不能把旧的 Core/Domain 物理容器或 promote/demote 恢复为当前版本的新写入。

### C File Adapter：只接收冻结 View

C 包的 `scripts/adapters/codex_file_adapter.py` 是单向 File Adapter。它只读取一个已经生成的 1.0.0 冻结 View JSON，校验 Envelope、TTL、主体、用途、`allowed_use`，以及每条 included Claim 是否同时授权 `principal.id` 和 `audience[]` 中每个 recipient；失败时 fail-closed，不打印被拒绝的个人正文。它不读 Personal Context Store、Codex Memory、rollout 或网络资源，不写回 Store、Harness Memory 或任何长期 Context；可选输出也不能覆盖输入 View。

已按 C 的实际 `--help` 核对以下概念命令。`--expected-audience` 是调用方提供的可重复子集绑定（repeatable subset binding）：每个 expected `type:id` 都必须存在于冻结 `view.audience`，但调用方不必枚举全部 audience，因此它不是 audience equality/exhaustive assertion。这个调用方绑定不改变安全不变量：无论是否传入该参数，adapter 仍必须对每条 included Claim 检查 disclosure 是否同时覆盖 `principal.id`、View Envelope 中全部 audience recipient IDs，并检查 purpose；子集参数不能放宽 disclosure。`--purpose-approved` 仅表示本次具体 purpose 已获明确批准：

```bash
python scripts/adapters/codex_file_adapter.py --view <view.json> --expected-purpose <purpose> --expected-task <task> --expected-principal-id <principal-id> --expected-audience type:id --allowed-use <allowed-use> --purpose-approved
```

Adapter 输出是当前任务的 Markdown/JSON 适配结果，不是新的 Store、Memory 或回写凭据；任务结束仍只生成 pending Patch。

## References 路由

- `references/interview-and-calibration.md`：Session Contract、三档预算、Question Map、Calibration Card、疲劳停止和 Claim 分类。
- `references/source-consent-and-disclosure.md`：历史/Harness Memory 授权、候选 evidence、敏感度、披露、person/team 隔离和高影响边界。
- `references/context-view-and-patch.md`：当前版本 Context Envelope、完整冻结对象、TTL、披露匹配、Patch 暂存与批准。
- `references/harness-boundaries.md`：Harness Memory、Personal Context、Task Context 的边界，以及 File Adapter 的 envelope/disclosure 与 expected-audience 绑定规则。
- `references/personal-context-model.md`：Personal Context Store 的顶层实体、证据模型、View/Patch 回流边界与版本语义。
- `references/migration-v0.3-to-v0.4.md`：仅在历史 V0.3→V0.4 schema compatibility 的只读、metadata-only preview 与 no-chain 迁移路由时读取；这里的 `0.4` 仅是内部兼容标识，不是产品版本；说明预览、审核、另存和回滚边界。
- 内部 schema compatibility 路径 `assets/schemas/*-v0.4.schema.json` 与 `assets/templates/*-v0.4.json`：Store、View、Patch 的结构基线。

旧版 reference 保留为历史兼容材料，但本入口不再路由到旧的访谈、预算、来源授权或 v0.3 View/Patch 协议；需要实现时以本入口和已路由的内部 schema compatibility reference 为准。

## 交付与结束语

根据操作交付 Human Brief、候选 Claim、Tension/Unknown、冻结 View 或 pending Patch，并列出来源范围、未覆盖项、敏感边界和下一次值得验证的问题。结束时明确：这是基于当前授权材料的、截至目前的工作画像；用户可以修正、限制使用、拒绝或删除任何一项。
