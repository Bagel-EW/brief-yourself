# Brief Yourself 1.0.1

[中文](README.md) | [English](README.en.md)

**Help an agent understand you without giving up control over access, disclosure, or long-term memory.**

Brief Yourself is a portable Personal Context Skill. It turns personal background into reviewable Context, compiles the minimum frozen input needed for each task, and returns new observations to the user as proposed changes.

> **Release status:** `Brief Yourself 1.0.1` is the current public open-source release under the MIT License. The internal schema compatibility id remains `0.4`; it only governs protocol compatibility and historical migration, not the product version. The packaged Skill matches the repository source; a GitHub Release and tag are outside this release scope.

## Eight terms to know

| Term | Chinese | Short meaning |
|---|---|---|
| Personal Context Store | 个人上下文存储 | The user-reviewable long-term source of truth; it is not an agent's built-in Memory. |
| Claim | 认识条目 / 可审核陈述 | A personal statement with provenance, scope, status, counterevidence, and sensitivity. |
| Context View | 上下文视图 / 冻结任务视图 | The minimum read-only input compiled for one specific task. |
| Context Patch | 上下文变更提案 | A proposed update after a task; it cannot be written back without user approval. |
| History-assisted | 历史辅助取证 | Purpose-limited retrieval from authorized history, not a read of all Memory. |
| Harness Memory | Agent 运行记忆 | Preferences and summaries maintained by an agent platform; not the Personal Context source of truth. |
| Principal / Audience | 实际执行者 / 允许接收者 | Who uses the Context and who may receive the input or output. |
| TTL | 有效期 | The expiry period after which a View is no longer valid. |

See the full [Glossary](docs/GLOSSARY.en.md) and formal [Governance Principles](docs/GOVERNANCE.en.md).

## Understand it in 30 seconds

Install it in environments that support Agent Skills:

```bash
npx skills add https://github.com/Bagel-EW/brief-yourself --skill brief-yourself
```

You can also copy `skills/brief-yourself/` into an agent's Skill directory or use `release/brief-yourself-1.0.1.zip`. The previous `release/brief-yourself-1.0.0.zip` is kept for verification.

Then start with a bounded request:

```text
Run a Standard Brief. First tell me which sources and scope you want to read,
why you need them, where anything will be stored, and how I can delete it.
Until I approve, use this conversation only and do not access project history or Memory.
```

Brief Yourself establishes a Session Contract before interviewing, calibrating evidence, and compiling a Context View. Installing the Skill does not automatically import history.

## What it solves

Ordinary agent Memory can mix preferences, summaries, inference, and platform state. Brief Yourself separates “remember me” into three governable actions:

```text
Personal Context Store
        ↓ filter by purpose, subject, audience, and expiry
Frozen Context View
        ↓ read-only input for the current task
Task output
        ↓ new observations are staged
Pending Context Patch → user approves / rejects each proposal → Store
```

- **Store:** the canonical long-term source containing reviewable Claims, sources, status, permissions, and revisions.
- **View:** a frozen input for one task, bound to purpose, principal, the complete audience, TTL, and disclosure boundaries.
- **Patch:** a proposed change after a task. It never enters the Store without item-by-item user approval.

Saved information is not automatically available to every agent, and an agent's observation is not automatically a long-term fact about the user.

## Good fit / not a fit

### Good fit

- Give different agents consistent but minimum-necessary personal background;
- Compile task-specific Context for writing, planning, reflection, or long-running projects;
- Use selected project history to improve questions while retaining per-session consent;
- Audit where a Claim came from, where it applies, and what contradicts it;
- Inspect, export, correct, retire, or delete controlled Personal Context.

### Not a fit

- Psychological diagnosis, personality scoring, or defining a user's “true self”;
- Silently injecting all conversations into every task;
- Turning a single agent inference into a long-term fact;
- Production systems that require concurrent writers to one Store;
- Treating passing tests as absolute safety or universal agent interoperability.

## Requirements and compatibility

The local tools use the Python standard library only. The Skill is designed for agent environments that can read Skill files, access user-authorized local files, and run Python.

| Environment | Status | Notes |
|---|---|---|
| Codex / local coding agent | Verified as a file-based Skill | Can run Store, View, Patch, and test scripts. |
| Other Skill-capable agents | Protocol is portable | Installation, Memory permissions, and invocation are platform-specific and require validation. |
| Browser-only chat | Limited | The interview method can be used, but local Store and script behavior cannot be guaranteed. |

Cross-agent interoperability exchanges bounded Views and Patches rather than sharing the whole Store. The current runtime is **single-writer** only.

## Install

### Option 1: Skills CLI

```bash
npx skills add https://github.com/Bagel-EW/brief-yourself --skill brief-yourself
```

### Option 2: Manual installation

1. Obtain this repository;
2. Put `skills/brief-yourself/` in your agent's Skill directory;
3. Confirm that `SKILL.md`, `agents/`, `scripts/`, `references/`, and `assets/` are present;
4. Run validation with synthetic data before introducing real personal information.

### Option 3: Versioned package

Extract `release/brief-yourself-1.0.1.zip`. `release/SHA256SUMS.txt` records its size and SHA-256 so the artifact can be verified.

## Example triggers

```text
Run a Brief Yourself interview using this conversation only.
```

```text
I authorize you to read the specified documents in project A to calibrate my product judgment.
Do not read other projects or all of Memory.
```

```text
Compile a Context View for this writing task from approved Personal Context. Set a 24-hour TTL.
```

