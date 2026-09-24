# Task Package Schema

**Contract:** `task-package-schema`  
**Canonical path:** `.github/task-agent-system/foundation/task-package-schema.md`  
**Schema version:** `1.0`

## 1. Purpose and scope

This Foundation contract defines the structural schema of a **Task Work Package** and the ten canonical artifacts exchanged among the approved Task-Agent System roles. A Task is a bounded unit of work intended to be small enough for an independent Copilot session and coherent enough to form one commit, whether or not a commit is ultimately created.

The approved Task-Agent System work plan is the primary authority for this contract. Approved Design, Component Implementation Plan, Component or Interface HLTP, repository evidence, and dedicated Foundation contracts are complementary authorities within their scopes.

This contract defines artifact shape, identity, references, applicability, availability, handoff relationships, and resume-relevant data. It does not define Agent procedures, complete authority or mutability rules, complete status semantics or transitions, detailed evidence validation, or recovery algorithms. Those concerns belong respectively to the conceptual **Task Authority Model**, **Task Status Model**, **Minimum Evidence Model**, and **Resume Invariant** contracts.

Conversation history, hidden model state, and unapproved drafts are not sources of truth.

## 2. Approved roles and lifecycle stages

The only Agent roles referenced by this schema are:

- `DP-Task-Planner`
- `DP-Task-Test-Designer`
- `DP-Task-Implementer`
- `DP-Task-Reviewer`
- `DP-Task-Orchestrator`

Producing stages in this document locate artifacts in the approved lifecycle. They do not prescribe an Agent workflow or grant authority to modify an artifact.

## 3. Normative conventions

The key words **MUST**, **MUST NOT**, **REQUIRED**, **SHOULD**, **SHOULD NOT**, and **MAY** are normative.

Requirement levels are:

- **Required:** the field MUST exist in every instance of that artifact.
- **Conditionally required:** the field MUST exist when its stated condition is true.
- **Optional:** the field MAY be omitted without concealing unknown, blocked, unavailable, or resume-critical information.

Cardinalities are written as `1`, `0..1`, `1..*`, or `0..*`.

A schema field named `*_ref` contains a stable typed reference, not copied source content. A field named `*_refs` is a collection of such references. Critical linkage MUST NOT depend only on a file name, heading, text matching, or conversational proximity.

## 4. Common structural types

### 4.1 StableRef

A `StableRef` identifies a target without defining a general revision system.

| Field | Requirement | Shape | Meaning |
|---|---|---|---|
| `target_type` | Required | string | Canonical artifact or external entity type. |
| `target_id` | Required | string | Stable identity within the target type's declared uniqueness scope. |
| `locator` | Conditionally required | string | Durable location when identity alone cannot retrieve the target. |
| `revision` | Conditionally required | string | Baseline, digest, commit, implementation, run, or other immutable locator when target revision matters. |
| `relationship` | Required | string | Meaning of the reference from the containing field. |

### 4.2 SourceRef

A `SourceRef` is a `StableRef` whose target is an approved Design, Component Implementation Plan, Component or Interface HLTP, approved decision, Foundation contract, Domain Skill, or repository evidence. It MUST identify the source kind and revision when revision affects meaning.

### 4.3 BlockingRef

A `BlockingRef` is a stable reference to documented blocking information. It identifies the blocker and its authoritative record but does not define the blocker's status semantics, owner behavior, or transition rules.

### 4.4 LocationRef

A `LocationRef` identifies repository or evidence content through repository identity plus an applicable baseline, worktree, implementation, run, path, symbol, or raw-output locator. A local path alone is insufficient when it could refer to different repository states.

### 4.5 Common artifact header

Every canonical artifact instance MUST contain:

| Field | Requirement | Shape | Meaning |
|---|---|---|---|
| `schema_ref` | Required | `StableRef`, cardinality `1` | This schema and the version governing the instance. |
| `artifact_type` | Required | string, cardinality `1` | One canonical artifact type from Section 6. |
| `artifact_id` | Required | string, cardinality `1` | Stable artifact-instance identity, unique within its `task_id` and `artifact_type`. |
| `task_id` | Required | string, cardinality `1` | Stable identity of the Task. |
| `producing_role_or_source` | Required | role/source reference, cardinality `1` | Role or non-Agent source that produced this artifact instance. This is provenance, not a complete authority rule. |
| `predecessor_artifact_ref` | Conditionally required | `StableRef`, cardinality `0..1` | Previous or replaced instance when lineage is needed to avoid mixing revisions or to resume correctly. |
| `source_refs` | Required | `SourceRef[]`, cardinality `1..*` | Authorities and evidence from which the artifact was constructed. |
| `created_at` | Required | timestamp, cardinality `1` | Durable creation time for identifying the instance. |
| `updated_at` | Required | timestamp, cardinality `1` | Durable last-content-update time. |

`artifact_id` is not a display title. A new materially changed artifact instance MUST be distinguishable from its predecessor. Full freeze, validity, invalidation, and supersession semantics are outside this contract.

### 4.6 Canonical identities

The following names are used consistently:

- `task_id`: Task identity.
- `artifact_id`: artifact-instance identity.
- `repository_id`: repository identity.
- `baseline_id`: observed or approved repository baseline identity.
- `worktree_id`: working-tree identity.
- `implementation_id`: identity of a specific implementation content state or attempt.
- `evidence_id`: identity of one evidence execution or evidence item.
- `requirement_id`: canonical Task requirement identity.
- `acceptance_criterion_id`: canonical acceptance-criterion identity.
- `review_result_id`: Task Review Result identity, equal to that artifact's `artifact_id`.

Different executions of the same command MUST have different `evidence_id` values. Evidence, implementation, repository observations, and reviews MUST carry the additional identities needed to prevent silent mixing.

### 4.7 Structured-field content contracts

A field whose shape is `object` or `object[]` is not an unstructured extension point. The following member contracts are part of the field schema. Unless a member below has an explicit conditional rule, it is required within each containing object. References use `StableRef`, `SourceRef`, `BlockingRef`, or `LocationRef` as named, and retain the containing artifact's `task_id` and applicable repository or implementation identity.

