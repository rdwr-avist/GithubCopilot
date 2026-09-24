# Task Status Model

**Canonical path:** `.github/task-agent-system/foundation/task-status-model.md`  
**Artifact class:** Foundation contract  
**Working-copy status:** Complete candidate under ordered Test Pack review  

## 1. Purpose

This Foundation contract defines the shared semantic meaning of Task statuses used by the approved five-Agent Task-Agent System. It defines who may emit each status, the effect on the current Task pipeline, who is responsible for resolving the represented condition, whether and where work may resume, when human intervention is required, which semantic outcomes may follow, which active artifacts lose validity, and which durable artifacts are required for resumption.

This contract complements the Task Package Schema, Task Authority Model, and Minimum Evidence Model. It does not replace their structural, authority, identity, freeze, validity, evidence, or resume responsibilities.

## 2. Authority Order

The approved `task-agent-work-plan-v1-final.md` is the primary authority for the five-Agent architecture, Agent responsibilities, pipeline boundaries, approved status vocabulary, review discipline, completion rules, and Foundation scope.

The approved `task-package-schema.md` is binding for canonical artifacts, identities, references, applicability, availability, provenance, cardinality, package assembly, Task State fields, lineage, and structural resume requirements.

The approved `task-authority-model.md` is binding for artifact ownership, bounded authority, read-only consumption, freeze, immutability, validity, selective invalidation, replacement, Manual Pipeline state management, and post-freeze escalation.

The approved `minimum-evidence-model.md` is binding for evidence identity, execution truth, context, mappings, proof strength, freshness, staleness, rerun, Reviewer evidence, and the distinction between success, failure, not-yet-run work, and blocked-before-execution work.

The F04 assignment is binding for the target artifact, required status coverage, the nine required dimensions per status, specific-status precedence over `BLOCKED`, and the prohibition on implementing the Orchestrator state machine here.

Approved Design, Component Implementation Plan, Component or Interface HLTP, repository evidence, Domain Skills, and durable human decisions remain authoritative only within their defined scopes. Conversation history, hidden state, remembered intermediate drafts, and unapproved drafts are not sources of truth.

When authorities are complementary, every applicable rule MUST be satisfied. A lower or derived authority MUST NOT override a higher authority within the higher authority's scope. A real conflict that cannot be resolved by the defined authority order MUST remain durably blocked and MUST NOT be resolved silently.

Architecture, authority, requirement, and domain-routing conflicts are distinct conflict classes. An unresolved conflict in any of these classes MUST remain durably blocked, MUST retain a `BlockingRef` and resolution owner, and MUST NOT be resolved silently by an Agent, consumer, DP-Task-Orchestrator, package assembler, or Manual Pipeline human state manager. Resolution requires the authority that owns the disputed architecture, authority boundary, requirement meaning, or routing decision.

Approved baselines attached to the current F04 work context are binding within their declared scopes. Previous-conversation content, hidden context, remembered drafts, and undocumented reasoning are not authority. Approved decisions MUST NOT be reopened during status-model authoring unless a higher applicable authority explicitly supersedes them. The target MUST NOT redesign the approved five-Agent architecture or create future artifacts while authoring this Foundation contract.

## 3. Normative Conventions

The key words **MUST**, **MUST NOT**, **REQUIRED**, **SHOULD**, **SHOULD NOT**, and **MAY** are normative.

- **MUST**, **MUST NOT**, and **REQUIRED** define mandatory contract rules.
- **SHOULD** and **SHOULD NOT** define strong expectations. A deviation requires a documented, reviewable reason and MUST NOT violate a mandatory rule.
- **MAY** defines an allowed choice, not a requirement.
- A rule qualified by a condition is mandatory only when that condition is true.
- Examples and explanatory text do not override normative rules.
- The singular includes the plural when the contract permits more than one referenced item. Explicit Schema cardinality remains controlling.

## 4. Scope

This contract governs:

- the canonical status vocabulary for a Task;
- the semantic claim made by each status;
- the Agent or operational role permitted to emit or record that status;
- the immediate pipeline effect of the status;
- the authority responsible for resolution;
- resume permission, prerequisites, and resume target;
- human-intervention requirements;
- semantically allowed next statuses;
- selective invalidation of active artifacts and claims;
- durable artifacts and identities required for resumption;
- compatibility with the ten canonical Task Work Package artifacts;
- compatibility with the Manual Pipeline before DP-Task-Orchestrator exists.

This contract governs the current Task only. It does not establish readiness for a future Task unless an approved source explicitly states such a dependency.

## 5. Explicit Non-Goals

This contract MUST NOT:

- implement the DP-Task-Orchestrator state machine, transition handler, scheduler, dispatcher, event loop, or delegation algorithm;
- define an automatic action merely because a next status is allowed;
- create a sixth Agent, Resume Agent, Skill, or additional canonical Task Work Package artifact;
- create a permission system, access-control system, lock, database, workflow engine, evidence service, evidence-validation daemon, recovery engine, logging transport, or component dependency scheduler;
- redefine the Task Package Schema, Task Authority Model, Minimum Evidence Model, or future Resume Invariant;
- change approved Design, requirements, acceptance criteria, HLTP semantics, repository truth, or another artifact's authority by assertion;
- define commit execution or perform a commit;
- treat conversation history as a durable resume input.

## 6. Concepts and Terminology

### 6.1 Status

A **status** is a canonical semantic outcome or current condition for one Task, emitted in a defined authority context and recorded durably through Task State or an equivalent Manual Pipeline record. A status is not an artifact type, workflow engine state, evidence item, approval, or implicit action.

### 6.2 Workflow Position

A **workflow position** identifies where orchestration currently operates, such as planning, test design, implementation, review, fix, or completion. Workflow positions are not canonical Task statuses unless a term is explicitly defined as a status in this contract. This contract does not define workflow-position mechanics.

### 6.3 Emitter

An **emitter** is the approved Agent that identifies and produces a status within its governed work. Where DP-Task-Orchestrator does not yet exist, the Manual Pipeline human state manager MAY record an already supported operational status, but recording does not create the underlying semantic authority.

### 6.4 Resolution Owner

A **resolution owner** is the Agent, human authority, source owner, dependency owner, infrastructure owner, or other approved authority qualified to remove the condition represented by the status. Emission, recording, package selection, and Task State ownership do not grant resolution authority.

### 6.5 Pipeline Effect

A **pipeline effect** is the immediate semantic consequence for the current Task, such as permitting handoff, stopping affected work, returning responsibility to an earlier authority, requiring correction, permitting completion evaluation, or establishing commit readiness. A pipeline effect does not itself execute the next action.

### 6.6 Resume Permission and Resume Target

**Resume permission** states whether durable work may continue while the status is active and which prerequisites must first be satisfied. The **resume target** is the approved Agent or operational stage that may continue after those prerequisites are verified. Resume permission does not define the recovery algorithm reserved for the Resume Invariant.

### 6.7 Human Intervention

**Human intervention** is required when an explicit human decision, approval, source-authority determination, Design determination, scope decision, or external ownership action is necessary before progress can continue. Human involvement does not allow evidence, authority, freeze, or identity rules to be bypassed.

### 6.8 Allowed Next State

An **allowed next state** is a canonical status that may truthfully follow the current status after all stated resolution, replacement, identity, evidence, review, and human-gate prerequisites are satisfied. Allowed does not mean automatic, required, or directly reachable without prerequisites.

### 6.9 Invalidation

**Invalidation** means that an artifact instance, evidence item, review result, readiness claim, or status basis is no longer valid for an intended active use. Invalidation is selective, preserves frozen historical instances, and does not mean deletion or in-place mutation.

### 6.10 Resumption Artifacts

**Resumption artifacts** are the durable artifacts, references, identities, evidence, review results, blocker resolutions, and human decisions needed to continue correctly without conversation history. They must be sufficient to identify the exact Task, repository state, implementation, evidence run, review attempt, and active authority as applicable.

## 7. Shared Status Rules

### 7.1 Status Context and Qualification

Every active status MUST be interpreted with its Task identity, emitter or recording role, supporting artifacts, applicable repository and baseline or worktree identity, implementation identity when one exists, evidence and review identities when applicable, blocker or human-decision reference when applicable, responsible stage, and resume target.

The same status token MAY occur in more than one approved stage only when the emitter, supporting artifacts, and pipeline effect make the claim unambiguous. A generic token MUST NOT be interpreted as a broader claim than the emitting context establishes.

### 7.2 Status, Applicability, Availability, and Blocking

Status, artifact applicability, artifact availability, and blocking are distinct concepts.

- **Status** describes the current semantic outcome or condition of the Task.
- **Applicability** states whether a canonical artifact or obligation applies.
- **Availability** states whether an applicable artifact instance is currently available.
- **Blocking** records why affected work or an applicable artifact cannot progress or become available.

A status MUST NOT conceal a required applicability indication, availability condition, or `BlockingRef`. An optional Schema field MUST NOT be used to represent an applicable but unavailable artifact as not applicable.

### 7.3 Emitter and Resolution-Owner Separation

The emitter MUST identify only conditions discovered within its governed work. The resolution owner MUST be selected according to the authority required to remove the condition. The emitter MAY also be the resolution owner only when its existing authority covers the correction.

DP-Task-Orchestrator or the Manual Pipeline human state manager MAY record, route, or assemble state from authoritative inputs. Neither role acquires planning, test-design, implementation, review, evidence, Design, requirement, dependency, or infrastructure authority merely by recording the status.

### 7.4 Specific-Status Precedence over `BLOCKED`

The most specific truthful canonical status MUST be used. `BLOCKED` MUST be used only when affected work cannot continue and no defined specific status accurately represents the known cause.

A known dependency readiness problem MUST use `DEPENDENCY_NOT_READY`; a known Design ambiguity MUST use `DESIGN_CLARIFICATION_REQUIRED`; a verified Design and code contradiction MUST use `DESIGN_CODE_CONFLICT`; an invalid review package MUST use `INVALID_REVIEW_INPUT`; an unavailable test mechanism for an otherwise valid obligation MUST use `TEST_INFRASTRUCTURE_BLOCKED`; and equivalent specific conditions MUST use their specific statuses.

Discovery that an initially generic blocker has a specific canonical cause MUST refine the active status without rewriting the historical blocked record.

### 7.5 Allowed-Next-State Semantics

Allowed-next-state lists are semantic constraints, not transition implementations. A listed successor MAY be recorded only after all prerequisites for that successor are positively established by the correct authority.

No allowed-next-state edge may:

- bypass an active blocker or required human decision;
- bypass correction or required reverification;
- carry evidence, review, or completion claims to a different implementation identity;
- treat package assembly or Task State as proof;
- permit a consumer to replace an owning role's frozen artifact;
- convert a partial or not-yet-run condition into success;
- imply commit execution.

A status not listed as an allowed successor MUST NOT directly follow without an intervening canonical status that truthfully represents the required resolution or work.

### 7.6 Resume Semantics

Resume is permitted only from durable source-of-truth material: Task State, active artifacts, repository and baseline or worktree identity, implementation identity, valid evidence, review results, blockers, approved decisions, and required replacements.

If resolution changes an upstream authoritative artifact, resume MUST return to the earliest affected owner and every affected downstream artifact MUST be replaced or revalidated before reuse. If resolution leaves all upstream authority valid, resume MAY return directly to the blocked stage identified by this contract.

Correct resumption MUST NOT depend on prior conversation history, hidden state, or remembered reasoning.

### 7.7 Selective Invalidation

A status MUST invalidate only the active claims and artifacts materially affected by the changed authority, identity, target, context, scope, mapping, blocker, evidence, review input, or human decision.

Invalidation MUST NOT automatically erase independent valid work or invalidate the entire Task Work Package. A frozen invalidated or superseded instance remains immutable historical material for lineage, audit, evidence interpretation, and correct resumption.

### 7.8 Freeze, Replacement, and Identity

A frozen artifact MUST NOT be materially corrected in place. Material correction requires a distinguishable replacement artifact identity and predecessor lineage when needed to prevent mixing.

