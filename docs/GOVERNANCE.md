# Brief Yourself 1.0 治理原则

[中文](GOVERNANCE.md) | [English](GOVERNANCE.en.md) | [术语表](GLOSSARY.md)

Brief Yourself 的核心不是让 Agent 保存更多个人信息，而是让用户控制个人信息如何被读取、解释、披露和更新。

1. **先授权，后读取。** 读取历史、项目、简历或 Harness Memory 前，说明来源、范围、目的、外部传输、保存位置和删除方式。
2. **最小必要。** History-assisted 只按当前目标读取必要历史，不默认扫描或导入全部 Memory。
3. **历史是证据。** Memory、项目记录和 Agent 判断只能成为候选 evidence，不能自动成为 confirmed Claim。
4. **Store、View、Patch 分权。** Store 是长期事实源；View 是冻结任务输入；Patch 是候选变更。
5. **用途和受众明确。** View 绑定 purpose、principal、完整 audience、revision、TTL 和持久化权限。
6. **用户审核写回。** 新认识只形成 pending Patch；用户逐项批准后才能 apply。
7. **保留张力与未知。** 反例、变化、Tension 和 Unknown 不被强行整理成人设。
8. **跨 Agent 共享受治理的 Context。** Agent 可以读取同一冻结 View、返回不同 Patch，但不自动共享全部 Memory 或直接写 Store。
9. **失败关闭。** 用途、受众、权限、版本或期限不明确时拒绝继续。
10. **用户保留控制权。** 用户可以查看、导出、修正、拒绝、限制、退休或请求删除可控副本。

## 当前边界

- 只实现 `subject.type=person`；
- File Adapter 不读取 Store 或 Memory API；
- runtime 只承诺本地 single-writer；
- Tension / Unknown 暂无正式 Patch 新增/更新入口；
- 协议可携带不等于所有 Agent 平台都已完成 Memory 接入和互操作验证。
