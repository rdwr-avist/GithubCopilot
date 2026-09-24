# Resume Invariant

## 1. Purpose

This Foundation contract defines the durable conditions under which work on a Task may be resumed after interruption, session restart, context compaction, blocking, correction, or another material change. It defines reconstruction, validation, ownership, and recovery boundaries. It does not execute the next action, replace another Foundation contract, or create new workflow authority.

## 2. Authority Order and Interpretation

The approved Task-Agent work plan is the primary authority for system architecture, delivery order, and phase intent. The Task Package Schema, Task Authority Model, Minimum Evidence Model, and Task Status Model are binding within their respective scopes. These authorities are complementary. Every applicable rule must be satisfied.

A derived artifact, Task State entry, package assembly decision, resume decision, or this contract must not override a governing source within that source's scope. A genuine conflict involving architecture, authority, requirements, domain routing, repository identity, evidence, or Review remains durably blocked and is escalated to the applicable owner or source authority. It must not be resolved silently.

Tests and validation criteria must not be weakened to obtain a PASS.

## 3. Scope

This contract governs only resume semantics for the existing five-Agent Task-Agent System. It applies to the reconstruction and validation of an existing Task and its authorized continuation.

It does not redesign the approved architecture, add an Agent, reopen an approved decision, create a future artifact, or grant any role authority that it does not already have. The Foundation artifact governed by F05 is `.github/task-agent-system/foundation/resume-invariant.md`.

## 4. Normative Conventions

The keywords **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** are normative. A conditional obligation is mandatory only when its stated activation condition is true. If applicability cannot be established from durable evidence, the affected continuation remains blocked rather than being treated as not applicable.

References to fields, identities, artifact types, statuses, ownership, evidence strength, and transition semantics use the definitions in the governing Foundation contracts. This contract does not redefine them.

## 5. Terminology

- **Resume**: validation and reconstruction of the durable state required to identify an authorized next action. Resume does not perform that action.
- **Minimum resumable package**: the smallest conditional set of durable state, active references, identities, blockers, and evidence required for the intended continuation.
- **Resume target**: the approved Agent, human source authority, or governed stage that owns the next authorized action.
- **Active reference**: a durable reference selected for current use and supported by the governing authority.
- **Historical material**: a frozen, superseded, stale, or invalidated instance retained for lineage or audit but not selected as active authority for unsupported uses.
- **Required revalidation**: the checks that must succeed before the authorized continuation may begin.

## 6. Resume Invariant

A Task may resume only when its exact Task identity, active package, current canonical status, relevant repository and implementation identities, applicable evidence and Review identities, unresolved blockers or gates, resume target, authorized next action, and required revalidation can be reconstructed and validated from durable artifacts and state.

Correct continuation MUST NOT depend on conversation history, hidden state, remembered reasoning, or an intermediate draft remembered by a participant. If the durable inputs are missing, ambiguous, mixed, conflicting, or insufficient, the Task remains blocked until the durable package is repaired by the proper owner.

The mere passage of time, restart of a conversation, or existence of an artifact does not authorize resume.

## 7. Durable Sources of Truth

Resume reconstruction uses the following durable sources within their bounded authority:

1. Task State for operational status, workflow position, active references, blocker and pending-action data that the Schema permits it to record.
2. Active Task artifacts for the semantics and decisions owned by those artifacts.
3. Repository state, including the applicable immutable baseline and relevant worktree state, when repository work applies.
4. Exact implementation identity after an implementation exists.
5. Exact evidence and Task Review Result identities when evidence or Review is applicable.

Task State is not evidence, approval, completion proof, or semantic ownership of a referenced artifact. Package assembly selects and relates artifacts but does not grant authority over their content.

## 8. Minimum Resumable Package

The minimum resumable package is conditional on the current Task state and intended continuation. It MUST contain or durably resolve:

- a valid Task State for the exact `task_id`;
- the current Task Contract once it exists;
- the active Task Work Package inventory once the package has a durable location;
- exactly one inventory slot for each of the ten canonical artifact types;
- exact active artifact instances for every applicable and available slot;
- current blockers relevant to package assembly;
- the repository, baseline, worktree, implementation, evidence, and Review identities required by the intended continuation;
- the resume target, authorized pending action, and required revalidation.

Applicability and availability remain separate. The package distinguishes applicable and available, applicable but not yet available, not applicable, and applicable but blocked. An applicable blocked artifact retains a durable `BlockingRef`. An available inventory entry resolves at least one exact artifact instance and is not represented by an empty entry.

Resume-critical external content uses durable identity plus a locator or revision whenever mutable retrieval could select different content. A filename, heading, text match, or mutable local path alone is not sufficient for critical linkage when it can resolve to another instance or repository state.

Full source documents, raw logs, Designs, HLTPs, and Skills SHOULD be referenced rather than copied unless a focused excerpt is structurally necessary. The package does not add an eleventh canonical artifact or inventory slot.

## 9. Resume Reconstruction

Resume reconstruction MUST identify:

- the exact Task by `task_id`;
- the active package inventory and each exact active artifact instance;
- predecessor lineage when required to prevent revision mixing;
- active authority separately from frozen historical material;
- the last durable Task State update;
- the current workflow position and current canonical Task status as distinct facts;
- the last completed governed stage, derived from authoritative artifacts and state rather than chat memory;
- the stage responsible for the next governed action;
- the resume target;
- the durable pending action when one exists.

Reconstruction is incomplete if any fact needed for the intended continuation is guessed, inferred from similarity, or available only in conversation history.

## 10. Identity Coherence

When repository work applies, resume identifies `repository_id`. When work or observations depend on a baseline, it identifies the applicable immutable `baseline_id`. When an established worktree remains relevant, it identifies `worktree_id`. After implementation exists, it identifies the exact current `implementation_id`.

A worktree name or mutable path does not substitute for immutable implementation identity. Task, repository, baseline, worktree, and implementation identities MUST be mutually coherent. Mixed Task identities, mixed repository or baseline identities, and mixed worktree or implementation identities invalidate the affected resume state.

A material implementation change receives a distinguishable `implementation_id`. Prior implementation-bound evidence remains bound to the implementation it evaluated. Prior Review PASS remains bound to the exact implementation and reviewed inputs. Evidence separately bound to an unchanged immutable target may remain applicable only when explicit independence from the changed implementation is established.

Identity reuse based on filename similarity, path reuse, text match, presumed equivalence, or relabeling is invalid.

## 11. Status, Blocker, Finding, and Human-Gate Recovery

Resume reconstructs the current canonical Task status from Task State and its authoritative basis. Workflow position and status remain structurally and semantically distinct. The active status MUST be one of the statuses defined by the Task Status Model. This contract creates no status token and does not redefine status meaning, emitter, pipeline effect, allowed next state, or status-specific resume target.

Resume permission is evaluated according to the active status. A status that prohibits resume remains active until its prerequisites are durably demonstrated.

When a blocker exists, resume identifies the blocker, its durable `BlockingRef`, and its resolution owner. A blocked status may be left only after resolution is verified. Resolution is durable and scoped to the exact blocker and affected identities.

On a FAIL or `FIX_REQUIRED` path, resume preserves the open finding, first failing check, required correction, required reverification, and checks passed before failure. For a blocked Review, it preserves the first blocked check and blocker without fabricating a correction.

When human approval is required, resume identifies the exact pending decision scope. Silence or informal conversation does not count as durable approval. Human approval cannot repair invalid evidence, an invalid Review Result, an identity mismatch, or missing authority.

## 12. Authorized Next Action and Resume Target

Resume identifies the authorized next action, not merely any action that might be allowed. An allowed next state is not an automatic action. The role that owns the governed work performs the next action.