A changed implementation requires a distinguishable `implementation_id`. A repeated execution requires a new `evidence_id`. A new review attempt requires a new Task Review Result. Prior evidence and review remain bound to the exact identities they evaluated and MUST NOT be relabeled for a replacement instance.

Freeze preserves an instance but does not guarantee continuing validity.

### 7.9 Evidence and Review Binding

A status that claims execution, verification, review, or completion MUST be supported by evidence valid for that intended use and bound to the exact target, context, scope, mappings, repository state, and implementation identity as applicable.

Successful execution, failed execution, not-yet-run work, partial execution, and blocked-before-execution work are distinct. Exit code alone does not establish semantic success. Evidence aggregation cannot repair invalid evidence, bridge implementations, produce Review PASS, or establish Task completion.

`PASS` and `FAIL` are Reviewer-owned substantive outcomes for exact reviewed inputs. Correction does not edit a prior Review Result. A corrected implementation or review package requires a distinguishable review attempt that restarts according to the governing review discipline.

### 7.10 Task State Boundary

Task State records or locates the active status, responsible stage, active artifacts, repository and baseline or worktree identity, implementation identity, evidence, review result, blocker, pending action, last durable update, and resume target as applicable.

Task State is not evidence, does not copy or change another artifact's semantics, does not grant mutation authority, does not define the allowed transition graph, and cannot make unevidenced completion authoritative. Task State MUST preserve lineage and MUST NOT self-reference.

### 7.11 Manual Pipeline Compatibility

Before DP-Task-Orchestrator exists, a human state manager MAY perform operational Task State maintenance and Task Work Package assembly. The human state manager MUST use the same status meanings, prerequisites, identities, evidence rules, authority boundaries, and resume rules defined by this contract.

The human state manager does not become a sixth Agent and MUST NOT create planning, test-design, implementation, review, evidence, Design, or requirement authority. A session restart MUST continue from Task State, active artifacts, repository state, and evidence identities without relying on conversation history.

## 8. Canonical Status Inventory

This contract defines exactly the following 23 canonical Task statuses. Workflow positions, package availability values, evidence results, Foundation gates, and program milestones are not additional Task statuses.

1. `READY`
2. `BLOCKED`
3. `TASK_REQUIREMENTS_INCOMPLETE`
4. `DEPENDENCY_NOT_READY`
5. `DESIGN_CLARIFICATION_REQUIRED`
6. `DESIGN_CODE_CONFLICT`
7. `TASK_SPLIT_REQUIRED`
8. `DOMAIN_SKILL_NOT_AVAILABLE`
9. `DOMAIN_ROUTING_AMBIGUOUS`
10. `DOMAIN_CONCERN_UNSUPPORTED`
11. `PLAN_UPDATE_REQUIRED`
12. `NO_TEST_CHANGES_REQUIRED`
13. `REQUIREMENT_NOT_TESTABLE`
14. `TEST_INFRASTRUCTURE_BLOCKED`
15. `IMPLEMENTED`
16. `PARTIALLY_IMPLEMENTED`
17. `READY_FOR_REVIEW`
18. `PASS`
19. `FAIL`
20. `INVALID_REVIEW_INPUT`
21. `FIX_REQUIRED`
22. `HUMAN_APPROVAL_REQUIRED`
23. `READY_FOR_COMMIT`

## 9. Canonical Status Definitions

Each definition below is normative and contains the same nine required fields. A field applies together with the shared rules in Sections 6 and 7. Where a definition lists more than one emitter, resolution owner, resume target, or successor, the actual choice MUST match the durable cause, authority, identities, and prerequisites of the current Task.

### 9.1 `READY`

- **Meaning:** The emitting pre-implementation stage completed its governed handoff obligations; READY is stage-qualified and is not global Task completion.
- **Emitter:** DP-Task-Planner or DP-Task-Test-Designer, each only for its own governed handoff.
- **Pipeline effect:** Permit handoff to the next applicable stage without automatically starting that stage.
- **Resolution owner:** The owner of any invalidated supporting artifact resolves a later defect; no separate resolver exists while READY remains valid.
- **Resume permission and target:** Resume is permitted at the next applicable stage: Test Designer or Implementer after Planner READY, and Implementer after Test Designer READY.
- **Human-intervention requirement:** Human intervention is not required unless a newly discovered issue requires human authority.
- **Allowed next states:** Allowed next statuses are NO_TEST_CHANGES_REQUIRED, READY, implementation outcomes, or the specific blocker/escalation discovered by the next stage; READY_FOR_REVIEW and READY_FOR_COMMIT are not aliases of READY.
- **Invalidated artifacts:** A material change to a supporting Contract, routing result, Repository Context, Plan, HLTP, source decision, or relevant identity invalidates only the affected READY basis and dependent active artifacts.
- **Artifacts required for resumption:** Resumption requires valid Task State and the exact frozen handoff artifacts, routing entries, repository/baseline identity, Plan, and applicable HLTP or no-test-change basis.

### 9.2 `BLOCKED`

- **Meaning:** Affected work cannot continue for a durable reason for which no more specific canonical status applies.
- **Emitter:** Any approved Agent may emit BLOCKED only for a blocker encountered in its governed work; Orchestrator or the Manual Pipeline human state manager may record and route it without acquiring resolution authority.
- **Pipeline effect:** Stop affected work and prohibit a positive handoff or completion claim while the blocker remains active.
- **Resolution owner:** The authority identified by the blocker resolves it.
- **Resume permission and target:** Resume is prohibited until durable blocker resolution is verified; resume returns to the blocked stage unless the resolution invalidates an earlier artifact and requires its owner.
- **Human-intervention requirement:** Human intervention is conditional when the resolution authority is human or no approved Agent can resolve the blocker.
- **Allowed next states:** Allowed next statuses are the specific status discovered after refinement, or a truthful readiness/progress status only after verified resolution and required replacement or reverification.
- **Invalidated artifacts:** BLOCKED does not invalidate the whole package; artifacts dependent on the unresolved or changed basis cannot remain active authority for the affected use.
- **Artifacts required for resumption:** Resumption requires Task State, BlockingRef, verified resolution record, replacement artifacts and identities, and applicable repository, implementation, evidence, and review references.

### 9.3 `TASK_REQUIREMENTS_INCOMPLETE`

- **Meaning:** Approved Task objective, scope, requirements, acceptance criteria, or fixed decisions are insufficient to form a complete Task Contract.
- **Emitter:** DP-Task-Planner.
- **Pipeline effect:** Stop contract formation and downstream planning; missing meaning must not be invented.
- **Resolution owner:** Approved requirement/source authority resolves the missing information; Planner then produces the replacement Contract.
- **Resume permission and target:** Resume is prohibited until authoritative requirements are durable; resume target is DP-Task-Planner.
- **Human-intervention requirement:** Human intervention is required when existing approved sources cannot supply the missing requirement information.
- **Allowed next states:** Allowed next statuses are READY, DESIGN_CLARIFICATION_REQUIRED, TASK_SPLIT_REQUIRED, DEPENDENCY_NOT_READY when an identified dependency is the remaining issue, or BLOCKED only for an otherwise unclassified blocker.
- **Invalidated artifacts:** An incomplete Contract is not active handoff authority; downstream Plan or HLTP based on invented or incomplete meaning is invalid for active use.
- **Artifacts required for resumption:** Resumption requires the authoritative decision/source, replacement Task Contract, remaining BlockingRefs, and updated Task State.

### 9.4 `DEPENDENCY_NOT_READY`

- **Meaning:** An identified required Task dependency exists but is not ready for the current Task.
- **Emitter:** DP-Task-Planner during readiness, or DP-Task-Implementer when verified execution reality reveals the condition; state management may record it.
- **Pipeline effect:** Stop dependent actions while preserving independently valid progress.
- **Resolution owner:** The dependency owner resolves readiness; Planner determines whether Task readiness and Plan remain valid.
- **Resume permission and target:** Resume requires durable readiness evidence; target is Planner when readiness or Plan must be reevaluated, otherwise Implementer.
- **Human-intervention requirement:** Human intervention is conditional when dependency resolution or scheduling is outside approved Agent authority.
- **Allowed next states:** Allowed next statuses are READY, PLAN_UPDATE_REQUIRED, TASK_SPLIT_REQUIRED, or another specific blocker after dependency resolution.
- **Invalidated artifacts:** Only Plan actions, implementation, evidence, or reviews materially dependent on the false readiness assumption lose active applicability.
- **Artifacts required for resumption:** Resumption requires dependency reference, readiness evidence/decision, valid Contract and Plan, refreshed Repository Context when needed, and updated Task State.

### 9.5 `DESIGN_CLARIFICATION_REQUIRED`

- **Meaning:** Approved Design authority is missing, ambiguous, or unresolved for a required semantic decision.
- **Emitter:** Planner, Test Designer, Implementer, or Reviewer may emit it within governed work; Orchestrator may route the recorded need only.
- **Pipeline effect:** Stop all work that depends on the unresolved Design meaning; no local interpretation is selected.
- **Resolution owner:** Approved human or source Design authority resolves it; affected artifact owners then replace their artifacts as needed.
- **Resume permission and target:** Resume requires a durable Design decision and returns to the earliest affected owner: Planner, Test Designer, Implementer, or Reviewer after corrected inputs.
- **Human-intervention requirement:** Human intervention is required unless the approved clarification already exists and only retrieval was missing.
- **Allowed next states:** Allowed next statuses are READY, TASK_REQUIREMENTS_INCOMPLETE, TASK_SPLIT_REQUIRED, PLAN_UPDATE_REQUIRED, REQUIREMENT_NOT_TESTABLE, READY_FOR_REVIEW after valid correction, HUMAN_APPROVAL_REQUIRED when approval itself remains, or a specific blocker.
- **Invalidated artifacts:** Artifacts whose meaning depends on the decision lose active validity; prior implementation-bound evidence and review do not transfer after material change.
- **Artifacts required for resumption:** Resumption requires the approved Design decision, every required replacement Contract/Plan/HLTP, exact implementation identity and new evidence/review as applicable, and updated Task State.

### 9.6 `DESIGN_CODE_CONFLICT`

- **Meaning:** Verified repository or code reality materially conflicts with approved Design for the Task.
- **Emitter:** Primarily DP-Task-Implementer; Planner or Reviewer may emit it when verified within their governed work.
- **Pipeline effect:** Stop affected implementation/review; no Agent silently chooses Design or code as the winner.
- **Resolution owner:** Human Design/source authority resolves the conflict, with Planner updating planning artifacts when required.
- **Resume permission and target:** Resume requires a durable conflict decision; target is Planner when Contract/Context/Plan changes, Implementer when the Plan remains valid, or Reviewer only after corrected inputs.
- **Human-intervention requirement:** Human intervention is required.
- **Allowed next states:** Allowed next statuses are DESIGN_CLARIFICATION_REQUIRED, PLAN_UPDATE_REQUIRED, TASK_SPLIT_REQUIRED, READY after authoritative resolution, READY_FOR_REVIEW after corrected implementation, or BLOCKED if authority remains unavailable.
- **Invalidated artifacts:** Repository Context, Plan, implementation, evidence, and review are invalidated selectively when they depend on the rejected side of the conflict.
- **Artifacts required for resumption:** Resumption requires conflict evidence, authoritative decision, refreshed Context/Plan as applicable, exact implementation identity, replacement evidence/review, and Task State.

### 9.7 `TASK_SPLIT_REQUIRED`

- **Meaning:** The Task cannot remain one bounded, coherent commit-scale unit.
- **Emitter:** DP-Task-Planner; later Agents report the condition to Planner rather than splitting silently.
- **Pipeline effect:** Stop execution of the unsplit Task as one implementable unit.
- **Resolution owner:** DP-Task-Planner, subject to source or human authority when the approved implementation plan must change.
- **Resume permission and target:** The unsplit Task does not resume unchanged; each approved replacement Task begins with its own task identity and Planner flow.
- **Human-intervention requirement:** Human intervention is conditional when Planner lacks authority to change source Task decomposition.
- **Allowed next states:** Allowed next statuses are READY for a valid replacement Task, TASK_REQUIREMENTS_INCOMPLETE, DEPENDENCY_NOT_READY, HUMAN_APPROVAL_REQUIRED, or BLOCKED.
- **Invalidated artifacts:** The unsplit Contract and Plan are not active authority for new Tasks; artifacts/evidence are not silently reassigned across task IDs.
- **Artifacts required for resumption:** Resumption requires approved split decision, distinct task IDs, separate Task Contracts and Task States, explicit dependencies, lineage, and updated package inventories.

