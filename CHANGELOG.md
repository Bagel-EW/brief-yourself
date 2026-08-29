# Changelog

本文件只记录正式公开版本；本地开发、私有候选和发布准备过程不进入公开 Changelog。

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