```text
Turn new observations from this task into a Pending Context Patch. Let me review each item; do not write anything back yet.
```

## Standard workflow

1. **Scope:** confirm sources, scope, purpose, principal, audience, storage, and deletion;
2. **Interview:** ask evidence-bounded questions from the conversation or authorized sources;
3. **Calibrate:** separate facts, self-reports, observations, inferences, counterevidence, tensions, and unknowns;
4. **Store:** retain only user-reviewed long-term entries;
5. **Compile View:** create a minimum-necessary, purpose-bound, expiring frozen input;
6. **Run Task:** let the downstream agent read the View, not the whole Store;
7. **Review Patch:** stage new observations and let the user approve, edit, or reject each one.

## Local commands

The runtime registers 17 operation names: the 14 formal V0.4 operations below, 2 migration commands, and `list` (a compatibility alias for `list-patches`, not a separate capability).

Run these commands inside `skills/brief-yourself/`, or use absolute paths. Entries with `--help` only print usage and read or write nothing. Full parameters and side effects live in `skills/brief-yourself/references/store-operations.md`.

### Store lifecycle

```bash
python scripts/context_store.py --help
python scripts/context_store.py init --help
python scripts/context_store.py validate --store <store>
python scripts/context_store.py inspect --store <store>
python scripts/context_store.py derive-core-summary --help
```

### Source registration

```bash
python scripts/context_store.py register-source --help
```

`register-source` is the only entry that writes an Evidence Source, and `consent` must be `explicit`. It is **not the read-consent surface**: the consent card shown before reading history, projects, resumes, Obsidian, or Harness Memory is governed by `source-consent-and-disclosure.md`. Run this command only after the user has authorized the read and approved long-term registration.

### View

```bash
python scripts/context_store.py create-view --help
python scripts/context_store.py validate-view --help
```

Sensitive scope and restrictions are decided at `create-view` through disclosure and user approval. `validate-view` only validates (expiry, purpose, sensitivity, permission structure); it neither modifies nor creates restrictions.

### Patch

```bash
python scripts/context_store.py stage-patch --help
python scripts/context_store.py list-patches --help
python scripts/context_store.py apply-patch --help
python scripts/context_store.py reject-patch --help
```

### Export and deletion

```bash
python scripts/context_store.py export --help
python scripts/context_store.py purge-plan --help   # preview only, zero writes
python scripts/context_store.py purge --help        # requires a preceding plan-token
```

`purge` deliberately ships no copy-paste runnable example. It requires a `--plan-token` from the immediately preceding `purge-plan`, plus both `--approve` and `--confirmed-by`. Any change to the Store after the preview invalidates the token, so `purge-plan` must be run again.

### Historical migration (migration scenarios only)

```bash
python scripts/context_store.py migrate-v02 --help
python scripts/context_store.py preview-migrate-v03 --help
```

These serve V0.2→V0.3 and V0.3→V0.4 migration only and are not part of the routine task surface. Read `skills/brief-yourself/references/migration-v0.3-to-v0.4.md` first.

## Privacy and governance

- Show the source consent card and wait for explicit authorization before reading history, projects, resumes, Obsidian, or Harness Memory. The card is governed by `source-consent-and-disclosure.md` and is a different gate from `register-source`;
- History-assisted sessions retrieve the minimum necessary scope and never scan all Memory by default;
- Historical content and agent inference remain candidate evidence;
- `private` and `restricted` content is excluded by default; a boundary mismatch fails closed. Scope and restrictions are decided by `create-view` disclosure and user approval, while `validate-view` only validates;
- Long-term write-back requires explicit approval of the concrete Patch; Core Summary is derived only from eligible confirmed Claims and cannot be promoted directly (`stage-patch` → `apply-patch` → `derive-core-summary`);
- Preserve counterevidence, tensions, change, and unknowns instead of compressing the user into a neat persona;
- Let the user inspect (`inspect`), export (`export`), restrict (`create-view` disclosure and `validate-view`), correct (`apply-patch`), reject (`reject-patch`), retire (`apply-patch`), or request deletion (`purge-plan` → `purge`) of controlled copies.

The repository and package contain no real Personal Context, private Store, conversation history, resume, or local machine path. See [Privacy](docs/PRIVACY.md) and [Governance](docs/GOVERNANCE.en.md).

## Current limitations

- `1.0.1` implements `subject.type=person` only;
- Tensions and Unknowns can be read as structured objects but do not yet have formal Patch write actions;
- Store, View, and Patch are not automatically synchronized in both directions;
- the runtime is single-writer and does not coordinate concurrent writes;
- Memory access and consent interfaces depend on the host agent platform; installing the Skill does not grant access to all history.

## Validation summary

The `1.0.1` synthetic acceptance baseline is `110 tests / 0 failures` (including 15 release gate checks). The release package contains `17` canonical Skill files and is checked item by item against `skills/brief-yourself/`.

These numbers describe the validation scope; they are not a promise of absolute safety or production readiness.

## Repository structure

```text
brief-yourself/
├─ skills/brief-yourself/   # canonical Skill
├─ docs/                    # architecture, privacy, governance, glossary
├─ release/                 # 1.0.0 Skill ZIP and SHA-256
├─ README.md                # Chinese entry
├─ README.en.md             # English entry
├─ SECURITY.md              # security reporting boundaries
└─ LICENSE                  # MIT License
```

## License

[MIT License](LICENSE) © 2026 Brief Yourself contributors.