### 9.8 `DOMAIN_SKILL_NOT_AVAILABLE`

- **Meaning:** A specifically selected required Domain Skill or authoritative lookup is unavailable or unverified.
- **Emitter:** The producing routing role or an approved consumer that requires the selected Skill for governed work.
- **Pipeline effect:** Stop only work dependent on that domain authority; unguided domain reasoning is not substituted.
- **Resolution owner:** Skill/inventory owner or human authority restores availability or supplies an approved alternative; producing routing role reroutes when needed.
- **Resume permission and target:** Resume requires a valid Skill/authority revision or replacement routing result; target is the routing role and then the named consumer.
- **Human-intervention requirement:** Human intervention is conditional when availability or approved substitution cannot be restored by an approved role.
- **Allowed next states:** Allowed next statuses are READY, DOMAIN_ROUTING_AMBIGUOUS, DOMAIN_CONCERN_UNSUPPORTED, or BLOCKED.
- **Invalidated artifacts:** The unavailable routing entry and downstream artifacts depending on unverified constraints cannot remain active authority; unrelated concerns remain valid.
- **Artifacts required for resumption:** Resumption requires valid Skill/authority reference, replacement routing result when changed, exact consumer mappings, affected replacement artifacts, and Task State.

### 9.9 `DOMAIN_ROUTING_AMBIGUOUS`

- **Meaning:** A domain concern is identified but valid routing cannot be uniquely selected among candidate Skills or authorities.
- **Emitter:** DP-Task-Planner or DP-Task-Orchestrator as the producing routing role.
- **Pipeline effect:** Stop affected consumers from selecting a route locally.
- **Resolution owner:** The producing routing role resolves routing; human source authority resolves professional ambiguity that inventory cannot decide.
- **Resume permission and target:** Resume requires a distinguishable resolved routing result and returns to each exact named consumer.
- **Human-intervention requirement:** Human intervention is conditional when approved sources and routing inventory cannot decide.
- **Allowed next states:** Allowed next statuses are READY, DOMAIN_SKILL_NOT_AVAILABLE, DOMAIN_CONCERN_UNSUPPORTED, DESIGN_CLARIFICATION_REQUIRED, or BLOCKED.
- **Invalidated artifacts:** Ambiguous routing is not active authority; downstream artifacts that selected an unresolved route are invalid for affected use.
- **Artifacts required for resumption:** Resumption requires resolved routing entry, Skill/authority revision, consumer mappings, blocker resolution, replacement dependents, and Task State.

### 9.10 `DOMAIN_CONCERN_UNSUPPORTED`

- **Meaning:** An in-scope domain concern has no approved Skill or authoritative lookup capable of supporting it.
- **Emitter:** The producing routing role after inspecting the available approved inventory and authorities.
- **Pipeline effect:** Stop work dependent on the unsupported concern; general knowledge is not substituted.
- **Resolution owner:** Human authority decides whether to add authority, create future capability, change scope, split the Task, or keep it blocked.
- **Resume permission and target:** Resume is prohibited until approved authority exists or scope is validly changed; target is Planner or the routing role.
- **Human-intervention requirement:** Human intervention is required.
- **Allowed next states:** Allowed next statuses are DOMAIN_SKILL_NOT_AVAILABLE if capability becomes identified but unavailable, READY after approved authority exists, TASK_SPLIT_REQUIRED, TASK_REQUIREMENTS_INCOMPLETE, HUMAN_APPROVAL_REQUIRED, or BLOCKED.
- **Invalidated artifacts:** Downstream artifacts based on unsupported assumptions are invalid; scope change selectively invalidates Contract and Plan dependencies.
- **Artifacts required for resumption:** Resumption requires approved authority/Skill, updated routing result, replacement Contract/Plan when scope or actions change, and Task State.

### 9.11 `PLAN_UPDATE_REQUIRED`

- **Meaning:** The frozen Execution Plan is materially insufficient or invalid for current executability, order, dependencies, boundaries, protected areas, or verification obligations.
- **Emitter:** Primarily DP-Task-Implementer; Reviewer may identify the unhandled need without replacing the Plan.
- **Pipeline effect:** Stop affected implementation; the frozen Plan is not edited in place.
- **Resolution owner:** DP-Task-Planner produces any replacement Plan.
- **Resume permission and target:** Resume returns to Planner; after replacement it passes through Test Designer when obligations changed, otherwise to Implementer.
- **Human-intervention requirement:** Human intervention is conditional when the update changes source scope, Design, or protected-area authority.
- **Allowed next states:** Allowed next statuses are READY, NO_TEST_CHANGES_REQUIRED, Test Designer READY, TASK_SPLIT_REQUIRED, DESIGN_CLARIFICATION_REQUIRED, DEPENDENCY_NOT_READY, or BLOCKED.
- **Invalidated artifacts:** The old Plan and dependent HLTP, implementation, evidence, or review lose active applicability only in materially changed scope.
- **Artifacts required for resumption:** Resumption requires replacement Plan, refreshed Repository Context as needed, replacement HLTP/no-test basis as needed, exact implementation identity, and Task State.

### 9.12 `NO_TEST_CHANGES_REQUIRED`

- **Meaning:** An approved test-design outcome determines that no Task-local HLTP change or slice is required.
- **Emitter:** DP-Task-Test-Designer or another explicitly approved authority recorded as the applicability basis.
- **Pipeline effect:** Mark Task-local HLTP not applicable and permit implementation without fabricating an empty HLTP; other verification duties remain.
- **Resolution owner:** No resolver while valid; Test Designer reevaluates after material scope, behavior, criterion, or Plan-verification change.
- **Resume permission and target:** Resume is permitted to DP-Task-Implementer.
- **Human-intervention requirement:** Human intervention is not required unless the no-test determination requires unavailable authority.
- **Allowed next states:** Allowed next statuses are IMPLEMENTED, PARTIALLY_IMPLEMENTED, READY_FOR_REVIEW, or a specific implementation blocker/escalation.
- **Invalidated artifacts:** Material behavior, criterion, scope, or verification change invalidates the no-test applicability basis and requires reassessment.
- **Artifacts required for resumption:** Resumption requires applicability_basis_ref, Contract, Plan, not-applicable HLTP inventory entry, repository identity, and Task State.

### 9.13 `REQUIREMENT_NOT_TESTABLE`

- **Meaning:** A required behavior or criterion cannot be expressed as a valid observable proof obligation without authoritative change or clarification.
- **Emitter:** DP-Task-Test-Designer; Implementer reports the issue when a frozen obligation proves unimplementable without semantic change.
- **Pipeline effect:** Stop affected test design or implementation; obligations are not weakened.
- **Resolution owner:** Test Designer resolves within test-design authority; Planner/source or human Design authority resolves requirement/Design changes.
- **Resume permission and target:** Resume requires clarified requirement, approved exception, or replacement HLTP and returns to the authority that changed meaning and then the appropriate downstream stage.
- **Human-intervention requirement:** Human intervention is required when resolution changes requirement, criterion, or Design.
- **Allowed next states:** Allowed next statuses are READY, NO_TEST_CHANGES_REQUIRED only with approved applicability basis, DESIGN_CLARIFICATION_REQUIRED, TASK_REQUIREMENTS_INCOMPLETE, PLAN_UPDATE_REQUIRED, or BLOCKED.
- **Invalidated artifacts:** An HLTP that weakens or hides the obligation is invalid; changed requirements selectively invalidate Contract, Plan, mappings, implementation, evidence, and review.
- **Artifacts required for resumption:** Resumption requires clarification/exception authority, replacement Contract/Plan/HLTP and mappings as applicable, and Task State.

### 9.14 `TEST_INFRASTRUCTURE_BLOCKED`

- **Meaning:** A valid test obligation exists, but required test implementation or execution infrastructure is unavailable or unusable.
- **Emitter:** DP-Task-Implementer; Reviewer may record evidence that required execution did not complete but does not repair infrastructure.
- **Pipeline effect:** Stop required verification and prohibit treating not-run or failed execution as success.
- **Resolution owner:** Test-infrastructure owner or project authority resolves infrastructure; Implementer reruns verification.
- **Resume permission and target:** Resume requires verified infrastructure resolution and returns to Implementer for a new execution.
- **Human-intervention requirement:** Human intervention is conditional when infrastructure ownership is outside the Task or approved Agent authority.
- **Allowed next states:** Allowed next statuses are READY_FOR_REVIEW after valid rerun/handoff, PARTIALLY_IMPLEMENTED, PLAN_UPDATE_REQUIRED, DEPENDENCY_NOT_READY, or BLOCKED.
- **Invalidated artifacts:** Failed execution remains evidence of failure; never-started work is not evidence; prior success does not transfer to a changed implementation without explicit independence.
- **Artifacts required for resumption:** Resumption requires infrastructure-resolution evidence, new Build/Test Evidence, exact implementation identity, updated Report and Criterion Evidence, and Task State.

### 9.15 `IMPLEMENTED`

- **Meaning:** The Implementer completed planned implementation work for an exact implementation identity; this is not proof, Review PASS, or completion.
- **Emitter:** DP-Task-Implementer.
- **Pipeline effect:** Permit completion of verification and reviewer-handoff assembly; READY_FOR_REVIEW remains distinct.
- **Resolution owner:** No resolver while valid; a discovered defect routes to Implementer or the affected upstream authority.
- **Resume permission and target:** Resume is permitted to Implementer for verification/handoff, or to Reviewer only after separate READY_FOR_REVIEW.
- **Human-intervention requirement:** Human intervention is not required by default.
- **Allowed next states:** Allowed next statuses are READY_FOR_REVIEW, PARTIALLY_IMPLEMENTED if incompleteness is discovered, TEST_INFRASTRUCTURE_BLOCKED, DESIGN_CODE_CONFLICT, PLAN_UPDATE_REQUIRED, or BLOCKED.
- **Invalidated artifacts:** Changed implementation requires a new implementation identity; prior Report, evidence, and review do not transfer automatically.
- **Artifacts required for resumption:** Resumption requires exact implementation identity, Implementation Report, repository/baseline/worktree associations, valid Plan/HLTP, available evidence, unresolved items, and Task State.

### 9.16 `PARTIALLY_IMPLEMENTED`

- **Meaning:** An exact implementation attempt completed some but not all required work or verification.
- **Emitter:** DP-Task-Implementer.
- **Pipeline effect:** Preserve durable progress without claiming complete implementation or review readiness.
- **Resolution owner:** Implementer resolves remaining in-Plan work; Planner, Design authority, dependency owner, or infrastructure owner resolves the specific external cause.
- **Resume permission and target:** Resume is conditional from the exact implementation/repository state to Implementer or the specific upstream resolution owner.
- **Human-intervention requirement:** Human intervention is conditional on the unresolved cause.
- **Allowed next states:** Allowed next statuses are IMPLEMENTED, READY_FOR_REVIEW, PLAN_UPDATE_REQUIRED, DESIGN_CODE_CONFLICT, TEST_INFRASTRUCTURE_BLOCKED, DEPENDENCY_NOT_READY, or BLOCKED.
- **Invalidated artifacts:** No blanket invalidation occurs; changed content state requires a distinguishable implementation identity and evidence remains bound to the attempt executed.
- **Artifacts required for resumption:** Resumption requires Report with attempted/implemented scope, exact implementation and worktree state, changed areas, unresolved items, evidence/blockers, valid Plan/HLTP, and Task State.

### 9.17 `READY_FOR_REVIEW`

