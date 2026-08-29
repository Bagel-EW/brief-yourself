# Changelog

本文件只记录正式公开版本；本地开发、私有候选和发布准备过程不进入公开 Changelog。

## 1.0.1 — Agent executability and governance revision · 2026-08-29

对外产品版本升级为 `1.0.1`；内部 schema compatibility id 仍为 `0.4`，仅用于协议兼容与历史迁移。

### Agent 可执行性

- `SKILL.md` 按场景路由全部 14 个正式 V0.4 操作：Store 生命周期（`init` / `validate` / `inspect` / `derive-core-summary`）、来源登记（`register-source`）、View（`create-view` / `validate-view`）、Patch（`stage-patch` / `list-patches` / `apply-patch` / `reject-patch`）、导出与删除（`export` / `purge-plan` / `purge`）。
- 2 个迁移操作（`migrate-v02` / `preview-migrate-v03`）只在「历史迁移路由」小节出现，不混入常规任务能力。
- `list` 明确为 `list-patches` 的兼容别名，不列入正式能力清单。
- 新增并显式路由 `references/store-operations.md`：全部操作的参数、副作用、文件系统限制、purge 边界与恢复方式。
- `purge` 只展示 `purge --help`，不提供绕过 `--plan-token` / `--confirmed-by` / `--approve` 的可执行删除示例。

### 同意语义

- 明确区分两道门：读取前的**授权卡**由 `source-consent-and-disclosure.md` 约束；`register-source` 只是读取之后、经用户批准的**来源登记写入入口**，不是读取授权界面。
- 修正 README 中两处误导映射：「读取前展示授权卡」不再等同于 `register-source`；`validate-view` 不再被描述为创建限制，限制发生在 `create-view` 的 disclosure 与用户批准阶段。

### 事实源治理

- active Skill 只保留 `source-consent-and-disclosure.md` 作为当前同意与披露事实源，消除 consent 双事实源。
- 6 份 legacy reference 与 3 份 V0.3 模板迁出 active 包，保存在项目 `archive/1.0.1-moved-out/`，不永久删除。
- `store-operations.md` 不再依赖 V0.3 `personal-context-contract.md`：Source Structure 改为内联字段，并对齐 `assets/schemas/personal-context-store-v0.4.schema.json`。
- 7 份保留 reference 与 3 份 v0.4 模板逐一写明用途与读取时机，不再只依赖通配说明。
- 发布包从 26 个文件精简为 17 个：`SKILL.md` + `agents/` + 3 schema + 3 template + 7 reference + 2 script。

### 自动门禁

- 新增 `tests/test_v101_release_gates.py`（19 项）：runtime 注册名总数 17；14 个正式操作可被发现与分组路由；2 个迁移操作只在迁移路由；`list` 为兼容别名；每个 active reference 可达；3 份正式模板有明确入口；active 包无新旧双事实源；`purge` 示例带治理约束；Markdown 链接、UTF-8、Python 编译与隐私扫描通过。
- 合成验收基线更新为 `114 tests / 0 failures`（原 95 项 + 新增 19 项门禁）。

### 制品

- `release/brief-yourself-1.0.1.zip`：17 个文件，77,473 bytes，SHA-256 `987DD5629EF2C8317BD40342D62B6C2CEC2DC2A13F3E9BC33D34E35713F0BF52`。
- `release/brief-yourself-1.0.0.zip` 原样保留（26 个文件，94,329 bytes，SHA-256 `877458D8A7D46CB891C85234D88BCC10A97777D2BD0B4BF6092058ECF92FA5FB`），两者哈希均记录在 `release/SHA256SUMS.txt`。

## 1.0.0 — Documentation revision · 2026-08-29

- 补全中英文 README 的 runtime command 清单：新增 `inspect`、`list-patches`、`register-source`、`validate-view`、`reject-patch`、`derive-core-summary`、`export`、`purge-plan` 与 `purge`。
- 在「隐私与治理」每条承诺后标注对应命令，消除承诺与执行路径脱节。
- `purge` 不给可直接复制执行的示例：它需要紧邻前一次 `purge-plan` 的 `--plan-token`，并同时需要 `--approve` 与 `--confirmed-by`。
- 补充仓库 description 与 topics。
- 本次为文档修订：Skill 包内容与 `release/brief-yourself-1.0.0.zip` 未变更，版本号不变。

## 1.0.0 — Initial public release · 2026-08-28

- 以 MIT License 发布可移植的 Brief Yourself Personal Context Skill。
- 提供 Personal Context Store、目的绑定的 Frozen Context View 和用户审核的 Pending Context Patch。
- 提供中英文 README、Glossary、Governance、Privacy、Architecture 与 Security 文档。
- 默认最小必要读取；历史与 Harness Memory 仅在明确授权后作为候选 evidence。
- 默认拒绝未授权受众、非公开披露不匹配、未审核条目和过期 View；当前 runtime 为 single-writer。
- 提供只含合成数据的 95 项验收基线，以及与 canonical Skill 一致的 1.0.0 ZIP 制品。
