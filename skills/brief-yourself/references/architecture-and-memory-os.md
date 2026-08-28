# Architecture And Memory OS

## Design Goal

让画像既能长期积累，又不会把所有历史、临时任务和隐私信息塞进每一次对话。

本设计借鉴 Hermes Memory OS 的四个原则：

1. 长期注入的信息必须精简，只保留未来仍有价值的内容。
2. 完整会话历史应按需搜索，而不是永久占用上下文。
3. 工作流程和方法属于 Skill，不属于个人记忆。
4. 任务使用冻结快照；中途写入不会静默改变当前运行语境。

Brief Yourself 不复制 Hermes 的实现。它在任何可用的记忆基础设施之上，建立一套可迁移、用户可控的 Personal Context 协议。

## Evidence Base

Evidence Base 是根系，不是画像层。它保存来源索引：历史对话、项目记录、简历、公开材料、用户原话和真实事件。

- 原始材料留在原系统或用户管理的位置。
- Context 只保存来源引用和必要的短摘要。
- 只有出现明确问题或证据缺口时才检索。
- 不把“没有记录”解释为“没有发生”。

## Layer 1: Core Self

Core Self 是主树干，只保留少量跨场景、相对稳定、经过用户确认的认识，例如：

- 关键事实与长期边界；
- 跨领域反复出现的价值取向；
- 稳定的沟通与决策偏好；
- 多个场景支持的能力模式和限制。

Core 应保持紧凑。默认软上限为 30 条 active Claim 或 8,000 个字符；超过时先合并重叠、降级为 Domain 或退休失效项，不静默丢弃。

Core Claim 必须：

- `user_status: confirmed`；
- `scope: cross-context`；
- 不是 `inference`；
- 至少有一个可识别证据来源；
- 没有未处理的关键反例；
- 预计在未来一段时间仍有价值。

## Layer 2: Domain Self

Domain Self 是枝干。每个领域单独加载和复核：

- `career-and-work`
- `working-style`
- `communication-and-expression`
- `decision-making`
- `relationships-and-collaboration`
- `public-self`
- `values-and-motivations`
- `goals-and-constraints`
- 其他用户定义领域

新认识默认先进入 Domain。Domain 允许 `self_report`、`observation` 和未决张力，也允许范围更窄、变化更快的认识。

## Layer 3: Task Context

Task Context 是枝叶：某一份 JD、某一次演讲、某篇文章或某项决策。它由 Context View 表达：

- 只包含完成任务所需的 Claim；
- 记录父画像版本；
- 记录用途、权限和到期时间；
- 任务运行中保持冻结；
- 不作为长期画像直接保存。

任务产生的新认识通过 Context Patch 返回，而不是直接写回 Core 或 Domain。

## Promotion And Demotion

### Task → Domain

在任务中发现的认识必须先成为 Patch。用户确认后，才能进入相关 Domain。

### Domain → Core

只有当认识在多个独立场景中重复出现、用户确认、反例已处理且具有长期价值时，才能晋升 Core。单次任务或单一 JD 永远不足以晋升。

### Core → Domain / Challenged / Retired

发现适用范围变窄时，把 Core 降为 Domain；发现冲突但尚未确定时标记 `challenged`；确认过时后标记 `retired`。不直接删除历史。

## Frozen Snapshot

Context View 记录 `parent_revision`。创建后，即使长期画像在另一个任务中被更新，当前 View 仍保持不变。需要新信息时，向用户说明差异并重新生成 View。

这可以避免同一任务中前后使用了两个不同版本的“用户”。

## Declarative Memory

把个人认识写成陈述：

- 推荐：`用户在高不确定任务中倾向先做小实验，再形成判断。`
- 避免：`遇到不确定任务时必须先做小实验。`

后者是命令，会在未来覆盖当前意图；流程应写进 Skill，个人 Context 只描述人。