- **Meaning:** The Implementer completed an integrity-bound handoff for an exact implementation with all Implementer-required review inputs.
- **Emitter:** DP-Task-Implementer.
- **Pipeline effect:** Permit independent Review; it does not predict PASS or prevent Reviewer input-integrity rejection.
- **Resolution owner:** No resolver while valid; any defective input returns to its owning role.
- **Resume permission and target:** Resume is permitted to DP-Task-Reviewer against the exact implementation and review-input set.
- **Human-intervention requirement:** Human intervention is not required by default.
- **Allowed next states:** Allowed next statuses are PASS, FAIL, INVALID_REVIEW_INPUT, DESIGN_CLARIFICATION_REQUIRED, or BLOCKED only for another unclassified review blocker.
- **Invalidated artifacts:** Any implementation or reviewed-input change invalidates this handoff readiness; mismatched evidence or broken integrity references also invalidate it.
- **Artifacts required for resumption:** Resumption requires Contract, Context when applicable, Plan, HLTP/no-test basis, Report, Criterion Evidence, Build/Test Evidence, exact implementation identity, integrity references, applicable Skills, and Task State.

### 9.18 `PASS`

- **Meaning:** Reviewer completed the full ordered review suite successfully for one exact implementation and exact reviewed inputs.
- **Emitter:** DP-Task-Reviewer only.
- **Pipeline effect:** Permit completion evaluation; PASS alone is not READY_FOR_COMMIT.
- **Resolution owner:** No resolver while valid; changed inputs require a new review rather than editing PASS.
- **Resume permission and target:** Resume is permitted to Orchestrator or Manual Pipeline completion management for the exact reviewed set.
- **Human-intervention requirement:** Human intervention is not required for PASS itself; a separate human gate may still apply.
- **Allowed next states:** Allowed next statuses are READY_FOR_COMMIT, HUMAN_APPROVAL_REQUIRED, INVALID_REVIEW_INPUT if integrity attribution is later disproven, READY_FOR_REVIEW for a new implementation, or a specific completion blocker.
- **Invalidated artifacts:** New implementation, changed reviewed input, evidence mismatch, broken integrity reference, or false attribution invalidates active applicability while preserving the historical PASS result.
- **Artifacts required for resumption:** Resumption requires PASS Review Result, exact reviewed implementation, complete ordered passed-check set, reviewed artifacts/evidence, Task State, and completion inputs.

### 9.19 `FAIL`

- **Meaning:** Reviewer found the first ordered review check that does not pass for the exact reviewed set.
- **Emitter:** DP-Task-Reviewer only.
- **Pipeline effect:** Stop Review at that check and prohibit later checks in the same attempt.
- **Resolution owner:** Implementer normally corrects the defect; the finding may identify another exact authority.
- **Resume permission and target:** Resume first goes to the correction owner; after correction/reverification a new review starts at the first review check.
- **Human-intervention requirement:** Human intervention is conditional when correction requires Design, scope, or source authority.
- **Allowed next states:** Allowed next statuses are FIX_REQUIRED, DESIGN_CLARIFICATION_REQUIRED, PLAN_UPDATE_REQUIRED, TASK_SPLIT_REQUIRED, READY_FOR_REVIEW after correction, or BLOCKED.
- **Invalidated artifacts:** FAIL does not blanket-invalidate implementation; a changed implementation gets a new identity and stale evidence does not transfer; prior FAIL remains frozen.
- **Artifacts required for resumption:** Resumption requires FAIL Review Result, first failing check, finding, correction, reverification, passed-before-failure set, exact inputs, and updated implementation/evidence after correction.

### 9.20 `INVALID_REVIEW_INPUT`

- **Meaning:** Substantive Review cannot start or continue because a required input is missing, invalid, unresolvable, stale, or identity-mismatched.
- **Emitter:** DP-Task-Reviewer.
- **Pipeline effect:** Stop at input integrity without asserting a substantive implementation defect; corresponding Task Review Result is BLOCKED.
- **Resolution owner:** The owner of the defective input or package selection corrects it; state management may correct selection only.
- **Resume permission and target:** Resume is prohibited until integrity is corrected; target is the defective-input owner, then a new READY_FOR_REVIEW and review attempt.
- **Human-intervention requirement:** Human intervention is conditional when the missing authority or identity conflict cannot be resolved by an approved role.
- **Allowed next states:** Allowed next statuses are READY_FOR_REVIEW, PLAN_UPDATE_REQUIRED, DESIGN_CLARIFICATION_REQUIRED, or BLOCKED.
- **Invalidated artifacts:** Current review-handoff readiness and any result based on the same false attribution lose active validity; no automatic code-defect conclusion follows.
- **Artifacts required for resumption:** Resumption requires corrected review package, exact implementation identity, artifact refs, integrity digests, corrected evidence mappings, blocker resolution, and Task State.

### 9.21 `FIX_REQUIRED`

- **Meaning:** The pipeline must perform the correction required by a valid FAIL or equivalent authoritative finding; it is not a new review judgment.
- **Emitter:** DP-Task-Orchestrator or Manual Pipeline human state manager records it from a valid finding.
- **Pipeline effect:** Route responsibility to Implementer or the exact authority named by the finding; Orchestrator does not fix it.
- **Resolution owner:** The correction owner identified by the finding.
- **Resume permission and target:** Resume is permitted to that owner, but return to Review requires correction, required reverification, new handoff, and a review restart.
- **Human-intervention requirement:** Human intervention is conditional when the finding requires human authority.
- **Allowed next states:** Allowed next statuses are PARTIALLY_IMPLEMENTED, IMPLEMENTED, READY_FOR_REVIEW, PLAN_UPDATE_REQUIRED, DESIGN_CLARIFICATION_REQUIRED, TEST_INFRASTRUCTURE_BLOCKED, or BLOCKED.
- **Invalidated artifacts:** Only finding-affected material is invalidated; changed implementation gets a new identity and prior FAIL remains frozen.
- **Artifacts required for resumption:** Resumption requires active FAIL finding, required correction/reverification, exact implementation identity, valid Plan/HLTP, responsible stage, and Task State.

### 9.22 `HUMAN_APPROVAL_REQUIRED`

- **Meaning:** Explicit human approval itself is the next required gate.
- **Emitter:** DP-Task-Orchestrator or an approved Agent that identifies an exact human gate; human state manager may record it from existing authority.
- **Pipeline effect:** Stop before the protected action; silence is not approval.
- **Resolution owner:** The named human or source authority decides.
- **Resume permission and target:** Resume requires a durable decision bound to exact artifacts/implementation and continues to completion or the stage required by the decision.
- **Human-intervention requirement:** Human intervention is required by definition.
- **Allowed next states:** Allowed next statuses are READY_FOR_COMMIT, FIX_REQUIRED, PLAN_UPDATE_REQUIRED, DESIGN_CLARIFICATION_REQUIRED, TASK_SPLIT_REQUIRED, or BLOCKED.
- **Invalidated artifacts:** Denial alone does not blanket-invalidate artifacts; a material approval decision selectively invalidates affected dependents and does not rewrite prior review/evidence.
- **Artifacts required for resumption:** Resumption requires approval request, durable human decision, exact affected artifacts/implementation, valid PASS when approval follows Review, and Task State.

### 9.23 `READY_FOR_COMMIT`

- **Meaning:** The current Task satisfies approved completion conditions and is ready for a coherent commit boundary; it is not committed and says nothing about a future Task.
- **Emitter:** DP-Task-Orchestrator, or the Manual Pipeline human state manager recording equivalent completion from authoritative inputs.
- **Pipeline effect:** End active Task work and permit only a separately authorized commit action unless readiness is invalidated.
- **Resolution owner:** No resolver while valid; commit execution remains a separate explicitly instructed action.
- **Resume permission and target:** Normal Task-work resume is not permitted while valid; continuation is to authorized commit, or back to the earliest affected authority after invalidation.
- **Human-intervention requirement:** Human intervention is conditional when policy requires approval to execute commit; readiness itself does not execute commit.
- **Allowed next states:** No internal post-commit status is defined here; before commit, a discovered defect/change may lead to FIX_REQUIRED, READY_FOR_REVIEW, INVALID_REVIEW_INPUT, or the specific blocker/escalation.
- **Invalidated artifacts:** Implementation change, invalid Review PASS, evidence mismatch, open deviation/blocker, relevant Contract/Plan/HLTP change, or repository/worktree mismatch invalidates active readiness.
- **Artifacts required for resumption:** Resumption/completion evidence requires valid Contract, routing/Context, Plan, applicable HLTP/no-test basis, Report, Criterion Evidence, valid Build/Test Evidence, exact PASS Review Result, no blocker/deviation, coherent diff boundary, and Task State.

## 10. Status-Selection Precedence

Status selection MUST describe the most specific semantic condition that is currently established by the emitting authority. Selection MUST NOT optimize for convenience, shorten the pipeline, conceal unavailable artifacts, or anticipate a result that has not yet been established.

Apply the following ordered rules:

1. **Identify the governed stage and emitter.** Determine which approved Agent discovered the condition and which artifact or activity is currently governed. A role MUST NOT emit a status whose semantic claim is outside that role's authority.
2. **Separate outcome from cause.** Preserve factual progress or outcome separately from the durable blocker, finding, invalid input, or human gate that controls continuation. A blocked partial implementation remains partial implementation with a blocker; the blocker does not erase completed work.
3. **Choose the most specific cause.** If a canonical status exactly represents the known cause, that status MUST be used instead of `BLOCKED`.
4. **Preserve semantic layer.** A producer outcome, review outcome, orchestration consequence, and completion result are different layers. A later layer MUST NOT replace or rewrite an earlier authoritative result. For example, `FAIL` remains the Reviewer result and `FIX_REQUIRED` is the routing consequence.
5. **Prefer current truth over anticipated success.** `READY`, `IMPLEMENTED`, `READY_FOR_REVIEW`, `PASS`, and `READY_FOR_COMMIT` MUST be emitted only when their own prerequisites are positively satisfied. The likelihood of satisfying a later status is irrelevant.
6. **Preserve identity and applicability.** A status claim MUST bind to the exact Task, artifacts, repository state, implementation, evidence, review attempt, and human decision to which the claim applies. A status valid for one identity MUST NOT be reused for another.
7. **Record the blocker durably.** Every blocking status MUST have a resolvable blocker or authoritative decision basis, responsible resolution owner, pending action, and resume target in Task State or the equivalent Manual Pipeline record.
8. **Do not collapse simultaneous truths.** When progress and a blocker coexist, the owning artifact records progress and unresolved items while Task State records the active status and blocker. No canonical status token is invented to combine the two.
9. **Escalate unresolved classification.** If available authority cannot determine which specific status is correct, the ambiguity itself MUST remain durably blocked. An Agent MUST NOT select a plausible status by guesswork.

A status-selection decision is valid only when the selected status, supporting artifacts, blocker or decision basis, and intended pipeline effect are mutually consistent.

## 11. Required Status Distinctions

### 11.1 `READY` versus `READY_FOR_REVIEW`

`READY` is a stage-qualified handoff outcome emitted by DP-Task-Planner or DP-Task-Test-Designer after that stage completes its governed obligations. `READY_FOR_REVIEW` is an implementation-specific handoff emitted only by DP-Task-Implementer after assembling the complete integrity-bound review package for one exact `implementation_id`.

`READY` MUST NOT be used as shorthand for implementation review readiness. `READY_FOR_REVIEW` MUST NOT be emitted merely because planning or test design is ready, because implementation work appears complete, or because an Implementation Report claims completion.

### 11.2 `READY_FOR_REVIEW` versus `READY_FOR_COMMIT`

`READY_FOR_REVIEW` permits independent Review to begin for an exact implementation and exact input set. It makes no claim that Review will pass or that Task completion conditions are satisfied.

`READY_FOR_COMMIT` is a completion result recorded only after the applicable planning, test, implementation, evidence, Review, blocker, deviation, unrelated-change, and commit-boundary requirements are all satisfied. It permits a separately authorized commit action but does not execute commit.

A Task MUST NOT move directly from `READY_FOR_REVIEW` to `READY_FOR_COMMIT` without a valid `PASS` for the exact implementation and completion evaluation of all additional prerequisites.

### 11.3 `IMPLEMENTED` versus `READY_FOR_REVIEW`

