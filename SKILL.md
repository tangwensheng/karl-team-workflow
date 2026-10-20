---
name: karl-team-workflow
description: Run a disciplined multi-agent software delivery workflow with a coordinating A lead, visible B workstreams, optional C subtasks, explicit write scopes, daily acceptance, blocker resolution, contract updates, integration gates, and truthful final reporting. Use when a project needs task decomposition, parallel team coordination, nightly/daily acceptance, blocked-work recovery, Git-based handoffs, or release readiness checks.
---

# Karl Team Workflow

Use this skill as the operating procedure for a software project that has one coordinating lead (A) and multiple accountable workstream owners (B11, B12, B13 or equivalent). Optimize for integrated, verifiable delivery—not activity or parallelism.

## 1. Establish control before coding

1. Treat the current conversation as A. A owns scope, contracts, cross-stream integration, acceptance, and release decisions.
2. Read the repository's current `AGENTS.md` and control-plane files before relying on prior summaries. At minimum locate:
   - active task register;
   - current contract/semantic rules;
   - assignment files for each workstream;
   - latest handback/status from each owner;
   - migration, API, event, and metric indexes when relevant.
3. Reconcile the requested outcome with the current contract. If sources conflict, stop only the affected work and record the conflict; do not guess.
4. Define each work item with: owner, branch/worktree, exact write scope, dependencies, acceptance criteria, required evidence, and integration obligations.
5. Never allow two active writers to modify the same file, module, shared contract, migration index, root build files, CI, or shared UI shell. Serialize those changes.

## 2. Delegate visibly and manage directly

When the host supports user-visible project tasks, create B tasks as visible child conversations and persist every returned identifier. B may create and manage its own C tasks; A should not routinely manage B's C tasks. Never substitute hidden workers for visible project tasks when visibility is required.

Use stable labels such as `B11`, `B12`, `B13`, and name tasks with the label plus a concise summary. A-to-B and B-to-C are the normal communication paths. A may escalate or take over only with a recorded reason.

A workstream handoff must include:

```text
owner:
branch/worktree:
write_scope:
read_dependencies:
contract_or_semantics:
acceptance_criteria:
required_tests:
integration_obligations:
blocked_by:
due_or_review_window:
```

A delivery is **ready for review**, not accepted, until the parent verifies the actual diff, tests, scope, contract semantics, and evidence.

## 3. Daily/nightly acceptance loop

When the user asks to start today's/tonight's acceptance, execute the full loop—not just a code review:

1. Read the current control plane and all three workstream handbacks.
2. Safely synchronize Git: inspect status, fetch/prune, compare local and remote main, and sync without force-pushing or overwriting unknown changes.
3. For each workstream, verify the actual branch/PR and full commit SHA, changed files, write scope, sensitive-data safety, contract compliance, tests, CI, and cross-stream obligations. Use only `ACCEPTED`, `CHANGES_REQUESTED`, `BLOCKED`, or `NO_NEW_DELIVERY` as acceptance outcomes.
4. Reproduce relevant checks. Record each as `passed`, `failed`, `not_run`, or `unavailable`; never convert unavailable checks into a pass.
5. Update the test/staging server only from an explicit SHA, preserving rollback. Run health, liveness, readiness, authorization, and data-boundary checks when runtime behavior changed.
6. Resolve A-owned blockers and integration work directly. Return business defects to the original owner on the original branch/PR; A must not silently rewrite another owner's business implementation.
7. Update assignments, the active task register, contracts/amendments, and next work. Keep roughly two working days of authorized, acceptance-ready work for steady workstreams; list rework separately and do not count it as new capacity. Preserve any special single-active-package rule defined by the current contract.
8. Run policy, diff, and status gates; commit only real control-plane/integrated changes; push only when authorized; verify the remote SHA and CI independently.
9. Report what was checked, what was accepted, every blocker, owner, next action, recovery condition, and whether changes were actually pushed/deployed.

Do not end the acceptance cycle with “looks good,” a local-only claim, or a push claim that was not verified.

## 4. Blocker protocol

Every blocker entry must contain:

```text
fact:
impact:
scope_stopped:
owner:
attempts:
next_action:
expected_time_or_unknown:
resume_condition:
verification_after_unblock:
```

Classify blockers as:

- **A-owned:** control-plane, environment, server, authorized integration, or release coordination. Resolve and re-run the affected check.
- **B-owned:** business implementation or tests within the assigned write scope. Return to the original owner and branch/PR.
- **Contract decision:** freeze affected work and ask the smallest decision question; do not invent semantics.
- **External/unavailable:** record evidence and continue independent work.

Stop only the affected dependency cone. Continue independent work. Repeated failure requires a narrower task, reduced parallelism, reassignment, or an explicit takeover—not silent drift.

## 5. Review and integration gates

Before accepting a B delivery, verify:

- reported SHA equals the actual commit/PR head;
- worktree and branch state are understood;
- changed files stay inside the approved write scope;
- no secrets, credentials, cookies, tokens, `.env`, or real production data entered the change;
- module tests and required CI checks pass or are truthfully marked unavailable;
- `git diff --check` and policy/security checks pass;
- business semantics match the frozen contract;
- mocks, inferred fields, and real supplier contracts are clearly distinguished;
- risks and integration obligations are recorded.

After accepted changes are integrated, run the repository's full required verification (typically build/verify, policy, diff check, status, and runtime health). A owns final acceptance and release readiness.

## 6. Safe defaults and domain guardrails

Preserve repository-specific rules; do not invent formulas or supplier contracts. Unless the project contract says otherwise:

- permissions missing, scope empty, or runtime conditions unsafe => deny by default;
- automation/publishing defaults off;
- unknown outcomes are not automatically retried;
- independent business metrics remain independently stored and displayed;
- authoritative upstream values are consumed rather than recomputed from convenient proxy roles;
- financial amounts with different meanings remain separate;
- unconfirmed external paths/fields remain explicitly pending confirmation;
- no production secrets or customer data in Git.

## 7. Final report format

End each delivery or acceptance cycle with:

1. **Scope and baseline** — commit(s), branches/PRs, and source-of-truth documents.
2. **Workstream results** — one line per B owner with acceptance outcome and evidence.
3. **Checks** — passed, failed, not run, unavailable.
4. **Blockers and A actions** — fact, impact, owner, next action, resume condition.
5. **Integration/release state** — local merge, remote push, CI, deployment, and exact SHA status.
6. **Next authorized tasks** — owner, write scope, dependency, acceptance criteria.
7. **Residual risks** — explicit and bounded.

Never claim completion when a required task, acceptance step, push, CI result, deployment, or handoff remains unverified.
