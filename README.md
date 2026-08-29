# Brief Yourself 1.0.1

[中文](README.md) | [English](README.en.md)

**让 Agent 在真正理解你的前提下工作，同时把读取、披露和长期记忆的决定权留给你。**

Brief Yourself 是一套可移植的 Personal Context Skill。它把个人背景整理成可审核的 Context，在每次任务开始前编译一份最小必要的冻结输入，并把任务中新出现的认识作为候选变更交回用户审核。

> **发布状态**：`Brief Yourself 1.0.1` 是当前正式开源版本，采用 MIT License。内部 schema compatibility id 仍为 `0.4`，只用于协议兼容与历史迁移，不是产品版本。仓库内的版本包与源码保持一致；GitHub Release 与 tag 不属于本次发布范围。

## 先认识 8 个词

| 英文术语 | 中文释义 | 一句话说明 |
|---|---|---|
| Personal Context Store | 个人上下文存储 | 用户可审核的长期事实源，不等于 Agent 自带的 Memory。 |
| Claim | 认识条目 / 可审核陈述 | 一条带来源、范围、状态、反例和敏感度的个人陈述。 |
| Context View | 上下文视图 / 冻结任务视图 | 为一次具体任务编译的最小必要、只读输入。 |
| Context Patch | 上下文变更提案 | 任务结束后的候选更新，未经用户批准不能写回。 |
| History-assisted | 历史辅助取证 | 授权后按当前目的读取必要历史，不是读取全部 Memory。 |
| Harness Memory | Agent 运行记忆 | Agent 平台维护的偏好与历史摘要，不是 Personal Context 的事实源。 |
| Principal / Audience | 实际执行者 / 允许接收者 | 谁使用 Context，以及谁可以看到输入或结果。 |
| TTL | 有效期 | View 到期后失效，不能继续作为默认上下文。 |

完整定义见 [术语表](docs/GLOSSARY.md)，正式边界见 [治理原则](docs/GOVERNANCE.md)。

## 30 秒了解怎么用

可从支持 Agent Skills 的环境安装：

```bash
npx skills add https://github.com/Bagel-EW/brief-yourself --skill brief-yourself
```

也可以把 `skills/brief-yourself/` 复制到 Agent 的 Skill 目录，或直接使用 `release/brief-yourself-1.0.1.zip`。历史版本 `release/brief-yourself-1.0.0.zip` 保留供校验。

安装后可以这样开始：

```text
帮我做一次 Standard Brief。先说明你准备读取哪些来源、范围、目的、保存位置和删除方式；
在我授权前，只使用当前对话，不读取历史项目或 Memory。
```

Brief Yourself 会先建立本次 Session Contract，再访谈、校准证据、生成 Context View。它不会因为安装了 Skill 就自动导入历史。

## 它解决什么问题

普通 Agent Memory 往往混合了偏好、摘要、推断和平台状态。Brief Yourself 把“记住我”拆成三个可治理动作：

```text
Personal Context Store
        ↓ 按用途、主体、受众和期限筛选
Frozen Context View
        ↓ 只读地交给当前任务
Task output
        ↓ 新认识先暂存
Pending Context Patch → 用户逐项批准 / 拒绝 → Store
```

- **Store** 是长期 canonical source，保存可审核的 Claim、来源、状态、权限和版本。
- **View** 是一次任务的冻结输入，绑定 purpose、principal、完整 audience、TTL 与披露边界。
- **Patch** 是任务后的候选变更。未经用户逐项批准，不会进入长期 Store。

这意味着“资料已经保存”不等于“每个 Agent 都能读取”，“Agent 得出一个判断”也不等于“它已经成为你的长期事实”。

## 适合与不适合

### 适合

- 希望不同 Agent 获得一致但最小必要的个人背景；
- 需要为写作、规划、复盘或长期项目生成任务专用 Context；
- 想利用历史项目辅助提问，同时保留逐次授权；
- 需要审计某条认识来自哪里、适用于什么范围、是否存在反例；
- 希望查看、导出、修正、退休或删除可控的个人 Context。