`IMPLEMENTED` means DP-Task-Implementer completed the planned implementation work for an exact implementation identity. It does not independently establish successful verification, complete acceptance-criterion mapping, valid evidence, input integrity, or reviewer-handoff completeness.

`READY_FOR_REVIEW` requires the implementation plus every Implementer-owned review input and integrity association required for independent Review. Therefore an implementation MAY be `IMPLEMENTED` while not yet `READY_FOR_REVIEW`.

### 11.4 `PARTIALLY_IMPLEMENTED` versus `BLOCKED`

`PARTIALLY_IMPLEMENTED` is a factual implementation-progress outcome. It states that an exact implementation attempt completed some, but not all, required work or verification. `BLOCKED` states that affected work cannot continue for a durable unclassified reason.

These meanings are not mutually exclusive at the artifact level. The Implementation Report MAY factually describe a partial attempt while Task State records a specific blocking status or, only when no specific status applies, `BLOCKED`. The blocker MUST NOT erase completed progress, and partial progress MUST NOT conceal inability to continue.

### 11.5 `FAIL` versus `FIX_REQUIRED`

`FAIL` is a substantive Reviewer-owned result for the first ordered review check that does not pass. It binds to exact reviewed inputs and records the finding, required correction, required reverification, and checks completed before failure.

`FIX_REQUIRED` is the orchestration or Manual Pipeline consequence derived from a valid `FAIL` or equivalent authoritative finding. It routes work to the correction owner. It is not a second review judgment, does not alter the finding, and does not authorize DP-Task-Orchestrator or the human state manager to perform the fix.

### 11.6 `INVALID_REVIEW_INPUT` versus `FAIL`

`INVALID_REVIEW_INPUT` applies when substantive Review cannot validly start or continue because a required input is missing, unresolvable, stale, invalid, or identity-mismatched. It does not assert a defect in implementation behavior.

`FAIL` applies only after a valid review input set allows substantive evaluation and the first ordered review check fails. Invalid input MUST NOT be reported as `FAIL`, and an implementation defect MUST NOT be hidden as input invalidity.

### 11.7 `INVALID_REVIEW_INPUT` versus `BLOCKED`

`INVALID_REVIEW_INPUT` is the specific Task status for a known review-input integrity problem. `BLOCKED` remains the fallback only for a review blocker that no defined specific status represents.

The associated Task Review Result MAY be structurally `BLOCKED` under the Task Package Schema because substantive Review could not proceed. This structural review result does not replace the more specific Task status `INVALID_REVIEW_INPUT`.

### 11.8 `PASS` versus `READY_FOR_COMMIT`

`PASS` means DP-Task-Reviewer completed the full ordered review suite successfully for one exact implementation and exact reviewed input set. Its authority is bounded to that review.

`READY_FOR_COMMIT` additionally requires all completion prerequisites, including valid evidence, correct identities, handled test obligations, no active blocker, no unresolved deviation, no unrelated change, preserved downstream-contract integrity, and a coherent commit boundary. `PASS` is necessary when Review applies but is not sufficient by itself for `READY_FOR_COMMIT`.

### 11.9 `DESIGN_CLARIFICATION_REQUIRED` versus `DESIGN_CODE_CONFLICT`

`DESIGN_CLARIFICATION_REQUIRED` applies when approved Design meaning is missing, ambiguous, or unresolved for a required decision. It does not require a demonstrated contradiction with repository reality.

`DESIGN_CODE_CONFLICT` applies when verified repository or code reality materially contradicts approved Design. Evidence of the contradiction is required. A mere implementation preference or incomplete inspection is neither status.

Both statuses stop affected work and require authoritative Design handling, but their resolution evidence and invalidation impact differ.

### 11.10 `TASK_REQUIREMENTS_INCOMPLETE` versus `DEPENDENCY_NOT_READY`

`TASK_REQUIREMENTS_INCOMPLETE` applies when objective, scope, requirements, acceptance criteria, or fixed decisions are insufficient to form a complete Task Contract. The missing Task meaning must be supplied by the approved requirement or source authority.

`DEPENDENCY_NOT_READY` applies when a required dependency is already identified but is not yet ready for the current Task. Dependency readiness evidence, rather than requirement invention, resolves the condition.

A dependency whose identity or necessity is itself undefined may contribute to incomplete requirements. Once the dependency is identified and only readiness remains, `DEPENDENCY_NOT_READY` MUST be used.

### 11.11 `PLAN_UPDATE_REQUIRED` versus `TASK_SPLIT_REQUIRED`

`PLAN_UPDATE_REQUIRED` preserves the current Task identity and bounded Task objective while requiring DP-Task-Planner to replace a materially insufficient or invalid Execution Plan.

`TASK_SPLIT_REQUIRED` means the Task boundary itself cannot remain one bounded, coherent commit-scale unit. Resolution creates separately identified replacement Tasks with their own Contracts, Task States, dependencies, and lineage.

A change outside an expected affected area is not, by itself, either status. The semantic impact on executability, scope, responsibility, protected areas, verification obligations, and Task coherence determines the correct status.

### 11.12 `REQUIREMENT_NOT_TESTABLE` versus `TEST_INFRASTRUCTURE_BLOCKED`

`REQUIREMENT_NOT_TESTABLE` applies when a required behavior or criterion cannot be represented as a valid observable proof obligation without authoritative clarification or change. It is a semantic testability problem.

`TEST_INFRASTRUCTURE_BLOCKED` applies when the obligation is valid and testable, but the required test implementation or execution mechanism is unavailable or unusable. It is an infrastructure or execution-mechanics problem.

Infrastructure difficulty MUST NOT weaken the obligation or convert it into `REQUIREMENT_NOT_TESTABLE`. A semantically invalid obligation MUST NOT be presented as an infrastructure outage.

### 11.13 Domain Status Distinctions

The three domain statuses are selected as follows:

- `DOMAIN_ROUTING_AMBIGUOUS` applies when a concern is known but the valid Skill or authoritative lookup cannot be uniquely selected.
- `DOMAIN_SKILL_NOT_AVAILABLE` applies when the appropriate Skill or lookup has been selected but is unavailable, unverified, or cannot be loaded for valid use.
- `DOMAIN_CONCERN_UNSUPPORTED` applies when no approved Skill or authoritative lookup is capable of supporting the in-scope concern.

Consumers MUST NOT resolve routing ambiguity locally, replace an unavailable selected Skill with unguided reasoning, or use general knowledge to cover an unsupported concern. A change from one domain status to another requires durable evidence that the underlying classification changed.

### 11.14 Specific Status versus `HUMAN_APPROVAL_REQUIRED`

`HUMAN_APPROVAL_REQUIRED` applies when explicit human approval itself is the next gate. It does not replace the specific reason that human authority is needed.

When the unresolved condition is incomplete requirements, Design ambiguity, Task split, unsupported domain concern, Plan change, or another defined status, the specific status MUST remain active until its resolution requirements are met. `HUMAN_APPROVAL_REQUIRED` MAY follow only when the remaining gate is the approval decision itself and the approval request is bound to exact artifacts and identities.

Human approval MUST NOT repair invalid evidence, rewrite a prior Review Result, transfer authority between Agents, or make an invalid artifact valid by assertion.

### 11.15 Specific Status versus `BLOCKED`

`BLOCKED` is a fallback for a durable inability to continue when no canonical specific status accurately describes the known cause. It MUST NOT replace any of the following when applicable:

- `TASK_REQUIREMENTS_INCOMPLETE`
- `DEPENDENCY_NOT_READY`
- `DESIGN_CLARIFICATION_REQUIRED`
- `DESIGN_CODE_CONFLICT`
- `TASK_SPLIT_REQUIRED`
- `DOMAIN_SKILL_NOT_AVAILABLE`
- `DOMAIN_ROUTING_AMBIGUOUS`
- `DOMAIN_CONCERN_UNSUPPORTED`
- `PLAN_UPDATE_REQUIRED`
- `REQUIREMENT_NOT_TESTABLE`
- `TEST_INFRASTRUCTURE_BLOCKED`
- `INVALID_REVIEW_INPUT`
- `HUMAN_APPROVAL_REQUIRED`

A `BLOCKED` record MUST identify why no specific status applies. When the cause becomes specific, the current status MUST be refined while preserving the historical record and blocker lineage.

## 12. Allowed-Next-State Contract

### 12.1 Graph Interpretation

The allowed-next-state fields in Section 9 define a closed semantic graph over the 23 canonical statuses in Section 8. Each status is one node. A listed allowed successor is an edge that constrains which status may truthfully be recorded after the current status.

The graph is declarative only. An edge:

- does not implement an Orchestrator state machine;
- does not trigger an Agent, tool, correction, rerun, approval, review, commit, or package update;
- does not grant authority to emit the successor;
- does not prove that the successor's prerequisites are satisfied;
- does not require the successor to occur;
- does not bypass the owner of an affected artifact.

A successor may be recorded only by its permitted emitter or by an operational state manager recording an already authoritative outcome. The successor MUST describe the current truth after all applicable prerequisites are satisfied.

Some Section 9 fields use bounded semantic families rather than enumerating every context-dependent member. These expressions have the following closed meanings:

- **implementation outcomes** means `IMPLEMENTED`, `PARTIALLY_IMPLEMENTED`, or `READY_FOR_REVIEW`, selected only when that status's own prerequisites hold;
- **truthful readiness or progress status** means `READY`, `IMPLEMENTED`, `PARTIALLY_IMPLEMENTED`, or `READY_FOR_REVIEW` as applicable to the resumed stage;
- **specific blocker or escalation** means one of `TASK_REQUIREMENTS_INCOMPLETE`, `DEPENDENCY_NOT_READY`, `DESIGN_CLARIFICATION_REQUIRED`, `DESIGN_CODE_CONFLICT`, `TASK_SPLIT_REQUIRED`, `DOMAIN_SKILL_NOT_AVAILABLE`, `DOMAIN_ROUTING_AMBIGUOUS`, `DOMAIN_CONCERN_UNSUPPORTED`, `PLAN_UPDATE_REQUIRED`, `REQUIREMENT_NOT_TESTABLE`, `TEST_INFRASTRUCTURE_BLOCKED`, `INVALID_REVIEW_INPUT`, or `HUMAN_APPROVAL_REQUIRED` when its exact semantics apply;
- **another specific blocker** and **specific status discovered after refinement** use the same specific-status set, narrowed by the emitting stage and known cause;
- **specific completion blocker** means a specific status from that set which invalidates or prevents completion evaluation;
- **specific blocker or escalation before commit** means a specific status from that set or `FIX_REQUIRED` when backed by an authoritative finding.

These family expressions MUST NOT introduce an unlisted status token or permit `BLOCKED` when a more specific member applies.

### 12.2 Referential Integrity

Every explicit successor token and every member of a bounded semantic family MUST resolve to one of the 23 canonical statuses. Workflow positions such as planning, test design, implementation, review, fix, and completion MUST NOT be used as status nodes.

The graph is validated under these rules:

1. Every Section 9 definition contains exactly one allowed-next-state field.
2. Every uppercase status token in an allowed-next-state field resolves to Section 8.
3. Every bounded family resolves only to the members defined in Section 12.1.
4. No edge invents a new Agent, artifact, evidence result, package state, or workflow position.
5. A self-edge is permitted only when a new stage-qualified instance of the same status truthfully represents a distinct handoff or when durable reevaluation confirms the same blocker without rewriting history.
6. `READY_FOR_COMMIT` has no internal success successor in this contract. Commit execution and post-commit state are outside scope.
7. Historical records remain immutable when the active status changes. A new status record supersedes active state without rewriting the prior record.

If a proposed successor cannot be resolved under these rules, the successor is invalid and MUST NOT be recorded.

### 12.3 Prerequisite Preservation

An allowed edge is usable only when every prerequisite of the destination status and every unresolved prerequisite of the source status are satisfied. At minimum, transition evaluation MUST preserve:

