# Brief Yourself 1.0 Governance Principles

[中文](GOVERNANCE.md) | [English](GOVERNANCE.en.md) | [Glossary](GLOSSARY.en.md)

Brief Yourself is not designed to make an agent store more personal information. It is designed to let the user control how personal information is accessed, interpreted, disclosed, and updated.

1. **Consent before access.** Disclose source, scope, purpose, external transfer, storage, and deletion before reading history, projects, resumes, or Harness Memory.
2. **Minimum necessary access.** History-assisted sessions retrieve only the history required for the current goal and do not scan or import all Memory by default.
3. **History is evidence.** Memory, project records, and agent judgments are candidate evidence and cannot automatically become confirmed Claims.
4. **Separate Store, View, and Patch.** The Store is the long-term source of truth, a View is frozen task input, and a Patch is a proposed change.
5. **Explicit purpose and audience.** A View binds purpose, principal, the complete audience, revision, TTL, and persistence permissions.
6. **User-controlled write-back.** New observations remain pending Patches until the user approves each proposal.
7. **Preserve tensions and unknowns.** Counterexamples, changes, Tensions, and Unknowns must not be forced into a neat persona.
8. **Share governed Context across agents.** Agents may read the same frozen View and return different Patches without sharing all Memory or directly editing the Store.
9. **Fail closed.** Reject access when purpose, audience, permission, revision, or expiry is unclear.
10. **User control.** The user may inspect, export, correct, reject, restrict, retire, or request deletion of controlled copies.

## Current boundaries

- Only `subject.type=person` is implemented.
- The File Adapter does not read the Store or a Memory API.
- The runtime currently guarantees local single-writer operation only.
- Tensions and Unknowns do not yet have formal Patch create/update actions.
- Protocol portability does not mean every agent platform has completed Memory integration or interoperability validation.