- `Task Contract.scope_boundaries`: `in_scope` (`string[]`, `1..*`) identifies included responsibilities or outcomes; `boundary_refs` (`SourceRef[]`, `1..*`) identifies their approved sources. Exclusions remain in `non_goals` and are not duplicated here.
- `Task Contract.requirements[]`: `requirement_id` (`string`, `1`), `statement` (`string`, `1`), and `source_ref` (`SourceRef`, `1`).
- `Task Contract.acceptance_criteria[]`: `acceptance_criterion_id` (`string`, `1`), `requirement_refs` (`StableRef[]`, `1..*`), `statement` (`string`, `1`), and `source_ref` (`SourceRef`, `1`).
- `Task Repository Context` referenced-observation entries: `observation_id` (`string`, `1`), `observation` (`string`, `1`), and `location_refs` (`LocationRef[]`, `1..*`). Entries in `similar_implementations` additionally require `applicability_limits` (`string[]`, `0..*`).
- `Task Repository Context.domain_concerns[]`: `concern_id` (`string`, `1`), `concern_ref` (`StableRef`, `1`), and `routing_entry_ref` (`StableRef`, `1`).
- `Task Repository Context.integration_prerequisites[]`: `prerequisite_id` (`string`, `1`), `statement` (`string`, `1`), and `basis_refs` (`StableRef[]`, `1..*`).
- `Task Execution Plan.requirement_change_map[]`: `requirement_ref` (`StableRef`, `1`), `action_refs` (`StableRef[]`, `1..*`), and `change_area_refs` (`LocationRef[]`, `1..*`).
- `Task Execution Plan.actions[]`: `action_id` (`string`, `1`), `sequence` (`integer`, `1`), `required_change` (`string`, `1`), `dependency_refs` (`StableRef[]`, `0..*`), and `verification` (`object[]`, `1..*`). Each verification entry requires `verification_id` (`string`, `1`), `obligation` (`string`, `1`), and `mapping_refs` (`StableRef[]`, `1..*`) targeting requirements, acceptance criteria, or Task-local test cases as applicable.
- `Task Execution Plan.dependencies[]`: `dependency_ref` (`StableRef`, `1`) and `affected_action_refs` (`StableRef[]`, `1..*`).
- `Task Execution Plan.risks[]`: `risk_id` (`string`, `1`), `description` (`string`, `1`), `affected_action_refs` (`StableRef[]`, `1..*`), and `mitigation_or_blocking_ref` (`StableRef`, `1`).
- `Task Execution Plan.diff_scope_boundary`: `included_area_refs` (`LocationRef[]`, `1..*`) and `excluded_area_refs` (`LocationRef[]`, `0..*`). These members summarize, rather than replace, the three dedicated change-area fields.
- `Task-local HLTP.test_scope`: `included_behaviors` (`string[]`, `1..*`) and `excluded_behaviors` (`string[]`, `0..*`).
- `Task-local HLTP.requirement_criterion_map[]`: `test_case_ref` (`StableRef`, `1`), `requirement_refs` (`StableRef[]`, `0..*`), and `acceptance_criterion_refs` (`StableRef[]`, `0..*`), with at least one requirement or criterion reference per entry.
- `Task-local HLTP.test_cases[]`: in addition to the separately listed fields, `test_case_id` (`string`, `1`) is required.
- `Task-local HLTP.deferred_coverage[]`: `obligation_ref` (`StableRef`, `1`), `reason` (`string`, `1`), and `authority_ref` (`SourceRef`, `1`).
- `Task-local HLTP.completion_criteria[]`: `completion_criterion_id` (`string`, `1`), `statement` (`string`, `1`), and `obligation_refs` (`StableRef[]`, `1..*`).
- `Implementation Report.implemented_scope`: `attempted_scope_refs` (`StableRef[]`, `1..*`) and `implemented_scope_refs` (`StableRef[]`, `0..*`).
- `Implementation Report.plan_action_mappings[]`: `action_ref` (`StableRef`, `1`) and `change_refs` (`LocationRef[]`, `1..*`).
- `Implementation Report.acceptance_criterion_mappings[]`: `acceptance_criterion_ref` (`StableRef`, `1`) and `change_refs` (`LocationRef[]`, `1..*`).
- `Implementation Report.deviations[]`: `deviation_id` (`string`, `1`), `obligation_or_plan_ref` (`StableRef`, `1`), `description` (`string`, `1`), and `authority_or_blocking_ref` (`StableRef`, `1`).
- `Implementation Report.changes_outside_expected_areas[]`: `change_ref` (`LocationRef`, `1`), `reason` (`string`, `1`), and `acceptance_criterion_refs` (`StableRef[]`, `1..*`).
- `Implementation Report.discovered_dependencies[]`: `dependency_ref` (`StableRef`, `1`), `affected_action_refs` (`StableRef[]`, `1..*`), and `blocking_ref` (`BlockingRef`, `0..1`, required when the dependency blocks progress).
- `Implementation Report.unresolved_items[]`: `unresolved_item_id` (`string`, `1`), `affected_obligation_or_action_ref` (`StableRef`, `1`), `description` (`string`, `1`), and exactly one or both of `evidence_ref` (`StableRef`, `0..1`) and `blocking_ref` (`BlockingRef`, `0..1`).
- `Implementation Report.reviewer_handoff`: `reviewed_implementation_id` (`string`, `1`), `review_input_refs` (`StableRef[]`, `1..*`), `unresolved_item_refs` (`StableRef[]`, `0..*`), and `blocking_refs` (`BlockingRef[]`, `0..*`).
- `Build/Test Evidence.result`: `outcome_summary` (`string`, `1`) and `result_ref` (`StableRef`, `0..1`, required when the detailed result is stored separately).
- `Build/Test Evidence.execution_context`: `executor_or_environment` (`string`, `1`) and `configuration_refs` (`StableRef[]`, `0..*`), plus any target-specific context required to distinguish the run.
- `Task Review Result.finding`: `finding_id` (`string`, `1`), `check_ref` (`StableRef`, `1`), `description` (`string`, `1`), and `evidence_refs` (`StableRef[]`, `0..*`).
- `Task Review Result.required_reverification`: `check_refs` (`StableRef[]`, `1..*`) and `required_evidence_or_execution_refs` (`StableRef[]`, `0..*`), with the latter populated when reverification requires a named evidence or execution obligation.

These definitions identify content and linkage only. They do not define ownership, mutation, transition, evidence-validity, or recovery semantics.

## 5. Task Work Package schema

### 5.1 Purpose

The Task Work Package is the coherent package-level view of all canonical artifacts for one Task. It is an assembly and resume boundary, not an Agent, execution plan, evidence database, or workflow engine.

### 5.2 Required fields

| Field | Requirement | Shape | Meaning |
|---|---|---|---|
| `schema_ref` | Required | `StableRef`, `1` | Governing Task Package Schema version. |
| `task_id` | Required | string, `1` | Identity shared by every package artifact. |
| `task_type` | Required | string, `1` | Task classification supplied by the Task Contract. This schema does not close the classification set. |
| `package_updated_at` | Required | timestamp, `1` | Last package-assembly update. |
| `artifact_inventory` | Required | `ArtifactInventoryEntry[]`, exactly one entry per canonical artifact type | Applicability, availability, identity, and location of package artifacts. |
| `active_repository_ref` | Conditionally required | `StableRef`, `0..1` | Repository identity and applicable baseline or worktree when repository work applies. |
| `active_implementation_id` | Conditionally required | string, `0..1` | Current implementation identity after one exists. |
| `active_evidence_refs` | Required | `StableRef[]`, `0..*` | Evidence instances relevant to the current package view. |
| `active_review_result_ref` | Conditionally required | `StableRef`, `0..1` | Current Task Review Result when one exists. |
| `task_state_ref` | Required | `StableRef`, `1` | Active Task State used to locate workflow position and resume data. |
| `blocking_refs` | Required | `BlockingRef[]`, `0..*` | Current documented blockers relevant to package assembly. |

A separate package identifier is not required in V1 because `task_id`, typed `artifact_id` values, and additional repository or implementation identities provide unambiguous association. Every inventory entry and every artifact MUST carry the same `task_id`.

