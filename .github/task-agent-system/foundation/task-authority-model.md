# Task Authority Model

**Contract:** `task-authority-model`  
**Canonical path:** `.github/task-agent-system/foundation/task-authority-model.md`  
**Model version:** 1.0

## 1. Purpose and authority order

This Foundation contract defines ownership, read-only consumption, authority scope, freeze, validity, invalidation, and post-freeze escalation for the ten canonical artifacts in the Task Work Package.

The approved `task-agent-work-plan-v1-final.md` is the primary authority. The approved `task-package-schema.md` is a binding Foundation authority for artifact structure, identities, applicability, availability, provenance, consumers, relationships, and resume-relevant fields. This model completes those contracts only within its authority scope and does not redefine their content.

Approved Design, Component Implementation Plan, Component or Interface HLTP, repository evidence, Domain Skills, and human decisions remain authoritative only within their respective scopes. Conversation history, hidden state, and unapproved drafts are not sources of truth.

This model is not a permission system, lock, database, scheduler, workflow engine, status-transition model, evidence-validation algorithm, recovery procedure, or general revision framework.

## 2. Normative concepts

- **Ownership:** responsibility for producing an artifact instance and maintaining its content before freeze, subject to its governing sources. Ownership does not grant authority to alter source truth or another artifact.
- **Authority:** the bounded set of assertions an artifact is qualified to establish for its valid read-only consumers.
- **Read-only consumption:** permission to locate, read, reference, and rely on an artifact within its valid authority scope. A read-only consumer may not alter its content, meaning, identity, or provenance.
- **Immutability:** a frozen artifact instance is not modified in place. Immutability preserves the instance; it does not guarantee continuing validity.
- **Freeze:** the lifecycle point after which content or meaning may not be silently changed. A material correction after freeze requires a distinguishable replacement instance.
- **Validity:** the condition in which an artifact may be relied upon within its authority scope because its governing sources, applicability, identities, dependencies, and required associations still hold.
- **Invalidation:** loss of eligibility to act as active authority in an affected scope. Invalidation does not mutate or delete the historical instance.

These concepts are independent. An owner may lack authority over a source assertion. A consumer may rely on an artifact without owning it. A frozen artifact may later become invalid. An invalid artifact remains immutable historical material but must not be represented as active authority.

## 3. Common authority rules

1. The common header field `producing_role_or_source` records provenance. It does not independently grant ownership or broader authority.
2. No artifact proves, approves, or completes itself. In particular, an Implementation Report is not proof; Build/Test Evidence alone is not completion; Acceptance Criteria Evidence alone is not Review PASS; Task State is not evidence.
3. A derived artifact cannot override an approved source within that source's scope. Repository-grounded assertions cannot be overridden by a plan, report, or state assertion.
4. Adding an artifact to Task State or the Task Work Package inventory does not grant the state or assembly manager authority to modify it.
5. Freeze applies to an artifact instance, not indefinitely to its subject matter. A material change to authority scope, obligations, identity binding, consumer reliance, or downstream decisions requires a distinguishable replacement `artifact_id`; `predecessor_artifact_ref` is used when needed to prevent mixing or preserve resume lineage.
6. A changed implementation receives a distinguishable `implementation_id`. A repeated execution receives a distinguishable `evidence_id`. A new review attempt receives a distinguishable Task Review Result. Existing evidence and reviews remain bound to the identities they evaluated.
7. Invalidation is selective. It follows the affected authority scope and identity dependency; a change does not invalidate unrelated artifacts automatically.
8. Invalidated or superseded instances remain available for lineage, audit, evidence interpretation, and resume, but are not active authority.
9. A conflict with a governing source or authoritative artifact is not silently resolved. The affected assertion is not relied upon until the proper authority resolves it, replaces the artifact where necessary, or records a durable blocker.
10. This contract does not create a general cosmetic-edit process. Any non-semantic correction must preserve meaning, identity integrity, and auditability and remains subject to stricter governing contracts.

## 4. Canonical artifact authority matrix

The consumer lists below match the Task Package Schema. Consumers are read-only unless they are also the owner of the particular instance before freeze.

### 4.1 Task Contract

- **Owner:** DP-Task-Planner.
- **Read-only consumers:** DP-Task-Planner, DP-Task-Test-Designer, DP-Task-Implementer, DP-Task-Reviewer, and DP-Task-Orchestrator.
- **Authority scope:** the bounded Task objective, scope, non-goals, dependencies, fixed applicable decisions, requirements, acceptance criteria, readiness references, and documented blockers derived from approved task sources.
- **Non-authority boundary:** it does not change Design, choose implementation, contain the implementation plan, or present repository observations as contract authority.
- **Freeze point:** planning handoff of the formed contract, including a durable non-ready contract when the blocking basis is recorded.
- **Validity conditions:** task and source identities resolve; applicable source revisions and human decisions remain current; scope, requirements, criteria, dependencies, and blocking references remain mutually coherent.
- **Invalidation triggers:** an approved material change to task objective, scope, requirement, acceptance criterion, dependency basis, fixed decision, or source revision.
- **Escalation after freeze:** return the issue to DP-Task-Planner. If the change requires source or Design authority, the Planner records and seeks the human authoritative decision. Consumers do not edit the frozen contract.