- **authority:** the destination status is emitted or operationally recorded from the correct authority basis;
- **resolution:** the source blocker, finding, ambiguity, approval gate, or unavailable input has been durably resolved or truthfully refined;
- **artifact ownership:** required replacement artifacts are produced by their owning roles, not edited by consumers or state management;
- **freeze and lineage:** material corrections use distinguishable replacement identities and predecessor lineage where required;
- **repository and implementation identity:** active claims refer to the exact repository, baseline or worktree, and implementation evaluated;
- **evidence identity and validity:** required execution or inspection occurred, evidence is valid for the intended use, and stale or mismatched evidence is not carried forward;
- **review discipline:** a corrected implementation or input set receives a new review attempt beginning at the first ordered check;
- **human authority:** a required human decision is durable, scoped to exact artifacts and identities, and does not repair invalid evidence or exceed the decision-maker's authority;
- **selective invalidation:** affected downstream artifacts and claims are replaced or revalidated before reuse, while independent valid material remains available;
- **resume basis:** Task State and all required resumption artifacts identify the correct resume target without conversation history.

When resolution changes an upstream artifact, a direct edge to a downstream readiness, review, pass, or completion status is prohibited. The graph MUST return to the earliest affected owner and proceed again through every invalidated handoff.

### 12.4 Prohibited Bypass Paths

The following direct paths are prohibited unless the required intermediate authoritative outcomes are independently re-established and durably recorded:

- `FAIL` to `PASS`: correction, required reverification, `READY_FOR_REVIEW`, and a new complete Review attempt are required.
- `FAIL` to `READY_FOR_COMMIT`: `FAIL` must first produce a valid correction path and later a valid `PASS` for the exact corrected implementation.
- `FIX_REQUIRED` to `PASS`: the correction owner must complete the correction and hand off a valid exact review package before Review restarts.
- `INVALID_REVIEW_INPUT` to `PASS`: the defective input must be corrected, a new `READY_FOR_REVIEW` handoff must be established, and Review must restart.
- `PARTIALLY_IMPLEMENTED` to `PASS` or `READY_FOR_COMMIT`: required implementation work, verification, handoff, and Review cannot be skipped.
- `IMPLEMENTED` to `PASS` or `READY_FOR_COMMIT`: `IMPLEMENTED` does not establish review-input completeness or Review success.
- `READY_FOR_REVIEW` to `READY_FOR_COMMIT`: exact-input Review `PASS` and all remaining completion checks are required.
- `PASS` to commit execution: `PASS` permits completion evaluation only; `READY_FOR_COMMIT` and separate commit authorization are still required.
- any active blocking status to `READY`, `IMPLEMENTED`, `READY_FOR_REVIEW`, `PASS`, or `READY_FOR_COMMIT` without verified resolution, required replacement, and revalidation;
- `TEST_INFRASTRUCTURE_BLOCKED` to a successful status without a new valid execution evidence item;
- `REQUIREMENT_NOT_TESTABLE` to `NO_TEST_CHANGES_REQUIRED` without an approved applicability basis or authoritative change;
- a domain routing status to a consumer outcome without a valid resolved Routing Result and available approved authority;
- `HUMAN_APPROVAL_REQUIRED` to a success status based on silence, informal conversation, or an unbound approval;
- any status on one `implementation_id`, evidence run, review attempt, Task, repository, baseline, or worktree to a success claim for a different identity without the required replacement and re-evaluation path;
- any path that uses Task State, package assembly, an Implementation Report, criterion mapping, or a human approval as a substitute for required evidence or Review.

When a path is prohibited, the current status remains active or is refined to the most specific truthful status. The pipeline MUST NOT fabricate an intermediate success status merely to make the graph appear connected.

## 13. Invalidation and Replacement Contract

### 13.1 Selective Invalidation

Invalidation MUST be evaluated against the exact authority, identity, target, context, scope, mapping, or prerequisite that changed. The active use of an artifact or claim is invalidated only when the change is material to that use.

At minimum:

- a requirement, scope, acceptance-criterion, fixed-decision, or dependency change invalidates the affected Task Contract assertions and every downstream artifact that relies on those assertions;
- a routing concern, consumer, Skill inventory, authority revision, or routing-conflict change invalidates the affected Routing Result entries and dependent work;
- a relevant repository, baseline, worktree, inspection, or limitation change invalidates affected Repository Context findings and planning assumptions;
- a material Plan change invalidates affected HLTP obligations, implementation work, evidence mappings, review inputs, and completion claims;
- a material HLTP semantic change invalidates affected test implementation, evidence, acceptance mappings, review results, and readiness claims;
- an implementation change invalidates implementation-bound evidence, review results, and completion claims for the old implementation;
- an evidence defect or stale applicability invalidates only the intended uses that require that evidence;
- a review-input or reviewed-identity change invalidates review readiness and active applicability of the prior Review Result;
- a status-basis or resume-reference defect invalidates the affected Task State claim and package selection.

Invalidation MUST NOT erase independent valid work, silently expand to unrelated artifacts, or be used as permission to edit an artifact owned by another role.

### 13.2 Frozen Historical Instances

A frozen artifact remains immutable after invalidation or supersession. Its historical meaning, identity, provenance, and relationships are preserved for audit, lineage, interpretation of prior evidence, and correct resumption.

Invalid, stale, or superseded does not mean deleted. Such an instance MUST NOT remain selected as active authority for a use it no longer supports, but it MAY remain referenced as predecessor or historical evidence of what occurred.

A status change does not rewrite the status record it supersedes. A correction does not rewrite a prior evidence item or Review Result. A replacement does not reuse the predecessor's identity.

### 13.3 Replacement Identities

A material post-freeze correction requires a distinguishable replacement artifact identity. The replacement MUST preserve predecessor lineage when necessary to prevent mixing and MUST carry its own provenance, freeze state, relationships, and consumer-visible identity.

Replacement is required when meaning, scope, binding identity, authority, or review-relevant content changes materially. Cosmetic formatting that does not alter interpretation MAY remain within the owning role's pre-freeze editing lifecycle, but MUST NOT be used to disguise a material change.

A consumer MAY report a conflict or invalidity but MUST NOT produce a replacement for an artifact outside the consumer's ownership. The owning role decides and produces the replacement, subject to higher source and human authority.

### 13.4 Implementation, Evidence, and Review Identity

Every changed implementation receives a distinguishable `implementation_id`. Evidence evaluating implementation is valid only for the exact implementation and target identities recorded by the evidence. A rerun creates a new `evidence_id` even when command text and result are unchanged.

Every substantive Review Result binds to one exact `implementation_id` and exact reviewed-input set. A new implementation, corrected review package, rerun needed for reverification, or new review attempt MUST NOT inherit prior `PASS` or relabel prior evidence.

Evidence separately bound to an unchanged immutable target MAY remain applicable only when the intended use and explicit independence from the changed implementation are established. Reuse by assumption is prohibited.

## 14. Resume Contract

### 14.1 Resume Permission

Resume permission is status-specific and conditional. Work MUST NOT resume merely because a blocker is believed to be resolved, time has passed, a conversation restarted, or an artifact exists.

Resume is permitted only when:

- the active status allows resume;
- the durable blocker, finding, human gate, or unavailable input is resolved or truthfully refined;
- required replacement artifacts are frozen and selected;
- repository, baseline or worktree, implementation, evidence, and review identities are coherent;
- selective invalidation has been applied;
- the destination stage has the artifacts and authority required by its contract.

A status that prohibits resume remains active until those conditions are demonstrated.

### 14.2 Resume Target

The resume target is the earliest approved role or stage whose governed work must be performed after resolution. If an upstream artifact changes, resume MUST return to that artifact's owner before any affected downstream handoff is reused.

If no upstream authority changed, resume MAY return to the stage that was blocked. Review resumes only through a new valid `READY_FOR_REVIEW` handoff and a new review attempt. Completion evaluation resumes only after valid `PASS` and all other completion prerequisites remain valid.

DP-Task-Orchestrator or a Manual Pipeline human state manager MAY route to the recorded resume target but MUST NOT perform the target role's governed work.

### 14.3 Durable Resumption Basis

The durable basis for resumption MUST include, as applicable:

- `task_id` and the current Task Contract;
- the active package inventory and exact artifact references;
- repository, baseline and worktree identity;
- current `implementation_id` and Implementation Report;
- valid Acceptance Criteria Evidence and Build/Test Evidence;
- the active Task Review Result;
- the current blocker, finding, pending action, responsible stage, and resume target;
- approved human, Design, requirement, routing, dependency, or infrastructure decisions;
- predecessor lineage for replaced artifacts;
- the last durable Task State update.

A local filename, heading, text match, mutable path, or conversation reference alone is not a sufficient resume locator for critical content.

### 14.4 Resume Invariant Boundary

This contract defines whether and where resume is semantically permitted and which durable artifacts are required. It does not define the future Resume Invariant's recovery algorithm, session reconstruction procedure, compaction method, or automated verification behavior.

Correct continuation MUST be possible from Task State, artifacts, repository state, and evidence identities. Conversation history, hidden context, and remembered reasoning MUST NOT be required.

## 15. Task State Integration

### 15.1 Status Location

Task State is the canonical durable location for the current status reference and orchestration position of one Task. It records the active status together with responsible stage, pending action, blocker when present, resume target, and last durable update.

The active status value MUST be one of the 23 canonical statuses in Section 8. Workflow positions remain structurally separate from status. A Manual Pipeline record MUST preserve the same separation.

### 15.2 Active References and Identities

Task State MUST locate the active artifact instances and identities needed to interpret the status, including repository and baseline or worktree identity, current implementation when one exists, active evidence, active Review Result, blocker, and predecessor lineage as applicable.

Task State references MUST resolve to the exact intended instances. Mixed Task, repository, baseline, worktree, implementation, evidence, or review identities make the affected state invalid. Package selection MUST NOT conceal such a mismatch.

### 15.3 Non-Evidence and Non-Authority Rules

Task State is not evidence and does not prove implementation, verification, Review success, blocker resolution, human approval, or Task completion. Task State MUST NOT:

- copy another artifact's complete semantics as a substitute for reference;
- change another artifact's meaning, validity, freeze, ownership, or authority;
- grant mutation authority to a state manager or consumer;
- make an unevidenced readiness or completion claim authoritative;
- define transition mechanics or execute the next action.

The status recorded in Task State is valid only to the extent that its referenced authoritative basis is valid.

### 15.4 State Lineage and Supersession

Task State MUST NOT self-reference. A successor state uses a distinguishable Task State artifact identity and predecessor reference when required by the Schema.

The prior state freezes when superseded or when the Task reaches an externally defined terminal condition. Supersession changes the active state selection without rewriting the historical state. A broken active reference, mixed identity, unsupported workflow position, or inconsistency between status and resume fields invalidates the affected Task State.

## 16. Evidence and Review Integration

### 16.1 Execution-State Distinctions

The following conditions are semantically distinct and MUST NOT be collapsed:

- **not yet run:** no execution began and no execution-result evidence exists;
- **blocked before execution:** execution did not begin because a durable prerequisite or infrastructure condition prevented it;
- **executed failure:** execution occurred and produced accurate evidence of failure;
- **partial execution:** execution occurred but did not cover the complete required scope;
- **successful execution:** execution occurred and valid evidence establishes success for the exact obligation and scope.

Zero exit code does not override no tests found, required skips, filtering, partial execution, timeout, abort, or contradictory output.

### 16.2 Evidence Validity and Freshness

Evidence is valid only for an intended use when its identity and references resolve, the represented execution or inspection occurred, the target and repository or implementation associations are exact, producer and material context are accurate, result and output are coherent, required mappings exist, proof strength is sufficient, and no material invalidating change or conflict exists.

Freshness is applicability to the current target, context, scope, mappings, and intended use, not a fixed age. Stale evidence remains historical but MUST NOT serve as active proof. Freeze does not guarantee continuing validity.

Multiple valid identity-compatible evidence items MAY jointly support one obligation. Aggregation MUST NOT repair invalid evidence, bridge implementations, turn traceability into proof, create Review `PASS`, or establish completion.