### 不适合

- 心理诊断、人格测评或替用户定义“真实自我”；
- 将全部聊天记录静默注入每个任务；
- 把 Agent 的一次推断直接升级为长期事实；
- 需要多人或多进程同时写同一个 Store 的生产系统；
- 把测试通过视为绝对安全或所有 Agent 平台已经互操作。

## 运行要求与兼容性

Skill 的本地工具只使用 Python 标准库。它适合能够读取 Skill 文件、访问获授权的本地文件并运行 Python 的 Agent 环境。

| 环境 | 状态 | 说明 |
|---|---|---|
| Codex / 本地 coding agent | 已按文件型 Skill 验证 | 可运行 Store、View、Patch 与测试脚本。 |
| 其他支持 Skills 的 Agent | 协议可移植 | 安装目录、Memory 权限和调用方式由平台决定，需单独验证。 |
| 纯网页聊天 | 有限 | 可使用访谈方法，但无法保证本地 Store 与脚本能力。 |

跨 Agent 互操作采用受限的 View / Patch 交换，而不是共享整个 Store。当前 runtime 只承诺 **single-writer**：同一时刻只有一个写入者。

## 安装

### 方法一：Skills CLI

```bash
npx skills add https://github.com/Bagel-EW/brief-yourself --skill brief-yourself
```

### 方法二：手动安装

1. 获取本仓库；
2. 将 `skills/brief-yourself/` 放入 Agent 的 Skill 目录；
3. 确认 `SKILL.md`、`agents/`、`scripts/`、`references/` 和 `assets/` 完整存在；
4. 用合成数据运行校验，不要先导入真实个人资料。

### 方法三：使用版本包

解压 `release/brief-yourself-1.0.1.zip`。`release/SHA256SUMS.txt` 提供文件大小和 SHA-256，用于确认制品未被替换。

## 常用触发方式

```text
帮我做一次 Brief Yourself 访谈，只使用当前对话。
```

```text
我授权你读取项目 A 的指定文档，辅助校准我的产品判断；不要读取其他项目或全部 Memory。
```

```text
基于已批准的 Personal Context，为这次写作任务生成一个 24 小时有效的 Context View。
```

```text
把这次任务发现的新认识做成 Pending Context Patch，逐条给我审核，不要直接写回。
```

## 标准工作流

1. **Scope**：确认来源、范围、目的、principal、audience、保存位置和删除方式；
2. **Interview**：从当前对话或已授权来源提出有证据边界的问题；
3. **Calibrate**：区分事实、自述、观察、推断、反例、矛盾与未知；
4. **Store**：只保存用户审核过的长期条目；
5. **Compile View**：按任务生成最小必要、目的绑定且有期限的冻结输入；
6. **Run Task**：下游 Agent 只读取 View，不直接读取整个 Store；
7. **Review Patch**：新认识先进入 pending，用户逐项批准、修改或拒绝。

## 本地命令

runtime 注册 17 个操作名：下列 14 个正式 V0.4 操作、2 个迁移命令，以及 `list`（`list-patches` 的兼容别名，不是独立能力）。

请在 `skills/brief-yourself/` 内运行这些命令，或使用对应的绝对路径。带 `--help` 的条目只查看用法，不读写任何数据。详细参数与副作用见 `skills/brief-yourself/references/store-operations.md`。

### Store 生命周期

```bash
python scripts/context_store.py --help
python scripts/context_store.py init --help
python scripts/context_store.py validate --store <store>
python scripts/context_store.py inspect --store <store>
python scripts/context_store.py derive-core-summary --help
```

### 来源登记

```bash
python scripts/context_store.py register-source --help
```

`register-source` 是 Evidence Source 的唯一写入入口，`consent` 必须为 `explicit`。它**不是读取授权界面**：读取历史、项目、简历、Obsidian 或 Harness Memory 前的授权卡由 `source-consent-and-disclosure.md` 约束；只有用户已授权读取并批准长期登记后才运行本命令。

