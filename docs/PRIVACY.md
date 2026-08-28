# Brief Yourself 1.0 隐私边界

[治理原则](GOVERNANCE.md) | [术语表](GLOSSARY.md) | [English README](../README.en.md)

## 本仓库包含什么

公开仓库包含 canonical Skill、公开技术文档、合成结构示例和版本制品。仓库与发布包均不包含真实 Personal Context、私密 Store、对话记录、简历、临时文件或本机路径。

## 本仓库不代表什么

private 仓库只表示访问范围受 GitHub 权限控制，不等于绝对安全、零风险、公开发布或生产环境认证。测试通过只说明对应输入与环境下的检查结果。

## 使用个人资料时

1. 先说明要读取的来源、范围、用途、外部传输、保存位置和删除方式。
2. 只读取当前任务的最小必要内容。
3. 把事实、自述、观察和推断分开；不把推断直接写成长期事实。
4. 用明确用途生成 Frozen View；默认排除 `private` / `restricted`。
5. 任务后的新判断只能形成 pending Patch，并等待用户逐项审核。

用户可以查看、限制、拒绝、修改或删除个人 Context。删除前应确认精确目标；不要把真实资料放进 issue、示例、图片、ZIP 或测试 fixture。

## 数据处理边界

Skill 本身不自动读取 Harness Memory、rollout、网络资源或其他项目历史，也不向外部服务上传个人 Context。下游 adapter 只接收带用途、受众、期限和披露边界的冻结 View。