### 5.3 ArtifactInventoryEntry

| Field | Requirement | Shape | Meaning |
|---|---|---|---|
| `artifact_type` | Required | canonical artifact type, `1` | Inventory slot type. |
| `task_id` | Required | string, `1` | Package association. |
| `applicability` | Required | reference or bounded indication, `1` | Distinguishes applicable from not applicable without defining Task Status Model values. |
| `applicability_basis_ref` | Conditionally required | `StableRef`, `0..1` | Required when a durable source is needed to justify why the artifact type is or is not applicable, including an approved no-test-change outcome. |
| `availability` | Required | reference or bounded indication, `1` | Distinguishes available, not yet available, and blocked-unavailable without defining status semantics. |
| `artifact_instances` | Required | `ArtifactInstanceEntry[]`, `0..*` | Existing instances of this artifact type. Empty when none exists. |
| `blocking_ref` | Conditionally required | `BlockingRef`, `0..1` | Required when the artifact applies but production is blocked. |

#### 5.3.1 ArtifactInstanceEntry

| Field | Requirement | Shape | Meaning |
|---|---|---|---|
| `artifact_ref` | Required | `StableRef`, `1` | Identity of one existing artifact instance. |
| `location_ref` | Conditionally required | `LocationRef`, `0..1` | Durable retrieval location when identity alone is insufficient. |
| `repository_ref` | Conditionally required | `StableRef`, `0..1` | Repository and baseline/worktree association when needed to prevent mixing. |
| `implementation_id` | Conditionally required | string, `0..1` | Exact implementation association when the artifact instance evaluates or reports an implementation. |
| `evidence_run_ref` | Conditionally required | `StableRef`, `0..1` | Execution/run association for evidence instances when needed to distinguish repeated actions. |

The inventory MUST distinguish these structural conditions: applicable and available; applicable but not yet available; not applicable; and applicable but unavailable due to a documented blocker. These are categories the schema must represent, not mandatory status enum values. `Optional` MUST NOT substitute for any of them.

When `availability` indicates that an artifact is available, `artifact_instances` MUST contain `1..*` entries. When no artifact exists, `artifact_instances` MUST be empty. Each instance carries its own repository, implementation, and run linkage, preventing parallel-array ambiguity and silent mixing across attempts or lineage.

### 5.4 Canonical inventory

The package contains exactly one inventory slot for each type below. An artifact instance may be absent only according to its artifact-specific optionality rule.

1. Task Contract
2. Domain Skill Routing Result
3. Task Repository Context
4. Task Execution Plan
5. Task-local HLTP
6. Implementation Report
7. Acceptance Criteria Evidence
8. Build/Test Evidence
9. Task Review Result
10. Task State

## 6. Canonical artifact schemas

Each subsection specifies all ten artifact dimensions: purpose, producing stage, producer/source, consumers, fields, identity and cross-references, creation preconditions, optionality, relationships, and resume data. The common artifact header is REQUIRED in addition to artifact-specific fields.

### 6.1 Task Contract

**Purpose:** Define the bounded Task handed into planning: objective, scope, dependencies, fixed decisions, non-goals, requirements, and acceptance criteria. It MUST NOT contain the implementation plan or present repository observations as contract authority.

**Producing stage and source:** Task intake and contract formation. Produced by `DP-Task-Planner` from approved task sources and any required human decision.

**Consumers:** `DP-Task-Planner`, `DP-Task-Test-Designer`, `DP-Task-Implementer`, `DP-Task-Reviewer`, and `DP-Task-Orchestrator`.

**Optionality:** Always required.

**Creation preconditions:** Task identity and approved task sources exist; applicable design and implementation-plan inputs are identifiable. Every non-ready outcome caused by a blocker, required design clarification, required Task split, or dependency-not-ready condition MUST be represented by at least one durable reference in `blocking_refs`. This requirement records the blocking basis without defining complete Task Status Model semantics.

**Fields:**

| Field | Requirement | Shape | Meaning |
|---|---|---|---|
| `task_type` | Required | string, `1` | Bounded Task classification. |
| `objective` | Required | string, `1` | Intended Task outcome. |
| `scope_boundaries` | Required | object, `1` | Explicit included scope. |
| `non_goals` | Required | string[], `0..*` | Explicit exclusions, including future Task work. |
| `dependency_refs` | Required | `StableRef[]`, `0..*` | Task dependencies and prerequisites. |
| `approved_decision_refs` | Required | `SourceRef[]`, `0..*` | Fixed decisions applicable to the Task. |
| `requirements` | Required | object[], `1..*` | Each item contains unique `requirement_id`, statement, and source reference. |
| `acceptance_criteria` | Required | object[], `1..*` | Each item contains unique `acceptance_criterion_id`, mapped `requirement_ids`, statement, and source reference. |
| `readiness_refs` | Required | `StableRef[]`, `0..*` | Durable readiness inputs required for planning handoff. |
| `blocking_refs` | Required | `BlockingRef[]`, `0..*` | Unresolved ambiguity, design gap, Task-split requirement, dependency-not-ready condition, or other blocker records. The collection MUST contain `1..*` references whenever the Task Contract represents a non-ready outcome caused by one of these conditions; it MAY be empty only when no such condition exists. |

**Identity and relationships:** `requirements[].requirement_id` and `acceptance_criteria[].acceptance_criterion_id` are unique within `task_id` and are canonical downstream references. Every non-ready Task Contract retains the durable blocking references that explain its handoff condition. Task Contract is referenced by routing, planning, test design, implementation reporting, evidence mapping, review, and Task State. It does not prove its own satisfaction.

**Resume-relevant fields:** `artifact_id`, `task_id`, `task_type`, scope, dependencies, fixed-decision references, canonical requirement and criterion identities, readiness references, every durable blocking reference for a non-ready outcome, and source revisions.

### 6.2 Domain Skill Routing Result

**Purpose:** Record selective domain routing for Task concerns, including the applicable Skill or authoritative lookup, without copying a Domain Skill or performing implementation reasoning.

**Producing stage and source:** Domain-skill routing associated with Task planning or orchestration. Produced by the approved role invoking the applicable routing capability, including `DP-Task-Planner` during selective planning-time domain routing and `DP-Task-Orchestrator` during orchestration-time routing. The producing role for each instance MUST be recorded explicitly.

**Consumers:** `DP-Task-Planner`, `DP-Task-Implementer`, `DP-Task-Reviewer`, and `DP-Task-Orchestrator`, according to the routed concern and lifecycle stage. Downstream artifacts MAY reference only the routing entries applicable to them.

**Optionality:** Conditionally required when the Task contains a concern requiring domain routing or authoritative lookup. Otherwise its inventory slot is marked not applicable.

**Creation preconditions:** A Task Contract exists; routed concerns are identifiable; the available Skill or authoritative-source inventory can be inspected. If planning-time routing must begin before a Task Contract can be completed, the routing result is not created until the Task Contract exists; the incomplete intake and any blocker remain represented by the applicable planning input or Task State. An unresolved routing conflict requires a blocking reference rather than a silent choice.