Operational state management MAY route to the resume target but MUST NOT perform that target's governed work. If an upstream artifact changes, continuation returns to that artifact's owner before affected downstream work is reused. If a blocker is resolved without an upstream authority change, work may return to the previously blocked stage after resolution and required validation.

Review resumes only through a new valid `READY_FOR_REVIEW` handoff and a new review attempt. Completion evaluation resumes only when the exact PASS and every other completion prerequisite remain valid.

## 13. Selective Invalidation and Replacement

Resume identifies every active artifact, claim, evidence item, Review Result, or completion claim invalidated for the intended continuation. Invalidation is selective to the materially affected authority, identity, target, context, scope, mapping, or prerequisite. It does not automatically erase independent valid work.

An invalidated frozen instance remains immutable historical material but is not selected as active authority for an unsupported use. A material post-freeze correction creates a distinguishable replacement artifact identity. Replacement preserves predecessor lineage when needed to prevent mixing.

A consumer MAY report invalidity but MUST NOT replace an artifact outside its ownership. After an upstream change, resume validation determines both which downstream artifacts require replacement and which unaffected artifacts may remain active.

## 14. Required Revalidation

Before continuation, resume validation identifies and performs the checks required by the intended next action. As applicable, revalidation verifies that:

- active references resolve;
- Task, repository, baseline, worktree, and implementation identities are coherent;
- the active status still has valid authoritative support;
- the resume target has the artifacts and authority required by its contract;
- required replacement artifacts are frozen and selected;
- relevant blocker, finding, or human-gate resolution is durable and valid;
- active evidence and Review remain applicable to the exact identities and inputs.

Task State is not proof of completion or blocker resolution. Package assembly is not proof. An Implementation Report is not independent proof. Acceptance Criteria Evidence mapping is not independent proof. Human approval does not repair invalid evidence or Review.

## 15. Evidence and Review Validity

Active evidence is identified by exact `evidence_id`. Before it is relied upon, resume verifies that its reference resolves, the represented execution or inspection occurred, it is bound to the exact intended target, its material execution context remains applicable, required raw or detail references resolve to the correct run and target, and its obligation-level mappings remain applicable.

Implementation-evaluating evidence matches the current `implementation_id`. Non-implementation repository evidence matches its immutable `baseline_id`. Evidence freshness is determined by current applicability rather than age. Stale evidence remains historical but is not active proof.

A relevant implementation or target change triggers rerun of affected evidence. Materially wrong, corrupted, misreported, or unverifiable evidence is rerun when required for the intended use. Every rerun creates a new `evidence_id`; prior evidence remains frozen and historical.

Not yet run, blocked before execution, executed failure, partial execution, and successful execution remain distinct. A zero exit code alone does not establish semantic success. Evidence proves no more than its actual target, scope, context, output, and mappings.

When a Task Review Result exists, resume identifies it by exact identity. A substantive PASS or FAIL is valid for exactly one reviewed implementation and exact reviewed inputs. A changed reviewed input invalidates active applicability of the prior result. A correction does not edit the prior result. A new review attempt creates a distinguishable result and restarts at the first ordered Review check. PASS alone does not establish `READY_FOR_COMMIT`.

## 16. Operational Ownership

Before A05 is available, a human state manager performs operational Task State maintenance and Task Work Package assembly needed to run the approved Agents manually. The human MAY initialize and update Task State according to the Schema, maintain the one-slot-per-canonical-type inventory, select active references only when supported by governing artifacts, record workflow position and resume facts, and coordinate handoff to the approved next Agent or human source authority.

The human state manager is not a sixth Agent and gains no planning, test-design, implementation, Review, evidence, or artifact-content authority through Task State or package assembly. The human MUST NOT edit a frozen artifact owned by another role or silently resolve a conflict. When required authority is unavailable, the human records a durable blocker and preserves affected artifacts as inactive or invalid for affected uses.