### View

```bash
python scripts/context_store.py create-view --help
python scripts/context_store.py validate-view --help
```

敏感范围与限制在 `create-view` 这一步经 disclosure 和用户批准决定；`validate-view` 只做验证（过期、用途、敏感级别、权限结构），不修改也不创建任何限制。

### Patch

```bash
python scripts/context_store.py stage-patch --help
python scripts/context_store.py list-patches --help
python scripts/context_store.py apply-patch --help
python scripts/context_store.py reject-patch --help
```

### 导出与删除

```bash
python scripts/context_store.py export --help
python scripts/context_store.py purge-plan --help   # 只预览，零写入
python scripts/context_store.py purge --help        # 需前置 plan-token，见下
```

`purge` 不提供可直接复制执行的示例。它要求 `--plan-token` 来自紧邻前一次的 `purge-plan`，并同时需要 `--approve` 与 `--confirmed-by`。Store 在预览之后发生任何变化都会使 token 失效，需要重新 `purge-plan`。

### 历史迁移（仅迁移场景）

```bash
python scripts/context_store.py migrate-v02 --help
python scripts/context_store.py preview-migrate-v03 --help
```

只服务 V0.2→V0.3 与 V0.3→V0.4 的历史迁移，不属于常规任务能力；先读 `skills/brief-yourself/references/migration-v0.3-to-v0.4.md`。

## 隐私与治理

- 读取历史、项目、简历、Obsidian 或 Harness Memory 前，先展示来源授权卡并等待明确授权；授权卡由 `source-consent-and-disclosure.md` 约束，与 `register-source` 是两个不同的门；
- History-assisted 只读取当前任务的最小必要范围，不默认扫描全部 Memory；
- 历史内容和 Agent 推断只能成为候选 evidence；
- `private` / `restricted` 默认排除，边界不匹配时 fail closed；范围与限制由 `create-view` 的 disclosure 和用户批准决定，`validate-view` 只负责验证；
- 长期写回必须由用户审核具体 Patch 后明确批准；Core Summary 只从符合条件的已确认 Claim 派生，不接受直接晋升（`stage-patch` → `apply-patch` → `derive-core-summary`）；
- 保留反例、矛盾、变化与未知，不把用户压缩成整齐人设；
- 用户可以查看（`inspect`）、导出（`export`）、限制（`create-view` 的 disclosure 与 `validate-view`）、修正（`apply-patch`）、拒绝（`reject-patch`）、退休（`apply-patch`）或请求删除（`purge-plan` → `purge`）可控副本。

仓库与发布包不包含真实 Personal Context、私密 Store、对话记录、简历或本机路径。详见 [隐私边界](docs/PRIVACY.md) 与 [治理原则](docs/GOVERNANCE.md)。

## 当前限制

- `1.0.1` 只实现 `subject.type=person`；
- Tension / Unknown 可作为结构对象读取，但尚无正式 Patch 写入口；
- Store、View 与 Patch 不会自动双向同步；
- runtime 是 single-writer，不提供并发写入协调；
- Memory 读取能力和授权界面取决于宿主 Agent 平台；安装 Skill 不等于获得所有历史权限。

## 验证摘要

`1.0.1` 的合成验收基线为 `110 tests / 0 failures`（含 15 项发布门禁检查）。发布包包含 `17` 个 canonical Skill 文件，并与 `skills/brief-yourself/` 逐项核对。

这些数字说明检查范围，不构成绝对安全或生产环境保证。

## 仓库结构

```text
brief-yourself/
├─ skills/brief-yourself/   # canonical Skill
├─ docs/                    # 架构、隐私、治理与术语
├─ release/                 # 1.0.0 Skill ZIP 与 SHA-256
├─ README.md                # 中文入口
├─ README.en.md             # English entry
├─ SECURITY.md              # 安全反馈边界
└─ LICENSE                  # MIT License
```

## License

[MIT License](LICENSE) © 2026 Brief Yourself contributors.
