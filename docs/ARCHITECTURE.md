# Brief Yourself 1.0 架构概览

[治理原则](GOVERNANCE.md) | [术语表](GLOSSARY.md) | [English README](../README.en.md)

## 一条任务链

```text
Store → View → Task → Pending Patch → 用户审核 → Store
```

### Store：可审核的事实源

Personal Context Store 保存 Claim、来源、状态、版本、敏感级别和披露边界。它不是一篇不断变长的“关于我”，也不是 Agent 可以默认整包读取的历史库。每条内容都应保留来源、适用范围、反例或未知项，并允许用户查看、修改、拒绝或删除。

### View：一次任务的冻结输入

View 从 Store 按用途编译，至少要明确：

- `purpose`：为什么读取；
- `principal`：哪个执行者使用；
- `audience`：输出会给谁；
- `TTL`：这份输入何时失效；
- `disclosure`：哪些内容可以披露、是否允许下游持久化。

View 创建后保持冻结。Store 后续变化不会静默改变已经开始的任务；换用途时应重新生成 View。

### Patch：受审核的长期回流

任务可以提出新的判断、修正、挑战或退休建议，但建议先停留在 pending Patch。只有用户逐项批准具体变更，runtime 才能将其写入 Store。拒绝或保留的项目不会成为长期事实。

## 设计边界

- 当前版本只实现 `subject.type=person`。
- Tension / Unknown 可以进入 Store / View 的结构，但当前没有对应的新增或更新 Patch 写入口。
- Harness Memory、rollout、项目文档和网络资源不是自动事实源；使用历史材料前必须取得明确授权。
- File Adapter 只接受已经生成且未过期的冻结 View，不读取 Store 或其他记忆系统，也不负责长期写回。

## 为什么分开三个对象

“保存过”“这次可读”“以后可保存”是三个不同决定。Store 负责可审核的长期内容，View 负责任务边界，Patch 负责把新判断交还给用户。分开对象可以让来源、用途、受众、期限和批准入口被单独检查。