**Fields:**

| Field | Requirement | Shape | Meaning |
|---|---|---|---|
| `task_contract_ref` | Required | `StableRef`, `1` | Task Contract supplying the routed Task concerns. |
| `routing_entries` | Required | object[], `1..*` | Selective routing results. |
| `routing_entries[].concern_id` | Required | string, `1` | Stable routed-concern identity within the artifact. |
| `routing_entries[].concern_ref` | Required | `StableRef`, `1` | Requirement, criterion, scope, dependency, or approved-decision concern. |
| `routing_entries[].routing_result` | Required | reference or bounded classification, `1` | Routing outcome whose complete canonical semantics remain external. |
| `routing_entries[].skill_ref` | Conditionally required | `SourceRef`, `0..1` | Applicable Skill when routing selects one. |
| `routing_entries[].authority_lookup_ref` | Conditionally required | `SourceRef`, `0..1` | Authoritative source or lookup when required. |
| `routing_entries[].applicability` | Required | indication, `1` | Applicability to this Task. |
| `routing_entries[].consumer_refs` | Required | role/artifact references, `1..*` | Exact downstream roles or artifact types for which this routing entry applies. Each consumer uses only entries that name it or its artifact type. |
| `routing_entries[].constraint_refs` | Required | `SourceRef[]`, `0..*` | Relevant constraints or required lookups. |
| `routing_entries[].blocking_ref` | Conditionally required | `BlockingRef`, `0..1` | Required for unresolved availability or routing conflict. |

**Identity and relationships:** Each `concern_id` is unique within this artifact. Each routing entry binds one routed concern and routing outcome to its exact `consumer_refs`; a downstream role or artifact consumes only entries that name that role or artifact type. Entries trace from Task Contract concerns into Task Repository Context and Task Execution Plan when applicable. Routing provenance MUST remain attached when used downstream.

**Resume-relevant fields:** Task Contract revision, concern identities, routing result references, Skill/authority revisions, applicability, entry-level consumer mappings, constraints, and blockers.

### 6.3 Task Repository Context

**Purpose:** Record focused, repository-grounded observations needed for this Task. It informs planning but does not replace approved Design or redefine requirements.

**Producing stage and source:** Repository analysis. Produced by `DP-Task-Planner` from verified repository exploration and applicable routed domain constraints.

**Consumers:** `DP-Task-Planner`, `DP-Task-Implementer`, `DP-Task-Reviewer`, and `DP-Task-Orchestrator`.

**Optionality:** Conditionally required for Tasks whose planning, implementation, verification, or review depends on repository truth. Otherwise marked not applicable.

**Creation preconditions:** Repository identity and an inspectable baseline or worktree exist; exploration scope is bounded by the Task; applicable routing references are available or blocking references are recorded.

**Fields:**

| Field | Requirement | Shape | Meaning |
|---|---|---|---|
| `task_contract_ref` | Required | `StableRef`, `1` | Governing Task Contract. |
| `routing_entry_refs` | Required | `StableRef[]`, `0..*` | Exact applicable routing entries consumed by this artifact. Each reference targets a specific entry within a Domain Skill Routing Result instance and preserves that instance's provenance. The collection is empty only when no routed concern applies. |
| `repository_id` | Required | string, `1` | Repository identity. |
| `baseline_id` | Conditionally required | string, `0..1` | Baseline observed when analysis is baseline-based. |
| `worktree_id` | Conditionally required | string, `0..1` | Worktree observed when analysis is worktree-based. |
| `exploration_scope` | Required | `LocationRef[]`, `1..*` | Bounded inspected areas. |
| `relevant_declarations` | Required | referenced observations[], `0..*` | Relevant types, symbols, contracts, or declarations. |
| `owners_and_consumers` | Required | referenced observations[], `0..*` | Repository-observed ownership and consumption facts. |
| `initialization_paths` | Required | referenced observations[], `0..*` | Relevant initialization paths. |
| `runtime_paths` | Required | referenced observations[], `0..*` | Relevant runtime paths. |
| `existing_tests` | Required | referenced observations[], `0..*` | Existing relevant tests. |
| `similar_implementations` | Required | referenced observations[], `0..*` | Relevant precedent with limits stated. |
| `verified_apis` | Required | referenced observations[], `0..*` | APIs verified against repository content. |
| `domain_concerns` | Required | object[], `0..*` | Applicable domain concerns and routing provenance. |
| `integration_prerequisites` | Required | object[], `0..*` | Repository-grounded prerequisites. |
| `evidence_refs` | Required | `StableRef[]`, `1..*` | Evidence supporting observations. |
| `observation_limits` | Required | string[], `0..*` | Known limits and areas not inspected. |

**Identity and relationships:** `repository_id` plus applicable `baseline_id` or `worktree_id` binds every observation and evidence reference to repository state. `routing_entry_refs` identifies every exact routing entry consumed and preserves its containing routing-result provenance. The context consumes Task Contract and applicable routing entries and is referenced by Task Execution Plan, implementation evidence, review, and Task State.

**Resume-relevant fields:** repository, baseline/worktree, exploration scope, exact applicable routing-entry references with routing-result provenance, all durable observation/evidence references, limits, integration prerequisites, and source artifact revisions.

### 6.4 Task Execution Plan

**Purpose:** Define an ordered, repository-grounded plan mapping Task requirements to bounded changes and verification. It is not repository truth and does not implement future Tasks.

**Producing stage and source:** Task planning. Produced by `DP-Task-Planner`.

**Consumers:** `DP-Task-Test-Designer`, `DP-Task-Implementer`, `DP-Task-Reviewer`, and `DP-Task-Orchestrator`.

**Optionality:** Always required for an implementable Task; when planning cannot complete, its absence is represented as applicable but blocked with a blocking reference.

**Creation preconditions:** Task Contract exists; required routing and repository context are available or explicitly not applicable; construction-blocking decisions are resolved.

**Fields:**

| Field | Requirement | Shape | Meaning |
|---|---|---|---|
| `task_contract_ref` | Required | `StableRef`, `1` | Governing Task Contract. |
| `routing_entry_refs` | Required | `StableRef[]`, `0..*` | Exact applicable routing entries consumed by this artifact. Each reference targets a specific entry within a Domain Skill Routing Result instance and preserves that instance's provenance. The collection is empty only when no routed concern applies. |
| `repository_context_ref` | Conditionally required | `StableRef`, `0..1` | Applicable repository context. |
| `requirement_change_map` | Required | object[], `1..*` | Maps canonical `requirement_id` values to plan action identities and change areas. |
| `actions` | Required | object[], `1..*` | Ordered items with unique `action_id`, required change, dependencies, and verification obligations. |
| `actions[].verification` | Required | object[], `1..*` | Verification expected for the action. |
| `dependencies` | Required | object[], `0..*` | Plan dependencies with stable references. |
| `risks` | Required | object[], `0..*` | Task-specific risks and affected action references. |
| `diff_scope_boundary` | Required | object, `1` | Bounded Task diff scope. |
| `required_change_areas` | Required | `LocationRef[]`, `1..*` | Areas directly required by the Task. |
| `expected_affected_areas` | Required | `LocationRef[]`, `0..*` | Repository-grounded forecast, not a rigid allowlist. |
| `protected_or_excluded_areas` | Required | `LocationRef[]`, `0..*` | Areas requiring escalation or approved plan update before modification. |