Evidence is bounded by the actual action or observation, immutable target, executed or inspected scope, context, output, and mappings. Evidence MUST NOT extrapolate from a focused command, selected test, component, file set, inspection scope, or repository slice to full-suite, whole-repository, or untested-behavior success. Any broader claim requires evidence whose executed or inspected scope positively covers that broader claim.

### 16.3 Review-Input Integrity

Before substantive Review, DP-Task-Reviewer MUST establish that the required package is available, references resolve, identities are coherent, reviewed artifacts are the intended active instances, evidence is valid for the claimed use, and the exact implementation is identifiable.

A missing, invalid, stale, unresolvable, or identity-mismatched required input produces `INVALID_REVIEW_INPUT` at Task-status level and a structurally blocked Task Review Result. Reviewer MUST NOT manufacture a code defect, edit the input, repair mappings, or continue substantive checks on an invalid set.

### 16.4 Review Outcome Binding

`PASS` and `FAIL` are Reviewer-owned substantive outcomes for one exact implementation and reviewed-input set. `FAIL` records the first failed ordered check, finding, required correction, required reverification, applicable evidence, and checks passed before failure. `PASS` records the complete ordered set of review checks that passed.

A Task Review Result is not implementation evidence and does not alter the reviewed artifacts. `PASS` does not by itself establish `READY_FOR_COMMIT`. A blocked result identifies the first blocked check and `BlockingRef` without fabricating correction.

### 16.5 Correction, Reverification, and New Review Attempts

Correction is performed by the authority identified by the finding. A corrected implementation receives a new `implementation_id`; material evidence correction requires rerun and a new `evidence_id`; corrected upstream artifacts receive replacement identities.

Required reverification MUST evaluate the corrected exact target. After valid correction and handoff, Reviewer creates a new Task Review Result and restarts at the first ordered review check. The prior result remains frozen historical material and MUST NOT be edited into `PASS`.

Reviewer-side execution creates new Reviewer evidence. It does not edit Implementer evidence, the implementation, or the prior Review Result.

## 17. Canonical Artifact Compatibility

### 17.1 Task Contract

The Task Contract is Planner-owned authority for Task identity, objective, bounded scope, requirements, acceptance criteria, dependencies, fixed decisions, and readiness basis. Status semantics MUST NOT add requirements or correct the Contract by implication.

`TASK_REQUIREMENTS_INCOMPLETE`, `TASK_SPLIT_REQUIRED`, `DEPENDENCY_NOT_READY`, and applicable Design statuses MUST carry durable blocking references. A material source, scope, requirement, criterion, or dependency change returns to DP-Task-Planner and may require human source authority. Downstream artifacts based on invalid Task meaning MUST NOT remain active.

### 17.2 Domain Skill Routing Result

The Routing Result is owned by its producing routing role and binds exact concerns to applicability, consumers, constraints, selected Skills or authoritative lookups, provenance, and blockers.

`DOMAIN_ROUTING_AMBIGUOUS`, `DOMAIN_SKILL_NOT_AVAILABLE`, and `DOMAIN_CONCERN_UNSUPPORTED` MUST preserve the exact concern and consumer mapping. Consumers MUST NOT select, substitute, or invent authority locally. Changed concern, consumer, Skill inventory, authority revision, or conflict requires rerouting and a distinguishable result.

### 17.3 Task Repository Context

Task Repository Context is Planner-owned repository evidence bound to exact repository and baseline or worktree identity and to the inspected scope and limits. It does not claim truth beyond inspection.

A relevant repository change, identity mismatch, newly discovered fact, or changed limitation may invalidate affected findings, Plan assumptions, and status bases. Refresh and replacement are required when meaning changes. Status semantics MUST NOT convert an uninspected assumption into repository truth.

### 17.4 Task Execution Plan

The Execution Plan is Planner-owned authority for ordered work, boundaries, dependencies, change-area guidance, verification obligations, and handoff expectations. It is frozen planning authority, not repository truth or execution evidence.

`PLAN_UPDATE_REQUIRED` returns ownership to DP-Task-Planner. A change outside an expected area is not automatically a violation when directly required, in scope, Design-preserving, not future work, and reported. Protected or excluded changes require escalation or an approved replacement Plan. A material Plan change selectively invalidates dependent HLTP, implementation, evidence, review, and completion claims.

### 17.5 Task-Local HLTP

Task-local HLTP is Test Designer-owned semantic authority for what must be proven, scenarios, preconditions, stimulus, expected behavior, negative, boundary and lifecycle obligations, mappings, and approved deferred coverage.

After Test Designer `READY`, HLTP meaning freezes. DP-Task-Implementer MAY choose implementation mechanics but MUST NOT silently change obligations, expected behavior, mappings, success conditions, mandatory coverage, or approved deferral. Semantic change returns to DP-Task-Test-Designer and, when needed, Design or requirement authority.

`NO_TEST_CHANGES_REQUIRED` requires a durable applicability basis and a not-applicable inventory entry; it is not an empty HLTP. `REQUIREMENT_NOT_TESTABLE` and `TEST_INFRASTRUCTURE_BLOCKED` MUST preserve their semantic distinction.

### 17.6 Implementation Report

The Implementation Report is Implementer-owned factual declaration for one exact implementation attempt, including attempted and implemented scope, actual changes, changed areas, unresolved items, execution references, and blockers.

A blocked or partial attempt MAY produce a Report. The Report is not independent proof, acceptance, Review `PASS`, or authority to change upstream artifacts. Material correction produces a distinguishable Report and implementation identity rather than rewriting historical execution truth.

### 17.7 Acceptance Criteria Evidence

Acceptance Criteria Evidence is Implementer-assembled traceability from criteria to supporting evidence, unresolved basis, blocker, or not-yet-evidenced state. Complete mapping is required, but mapping alone does not validate evidence or determine satisfaction.

Reviewer MUST NOT edit the mapping to obtain `PASS`. A criterion marked evidenced or satisfied MUST reference valid applicable evidence. Zero-evidence criteria require a durable unresolved, blocked, not-applicable, or not-yet-evidenced basis allowed by governing authority.

### 17.8 Build/Test Evidence

Build/Test Evidence is the canonical execution-evidence artifact. Each item requires a unique `evidence_id`, actual producer, precise action or command, exact target identity, actual result, relevant output or durable output reference, material context, obligation mappings, and execution time as applicable.

Plans, recommendations, prepared commands, expected actions, Implementation Reports, Task State, package assembly, and traceability alone are not execution evidence. A material defect, mismatch, corruption, insufficient output, relevant target change, or required reverification requires rerun and a new evidence identity. Prior evidence remains frozen.

### 17.9 Task Review Result

Task Review Result is Reviewer-owned authority for one review attempt. Substantive `PASS` or `FAIL` binds to exactly one implementation and exact reviewed inputs. Input-integrity blocking may omit implementation identity only when missing or invalid identity is the documented blocker.

A new review attempt creates a distinguishable result. Reviewer does not edit prior results or consumer artifacts. Status `INVALID_REVIEW_INPUT` remains the specific Task status while the structural Review Result is blocked.

### 17.10 Task State

Task State is operational state owned by DP-Task-Orchestrator when available, or maintained operationally by the Manual Pipeline human state manager before that Agent exists. It records status, workflow position, active references and identities, blocker, pending action, responsible stage, durable update, and resume target.

Task State MUST follow Section 15. It neither owns nor replaces the semantics of referenced artifacts, is not evidence, and cannot establish completion by assertion.

### 17.11 Task Work Package Assembly

The Task Work Package is the assembly and resume boundary for exactly the ten canonical artifact slots defined by the Schema. It is not an eleventh canonical artifact and this Status Model creates no additional slot.

Assembly selects active artifact instances and records applicability and availability without granting mutation authority. Conflicting instances, broken references, duplicate active selections, or identity mismatch produce a durable blocker. Package assembly MUST NOT present a conflicting instance as valid active authority or treat an applicable unavailable artifact as not applicable.

## 18. Manual Pipeline Compatibility

The Manual Pipeline MUST apply this contract without weakening or replacing any Agent authority. Manual selection of the next Agent, operational status recording, package assembly, or human approval does not change the meaning of a status and does not satisfy a missing artifact, evidence item, review result, blocker resolution, or identity.

A Manual Pipeline handoff MUST be reproducible from durable Task State and referenced artifacts. When DP-Task-Orchestrator becomes available, the same durable status semantics remain binding; only orchestration mechanics move from manual operation to the approved Orchestrator package.

## 19. Adversarial Interpretation Rules

The following interpretations are invalid even when they would shorten the pipeline or produce a superficially successful outcome.

### 19.1 Generic `BLOCKED` Misuse

A known specific cause MUST NOT be recorded as `BLOCKED`. Dependency readiness, Design ambiguity, Design-code conflict, Task splitting, domain routing, Plan replacement, testability, test infrastructure, review-input integrity, and human approval MUST use their specific statuses when applicable.

A `BLOCKED` record without a durable reason, resolution owner, pending action, and resume target is invalid. Refinement to a specific status preserves the historical `BLOCKED` record and blocker lineage.

### 19.2 Authority Confusion

An emitter, Task State owner, package assembler, consumer, DP-Task-Orchestrator, or Manual Pipeline human state manager MUST NOT acquire another role's authority by recording, routing, selecting, or consuming a status.

DP-Task-Orchestrator and the human state manager MUST NOT plan, write or change HLTP semantics, implement or fix code, create implementation evidence, perform substantive Review, approve on behalf of a named human authority, or replace an artifact owned by another role.

### 19.3 Identity Mixing

A status, evidence item, review result, readiness claim, blocker resolution, or completion result bound to one Task, repository, baseline, worktree, artifact revision, `implementation_id`, `evidence_id`, or review attempt MUST NOT support a different identity by filename similarity, text match, path reuse, presumed equivalence, or relabeling.

Any mixed-identity package or state is invalid for the affected use. The correct response is replacement, rerun, new handoff, or new Review as required, not reinterpretation of the old record.

### 19.4 No-Tests-Found False `PASS`

A zero exit code with no tests discovered, required tests skipped, filtered coverage, timeout, abort, incomplete execution, contradictory output, or wrong target is not semantic success.

Such evidence MUST be classified according to what actually occurred. `PASS`, `READY_FOR_REVIEW`, and `READY_FOR_COMMIT` MUST NOT rely on an execution claim that was not performed over the required target and scope.

### 19.5 Partial-Work Completion

`PARTIALLY_IMPLEMENTED`, a partially executed verification set, an unresolved criterion, or a blocked implementation MUST NOT be presented as complete implementation, review readiness, Review `PASS`, or commit readiness.

Completed partial work remains factual and reusable only within its valid identity and scope. Remaining work, blocker, required evidence, and resume target remain explicit.

### 19.6 Test Weakening

Technical difficulty, unavailable infrastructure, time pressure, implementation preference, or a desire to obtain `PASS` MUST NOT weaken an approved test obligation, expected behavior, success condition, mapping, mandatory scenario, boundary case, lifecycle obligation, or approved deferred-coverage rule.

A semantic testability problem uses `REQUIREMENT_NOT_TESTABLE`. An execution-mechanics or infrastructure problem uses `TEST_INFRASTRUCTURE_BLOCKED`. DP-Task-Implementer MAY choose mechanics but MUST NOT alter HLTP meaning.

### 19.7 Approval-Based Evidence Repair

Human approval, management preference, reviewer discretion, Task State, package assembly, an Implementation Report, criterion mapping, or a status assertion MUST NOT repair invalid evidence, create missing execution, bridge identities, overwrite a failed result, or convert an invalid review package into `PASS`.

Human approval is authoritative only for the exact decision within the named human authority's scope. Evidence and Review prerequisites remain independently required.

### 19.8 Readiness Regression

A readiness or success status loses active applicability when a material supporting artifact, implementation, evidence item, review input, approval, blocker basis, repository identity, or commit boundary changes or is disproven.

The pipeline MUST regress to the earliest affected authority or specific truthful status. A prior `READY`, `IMPLEMENTED`, `READY_FOR_REVIEW`, `PASS`, or `READY_FOR_COMMIT` record remains frozen historical material and MUST NOT remain active by inertia.

### 19.9 Split-Lineage Mixing

