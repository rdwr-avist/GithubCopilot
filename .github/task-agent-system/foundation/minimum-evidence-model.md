# Minimum Evidence Model

**Contract:** minimum-evidence-model  
**Canonical path:** `.github/task-agent-system/foundation/minimum-evidence-model.md`  
**Contract version:** 1.1  
**Contract kind:** Foundation contract

## 1. Purpose and authority order

This Foundation contract defines minimum evidence validation, freshness and staleness, trust and provenance, proof strength, action or command controls, execution-verification policy, compact output handling, and Reviewer verification and reproduction needs.

The approved `task-agent-work-plan-v1-final.md` is the primary authority. The approved `task-package-schema.md`, Schema version 1.0, is the binding structural authority for canonical artifacts, fields, identities, references, conditionality, cardinalities, and relationships. The approved `task-authority-model.md`, Model version 1.0, governs ownership, read-only consumption, authority scope, freeze, validity, invalidation, and post-freeze handling. This contract completes those authorities only within evidence scope and MUST NOT redefine them.

Approved Design, Component Implementation Plan, Component or Interface HLTP, repository evidence, Domain Skills, human decisions, and other Foundation contracts remain authoritative only within their scopes. Conversation history, hidden state, and unapproved drafts are not sources of truth.

## 2. Normative language and boundaries

**MUST**, **MUST NOT**, and **REQUIRED** are mandatory. **SHOULD** and **SHOULD NOT** are guidance that may be departed from only for a documented, use-specific reason. **MAY** states permission. A conditional rule applies only when its stated condition is true.

`Build/Test Evidence` remains the canonical execution-evidence artifact. This contract preserves every existing field, conditionality, shape, cardinality, canonical artifact, and inventory slot. Evidence categories below classify evidence use; they are not new artifacts or inventory slots.

Ownership remains with the approved role that executed and recorded the action, normally DP-Task-Implementer and, for approved review-side execution, DP-Task-Reviewer. Existing consumers remain read-only. Evidence freezes when its record is created after the action occurred. Material correction is not made in place. This contract creates no ACL or alternate ownership model.

## 3. Minimum evidence record

A `Build/Test Evidence` instance MUST retain the common artifact header and all fields defined by the Task Package Schema.

### 3.1 Identity, type, and producer

- `evidence_id` is REQUIRED, equals `artifact_id`, and identifies one evidence item or execution.
- Every repeated execution receives a new `evidence_id`, even when command, target, context, and result are unchanged.
- `evidence_type` is REQUIRED. It describes the evidence nature, not outcome alone; it does not replace `action_or_command`, grant validity, or close the open taxonomy.
- Producer is represented by REQUIRED `producing_role_or_source`. It identifies the actual executor and recorder, or actual non-Agent source. Reviewer-side execution identifies DP-Task-Reviewer as producer of the new record.
- Provenance does not broaden authority. An approved role's assertion is not automatically evidence.

### 3.2 Action or command

`action_or_command` is REQUIRED. When a command exists, it records the command actually executed with material arguments, target, filters, and options. `ran tests` is insufficient when the exact command is available. A non-command inspection identifies the operation precisely enough to verify. Intent, plans, recommendations, prepared-but-not-run commands, and expected actions are not completed evidence.

Secrets MUST NOT be copied. Redaction is acceptable only when it does not conceal material scope or prevent verification; protected configuration SHOULD use a resolvable approved reference.

### 3.3 Target, worktree, baseline, and implementation

- `target_or_worktree` is REQUIRED as a `LocationRef` and identifies the actual target. An ambiguous local path is insufficient.
- `repository_id` is REQUIRED when repository content is targeted, agrees with the target, and is not fabricated for non-repository evidence.
- Non-implementation repository evidence requires immutable `baseline_id` matching the inspected state.
- Evidence evaluating implementation requires exact `implementation_id`. Baseline or worktree name does not substitute for it.
- A mutable worktree is bound to `implementation_id` or an immutable revision in `target_or_worktree`.
- Identity binding agrees with associated Implementation Report, Acceptance Criteria Evidence, Task Review Result, Task State, and package references.

### 3.4 Result, exit code, output, context, and time

- `result` and `result.outcome_summary` are REQUIRED. `result.result_ref` is REQUIRED when detail is stored separately. Result records the actual outcome, not completion or Review PASS.
- `exit_code` is REQUIRED when produced and MAY be absent only when none is produced. It does not replace result or output.
- Zero does not override no tests found, required skips, filtering, partial or not-run work, timeout, or abort. Nonzero does not hide relevant failure detail.
- `output_summary` is REQUIRED, concise, relevant, materially complete, and does not duplicate a long raw log.
- `execution_context` and `executor_or_environment` are REQUIRED for execution evidence. Material configuration, environment, toolchain, filters, modes, and target-specific context are included; irrelevant detail need not be copied.
- `executed_at` is REQUIRED and differs from `created_at` and `updated_at`. Age alone does not cause staleness.