**Identity and relationships:** `actions[].action_id` is unique within the plan. Requirement mappings use Task Contract identities. `routing_entry_refs` identifies every exact routing entry consumed and preserves its containing routing-result provenance. The plan consumes applicable routing entries and repository context and is referenced by Task-local HLTP, Implementation Report, Build/Test Evidence, review, and Task State.

**Resume-relevant fields:** input artifact revisions, exact applicable routing-entry references with routing-result provenance, action order and identities, requirement mappings, change boundaries, dependencies, risks, verification obligations, and blockers referenced through package inventory or Task State.

### 6.5 Task-local HLTP

**Purpose:** Define Task-local semantic test obligations and expected behavior, either by slicing an approved HLTP or completing missing Task-level coverage. It is distinct from executed Build/Test Evidence.

**Producing stage and source:** Task test design. Produced by `DP-Task-Test-Designer`.

**Consumers:** `DP-Task-Implementer`, `DP-Task-Reviewer`, and `DP-Task-Orchestrator`.

**Optionality:** Conditionally required when the Task changes or requires test obligations. It may be not applicable when no Task-local test change or slice is required, provided the inventory records the applicable no-test-change outcome reference.

**Creation preconditions:** Task Contract and Task Execution Plan exist; applicable source HLTP content is available when slicing; semantic gaps requiring human/design authority are resolved or blocked.

**Fields:**

| Field | Requirement | Shape | Meaning |
|---|---|---|---|
| `task_contract_ref` | Required | `StableRef`, `1` | Governing Task Contract. |
| `execution_plan_ref` | Required | `StableRef`, `1` | Plan whose obligations are tested. |
| `source_hltp_refs` | Conditionally required | `SourceRef[]`, `1..*` | Required when created by slicing existing HLTP content. |
| `test_scope` | Required | object, `1` | Task-local behaviors and exclusions. |
| `requirement_criterion_map` | Required | object[], `1..*` | Maps test cases to canonical requirements and acceptance criteria. |
| `test_cases` | Required | object[], `1..*` | Semantic obligations with unique `test_case_id`. |
| `test_cases[].preconditions` | Required | string[], `0..*` | Required starting conditions. |
| `test_cases[].stimulus` | Required | string, `1` | Action or event under test. |
| `test_cases[].expected_behavior` | Required | string, `1` | Observable required result. |
| `test_cases[].test_double_obligations` | Required | string[], `0..*` | Required doubles at obligation level, not fixture mechanics. |
| `error_cases` | Required | test-case references/content[], `0..*` | Error obligations. |
| `boundary_cases` | Required | test-case references/content[], `0..*` | Boundary obligations. |
| `lifecycle_cases` | Required | test-case references/content[], `0..*` | Lifecycle obligations. |
| `deferred_coverage` | Required | object[], `0..*` | Explicit deferred obligations and authority/reason references. |
| `completion_criteria` | Required | object[], `1..*` | Conditions for completing the Task-local obligations. |

**Identity and relationships:** When no Task-local HLTP artifact is produced, its package inventory entry MUST carry the applicability indication and an `applicability_basis_ref` to the stable no-test-change outcome or authority record. `test_case_id` is unique within `task_id` and this HLTP. Every test case maps to requirement and/or criterion identities and, where applicable, plan actions. Planned tests are referenced by Build/Test Evidence.

**Resume-relevant fields:** source HLTP revisions, test scope, test identities and mappings, deferred coverage, completion criteria, and execution-plan revision.

### 6.6 Implementation Report

**Purpose:** Report a specific implementation attempt, including completed changes, partial changes, or a blocker reached before repository modification, and provide reviewer handoff. It is not independent proof, does not decide review PASS, and does not modify Task Contract or HLTP obligations.

**Producing stage and source:** Implementation-attempt reporting during or after implementation. Produced by `DP-Task-Implementer` when an implementation attempt reaches a durable reporting point, including successful implementation, partial implementation, completion of implementation-side verification, or a blocker that prevents repository modification or further implementation.

**Consumers:** `DP-Task-Reviewer` and `DP-Task-Orchestrator`.

**Optionality:** Conditionally required once an implementation attempt exists. Before that it is applicable but not yet available, or blocked when implementation production is blocked.

**Creation preconditions:** Task Execution Plan exists; applicable Task-local HLTP is available or not applicable; a specific `implementation_id` exists; and the target repository and starting baseline are identifiable. A `worktree_id` is required when a worktree was established for the attempt. An attempt blocked before worktree establishment or repository modification MAY still produce an Implementation Report when the blocker and any missing worktree are recorded in `unresolved_items`.

**Fields:**

| Field | Requirement | Shape | Meaning |
|---|---|---|---|
| `implementation_id` | Required | string, `1` | Specific implementation content state/attempt. |
| `execution_plan_ref` | Required | `StableRef`, `1` | Plan used by implementation. |
| `task_local_hltp_ref` | Conditionally required | `StableRef`, `0..1` | Applicable Task-local HLTP. |
| `repository_id` | Required | string, `1` | Target repository for the implementation attempt, whether or not the attempt modified it. |
| `baseline_id` | Required | string, `1` | Starting repository baseline. |
| `worktree_id` | Conditionally required | string, `0..1` | Worktree associated with the implementation attempt. Required when a worktree was established for the attempt. It MAY be absent only when the attempt was blocked before a worktree could be established, and that condition is recorded in `unresolved_items`. |
| `implemented_scope` | Required | object, `1` | Task scope attempted and the portion actually implemented; records an empty implemented portion when blocked before change. |
| `changed_areas` | Required | `LocationRef[]`, `0..*` | Actual changed areas; may be empty for an implementation attempt blocked before any repository change. |
| `changed_files_or_symbols` | Required | `LocationRef[]`, `0..*` | Concrete file/symbol changes; may be empty for an implementation attempt blocked before any repository change. |
| `plan_action_mappings` | Required | object[], `0..*` | Implemented changes mapped to plan `action_id` values; may be empty when no plan action was implemented. |
| `acceptance_criterion_mappings` | Required | object[], `0..*` | Implemented changes mapped to canonical criteria; may be empty when an implementation attempt is blocked before any change or criterion evaluation. |
| `deviations` | Required | object[], `0..*` | Every plan or obligation deviation. |
| `changes_outside_expected_areas` | Required | object[], `0..*` | Every actual change beyond forecast areas. |
| `discovered_dependencies` | Required | object[], `0..*` | Newly discovered dependencies with references. |
| `unresolved_items` | Required | object[], `0..*` | Remaining issues, blockers, or unresolved implementation-side verification failures. The collection MUST be non-empty when an attempt is blocked or when implementation-side verification has an unresolved failure. Each item MUST identify the affected obligation or action and reference the applicable `evidence_ref` or `blocking_ref`; an attempt blocked before worktree establishment MUST also record that no worktree was established. |
| `code_change_refs` | Required | `LocationRef[]`, `0..*` | Relevant production change references; empty when the Task changes tests only. |
| `test_change_refs` | Required | `LocationRef[]`, `0..*` | Relevant test change references; empty when the Task does not change tests. |
| `evidence_refs` | Required | `StableRef[]`, `0..*` | Evidence available at report creation. |
| `reviewer_handoff` | Required | object, `1` | Focused review inputs and relevant identities. It MUST include every unresolved implementation-side verification failure and its applicable evidence reference, plus every blocker relevant to review. |

