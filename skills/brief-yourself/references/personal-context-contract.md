# Personal Context Contract v0.3

## Canonical Store

```json
{
  "schema_version": "0.3",
  "profile_id": "user-controlled-id",
  "subject": {
    "preferred_name": "",
    "preferred_languages": []
  },
  "policy": {
    "core_claim_soft_limit": 30,
    "core_char_soft_limit": 8000,
    "patch_approval_required": true,
    "default_view_ttl_days": 7
  },
  "coverage": {
    "depth": "quick",
    "included_domains": [],
    "missing_domains": []
  },
  "core": {
    "claims": [],
    "tensions": [],
    "unknowns": []
  },
  "domains": {},
  "sources": [],
  "revision": {
    "version": 1,
    "created_at": "ISO-8601",
    "updated_at": "ISO-8601",
    "last_reviewed_at": null
  }
}
```

## Domain Structure

```json
{
  "career-and-work": {
    "claims": [],
    "tensions": [],
    "unknowns": [],
    "updated_at": "ISO-8601"
  }
}
```

## Claim Structure

```json
{
  "id": "claim-001",
  "statement": "在目标不清楚时，用户倾向先通过小实验形成判断。",
  "layer": "domain",
  "domain": "decision-making",
  "kind": "observation",
  "scope": "domain",
  "durability": "evolving",
  "confidence": "medium",
  "user_status": "unreviewed",
  "status": "active",
  "sensitivity": "private",
  "evidence_refs": ["source-002#message-18"],
  "counterevidence_refs": [],
  "promotion_evidence": [],
  "observed_at": "2026-07-24",
  "valid_from": null,
  "last_confirmed_at": null,
  "review_after": null,
  "expires_at": null,
  "supersedes": [],
  "notes": "在高风险财务决策中尚未验证。"
}
```

## Enumerations

### `layer`

- `core`
- `domain`

Task-specific认识不进入 Canonical Store，而进入 Context View 或 Patch。

### `kind`

- `fact`：记录或明确事件支持的事实。
- `self_report`：用户对自己的描述、感受或判断。
- `observation`：多个具体行为中观察到的模式。
- `inference`：Agent 提出的解释或假设。

不得自动把后三者升级为 `fact`。`inference` 不得进入 Core。

### `scope`

- `cross-context`
- `domain`
- `situation`

### `durability`

- `stable`
- `evolving`
- `situational`

### `confidence`

- `high`：多个独立证据、范围清晰、用户已确认。
- `medium`：有证据，但存在范围限制或反例。
- `low`：单一来源、模糊印象或尚未校准。

### `user_status`

- `confirmed`
- `corrected`
- `rejected`
- `unreviewed`
- `unresolved`

### `status`

- `active`
- `challenged`
- `retired`

### `sensitivity`

- `public`
- `private`
- `restricted`

## Source Structure

```json
{
  "id": "source-002",
  "type": "conversation",
  "title": "用户可识别的来源名称",
  "locator": "system-or-user-managed-location",
  "access_scope": "实际读取范围",
  "collected_at": "ISO-8601",
  "consent": "explicit",
  "retention": "source-managed",
  "sensitivity": "private"
}
```

只记录实际访问过的来源。原始材料默认留在来源系统，Canonical Store 保存引用和必要摘要。

## Tensions

```json
{
  "id": "tension-001",
  "statement_a": "用户希望拥有较高自主权。",
  "statement_b": "目标高度模糊时需要明确反馈。",
  "interpretation": "需要方向校准，而非过程微管理。",
  "user_status": "confirmed",
  "status": "active",
  "sensitivity": "private",
  "evidence_refs": []
}
```

不要为了统一人设而删除张力。

## Unknowns

```json
{
  "id": "unknown-001",
  "question": "用户在长期高压环境中的恢复方式是什么？",
  "reason": "当前材料只有一次短期项目经历。",
  "priority": "low",
  "revisit": "only-if-relevant",
  "user_status": "unreviewed",
  "status": "active",
  "sensitivity": "private",
  "evidence_refs": []
}
```

Tension 与 Unknown 和 Claim 使用同一组 `user_status`、`status`、`sensitivity` 过滤规则。v0.2 中缺少这些字段时只允许只读检查；显式迁移会保守地补为 `unreviewed`、`active`、`private`。

## Schema Compatibility

- 新建 Store 使用 `0.3`。
- `validate`、`inspect` 与 `export` 可以只读处理 `0.2`，不会静默改写。
- 任何写操作前必须显式运行 `migrate-v02 --approve --confirmed-by ...`。
- 迁移保留旧 JSON 快照，递增 revision，并把迁移写入 `history/changes.jsonl`。
- v0.2 中指向未登记 Source 的历史 `evidence_refs` 只产生兼容 warning。迁移不会伪造授权或删除引用，而是在 `migration.unresolved_evidence_source_ids` 中保留待补登记的 Source ID；只有同时带有 `from_schema: "0.2"`、合法 UTC `migrated_at` 和非空 `confirmed_by` 的迁移记录才能启用该例外。新产生的未知 Source 引用或不完整的迁移元数据仍是 error。

## Human-readable Brief

Human Brief 至少呈现：稳定认识、领域差异、重要张力、正在变化的部分、未知项、来源范围、敏感边界，以及用户希望 Agent 如何协作。它是可读视图，不取代 Canonical JSON。