### 3.5 Raw output

`raw_output_ref` is REQUIRED when raw output is available or detailed output is needed. It is a durable `LocationRef` bound to the correct run, target, and immutable revision. Output from another run is rejected. Long raw logs are referenced, not copied.

When raw output did not exist, was not retained, or is unavailable, the evidence states that limitation when it affects verification and never fabricates output. Loss requires rerun only when remaining evidence cannot verify the intended use. A broken required reference invalidates that use; the frozen record is not repaired in place. The action is rerun and a new evidence instance is created.

### 3.6 Semantic mappings and references

- `requirement_refs` maps evidence checking requirements.
- `acceptance_criterion_refs` maps criterion evaluation.
- `review_check_refs` maps Reviewer evidence to checks.
- `task_local_test_case_refs` maps planned execution to the active Task-local HLTP.
- `unplanned_test_authority_ref` is REQUIRED for authorized unplanned tests.

Inapplicable collections MAY be empty. Evidence used as proof has at least one applicable obligation-level mapping; task-level association does not replace it. References are stable and typed, with target type, identity, relationship, and revision or locator when needed. Linkage MUST NOT depend only on filename, heading, text match, local path, or conversation proximity.

Mapping proves traceability, not satisfaction. Unplanned tests are not represented as planned and do not silently modify HLTP obligations.

## 4. Evidence categories

### 4.1 Execution evidence

Execution evidence is a factual record of build, test, analysis, or verification actually run against an identified target. It may record success or failure, excludes planned or not-started commands, and proves only executed scope.

### 4.2 Inspection evidence

Inspection evidence records inspection actually performed against identified content, path, symbol, diff, artifact, or output. It identifies target identity, inspected scope, observations, and material limits. It does not claim repository truth beyond the inspected scope. If represented outside Build/Test Evidence, stable provenance, target, scope, observation/result, and mapping remain required without creating a new canonical artifact.

### 4.3 Traceability evidence

Traceability evidence is a resolvable mapping from an obligation to evidence, test, change, or review check. It proves linkage and structural coverage only. It does not independently prove semantic satisfaction and cannot repair invalid, stale, corrupted, or mismatched evidence.

### 4.4 Reviewer evidence

Reviewer evidence is independently inspected or produced by DP-Task-Reviewer and is linked to the applicable review check, exact reviewed inputs, and exact implementation when applicable. Reviewer execution creates new evidence with Reviewer producer, new ID, target, context, time, result, output, and mappings. Reviewer evidence is distinct from Task Review Result, does not grant Review PASS, and does not edit Implementer evidence or change implementation.

### 4.5 Unsupported declarative claim

Bare claims such as `build passed`, `tests passed`, `implementation complete`, `criterion satisfied`, or `reviewed` are unsupported when required action, immutable target, context, result, material output, or mapping is missing, stale, invalid, or mismatched. They may be reports but are not independent proof. Role, ownership, provenance, or confidence does not cure missing evidence.

## 5. Validity and trust

Validity is use-specific. Evidence is valid for an intended use only when, as applicable to its evidence category:

1. its ID and references resolve;
2. the execution or inspection actually occurred, or the traceability linkage actually resolves;
3. action or command is accurate when the category records one;
4. the exact target and coherent repository, baseline, worktree revision, and implementation associations are present as applicable;
5. producer and material context are accurate;
6. result, exit code when applicable, summary, time, and required detail are uncorrupted and coherent when the category records them;
7. required output references resolve to the correct run and target;
8. required mappings resolve and match what the execution, inspection, or traceability linkage can prove;
9. proof strength is sufficient for the obligation; and
10. no material invalidating change or conflict exists.

Evidence may remain historically true but unsuitable for another criterion, implementation, review check, or claim. Trust derives from resolvable provenance, coherent identities, inspectable or reproducible information, and consistent result/output, not role alone.

## 6. Freshness, staleness, and invalidation

Freshness means evidence still applies to its target, context, scope, mappings, and intended use. It is not a fixed age limit.

Stale evidence remains a correct historical record but a later material change prevents active reliance for an affected use. Staleness differs from corruption or misreporting, broken retrieval, and deletion. Stale or invalid evidence remains immutable historical material but is not active proof. Freeze is not continuing validity.

A new implementation does not inherit prior implementation-bound evidence. Such evidence MUST NOT be reused or relabeled as proof for the new implementation. Verification evaluating the new implementation is rerun against its exact `implementation_id`; prior Review PASS and criterion coverage do not roll over.

Separately bound evidence MAY remain applicable only when its verified subject and immutable target are unchanged, it does not evaluate the changed implementation, and explicit independence is established from direct identity, scope, dependency, and mapping evidence. This continued applicability is explicit and selective.