At least one of `code_change_refs` or `test_change_refs` MUST be non-empty for an implementation attempt that reports repository changes.

**Identity and relationships:** `implementation_id` binds the report, evidence, acceptance mappings, review, and Task State. All deviations, unexpected areas, blockers, and unresolved implementation-side verification failures MUST be explicit. Each unresolved item references the applicable evidence or blocker record. Evidence is referenced, not replaced by the report.

**Resume-relevant fields:** baseline identity, worktree identity when one exists, implementation identity, actual changes, mappings, deviations, dependencies, unresolved items including any pre-worktree blocker or unresolved implementation-side verification failure, their applicable evidence or blocker references, and reviewer handoff.

### 6.7 Acceptance Criteria Evidence

**Purpose:** Map every Task acceptance criterion to evidence or to an explicit unresolved/not-yet-evidenced/blocking indication. It records coverage structure but does not define detailed evidence validation or determine review PASS by itself.

**Producing stage and source:** Evidence assembly during or after implementation verification. Produced from Task Contract criteria and implementation-linked evidence, primarily by `DP-Task-Implementer`; consumed as review input.

**Consumers:** `DP-Task-Reviewer` and `DP-Task-Orchestrator`.

**Optionality:** Conditionally required once criteria are evaluated against an implementation. Before implementation it is applicable but not yet available; Tasks without implementation evaluation may mark it not applicable only when approved task scope permits.

**Creation preconditions:** Task Contract exists; criteria have stable identities; when evaluated against code, `implementation_id` exists; referenced evidence items exist or the criterion carries a durable unresolved indication.

**Fields:**

| Field | Requirement | Shape | Meaning |
|---|---|---|---|
| `task_contract_ref` | Required | `StableRef`, `1` | Source of canonical criteria. |
| `implementation_id` | Conditionally required | string, `0..1` | Required when criteria are evaluated against implementation. |
| `criterion_entries` | Required | object[], exactly one per applicable acceptance criterion | Complete criterion mapping. |
| `criterion_entries[].acceptance_criterion_id` | Required | string, `1` | Canonical criterion identity. |
| `criterion_entries[].criterion_source_ref` | Required | `StableRef`, `1` | Exact Task Contract criterion. |
| `criterion_entries[].evidence_refs` | Required | `StableRef[]`, `0..*` | Evidence supporting this criterion. |
| `criterion_entries[].evidence_result_summary` | Conditionally required | string, `0..1` | Required when evidence exists. |
| `criterion_entries[].coverage_indication` | Required | reference or bounded indication, `1` | Structural coverage/satisfaction indication governed externally. |
| `criterion_entries[].unresolved_or_blocking_ref` | Conditionally required | `StableRef`, `0..1` | Required when no evidence exists or evaluation is blocked/unresolved. |

**Identity and relationships:** An entry represented as evidenced or satisfied MUST reference `1..*` evidence items. An entry with zero evidence references MUST have an unresolved, blocking, or equivalent not-yet-evidenced reference. Criteria map to Implementation Report and Build/Test Evidence by canonical identities and `implementation_id`.

**Resume-relevant fields:** Task Contract revision, implementation identity, complete criterion inventory, evidence identities and run associations, summaries, coverage indications, and unresolved references.

### 6.8 Build/Test Evidence

**Purpose:** Record durable execution evidence for a build, test, analysis, or other verification action against a specific repository or implementation identity. A statement such as "build passed" without run details is insufficient.

**Producing stage and source:** Implementation verification and any approved review-side verification. Produced by the role executing the action, commonly `DP-Task-Implementer`; review-side executions may be produced by `DP-Task-Reviewer` without implying general ownership rules.

**Consumers:** `DP-Task-Implementer`, `DP-Task-Reviewer`, and `DP-Task-Orchestrator`; Acceptance Criteria Evidence references relevant items.

**Optionality:** Conditionally required whenever an execution result is asserted or needed for acceptance/review. Before execution it is applicable but not yet available; if execution is blocked the inventory records a blocking reference.

**Creation preconditions:** The action is known and the execution has occurred, or no evidence artifact is created. The execution target MUST be bound to an immutable identity: `implementation_id` when the execution evaluates an implementation; `baseline_id` when repository evidence does not evaluate an implementation; or, for execution against a mutable worktree, either `implementation_id` or an immutable `revision` within `target_or_worktree`. The complete validation algorithm remains governed by the Minimum Evidence Model.

**Fields:**

| Field | Requirement | Shape | Meaning |
|---|---|---|---|
| `evidence_id` | Required | string, `1` | Same value as `artifact_id`; unique execution/item identity. |
| `evidence_type` | Required | string or external reference, `1` | Kind of evidence without defining the complete canonical taxonomy. |
| `action_or_command` | Required | string, `1` | Action or exact command executed. |
| `repository_id` | Conditionally required | string, `0..1` | Repository target when applicable. |
| `target_or_worktree` | Required | `LocationRef`, `1` | Execution target and worktree/location context. When it identifies a mutable worktree and `implementation_id` is absent, its `revision` MUST contain an immutable locator for the executed repository state. |
| `baseline_id` | Conditionally required | string, `0..1` | Required for repository evidence that does not evaluate an implementation. It identifies the immutable repository baseline on which the action ran. |
| `implementation_id` | Conditionally required | string, `0..1` | Required whenever the execution evaluates an implementation, including execution on a mutable worktree whose state is represented by that implementation identity. |
| `result` | Required | structured summary, `1` | Recorded outcome. |
| `exit_code` | Conditionally required | integer, `0..1` | Required when the executed action produces an exit code. |
| `output_summary` | Required | string, `1` | Relevant concise output. |
| `raw_output_ref` | Conditionally required | `LocationRef`, `0..1` | Durable raw-output location when available or needed; long raw logs are not copied. |
| `requirement_refs` | Required | `StableRef[]`, `0..*` | Mapped requirements. |
| `acceptance_criterion_refs` | Required | `StableRef[]`, `0..*` | Mapped criteria. |
| `review_check_refs` | Required | `StableRef[]`, `0..*` | Mapped review checks when applicable. |
| `task_local_test_case_refs` | Required | `StableRef[]`, `0..*` | Planned Task-local tests executed by this action. |
| `unplanned_test_authority_ref` | Conditionally required | `SourceRef`, `0..1` | Required when recording an authorized unplanned test. |
| `execution_context` | Required | object, `1` | Environment/configuration details needed to identify the run. |
| `executed_at` | Required | timestamp, `1` | Execution time. |