When `TASK_SPLIT_REQUIRED` is resolved, each replacement Task requires its own `task_id`, Task Contract, Task State, package inventory, dependencies, artifact identities, evidence mappings, and lineage.

Artifacts, implementation, evidence, review results, blockers, or completion claims from the unsplit Task MUST NOT be silently assigned to a replacement Task. Reuse requires explicit applicability, identity compatibility, and the owning authority's valid artifact for the new Task.

### 19.10 Unsupported-Domain Guessing

A consumer MUST NOT replace ambiguous routing, an unavailable selected Skill, or an unsupported concern with general knowledge, a guessed API, an invented constraint, or a locally selected alternative authority.

`DOMAIN_ROUTING_AMBIGUOUS`, `DOMAIN_SKILL_NOT_AVAILABLE`, or `DOMAIN_CONCERN_UNSUPPORTED` remains active until an approved routing role or human authority provides a durable valid basis for continuation.

### 19.11 Conversation-Dependent Resume

A Task MUST NOT resume from remembered chat context, hidden state, an unavailable prior conversation, or unstored reasoning. If Task State, artifact references, identities, blockers, evidence, or the resume target are insufficient, work remains blocked until the durable package is repaired.

### 19.12 Task State as Completion Proof

Task State records active operational state but is not evidence and is not completion authority. A Task State entry saying `PASS` or `READY_FOR_COMMIT` is invalid unless the required exact evidence, Review Result, approvals, artifact validity, and completion prerequisites independently support that status.

## 20. Final Invariants

The following invariants MUST remain true for every Task and every status record:

1. **Canonical vocabulary:** The active status is exactly one of the 23 statuses in Section 8. No workflow position, evidence result, package availability value, milestone, or invented token becomes an additional status.
2. **Complete definition:** Every canonical status retains the nine dimensions defined in Section 9 and is interpreted together with the shared rules in Sections 6 and 7.
3. **Specificity:** The most specific truthful status is selected. `BLOCKED` is used only when no specific canonical status applies.
4. **Bounded emission:** Every status is emitted by an approved role within that role's governed work or operationally recorded from an already authoritative basis.
5. **Resolution authority:** The resolution owner is determined by the authority needed to remove the condition. Emission, state ownership, package assembly, or orchestration does not transfer that authority.
6. **No automatic transition:** An allowed successor never triggers work or proves its prerequisites. Every successor is independently established by the proper authority.
7. **No bypass:** Blocker resolution, correction, replacement, reverification, Review restart, evidence validity, human gates, and completion checks cannot be skipped by a graph edge or status assertion.
8. **Selective invalidation:** Only materially affected active uses are invalidated. Independent valid work is preserved.
9. **Frozen history:** Frozen, invalidated, stale, failed, blocked, superseded, and replaced instances remain immutable historical records. They are not rewritten into current success.
10. **Distinct replacement:** Material correction creates distinguishable artifact, implementation, evidence, state, or review identities as required, with predecessor lineage where needed.
11. **Identity coherence:** Task, repository, baseline or worktree, artifact revision, implementation, evidence, review attempt, blocker, and human decision identities remain mutually coherent for the claimed use.
12. **Evidence truth:** Execution and inspection claims reflect what actually occurred. Not run, blocked before execution, partial execution, failure, and success remain distinct.
13. **Evidence bounds:** Evidence proves no more than its actual target, context, scope, mappings, output, and intended use. Aggregation does not repair invalidity or cross identity boundaries.
14. **Review independence:** DP-Task-Reviewer consumes reviewed inputs read-only, stops at the first failed or blocked ordered check, and creates a new immutable result for every review attempt.
15. **Review-result binding:** `PASS` and `FAIL` bind to one exact implementation and input set. Correction and rerun do not alter prior evidence or Review Results.
16. **Completion discipline:** `PASS` alone is not `READY_FOR_COMMIT`; `READY_FOR_COMMIT` is not commit execution; Task completion applies only to the current Task and preserves known downstream-contract integrity.
17. **Task State boundary:** Task State records and locates state but is not evidence, does not redefine an artifact, does not grant authority, and does not establish completion by assertion.
18. **Package boundary:** The Task Work Package contains exactly the ten canonical artifact slots. Assembly is not an eleventh artifact and creates no new authority.
19. **Manual Pipeline equivalence:** Before DP-Task-Orchestrator exists, the human state manager applies the same meanings, prerequisites, identities, evidence rules, and authority boundaries without becoming a sixth Agent.
20. **Durable resume:** Correct continuation is reconstructible from Task State, active artifacts, repository state, identities, blockers, evidence, review results, and durable decisions without conversation history.
21. **Foundation boundaries:** This contract creates no Orchestrator state machine, additional Agent, Skill, service, permission system, evidence engine, recovery engine, Resume Agent, or new canonical artifact and does not redefine another Foundation contract.
22. **Commit boundary:** No status performs a commit. Commit execution requires separate explicit authorization outside this contract.
23. **Truth over progress:** When required authority or evidence is unavailable, the Task remains in the truthful specific status rather than fabricating readiness, success, or completion.

## 21. Cross-Authority Compatibility Summary

This section consolidates compatibility obligations that govern use of the Status Model. It does not copy the Test Pack, create new authority, or replace detailed rules in the Foundation contracts.

### 21.1 Task Formation and Current-Task Completion

A Task must be bounded, buildable, testable, explicitly scoped, dependency-aware, acceptance-criterion driven, free of unresolved Design decisions, and coherent enough for one commit-scale unit. Completion applies only to the current Task, preserves known downstream-contract integrity, and does not establish readiness for a future Task.

### 21.2 Approved Roles and Vocabulary

The only Agent roles are DP-Task-Planner, DP-Task-Test-Designer, DP-Task-Implementer, DP-Task-Reviewer, and DP-Task-Orchestrator. Each role uses only its approved outcome vocabulary. `RECEIVED`, `PLANNING`, `TEST_DESIGN`, `IMPLEMENTATION`, `REVIEW`, `FIX`, and `COMPLETION` are workflow positions, not additional statuses.

### 21.3 HLTP Authority and Test Mechanics

Task-local HLTP owns proof semantics, scenarios, preconditions, stimulus, expected behavior, negative, boundary, lifecycle, mapping, success, mandatory-coverage, and deferred-coverage obligations. After Test Designer `READY`, those semantics freeze. Implementer may select mechanics but cannot weaken or silently change frozen test meaning. A no-test-change outcome requires a durable applicability basis rather than an empty HLTP.

### 21.4 Change-Area Discipline

Required change areas identify work directly required by the Task. Expected affected areas are repository-grounded guidance rather than a rigid allowlist. Protected or excluded areas require escalation or approved Plan replacement. A change outside expected areas is acceptable only when acceptance-criterion required, in scope, responsibility-consistent, Design-preserving, not future work, and reported in the Implementation Report.

### 21.5 Review Result Content and Order

Reviewer operates independently in fresh context. Substantive Review uses a deterministic ordered list and stops at the first failed or blocked check. `FAIL` records the first failing check, finding, applicable evidence, required correction, required reverification, and only checks completed before failure. A blocked review records the first blocked check, finding, and blocking reference without fabricating correction. `PASS` records the complete ordered set of passed checks. Every corrected attempt restarts at the first review check and produces a distinguishable result.

### 21.6 Commit Readiness

`READY_FOR_COMMIT` requires Planner `READY`, applicable test obligations handled, Implementer `READY_FOR_REVIEW`, Reviewer `PASS`, all required builds and tests passing, evidence bound to the correct repository and implementation, no open deviation, no unrelated change, a coherent commit boundary, and no broken known downstream contract. Orchestrator does not plan, define HLTP, modify code, perform Review, fix findings, change Design, or commit without explicit instruction.

### 21.7 Schema Preservation

The Status Model preserves Schema meanings and cardinalities for required, conditionally required, and optional fields. Every canonical artifact retains its common header and identity semantics. The package has exactly ten canonical artifact slots. Applicability, availability, blocking, and status remain distinct, including applicable-and-available, applicable-but-not-yet-available, not-applicable, and applicable-but-blocked inventory states.

### 21.8 Durable References and Routing

Every material reference identifies target type, target identity, relationship meaning, a locator when identity alone cannot retrieve the target, and revision when revision affects meaning. Critical linkage cannot rely only on filename, heading, text match, local path, or conversation proximity. Routing preserves concern identity, applicability, consumer mappings, constraints, selected Skill or authority, provenance, and blocker. Unresolved routing conflict never becomes a silent consumer choice.

### 21.9 Artifact-Specific Partial and Review Semantics

A blocked or partial Implementation Report may exist and preserves implementation, repository, baseline, worktree, attempted and completed scope, actual changes, unresolved items, and evidence or blocker references. The Report is not proof, acceptance, Review `PASS`, or upstream authority. Substantive Review Results bind to one implementation and exact inputs. Input-integrity blocking may omit implementation identity only when that missing or invalid identity is the documented blocker.

### 21.10 Identity, Lineage, and State

Task, artifact, repository, baseline, worktree, implementation, evidence, requirement, criterion, and Review Result identities cannot be silently mixed. Replacement artifacts use distinguishable identities and predecessor lineage when required. Task State records workflow or status reference, active artifacts, repository and baseline or worktree, implementation, evidence, Review Result, blocker, pending action, responsible stage, durable update, and resume target. Task State never self-references and freezes when superseded or when an external terminal condition is reached.

### 21.11 Authority Ownership

Provenance does not grant ownership. No artifact proves, approves, or completes itself. Derived artifacts cannot override approved sources, and package selection or Task State reference does not grant mutation authority. Planner owns Contract, Repository Context, and Plan; the producing routing role owns routing; Test Designer owns test semantics; Implementer owns the Report and criterion mapping; the executing role owns evidence; Reviewer owns Review Results; Orchestrator owns Task State when available. In the Manual Pipeline, the human state manager owns only operational state and assembly.

### 21.12 Evidence Identity and Execution Truth

Build/Test Evidence remains the only canonical execution-evidence artifact. Each item has a unique evidence identity, actual producer, precise executed action, exact target, actual result, relevant output or resolvable output reference, material context, obligation mappings, and execution time. Plans, recommendations, prepared commands, expected actions, mappings, reports, state, and assembly are not completed execution evidence.

### 21.13 Result Interpretation and Output Integrity

Exit code does not replace result or output. Zero exit code cannot override no tests found, required skips, filtering, partial execution, work not run, timeout, or abort. Required raw or detail references must resolve to the correct run and target. A broken required output reference invalidates the intended use and material repair requires rerun with a new evidence identity.

### 21.14 Evidence Categories, Validity, and Freshness

Execution evidence, inspection evidence, traceability evidence, Reviewer evidence, and unsupported declarative claims remain distinct evidence categories. Traceability proves linkage, not satisfaction. Evidence validity is use-specific and requires resolvable identities, actual occurrence, exact target associations, accurate producer and context, coherent result, output, time and mappings, sufficient proof strength, and no material invalidating conflict. Freshness means continued applicability, not a fixed age. Stale evidence is frozen history, not active proof.

### 21.15 Rerun and Aggregation

A relevant target, implementation, or context change, wrong, corrupted, or materially misreported evidence, insufficient required output that prevents verification, or required reverification triggers rerun. Every rerun creates a new evidence identity and preserves prior evidence historically. Failed execution is valid evidence of failure but not successful proof and differs from work never started. Multiple valid identity-compatible items may jointly support a criterion, but aggregation cannot repair invalid evidence, bridge implementations, create Review `PASS`, or establish completion.

### 21.16 Boundary Integrity

This Status Model creates no additional Agent, Skill, canonical artifact, execution-evidence artifact, evidence ownership model, database, store, query service, logging collector, uploader, transport, retention framework, raw-log repository, validation daemon, execution infrastructure, automated trust service, ACL, permission or locking system, general revision framework, scheduler, workflow engine, recovery procedure, Resume procedure, or alternate Foundation responsibility.

The Task Status Model is a logical Foundation contract, not an Agent or Skill. It creates neither a complex artifact-permission system nor complex revision infrastructure.