### 4.2 Domain Skill Routing Result

- **Owner:** the approved producing role recorded for the instance: DP-Task-Planner for planning-time routing or DP-Task-Orchestrator for orchestration-time routing.
- **Read-only consumers:** DP-Task-Planner, DP-Task-Implementer, DP-Task-Reviewer, and DP-Task-Orchestrator, limited for each entry to its declared `consumer_refs` and lifecycle applicability.
- **Authority scope:** the routing outcome for each identified concern, including the selected Domain Skill or authoritative lookup, applicability, consumer mapping, constraint references, and blocker when routing cannot be resolved.
- **Non-authority boundary:** it does not define requirements, architecture, implementation, or review judgment and does not copy Domain Skill content.
- **Freeze point:** publication of the routing result for its named consumers.
- **Validity conditions:** the Task Contract concern, producing-role provenance, consumer mapping, routing inventory, selected Skill or authority revision, applicability, and constraints remain applicable.
- **Invalidation triggers:** a material change to the concern or consumers; routing inventory or selected authority becoming unavailable or superseded; a relevant source revision; or a discovered routing conflict.
- **Escalation after freeze:** return to the applicable producing routing role for rerouting. If an authoritative source decision is required, record a blocker and seek human authority. A changed result is a distinguishable replacement.

### 4.3 Task Repository Context

- **Owner:** DP-Task-Planner.
- **Read-only consumers:** DP-Task-Planner, DP-Task-Implementer, DP-Task-Reviewer, and DP-Task-Orchestrator.
- **Authority scope:** focused observations actually verified within the recorded exploration scope against `repository_id` and the applicable `baseline_id` or `worktree_id`, including observation evidence and known limits.
- **Non-authority boundary:** it does not define requirements or Design and does not claim repository truth beyond the inspected identity and scope.
- **Freeze point:** repository-analysis handoff for planning or downstream use.
- **Validity conditions:** repository and baseline/worktree identities match the relied-upon observations; evidence and routing provenance resolve; observation limits remain adequate for the use being made.
- **Invalidation triggers:** a relevant repository-state or identity change, contradictory repository evidence, broken evidence or routing linkage, or an observation limit becoming material.
- **Escalation after freeze:** return to DP-Task-Planner for refreshed analysis and, when meaning changes, a replacement context instance. Downstream roles report conflicts and do not rewrite observations.

### 4.4 Task Execution Plan

- **Owner:** DP-Task-Planner.
- **Read-only consumers:** DP-Task-Test-Designer, DP-Task-Implementer, DP-Task-Reviewer, and DP-Task-Orchestrator.
- **Authority scope:** intended execution, ordered actions, requirement mappings, dependencies, risks, required change areas, expected affected areas, protected or excluded areas, diff boundary, and verification obligations.
- **Non-authority boundary:** it is not repository truth, proof of execution, a Design change, or permission to modify protected areas.
- **Freeze point:** planning handoff of an implementable plan.
- **Validity conditions:** its Task Contract, applicable routing entries, Repository Context, dependencies, and protected-area assumptions remain valid and sufficient for execution.
- **Invalidation triggers:** a material change to the contract, repository context, routing constraint, dependency, protected-area requirement, or repository reality that changes executability, ordering, boundaries, or verification.
- **Escalation after freeze:** DP-Task-Implementer or another consumer reports the conflict. DP-Task-Planner decides whether a replacement plan is required; source-level conflicts are escalated to human authority.

### 4.5 Task-local HLTP

- **Owner:** DP-Task-Test-Designer.
- **Read-only consumers:** DP-Task-Implementer, DP-Task-Reviewer, and DP-Task-Orchestrator.
- **Authority scope:** Task-local semantic test obligations, expected behavior, preconditions, stimulus, negative, boundary and lifecycle obligations, requirement and criterion mappings, approved deferred coverage, and completion criteria.
- **Non-authority boundary:** it does not implement tests, select non-contractual fixture mechanics, change production Design, or adapt obligations to a chosen implementation.
- **Freeze point:** test-design handoff after READY, or durable recording of a blocked test-design outcome where permitted.
- **Validity conditions:** the bound Task Contract, Execution Plan, approved source HLTP revisions, mappings, and deferred-coverage authorities remain applicable and coherent.
- **Invalidation triggers:** a material requirement, criterion, plan obligation, source HLTP, expected-behavior, or approved deferred-coverage change; or a discovered semantic contradiction.
- **Escalation after freeze:** semantic changes return to DP-Task-Test-Designer and, where required, the source or human Design authority. DP-Task-Implementer may choose mechanics but must not weaken or reinterpret obligations.