Repository, configuration, environment, toolchain, and target-specific changes are assessed for material impact. Relevant material change requires rerun. Immutable revision preserves historical identity but not applicability to changed content. Worktree or branch name alone is insufficient. Unrelated or nonmaterial change does not blanket-invalidate evidence. Invalidation follows affected identity, context, scope, output, and mapping dependencies; unrelated evidence remains unaffected.

## 7. Rerun and correction

Rerun is REQUIRED when material to intended use because:

- implementation or relevant target changed;
- material context differs;
- prior action did not cover the obligation;
- the record is materially wrong, corrupted, or misreported;
- required output cannot verify the claim; or
- Review requires named evidence, execution, or check reverification.

Rerun is not universal because evidence is old, unrelated content changed, or Reviewer did not personally execute. Valid proportionate evidence may be inspected unless review obligation or material verification gap requires reproduction.

Every rerun creates a new `evidence_id`, `executed_at`, target/context record, result, exit code when applicable, output, mappings, and a raw-output reference when raw output is available or needed. Prior evidence remains frozen and historical.

Material change to action, target, identity binding, result, interpretation-changing output, proof-scope mapping, or material context is not cosmetic. Frozen evidence is not edited or repaired. The executing role reruns and creates a distinguishable instance. A prior Task Review Result is not edited; after correction and reverification, Reviewer creates a new result.

## 8. Proof strength and result interpretation

Exit code is interpreted with command, target, output, context, and obligation. Zero with no tests found or skipped required tests is not semantic PASS. Accurate failed execution is valid evidence of failure, not successful proof, and differs from an action never started.

Evidence is bounded by actual action or observation, immutable target, executed or inspected scope, context, output, and mappings. It MUST NOT extrapolate from focused tests to full-suite success, from inspected subset to whole repository, or to untested behavior.

Multiple valid, identity-compatible evidence items MAY jointly support a criterion. Aggregation cannot repair invalid items or identity mismatch, bridge implementations, turn traceability into proof, create Review PASS, or establish completion.

Acceptance Criteria Evidence remains mapping. An evidenced/satisfied criterion references applicable evidence; zero evidence has durable unresolved, blocked, or not-yet-evidenced basis. Reviewer does not edit mapping to obtain PASS.

## 9. Compact logs

Long raw logs MUST NOT be copied into Build/Test Evidence, Implementation Report, Acceptance Criteria Evidence, Task Review Result, or Task State. A focused excerpt MAY appear only when necessary and not as duplication.

A compact summary states when material: what ran; suite/target and configuration/filter; executed, passed, failed, skipped, and not-run counts; first relevant failure; timeout, abort, crash, interruption, or partial execution; scope restriction; and execution precondition or blocker. It omits nothing that changes interpretation.

## 10. Reviewer verification and reproduction

Reviewer can identify producer; action/command; repository when applicable; baseline, immutable worktree revision, or implementation; target/scope; material context; time; result and exit code; summary and required raw/detail references; and semantic mappings. Missing, ambiguous, mismatched, stale, or unverifiable required information is not valid proof.

When reproduction is required, evidence provides or references action/command; immutable target; repository/worktree locator; implementation or baseline; material inputs, configuration, filters, and modes; relevant environment/toolchain; expected output interpretation; and obligation or review check.

Reproduction is proportionate, not universal. Inspection may suffice for complete valid evidence. Reproduction is required by governing review obligation or material identity mismatch, insufficient output, context ambiguity, unverifiable result, or required reverification. This creates no risk engine.

Reviewer-side execution creates new Reviewer evidence and does not edit Implementer evidence, implementation, or prior review result.

## 11. Cross-artifact rules

- Task Repository Context observations remain bound to repository/baseline or worktree and inspection limits.
- Task Execution Plan verification entries are obligations, not evidence of execution.
- Task-local HLTP defines semantics; evidence references tests without changing obligations.
- Implementation Report is factual declaration and evidence reference, not proof.
- Acceptance Criteria Evidence is complete mapping, not validation or Review PASS.
- Build/Test Evidence is factual execution, not criterion satisfaction, Review PASS, or completion by itself.
- Task Review Result is separate Reviewer judgment over exact inputs and implementation.
- Task State and package assembly locate evidence but are not evidence, do not validate it, and do not increase proof strength.

No execution evidence exists before execution. Not-yet-run is represented as not yet available. Blocked-before-execution uses blocked availability and durable blocking reference. Executed failure has failure evidence. These states remain distinct.

## 12. Explicit non-goals

This contract does not create or define:

- evidence database, centralized store, index, query API, or evidence service;
- logging collector, uploader, transport, retention framework, or raw-log repository;
- validation service, daemon, engine, execution infrastructure, or automated trust service;
- additional Agent, Skill, canonical artifact, or inventory slot;
- Task Status Model meanings or transitions;
- resume or recovery procedure;
- ACL, permission, locking, or mutation system;
- general revision framework; or
- redesign or authority redistribution among the five approved Agents.
