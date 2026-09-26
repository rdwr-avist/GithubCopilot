---
name: task-intake
description: Convert exactly one approved Implementation Plan Task into a source-bound Task Contract, evaluate Task-level requirement sufficiency and commit-scale coherence, and emit exactly one canonical intake status. Use only for Task intake before repository analysis, Domain Skill routing, Task-local test design, or implementation planning.
---

# Task Intake

## Purpose

Convert exactly one identifiable Task from an approved Implementation Plan into one durable Task Contract. Evaluate requirement sufficiency only at Task level and emit exactly one canonical intake status.

This Skill is the first `DP-Task-Planner` Skill. It forms the contract that later Planner, Test Designer, Implementer, Reviewer, and Orchestrator work may consume. It does not perform any of that later work.

## Governing Authorities

Apply these repository authorities in their approved order and bounded scope:

1. `task-agent-work-plan-v1-final.md` is the primary authority.
2. `.github/task-agent-system/foundation/task-package-schema.md` governs the Task Contract schema and canonical references.
3. `.github/task-agent-system/foundation/task-authority-model.md` governs authority, provenance, freeze, replacement, and conflicts.
4. `.github/task-agent-system/foundation/minimum-evidence-model.md` governs evidence validity, scope, target binding, freshness, and proof limits.
5. `.github/task-agent-system/foundation/task-status-model.md` governs status vocabulary, specificity, precedence, blocking, and resume semantics.
6. `.github/task-agent-system/foundation/resume-invariant.md` governs durable reconstruction, identity, selective invalidation, and replacement.
7. `.github/task-agent-system/foundation/domain-skill-inventory-schema.md` and `.github/task-agent-system/foundation/domain-skill-routing-contract.md` define downstream Domain Skill boundaries. They do not authorize this Skill to inspect inventory or route.

Use the exact active revisions supplied for the Task. Conversation history, hidden state, remembered drafts, confidence, filename similarity, record order, and unstored reasoning are not authority.

## Required Input

Require all of the following before contract construction:

- exactly one identifiable Task selected from an approved Implementation Plan;
- the approved Task sources that establish its meaning;
- stable Task identity;
- applicable approved Design decisions, when they govern the Task;
- dependency and prerequisite references, when applicable;
- relevant existing Component or Interface HLTP/readiness references, when applicable;
- stable, typed, relationship-qualified source references, with immutable revision or durable locator whenever identity alone is insufficient or revision affects meaning.

If zero Tasks or multiple Tasks are presented without one selected Task, stop before Task Contract construction. Request exactly one identifiable Task and report `BLOCKED`; do not merge Tasks and do not use `TASK_SPLIT_REQUIRED` for the absence of a single selected Task.

If the Task is identifiable and enough approved information exists to identify a missing or conflicting obligation, produce a structurally valid non-ready Task Contract instead of abandoning intake.

## Scope Boundary

Perform only Task-level intake and contract formation.

You may:

- identify the Task objective;
- extract sourced Task requirements and required behavior;
- identify explicit in-scope boundaries;
- preserve non-goals and future work;
- identify dependencies and prerequisites;
- preserve applicable approved Design decisions;
- extract and map acceptance criteria;
- preserve relevant existing HLTP or readiness references;
- detect missing meaning, ambiguity, contradiction, Design gaps, blockers, and dependency-readiness issues;
- evaluate whether the Task is one bounded, buildable, testable, acceptance-criterion-driven, dependency-aware, commit-scale unit;
- identify the authority, pending action, and resume target needed to clear a blocker.

You must not:

- perform Feature-level requirement analysis;
- perform Component-level requirement analysis or invoke `component-requirement-analysis` as a substitute;
- invent, strengthen, weaken, reinterpret, or complete requirements or acceptance criteria;
- modify Design, select architecture, choose among unresolved Design alternatives, or silently close a Design gap;
- explore the repository, inspect code, verify APIs, discover owners or consumers, or use repository observations as contract authority;
- inspect a Domain Skill inventory, discover candidates, select or load a Domain Skill, choose an authoritative lookup, or produce a Domain Skill Routing Result;
- design Task-local tests, expand HLTP mechanics, or implement tests;
- plan implementation, sequence actions, identify files or change areas, define verification steps, or produce a Task Execution Plan;
- create child Tasks, assign new Task IDs, alter the Implementation Plan, implement, review, update Task State, claim completion, or create a commit;
- fabricate Build/Test Evidence or treat traceability, provenance, confidence, or state as semantic proof.

## Intake Procedure

Execute the following steps in order.

### 1. Bind the Authority Set