### 4.6 Implementation Report

- **Owner:** DP-Task-Implementer.
- **Read-only consumers:** DP-Task-Reviewer and DP-Task-Orchestrator.
- **Authority scope:** the Implementer's factual declaration of what was attempted, implemented, changed, deferred, blocked, deviated, discovered, left unresolved, and handed off for the exact `implementation_id`.
- **Non-authority boundary:** it is not independent proof, acceptance, Review PASS, or authority to alter the Task Contract, Execution Plan, routing result, or HLTP.
- **Freeze point:** durable implementation-attempt handoff to review or orchestration, including a partial or blocked attempt.
- **Validity conditions:** the exact implementation, baseline, worktree when one exists, plan, applicable HLTP, mappings, evidence references, deviations, unresolved items, and handoff inputs remain correctly associated.
- **Invalidation triggers:** use against another implementation; a material mismatch between report and repository state; broken input or evidence linkage; or discovery that a reported assertion is materially incorrect.
- **Escalation after freeze:** DP-Task-Implementer produces a distinguishable corrected or replacement report. The historical report is not silently rewritten, and the correction does not alter upstream authority.

### 4.7 Acceptance Criteria Evidence

- **Owner:** DP-Task-Implementer for normal implementation evidence assembly, while provenance remains the producer recorded for the instance.
- **Read-only consumers:** DP-Task-Reviewer and DP-Task-Orchestrator.
- **Authority scope:** complete structural mapping of each applicable acceptance criterion to evidence references or a durable unresolved, not-yet-evidenced, or blocking reference for the bound implementation.
- **Non-authority boundary:** it does not independently validate evidence, prove satisfaction, determine Review PASS, or declare task completion.
- **Freeze point:** evidence-package handoff for review or orchestration.
- **Validity conditions:** Task Contract criterion identities, the applicable `implementation_id`, coverage entries, evidence identities and run associations, result summaries, and unresolved references remain exact and valid.
- **Invalidation triggers:** criterion revision, implementation change, incomplete or broken criterion mapping, or invalidation or identity mismatch of linked evidence.
- **Escalation after freeze:** resolve the authoritative criterion or evidence issue, then DP-Task-Implementer assembles a distinguishable replacement. Reviewers do not edit the mapping to obtain PASS.

### 4.8 Build/Test Evidence

- **Owner:** the approved role that executed and recorded the action, normally DP-Task-Implementer and, for approved review-side execution, DP-Task-Reviewer.
- **Read-only consumers:** DP-Task-Implementer, DP-Task-Reviewer, and DP-Task-Orchestrator. Acceptance Criteria Evidence may reference applicable evidence instances.
- **Authority scope:** the factual record of one build, test, analysis, or verification execution, including action, immutable target identity, result, context, mappings, and time.
- **Non-authority boundary:** it does not independently determine criterion satisfaction, Review PASS, or task completion.
- **Freeze point:** creation of the evidence record after the execution occurred.
- **Validity conditions:** `evidence_id`, immutable target, repository/baseline or implementation identity, command or action, result, execution context, output references, mappings, and timestamp consistently identify the actual run.
- **Invalidation triggers:** target or implementation mismatch, invalid run association, corrupted or misreported result, broken output reference, material context mismatch, or invalidated source/result used by the evidence.
- **Escalation after freeze:** rerun the action and create a new evidence instance with a new `evidence_id`. The prior execution record remains unchanged and bound to its original target.

### 4.9 Task Review Result

- **Owner:** DP-Task-Reviewer.
- **Read-only consumers:** DP-Task-Implementer and DP-Task-Orchestrator.
- **Authority scope:** PASS, FAIL, or BLOCKED review status for the exact review-input set and, for substantive review, the exact `reviewed_implementation_id`, including ordered checks, the first non-passing check, finding, correction, reverification, evidence, and blocker as applicable.
- **Non-authority boundary:** it does not change implementation, evidence, planning artifacts, implementation-owned reports, review-suite definitions, or source authority.
- **Freeze point:** publication of the result for one review attempt.
- **Validity conditions:** reviewed implementation, reviewed artifact instances, evidence set, review-input integrity references, requirement and criterion mappings, and ordered-check record remain exact and valid.
- **Invalidation triggers:** a new or changed implementation, any changed reviewed input, evidence mismatch, broken integrity reference, or discovery that attributed input integrity did not hold.
- **Escalation after freeze:** DP-Task-Implementer corrects a FAIL without changing the result; missing authority or input remains blocked; DP-Task-Reviewer performs a new review and creates a new result. A prior result is never edited to pass.

