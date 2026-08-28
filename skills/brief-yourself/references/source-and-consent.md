# Source And Consent Rules

## Consent Before Access

在读取历史对话、项目文件、简历、公开主页或第三方材料前，说明：

- 准备读取什么；
- 为什么需要；
- 读取范围与时间范围；
- 是否会调用外部服务；
- 形成的认识将保存在哪里；
- 用户如何审查、删除或撤回。

只有用户明确同意后才能读取。一次授权不自动覆盖未来的新来源或新的使用目的。

## Source Boundaries

- 只声称分析了实际可访问、实际读取的材料。
- 把用户自述与外部记录分开，不以数量多的一方自动胜出。
- 公开可见不等于适合进入私人画像；仍需说明用途。
- 第三方对用户的评价只是一个来源，不能自动成为事实。
- 对亲密关系、健康、财务、政治、宗教、创伤、身份号码、精确住址等信息采用 `restricted`。

## Data Minimization

优先把原始材料留在 Evidence Base，只在画像中保存可复用的认识和可定位的证据引用，不复制整段敏感原文。任务只需知道“偏好异步沟通”时，不传递导致该偏好的私人经历。

## Transparency

向用户展示：

- 使用过的来源清单；
- 每项关键推断的依据；
- 无法访问或没有覆盖的来源；
- 哪些内容由用户确认，哪些仍是 Agent 推断；
- 是否存在外部传输或持久化。

禁止隐藏记忆结构、把历史内容伪装成 Agent 自己的直觉，或在普通对话中静默建立长期画像。

## Retention And Deletion

如果运行环境支持持久化，优先使用用户可查看和编辑的本地文件。标明保存位置与更新时间。用户要求删除或撤回时，删除可控副本或清楚说明无法控制的副本与限制。

来源记录至少包含安全 ID、类型、标题、实际读取范围、带时区的采集时间、`consent: explicit`、保留方式和敏感级别。Claim、Tension 与 Unknown 的 `evidence_refs` 必须指向已登记的 Source ID。

本地 Store 使用 `context_store.py register-source --approve --confirmed-by ...` 登记新 Source；不要直接编辑 `context.json` 或 `evidence/index.json` 绕过 revision、备份与审计记录。

撤回分两类：`retire` 保留历史并使认识逻辑失效；`purge` 先列出当前 Store 内全部可控副本，再经精确目标、计划 token、确认主体和 `--approve` 执行。原始来源与 Store 外副本始终单列说明，不能声称已代为删除。

如果运行环境不支持可靠持久化，明确告诉用户并提供可复制的 Human-readable Brief 与 Structured Context。

## Downstream Use

每个下游任务都要声明用途。默认排除 `restricted` Claim，并根据最小必要原则生成带父版本的冻结 Context View。任务期间不得静默加载新增画像。不得用个人画像进行未经授权的就业、信贷、保险、医疗或其他高影响自动决策。