After A05 is available, DP-Task-Orchestrator owns operational Task State maintenance and active package assembly. That ownership remains operational. It grants no authority over another Agent's artifact content. The Orchestrator does not plan, write or change HLTP semantics, implement or fix code, perform substantive Review, change Design, or commit without explicit instruction.

Adoption of A05 preserves the same status, identity, evidence, authority, and resume semantics that applied during manual operation.

## 17. Validation Levels

### 17.1 Foundation validation

Foundation validation verifies that this contract defines the Resume Invariant, remains compatible with the other four Foundation contracts, reconstructs resume without conversation history, and remains declarative rather than implementing a recovery engine.

### 17.2 Manual Pipeline validation

During the Manual Pipeline, validation performs at least one explicit session-restart test. The restarted session continues from artifacts and Task State without conversation history, validates evidence against the correct baseline or implementation identity, and uses human operational ownership without creating a sixth Agent.

### 17.3 Orchestrator Package validation

After A05 is available, validation tests full resume behavior, failure and resume paths, state transitions while preserving the Status Model as semantic authority, continuation after a new session, and the prohibition on the Orchestrator performing another Agent's work.

### 17.4 Hardening validation

Hardening tests behavior after context compaction, recovery after BLOCKED, partial implementation recovery, Task-split recovery and lineage, evidence validity after implementation identity changes, artifact freeze and authority boundaries, status transitions, and resume targets.

## 18. Failure and Recovery Rules

A resume decision has a deterministic result:

- **Valid continuation**: all durable inputs required for the intended continuation resolve, identities are coherent, the active status permits resume, applicable evidence and Review remain valid, blockers or gates are resolved, and the next action and owner are authorized.
- **Blocked repair**: required durable data, authority, identity, artifact, evidence, Review input, blocker resolution, or approval is missing, ambiguous, conflicting, or invalid.

Blocked repair never guesses an identity, invents a correction, promotes historical material to active authority, or substitutes conversation memory. The blocker is recorded durably and routed to the earliest affected owner or source authority.

After repair, selective invalidation and required revalidation are performed before continuation. A fresh session with no conversation history must reach the same authorized result from the same durable package.

## 19. Explicit Non-Goals

This contract does not:

- create a Resume Agent or sixth Agent;
- create unnecessary storage infrastructure, an evidence database, centralized store, index, or query API;
- create a recovery service, scheduler, dispatcher, event loop, or workflow engine;
- implement the DP-Task-Orchestrator state machine;
- add a canonical artifact or inventory slot;
- create a Skill, ACL, permission system, lock system, mutation-control system, or general revision framework;
- redefine Schema fields, shapes, identities, or cardinalities;
- redefine artifact ownership, freeze, authority, evidence validity, proof strength, status semantics, or transitions;
- perform or define commit execution;
- establish readiness for a future Task;
- copy complete artifacts into Task State as a substitute for references;
- turn Task State into evidence, approval, or completion authority;
- execute the authorized next action as part of resume validation.

## 20. Final Invariants

1. Correct continuation is reconstructible from durable artifacts and state without conversation history, hidden state, or remembered reasoning.
2. Resume binds to exact Task, artifact, repository, implementation, evidence, and Review identities as applicable.
3. Missing, mixed, conflicting, or insufficient durable inputs produce blocked repair, not guessed continuation.
4. Status semantics, authority, ownership, evidence strength, and artifact structure remain governed by their Foundation contracts.
5. Invalidation is selective, frozen history remains immutable, and material replacements receive distinguishable identities.
6. Resume identifies the authorized next action and owner but does not perform another role's governed work.
7. Evidence and Review remain bound to the exact targets and inputs they evaluated.
8. Human and Orchestrator state management remain operational and do not create professional authority.
9. Required revalidation completes before continuation.
10. Tests and governing rules are not weakened to obtain PASS.