### 4.10 Task State

- **Owner:** DP-Task-Orchestrator when available. During the Manual Pipeline, the human state manager performs this operational ownership only.
- **Read-only consumers:** DP-Task-Orchestrator and the approved stage or Agent selected for continuation; all five approved Agents may read it to locate active artifacts.
- **Authority scope:** current workflow position or status reference, active artifact graph, package inventory reference, repository/worktree and current implementation identities, active evidence and review, current blocker, pending action, responsible stage, durable update, and resume target.
- **Non-authority boundary:** it is not evidence, does not copy or change another artifact's semantics, does not perform another Agent's work, does not define transitions, and cannot make unevidenced completion authoritative.
- **Freeze point:** a durable Task State record is frozen when superseded by the next durable state record or when the Task reaches a terminal state defined externally by the Task Status Model. Before freeze, only the authorized state manager may update it according to the schema. When a later state must remain separately identifiable to prevent mixing or preserve resume correctness, it uses a distinguishable instance and predecessor lineage where needed. This does not define a general revision framework.
- **Validity conditions:** active references resolve to the intended task and artifact identities; repository, implementation, evidence, review, blocker, responsible-stage, and resume references are coherent; the recorded position is supported by authoritative artifacts and external status/resume contracts.
- **Invalidation triggers:** broken or mixed active references, unsupported workflow position, repository or implementation identity mismatch, an active reference becoming invalid without a corresponding state update, or inconsistency with the Task Status Model or Resume Invariant.
- **Escalation after freeze:** the authorized state manager records a distinguishable next Task State instance with corrected active references, blocker, pending action, or resume target. State management never rewrites another artifact or assumes its authority.

## 5. Task Work Package assembly authority

The Task Work Package is an assembly and resume boundary, not an eleventh canonical artifact. DP-Task-Orchestrator maintains its active assembly when available. During the Manual Pipeline, the human state manager maintains the assembly view.

Assembly authority is limited to recording package update time, inventory applicability and availability, active references, repository and implementation associations, evidence and review references, Task State reference, and blockers according to the schema. Assembly maintenance does not grant authority over artifact content, applicability sources, evidence truth, review judgment, or professional decisions owned by another Agent.

An available artifact remains the artifact owner's frozen instance. Selecting it as active does not mutate it. If the assembly detects an unresolved identity, validity, applicability, or authority conflict, it records a durable blocker and does not present the conflicting instance as valid active authority.

## 6. Manual Pipeline before DP-Task-Orchestrator

Before DP-Task-Orchestrator exists, a human performs only the operational state-management and package-assembly duties needed to run the approved Agents manually.

The human state manager may:

- initialize and update Task State according to the schema;
- maintain the one-slot-per-canonical-type package inventory;
- select active artifact, repository, implementation, evidence, and review references when those selections are supported by their governing artifacts;
- record the workflow position, responsible stage, blocker, pending action, last durable update, and resume target;
- coordinate handoff to the approved next Agent or human source authority.

The human state manager must not:

- become a sixth Agent;
- create planning, test-design, implementation, review, or evidence authority through Task State;
- edit a frozen artifact owned by another role;
- silently resolve a source, architecture, routing, requirement, repository, evidence, or review conflict;
- mark an artifact valid or a task complete without the required authoritative basis.

When a required authority is unavailable, the human records a durable blocker and preserves the affected artifact as inactive or invalid for the affected use. Manually maintained Task State and package references must remain sufficient for DP-Task-Orchestrator to resume later from durable artifacts and repository state without conversation history.

## 7. Post-freeze escalation summary

- Contract or planning meaning: DP-Task-Planner; human source authority when the approved source must change.
- Routing outcome: the producing DP-Task-Planner or DP-Task-Orchestrator; human authority when routing requires a source decision.
- Test semantics: DP-Task-Test-Designer; source or human Design authority when required.
- Implementation conflict or report correction: DP-Task-Implementer reports and produces a new implementation or report identity as applicable.
- Evidence correction: the executing role reruns and records a new evidence instance.
- Review outcome: DP-Task-Reviewer performs a new review; no one edits the prior result.
- State or assembly inconsistency: DP-Task-Orchestrator, or the human state manager during the Manual Pipeline, records the next valid state or a durable blocker without altering referenced artifacts.

If the required authority cannot resolve the issue, the conflict remains durably blocked. No artifact affected by that unresolved conflict is represented as valid active authority for the affected scope.