**Identity and relationships:** Every evidence instance MUST have at least one immutable target identity. Evidence that evaluates an implementation MUST bind to `implementation_id`. Repository evidence that does not evaluate an implementation MUST bind to `baseline_id`. Evidence executed on a mutable worktree MUST additionally bind either to `implementation_id` or to an immutable `revision` in `target_or_worktree`. It also binds to its execution context and maps to requirements, criteria, Task-local test cases, and review checks as applicable. These are structural identity requirements, not the complete evidence-validation algorithm. Evidence does not alone determine completion or review PASS.

**Resume-relevant fields:** evidence identity, command/action, `target_or_worktree` including its immutable revision when required, `baseline_id` for non-implementation repository evidence, `implementation_id` for implementation evaluation, outcome, output/raw-output references, mappings, execution context, and timestamp.

### 6.9 Task Review Result

**Purpose:** Record the Reviewer result for an exact review-input set and, when identifiable, the specific implementation under review. It also permits a blocked input-integrity result when a required implementation identity or another required review input is missing or invalid. It does not redefine review checks or full review status semantics.

**Producing stage and source:** Task review. Produced by `DP-Task-Reviewer`.

**Consumers:** `DP-Task-Implementer` for required correction and `DP-Task-Orchestrator` for orchestration and completion decisions.

**Optionality:** Conditionally required after a review attempt. Before review it is applicable but not yet available; if review cannot run it is blocked with a documented reference.

**Creation preconditions:** A review attempt has been initiated and the received review-input set can be identified sufficiently to record its integrity result. For a substantive review, a specific implementation and the required review inputs MUST be identifiable. When an implementation identity or another required input is missing or invalid, the Task Review Result MAY be created as blocked, with the missing or invalid input represented through `review_input_integrity_refs` and `blocking_ref`. A blocked result does not require usable execution evidence that could not yet be produced.

**Fields:**

| Field | Requirement | Shape | Meaning |
|---|---|---|---|
| `review_result_id` | Required | string, `1` | Same value as `artifact_id`. |
| `reviewed_implementation_id` | Conditionally required | string, `0..1` | Required when a specific implementation can be identified. It MAY be absent only when review is blocked because the implementation identity is missing or invalid, and that condition is recorded through `review_input_integrity_refs` and `blocking_ref`. |
| `reviewed_artifact_refs` | Required | `StableRef[]`, `0..*` | Exact artifact instances received and reviewed. The collection MAY be empty only when review is blocked before any valid artifact instance can be identified, and that condition is recorded through `review_input_integrity_refs` and `blocking_ref`. |
| `review_input_integrity_refs` | Required | `StableRef[]`, `1..*` | Revisions/digests needed to identify the review package. |
| `review_result` | Required | external status reference or bounded result, `1` | Supports PASS, FAIL, or BLOCKED representation while full semantics remain external. |
| `first_failing_check_ref` | Conditionally required | `StableRef`, `0..1` | Required for `FAIL` or `BLOCKED`; identifies the first ordered review check that did not pass. The field name follows the approved output contract and does not imply that a blocked check is a failure. |
| `finding` | Conditionally required | object, `0..1` | Required for FAIL or BLOCKED as governed by review/status contracts. |
| `evidence_refs` | Required | `StableRef[]`, `0..*` | Evidence items evaluated or used before the review stopped. The collection MAY be empty when the review reaches `FAIL` or `BLOCKED` before any usable evidence item is required or evaluated. A `FAIL` with no evidence references MUST still identify the failing check, finding, reviewed inputs, and required correction. A `BLOCKED` result with no evidence references MUST identify the blocking condition through `blocking_ref`. |
| `requirement_refs` | Required | `StableRef[]`, `0..*` | Canonical Task requirements directly evaluated or implicated by the review result. The collection is empty only when substantive review is blocked before any requirement can be evaluated. |
| `acceptance_criterion_refs` | Required | `StableRef[]`, `0..*` | Canonical acceptance criteria directly evaluated or implicated by the review result. The collection is empty only when substantive review is blocked before any criterion can be evaluated. |
| `required_correction` | Conditionally required | string, `0..1` | Required for `FAIL`. Describes the single correction required for the first failing check. It MUST be absent for `PASS`. For `BLOCKED`, the missing or invalid input is represented by `blocking_ref` rather than by a fabricated correction. |
| `required_reverification` | Conditionally required | object, `0..1` | Required for `FAIL`. Identifies the checks, evidence, or execution that MUST be repeated after the required correction. It MUST be absent for `PASS`. For `BLOCKED`, reverification is not required until the blocking condition is resolved, unless a dedicated governing contract explicitly requires it. |
| `checks_passed_before_failure` | Required | `StableRef[]`, `0..*` | Result-specific ordered check record. For `PASS`, it contains the complete ordered set of review checks that passed. For `FAIL` or `BLOCKED`, it contains the ordered checks completed before `first_failing_check_ref`. |
| `blocking_ref` | Conditionally required | `BlockingRef`, `0..1` | Required for a blocked review result. |

**Identity and relationships:** A substantive review, including every PASS or FAIL result, MUST reference exactly one `reviewed_implementation_id`. A BLOCKED result MAY omit it only when a missing or invalid implementation identity is itself the documented reason review cannot proceed. Review PASS is attributable only to `reviewed_implementation_id` and the exact reviewed artifacts and applicable evidence. For `FAIL` or `BLOCKED`, `first_failing_check_ref` identifies the first ordered non-passing check. A new implementation MUST NOT silently inherit the prior review. Findings map to canonical requirement and acceptance-criterion references, supporting evidence, and the first failing check without redefining the review suite.

**Resume-relevant fields:** reviewed implementation, reviewed artifact revisions, integrity references, result reference, canonical requirement and acceptance-criterion references, the complete ordered passed-check set for `PASS` or the checks passed before the first non-passing check for `FAIL` or `BLOCKED`, first failure/finding when applicable, evidence, correction, reverification, and blocker.

### 6.10 Task State

**Purpose:** Provide the durable package index for current workflow position and resumption. It locates sources of truth rather than copying complete artifacts or converting assertions into evidence.

**Producing stage and source:** Task orchestration and durable state update. Produced by `DP-Task-Orchestrator` from the package artifacts and repository state.

**Consumers:** `DP-Task-Orchestrator` and the approved stage/Agent selected for continuation; all Agents MAY read it when locating active artifacts.

**Optionality:** Always required. Its initial instance exists as soon as a Task Work Package is established.

**Creation preconditions:** `task_id` exists and the package inventory can be initialized. The initial Task State MAY be created immediately before or atomically with the first durable Task Work Package assembly; in that initial instance only, `artifact_inventory_ref` may be absent until the package obtains a durable location. Repository identity is recorded when already applicable. No conversation history is required.

**Fields:**