1. Resolve the selected Task and every material approved source.
2. Record stable identity, type, relationship, locator, and revision as required by the Foundation schema.
3. Reject non-authoritative chat summaries, memory, unstored reasoning, and unresolved mutable filename-only pointers.
4. If a mutable or filename-only source pointer is material to approved Task meaning but cannot be bound to a durable locator or revision, emit `TASK_REQUIREMENTS_INCOMPLETE`, preserve that unresolved pointer as a durable blocker, identify the Task authority responsible for supplying the approved source meaning, and do not guess.
5. If a material source identity or revision required to establish the active authority or reconstruct a resumable artifact identity is missing, stale, ambiguous, or unresolvable, emit `BLOCKED`; preserve the unresolved reference as the durable blocking basis; identify the authority responsible for supplying the identity or revision; and do not guess, execute the next action, or perform another role's work. This identity rule takes precedence over `TASK_REQUIREMENTS_INCOMPLETE`.
6. `TASK_REQUIREMENTS_INCOMPLETE` applies to missing approved Task meaning only after the active source identity and every material revision required to determine active authority have been established.
7. If two applicable authorities conflict, apply the approved authority order only when it resolves the conflict. Otherwise preserve both sides, select neither, identify the resolution authority, and stop affected construction after producing the non-ready contract when possible.
8. For an unresolved same-scope conflict between equally applicable approved authorities, when no specific canonical status truthfully classifies the conflict, emit fallback `BLOCKED`, record why no specific status applies, preserve both source references, and identify the authority entitled to resolve it.

### 2. Establish the Single Task Boundary

1. Confirm that all extracted material belongs to the selected Task.
2. Exclude unrelated Feature, Component, neighboring-Task, and future-Task responsibilities.
3. Preserve excluded material as a non-goal only when an approved source establishes that exclusion.
4. Do not absorb future work to make the current Task appear complete.

### 3. Extract Task Meaning

Create source-bound content for:

- one clear `objective`;
- `task_type`, preserving the approved bounded classification without inventing a closed taxonomy;
- one or more independently traceable requirements with unique IDs;
- explicit `scope_boundaries.in_scope` entries;
- durable `scope_boundaries.boundary_refs`;
- explicit `non_goals`;
- `dependency_refs` and prerequisites;
- `approved_decision_refs` for applicable fixed Design decisions;
- one or more acceptance criteria with unique IDs, statements, approved source references, and mappings to one or more requirements;
- existing `readiness_refs`, including relevant HLTP references without test design;
- `blocking_refs` for every active blocker.

Normalize duplicate approved wording only when meaning is unchanged and all distinct source relationships remain durable. Never merge materially different obligations into one unverifiable paragraph.

### 4. Evaluate Sufficiency

Evaluate each item independently:

- Task identity is stable and singular.
- Objective is clear and approved.
- Scope is explicit and bounded.
- Required behavior is represented by sourced requirements.
- Every material conclusion has a durable source reference.
- Every acceptance criterion is observable enough for the approved Task meaning, has a unique identity, and maps to at least one requirement.
- Every requirement needed for readiness has approved acceptance coverage.
- Applicable Design meaning is fixed rather than missing or unresolved.
- Dependencies are identified separately from their readiness.
- Relevant existing HLTP/readiness references are preserved without designing tests.
- The Task is buildable and testable in principle without repository exploration.
- The Task forms one coherent commit-scale unit for one independent session.
- No active contradiction, ambiguity, or blocker is hidden.

A traceability mapping proves linkage only. It does not prove semantic sufficiency or satisfaction. Bare statements such as "requirements complete" or "Task ready" are not evidence.

### 5. Evaluate Commit Coherence

Treat the Task as coherent only when it has:

- one bounded outcome;
- explicit scope and exclusions;
- identified dependencies;
- source-backed requirements;
- mapped acceptance criteria;
- testability and buildability in principle;
- no unresolved Design decision;
- no independent responsibility that should be a separate Task.

If it cannot remain one coherent commit-scale unit, do not split it. Preserve the evidence, emit `TASK_SPLIT_REQUIRED`, identify the responsible authority, and record the required plan-update action and resume target.

### 6. Select Exactly One Status

Evaluate specific conditions before fallback `BLOCKED`. Emit exactly one of these intake statuses:

1. `TASK_SPLIT_REQUIRED` when the selected Task cannot remain one bounded, coherent commit-scale unit.
2. `DESIGN_CLARIFICATION_REQUIRED` when approved Design meaning required by the Task is missing, ambiguous, or unresolved.
3. `DEPENDENCY_NOT_READY` when a required dependency is identified and its readiness is the unresolved condition.
4. `TASK_REQUIREMENTS_INCOMPLETE` when objective, scope, requirements, acceptance criteria, fixed decisions, or material approved source meaning are insufficient to form a complete Task Contract.
5. `BLOCKED` only for a durable inability to continue when no more specific canonical status applies. Record why no specific status applies.
6. `READY` only when every intake and Task Contract formation obligation is complete and the handoff basis is valid.

When more than one condition is present, apply the precedence and allowed-next-state semantics from `task-status-model.md`. Do not create a status token. Do not emit Domain routing statuses as local intake outcomes.

`status` states the semantic condition. `blocking_refs` identify the durable blocking basis. Do not use either as a substitute for the other.

### 7. Build the Task Contract

Produce exactly one Task Contract conforming to `.github/task-agent-system/foundation/task-package-schema.md`.

#### Common Header

Include:

- `schema_ref`;
- `artifact_type` with value `Task Contract`;
- stable `artifact_id`, unique within `task_id` and artifact type;
- `task_id`;
- `producing_role_or_source` identifying `DP-Task-Planner` or the authorized source;
- `predecessor_artifact_ref` when lineage is required;
- `source_refs` with cardinality `1..*`;
- `created_at`;
- `updated_at`.

#### Artifact-Specific Fields

Include exactly these Task Contract fields:

- `task_type`;
- `objective`;
- `scope_boundaries`;
- `non_goals`;
- `dependency_refs`;
- `approved_decision_refs`;
- `requirements`;
- `acceptance_criteria`;
- `readiness_refs`;
- `blocking_refs`.

Apply these constraints:

- `scope_boundaries.in_scope` has cardinality `1..*`.
- `scope_boundaries.boundary_refs` has cardinality `1..*`.
- `requirements` has cardinality `1..*`.
- Each requirement contains unique `requirement_id`, `statement`, and `source_ref`.
- `acceptance_criteria` has cardinality `1..*`.
- Each criterion contains unique `acceptance_criterion_id`, `requirement_refs` with cardinality `1..*`, `statement`, and `source_ref`.
- `non_goals`, `dependency_refs`, `approved_decision_refs`, `readiness_refs`, and `blocking_refs` are present even when empty.
- Every non-ready outcome with an identifiable blocking basis has at least one durable `blocking_ref`.
- Long authorities and raw logs are referenced, not copied, unless a focused excerpt is structurally necessary.
- Do not include repository observations as authority, a Domain routing outcome, an implementation plan, copied authority bodies, or foreign artifact content.

Return the Task Contract plus the one selected status. Do not continue into another artifact or role.

## Evidence Rules

For every requirement, acceptance criterion, in-scope boundary, approved decision, and material intake conclusion:

1. Bind the claim to a specific durable source reference.
2. Preserve source identity, relationship, locator, revision, and exact scope when required.
3. Distinguish authority from evidence and evidence from semantic proof.
4. Record the exact blocker basis, resolution authority, pending action, and resume target for every non-ready outcome.
5. Never claim execution evidence unless the represented action actually occurred.
6. Treat changed artifact bytes or changed material authorities as requiring new applicable evidence and revalidation.

## Ambiguity and Conflict Rules

- Never guess missing Task meaning.
- Never resolve a same-scope authority conflict by convenience, confidence, file order, conversation order, or general knowledge.
- Preserve unresolved alternatives and exact source references.
- Identify the authority entitled to resolve the issue.
- Select the most specific truthful non-ready status.
- Use fallback `BLOCKED` only when the canonical specific statuses do not truthfully classify the durable blocker.
- Do not inspect the repository, invoke Domain Skills, or plan implementation to repair missing authority.

## Resume, Freeze, and Replacement

A Task Contract must support continuation from durable artifacts without conversation history.

For a non-ready result, preserve enough information to reconstruct:

- exact Task identity;
- active Task Contract identity;
- source identities and revisions;
- current status and durable blocker basis;
- responsible resolution authority;
- pending decision or action;
- resume target.

Mixed, missing, ambiguous, stale, or unresolvable identities block continuation rather than being guessed.

After freeze:

- keep the frozen artifact immutable as historical material;
- invalidate only material affected by a verified change;
- keep unaffected valid material active;
- create a distinguishable replacement for a material correction;
- use `predecessor_artifact_ref` when lineage is needed to prevent revision mixing or support resume;
- never edit a frozen Task Contract in place.

This Skill may identify the next authorized owner and action. It must not execute them.

## Termination

Terminate after producing:

1. one Task Contract, when Task identity and sufficient source basis exist to construct it; and
2. exactly one canonical intake status.

For non-ready outcomes, also include the required durable blocker and resume metadata. `READY` permits the next applicable Planner work but does not start it.

Never continue into Domain Skill routing, Task Repository Context, Task-local HLTP creation, Task Execution Plan, implementation, review, Task State management, completion, or commit.