| Field | Requirement | Shape | Meaning |
|---|---|---|---|
| `workflow_position_or_status_ref` | Required | `StableRef`, `1` | Current position/status governed by the Task Status Model. |
| `active_artifact_refs` | Required | `StableRef[]`, `0..*` | Active package artifact instances other than the current Task State. The collection may be empty only in the initial Task State before another artifact exists. Task State lineage uses the common `predecessor_artifact_ref`; the field MUST NOT self-reference the containing Task State. |
| `artifact_inventory_ref` | Conditionally required | `StableRef`, `0..1` | Reference to the current Task Work Package inventory. It MAY be absent only in the initial Task State created while the package and its first inventory are being established; after the package has a durable location, this field is required. The reference uses the containing `task_id` as the Task Work Package target identity, so no separate package identifier is introduced. |
| `repository_id` | Conditionally required | string, `0..1` | Applicable repository. |
| `baseline_id` | Conditionally required | string, `0..1` | Applicable repository baseline. |
| `worktree_id` | Conditionally required | string, `0..1` | Active worktree. |
| `current_implementation_id` | Conditionally required | string, `0..1` | Current implementation after one exists. |
| `active_evidence_refs` | Required | `StableRef[]`, `0..*` | Current resume-relevant evidence. |
| `active_review_result_ref` | Conditionally required | `StableRef`, `0..1` | Active review result after one exists. |
| `blocking_ref` | Conditionally required | `BlockingRef`, `0..1` | Current blocker when one exists. |
| `pending_action_ref` | Conditionally required | `StableRef`, `0..1` | Durable pending action reference when one exists. |
| `responsible_stage_ref` | Required | role/stage reference, `1` | Stage responsible for the next governed action, without defining transition rules. |
| `resume_target_ref` | Required | role/stage/artifact reference, `1` | Target needed to continue under the Resume Invariant. |
| `last_durable_transition_or_update_ref` | Required | `StableRef`, `1` | Record of the latest durable state update. |

**Identity and relationships:** Task State references the active instances of all applicable package artifacts, current repository state, implementation, evidence, review, blocker, and continuation target. It MUST NOT duplicate full artifact content, define allowed transitions, or make an unevidenced completion assertion authoritative.

**Resume-relevant fields:** All Task State fields are resume-relevant. Correct continuation uses Task State, referenced artifacts, and repository state, never prior conversation history.

## 7. Cross-artifact relationship contract

The following relationships are structural traceability, not a mandatory creation order for every artifact:

1. Task Contract supplies canonical Task, requirement, criterion, dependency, scope, and approved-decision identities.
2. Domain Skill Routing Result maps selected Task concerns to Skills or authoritative lookups and preserves provenance.
3. Task Repository Context binds verified observations to repository and baseline/worktree identity.
4. Task Execution Plan maps requirements to ordered plan actions, bounded change areas, and verification obligations.
5. Task-local HLTP, when applicable, maps semantic test cases to requirements, criteria, and plan actions.
6. Implementation Report binds the outcome of a specific implementation attempt, including actual changes, partial changes, or a documented blocker reached before change, to its `implementation_id`, plan, and applicable HLTP.
7. Build/Test Evidence binds executed actions to the repository state or `implementation_id` on which they ran.
8. Acceptance Criteria Evidence maps every applicable criterion to implementation-linked evidence or an explicit unresolved reference.
9. Task Review Result binds a substantive review outcome to an exact implementation and reviewed-input/evidence set. A blocked input-integrity result MAY omit the implementation reference only when the missing or invalid implementation identity is itself the documented reason that review cannot proceed.
10. Task State identifies the active artifact graph, repository state, implementation, evidence, review, blocker, pending action, and resume target.

Material references MUST state target type, relationship meaning, applicable cardinality, and the identity needed to prevent mixing. In particular:

- Task Contract to each applicable downstream artifact is `1 -> 0..*` across lineage.
- One Task Execution Plan instance maps `1 -> 1..*` plan actions.
- One applicable Task-local HLTP maps `1 -> 1..*` test cases.
- One acceptance criterion maps to `0..*` evidence items, but an evidenced/satisfied indication requires `1..*`; zero evidence requires an unresolved reference.
- One implementation may have `0..*` evidence items and `0..*` review attempts.
- Each substantive Task Review Result references exactly one implementation and `1..*` reviewed artifact instances. A blocked result MAY lack an implementation reference or reviewed artifact instance only when the missing or invalid input is itself the documented reason that review cannot proceed; `review_input_integrity_refs` and `blocking_ref` remain required for that condition.
- Each Task Review Result references `0..*` evidence items evaluated or used before review stopped. The collection MAY be empty when a `FAIL` or `BLOCKED` result occurs before usable evidence is required or evaluated. An evidence-free `FAIL` MUST still identify its failing check, finding, reviewed inputs, required correction, and required reverification. An evidence-free `BLOCKED` result MUST identify its blocking condition through `blocking_ref`.
- A `PASS` Task Review Result records the complete ordered set of review checks in `checks_passed_before_failure`; the field name is retained from the approved output contract even though no failure exists. A `FAIL` or `BLOCKED` result records only the ordered checks completed before `first_failing_check_ref`.
- Every `FAIL` Task Review Result identifies exactly one first failing check, its finding, the required correction, and the required reverification. A `BLOCKED` result identifies the first blocked check, its finding, and its `blocking_ref`, without inventing a correction for a missing authoritative input.
- One active Task State references `0..*` other active artifact instances and `0..*` active evidence items. Zero other artifacts are permitted only for the initial Task State before another artifact exists; Task State MUST NOT reference itself.

No artifact may prove or approve itself. Implementation Report is not proof; Build/Test Evidence alone is not completion; Acceptance Criteria Evidence alone is not review PASS; Task State is not evidence; Task Review Result relies on independently identifiable inputs and evidence.

## 8. Applicability, lineage, and resume invariants

- Applicability, availability, blocking, and status are distinct. The schema records their references but does not define the full status vocabulary or transitions.
- A conditionally required artifact that does not apply MUST have a not-applicable inventory indication and reason/authority reference when required by the governing contract.
- An applicable artifact not yet produced MUST remain distinguishable from a blocked artifact.
- A replacement artifact MUST receive a distinguishable `artifact_id` and SHOULD reference its predecessor when needed for traceability or resume.
- An implementation change MUST create a distinguishable `implementation_id`; prior evidence and review MUST remain tied to the implementation they evaluated.
- Repository observations MUST retain `repository_id` and applicable `baseline_id` or `worktree_id`.
- Resume-critical external content MUST use durable identity plus revision/location when mutable retrieval could select the wrong content.
- Raw logs, complete source files, full Designs, full HLTPs, and Domain Skills SHOULD be referenced rather than copied unless a focused excerpt is structurally necessary.

## 9. Foundation separation

This schema intentionally leaves the following to dedicated contracts:

- **Task Authority Model:** ownership, authority scope, write/read permissions, freeze points, mutability, validity, and invalidation.
- **Task Status Model:** complete status meanings, owners, pipeline effects, allowed next states, resume effects, and human-intervention rules.
- **Minimum Evidence Model:** validation, freshness, trust, proof strength, command controls, and execution-verification policy. This schema only carries minimum evidence fields and relationships.
- **Resume Invariant:** recovery behavior, compaction recovery, blocked recovery, partial-implementation recovery, and orchestration procedure. This schema only ensures durable resume inputs exist or are referenced.

This contract MUST NOT be interpreted as creating an evidence database, permission system, general revision system, scheduling infrastructure, additional Agent, or additional canonical artifact.
