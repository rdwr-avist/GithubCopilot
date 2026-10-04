# Domain Skill Inventory Schema

**Canonical target path:** `.github/task-agent-system/foundation/domain-skill-inventory-schema.md`

## 1. Purpose

This document defines the normative schema for a durable Domain Skill inventory document and its reusable Domain Skill entries. The schema supports deterministic routing, progressive disclosure, validation tracking, durable resume, and controlled replacement without creating a live inventory or performing Task-specific routing.

An inventory entry describes a capability and the conditions under which an authorized routing producer may consider it. It does not select that capability for a Task, define Task requirements, identify Task consumers, or perform implementation reasoning.

## 2. Authority Order

The primary authority is the approved Task Agent work plan. The following Foundation contracts apply within their bounded subjects:

1. `task-package-schema.md` governs references, canonical artifacts, `Domain Skill Routing Result`, and Task package boundaries.
2. `task-authority-model.md` governs authority, provenance, freeze, replacement, and conflict handling.
3. `minimum-evidence-model.md` governs evidence validity, target binding, scope, freshness, and rerun requirements.
4. `task-status-model.md` governs canonical statuses, including the three Domain routing statuses.
5. `resume-invariant.md` governs durable reconstruction, selective invalidation, and no-memory resume.

Artifact-specific scope constraints do not override a governing Foundation contract within that contract's authority. Durable authority does not come from conversation memory, hidden state, or unstored reasoning.

Use the supplied active authority set.

Do not substitute or infer authorities.

## 3. Normative Conventions

`MUST`, `MUST NOT`, `REQUIRED`, `SHOULD`, `SHOULD NOT`, and `MAY` are normative. A field contract specifies requirement level, shape, cardinality, meaning, and an activation condition when conditional.

Cardinality notation is:

- `1`: exactly one value.
- `0..1`: optional single value.
- `1..*`: one or more values.
- `0..*`: zero or more values.

A `StableRef` is a durable typed identity reference. A `SourceRef` is a `StableRef` plus the relationship of the source to the subject. A `LocationRef` supplies a durable locator when identity alone cannot retrieve the subject. A `BlockingRef` identifies a durable blocker basis. These terms retain their Foundation meanings.

Examples are synthetic, non-normative, and subordinate to this schema.

### 3.1 StableRef

| Member | Requirement | Shape | Cardinality | Meaning | Activation condition |
|---|---|---:|---:|---|---|
| `subject` | REQUIRED | stable identifier string | 1 | Durable identity of the referenced subject. | Always. |
| `revision` | CONDITIONAL | immutable revision string | 0..1 | Exact referenced revision. | REQUIRED when revision affects meaning or the subject may change. |


#### Foundation compatibility mapping

This inventory-local `StableRef` serialization is a bounded projection of the canonical `StableRef` in `task-package-schema.md`; it does not redefine that Foundation type. `subject` MUST encode the canonical typed identity (`target_type` plus `target_id`) without loss. The containing member supplies the canonical relationship, `revision` supplies the immutable revision when material, and a durable locator MUST be supplied when identity alone cannot retrieve the target. The containing structure MUST supply a complete canonical reference; consumers MUST use it and MUST NOT infer missing identity or revision.

### 3.2 SourceRef

| Member | Requirement | Shape | Cardinality | Meaning | Activation condition |
|---|---|---:|---:|---|---|
| `subject_ref` | REQUIRED | `StableRef` | 1 | Durable identity of the source subject. | Always. |
| `relationship` | REQUIRED | governed relationship string | 1 | Relationship of the source to the represented claim. | Always. |
| `locator_ref` | CONDITIONAL | `LocationRef` | 0..1 | Durable retrieval location. | REQUIRED when identity alone cannot retrieve the source. |


#### Foundation compatibility mapping

This inventory-local `SourceRef` is a structured serialization of the canonical `SourceRef` from `task-package-schema.md`, not a competing definition. `subject_ref` provides canonical target type and target identity through the mapping in Section 3.1, `relationship` is the canonical relationship, and `locator_ref` supplies canonical locator and revision information when required. Its target MUST remain one of the source kinds authorized by the Foundation schema.

### 3.3 LocationRef

| Member | Requirement | Shape | Cardinality | Meaning | Activation condition |
|---|---|---:|---:|---|---|
| `scheme` | REQUIRED | governed identifier string | 1 | Locator scheme. | Always. |
| `value` | REQUIRED | non-empty string | 1 | Scheme-specific durable locator value. | Always. |
| `revision` | CONDITIONAL | immutable revision string | 0..1 | Exact located revision. | REQUIRED when the locator may resolve to more than one material revision. |


#### Foundation compatibility mapping

This inventory-local `LocationRef` is a bounded structured serialization of the canonical `LocationRef` from `task-package-schema.md`, not a redefinition. The containing source or repository reference MUST supply repository identity and the applicable baseline, worktree, implementation, run, or evidence identity; `scheme` and `value` supply the durable path, symbol, range, output, or equivalent locator; and `revision` preserves the exact immutable revision when material. A mutable local path alone is invalid.

### 3.4 BlockingRef

| Member | Requirement | Shape | Cardinality | Meaning | Activation condition |
|---|---|---:|---:|---|---|
| `blocker_id` | REQUIRED | stable identifier string | 1 | Durable identity of the blocker basis. | Always. |
| `source_ref` | REQUIRED | `SourceRef` | 1 | Source establishing the blocker. | Always. |
| `scope` | REQUIRED | bounded string | 1 | Exact blocked concern or action. | Always. |


#### Foundation compatibility mapping

This inventory-local `BlockingRef` is a bounded structured serialization of the canonical `BlockingRef` from `task-package-schema.md`, not a redefinition. `blocker_id` is the stable blocker identity, `source_ref` identifies its authoritative record under the canonical source-reference rules, and `scope` bounds the blocked concern. This structure does not define status semantics, resolution ownership, transition behavior, or authority beyond the governing Foundation contracts.

## 4. Scope

This schema defines:

- one inventory-document contract;
- one reusable Domain Skill entry contract;
- identity, provenance, revision, and lineage;
- availability and validation as separate dimensions;
- concerns, selection conditions, prerequisites, and workflow applicability;
- entrypoints and progressive-disclosure resources;
- neighboring and overlapping relationships;
- escalation metadata and canonical Domain status integration;
- compatibility with `Domain Skill Routing Result.skill_ref`;
- freeze, invalidation, replacement, and resume requirements.

## 5. Explicit Non-Goals

This schema MUST NOT:

- instantiate or populate a live inventory;
- claim that any named Domain Skill exists, is installed, is available, or is validated;
- define routing implementation behavior, a routing service, a Resume Agent, a routing Agent, or another future Agent;
- add an Agent, Skill, canonical artifact, or Task Work Package inventory slot;
- change the five-Agent architecture, ten canonical artifacts, 23 canonical statuses, Task State semantics, evidence validity, or `Domain Skill Routing Result` structure;
- store Task-specific selection, concern mapping, consumer mapping, blocker outcome, or implementation decisions.

The schema is neutral about whether any routing implementation exists, does not exist, is installed, or is available.

## 6. Core Concepts

- **Inventory document:** a versioned container of reusable entries. It is not a Task artifact and does not perform routing.
- **Entry:** reusable metadata for one distinguishable capability identity.
- **Stable identity:** immutable identity for one entry meaning.
- **Display name:** human-readable label that may change without changing identity when meaning is unchanged.
- **Availability:** whether the exact capability content can currently be located and used.
- **Validation:** whether authorized evidence supports a bounded claim about an exact target or revision.
- **Concern:** a structured capability area handled by an entry, not a Task-specific concern instance.
- **Selection condition:** a reusable positive or negative condition evaluated by an authorized routing producer.
- **Prerequisite:** a mandatory condition that must hold before selection or use.
- **Entrypoint:** the primary durable reference for loading the approved capability content.
- **Resource:** a focused supporting reference loaded only when its activation condition applies.
- **Relationship:** a typed link to another entry identity without asserting that the target currently exists.
- **Escalation:** a durable mapping from a routing-blocking condition to a canonical status, resolution authority, and blocker basis.

## 7. Inventory Document Schema

### 7.1 InventoryDocument fields

| Field | Requirement | Shape | Cardinality | Meaning | Activation condition |
|---|---|---:|---:|---|---|
| `schema_id` | REQUIRED | stable identifier string | 1 | Identity of this schema family. | Always. |
| `schema_revision` | REQUIRED | immutable revision string | 1 | Exact schema revision used to interpret the document. | Always. |
| `inventory_id` | REQUIRED | stable identifier string | 1 | Identity of this inventory document lineage. | Always. |
| `inventory_revision` | REQUIRED | immutable revision string | 1 | Exact content revision of the inventory document. | Always. |
| `authority_refs` | REQUIRED | array of `SourceRef` | 1..* | Durable authorities governing the inventory document. | Always. |
| `entries` | REQUIRED | array of `DomainSkillEntry` | 0..* | Reusable entries. An empty document is valid and does not mean all concerns are unsupported. | Always. |
| `provenance` | REQUIRED | `ProvenanceRecord` | 1 | Origin of this document revision without independently granting authority. | Always. |
| `predecessor_ref` | CONDITIONAL | `StableRef` | 0..1 | Prior inventory revision in the same lineage. | REQUIRED when this revision replaces an earlier revision. |
| `frozen_digest` | CONDITIONAL | SHA-256 string | 0..1 | Digest of the frozen bytes. | REQUIRED after the document revision is frozen. |

### 7.2 Document invariants

- `inventory_id` MUST remain stable across revisions in one lineage.
- `inventory_revision` MUST change after any material document change.
- Entry order MUST NOT affect selection, precedence, availability, validation, or authority.
- Duplicate active `entry_id` values are invalid.
- The document MUST NOT contain Task IDs, Task concern IDs, Task consumer references, or Task routing outcomes.

## 8. Domain Skill Entry Schema

### 8.1 DomainSkillEntry fields

| Field | Requirement | Shape | Cardinality | Meaning | Activation condition |
|---|---|---:|---:|---|---|
| `entry_id` | REQUIRED | stable identifier string | 1 | Immutable identity of this exact entry meaning. | Always. |
| `display_name` | REQUIRED | non-empty string | 1 | Human-readable Skill name, separate from identity. | Always. |
| `entry_revision` | REQUIRED | immutable revision string | 1 | Exact revision of this entry. | Always. |
| `purpose` | REQUIRED | bounded string | 1 | Reusable capability purpose, excluding Task-specific outcomes and implementation instructions. | Always. |
| `concerns` | REQUIRED | array of `ConcernDescriptor` | 1..* | Structured reusable concerns handled by the entry. | Always. |
| `positive_selection_conditions` | REQUIRED | array of `SelectionCondition` | 1..* | Conditions supporting possible selection. | Always. |
| `negative_selection_conditions` | REQUIRED | array of `SelectionCondition` | 0..* | Conditions that prohibit selection when matched. | Always; empty only when no authorized exclusion is known. |
| `prerequisites` | REQUIRED | array of `Prerequisite` | 0..* | Mandatory preconditions for selection or use. | Always. |
| `workflow_stage_applicability` | REQUIRED | array of `WorkflowApplicability` | 1..* | Approved stages where the entry may be relevant. | Always. |
| `availability` | REQUIRED | `AvailabilityRecord` | 1 | Current availability claim and basis. | Always. |
| `validation` | REQUIRED | `ValidationRecord` | 1 | Current validation state and bounded evidence relationship. | Always. |
| `entrypoint` | REQUIRED | `ContentReference` | 1 | Primary durable loading reference. | Always. |
| `resources` | REQUIRED | array of `ProgressiveResource` | 0..* | Focused progressive-disclosure resources. | Always. |
| `relationships` | REQUIRED | array of `SkillRelationship` | 0..* | Neighbor, overlap, or replacement relationships. | Always. |
| `escalations` | REQUIRED | array of `EscalationRule` | 1..* | Specific escalation mappings relevant to the entry. | Always. |
| `provenance` | REQUIRED | `ProvenanceRecord` | 1 | Origin of this entry revision. | Always. |
| `predecessor_ref` | CONDITIONAL | `StableRef` | 0..1 | Prior distinguishable entry in a replacement lineage. | REQUIRED for a material replacement. |
| `replacement_ref` | CONDITIONAL | `StableRef` | 0..1 | Authorized successor identity. | REQUIRED when the entry is deprecated and an approved replacement exists. |
| `maintenance_authority_ref` | CONDITIONAL | `SourceRef` | 0..1 | Durable maintenance authority. | REQUIRED only when supported by authority; otherwise omitted. |
| `frozen_digest` | CONDITIONAL | SHA-256 string | 0..1 | Digest of frozen entry bytes or canonical serialization. | REQUIRED after entry freeze when independently frozen. |

The entry MUST NOT contain `task_id`, Task-specific `concern_id`, Task-specific `consumer_refs`, `routing_result`, selected implementation authority, or Task blocker outcome.

## 9. Identity, Provenance, Revision, and Lineage

### 9.1 ProvenanceRecord

| Member | Requirement | Shape | Cardinality | Meaning | Activation condition |
|---|---|---:|---:|---|---|
| `producer_ref` | REQUIRED | `StableRef` | 1 | Identity of the producer or source. | Always. |
| `source_refs` | REQUIRED | array of `SourceRef` | 1..* | Durable sources from which the record was produced. | Always. |
| `produced_revision` | REQUIRED | immutable revision string | 1 | Revision produced by the identified producer. | Always. |
| `locator_ref` | CONDITIONAL | `LocationRef` | 0..1 | Retrieval location for the provenance object. | REQUIRED when identity alone cannot retrieve it. |

Provenance identifies origin. It MUST NOT independently grant routing, validation, review, maintenance, or implementation authority.

A material change to purpose, concern boundaries, selection semantics, prerequisite obligations, authority, availability meaning, validation meaning, entrypoint identity, or relationship precedence MUST create a distinguishable replacement identity. A non-material presentation correction MAY retain `entry_id` but MUST change `entry_revision` when frozen bytes change.

Critical linkage MUST use durable typed references and MUST NOT rely only on filename, heading, text matching, local path, list position, or conversation proximity.

## 10. Availability Model

### 10.1 AvailabilityRecord

| Member | Requirement | Shape | Cardinality | Meaning | Activation condition |
|---|---|---:|---:|---|---|
| `state` | REQUIRED | enum | 1 | One of `available`, `unavailable`, `deprecated`, `ambiguous`. This field describes retrievability or availability only and never carries validation status. | Always. |
| `basis_refs` | REQUIRED | array of `SourceRef` or `BlockingRef` | 1..* | Durable basis for the availability assertion. | Always. |
| `observed_revision` | REQUIRED | revision string | 1 | Entry or content revision to which the assertion applies. | Always. |
| `locator_ref` | CONDITIONAL | `LocationRef` | 0..1 | Current retrieval locator. | REQUIRED for an available state when identity alone cannot retrieve content. |
| `checked_at` | REQUIRED | timestamp string | 1 | Observation time, not proof by age alone. | Always. |

State semantics:

- `available`: exact content is currently retrievable at the recorded revision. This state makes no validation claim. The separate `ValidationRecord.state` may independently be `validated`, `unvalidated`, `stale`, or `invalid`.

The combined conditions required by consumers are derived without creating compound availability states:

- **available and validated** means `availability.state = available` and `validation.state = validated` for the same material revision and applicable scope.
- **available but unvalidated** means `availability.state = available` and `validation.state` is `unvalidated`, `stale`, or `invalid` for the applicable use, or no applicable active validation claim exists.
- `unavailable`: a selected or referenced capability identity is known, but required content cannot currently be loaded or verified. It is not the same as an unsupported concern.
- `deprecated`: the entry remains immutable historical material and is not active authority unless a governing contract explicitly permits historical use.
- `ambiguous`: availability or route applicability cannot be uniquely determined from authorized information. It is not the same as unavailable.

Availability MUST NOT be inferred from validation, and validation MUST NOT be inferred from availability. Changing `validation.state` MUST NOT change `availability.state`; changing `availability.state` MUST NOT change `validation.state`. The two records MAY vary independently within their own constraints. Consumers that require both retrievability and validated authority MUST evaluate both records explicitly. An available entry with `unvalidated`, `stale`, or `invalid` validation MUST remain `availability.state = available` and MUST NOT be treated as validated authority.

## 11. Validation Model

### 11.1 ValidationRecord

| Member | Requirement | Shape | Cardinality | Meaning | Activation condition |
|---|---|---:|---:|---|---|
| `state` | REQUIRED | enum | 1 | One of `validated`, `unvalidated`, `stale`, `invalid`. | Always. |
| `target_ref` | REQUIRED | `StableRef` | 1 | Exact validated subject. | Always. |
| `target_revision` | REQUIRED | immutable revision string | 1 | Exact revision bound to evidence. | Always. |
| `scope` | REQUIRED | `ValidationScope` | 1 | Bounded validated claim. | Always. |
| `evidence_refs` | REQUIRED | array of `SourceRef` | 0..* | Evidence supporting the state. | REQUIRED and non-empty when `state` is `validated`; otherwise MAY be empty. |
| `validation_authority_ref` | CONDITIONAL | `SourceRef` | 0..1 | Authority entitled to validate the bounded claim. | REQUIRED when validation depends on a distinct validating authority. |
| `context_refs` | REQUIRED | array of `SourceRef` | 0..* | Dependencies and context affecting applicability. | Always. |
| `invalidating_conditions` | REQUIRED | array of strings or typed condition references | 0..* | Material changes that make active evidence stale or invalid. | Always. |

### 11.2 ValidationScope

| Member | Requirement | Shape | Cardinality | Meaning | Activation condition |
|---|---|---:|---:|---|---|
| `concern_refs` | REQUIRED | array of local concern IDs | 1..* | Entry concerns covered by validation. | Always. |
| `selection_condition_refs` | REQUIRED | array of local condition IDs | 0..* | Selection semantics covered by validation. | Always. |
| `resource_refs` | REQUIRED | array of `StableRef` | 0..* | Entrypoint or resources covered by validation. | Always. |
| `limitations` | REQUIRED | array of bounded strings | 0..* | Explicit exclusions and limits. | Always. |

Validation freshness is determined from exact target revision, scope, context, dependencies, and invalidating changes, not age alone. Stale or invalid evidence remains immutable historical material but MUST NOT serve as active proof. Unsupported assertions, producer identity, availability, confidence, or mere presence MUST NOT become validation. Material change requires new evidence or a distinguishable replacement as governed by the change.

## 12. Concern and Selection Model

### 12.1 ConcernDescriptor

| Member | Requirement | Shape | Cardinality | Meaning | Activation condition |
|---|---|---:|---:|---|---|
| `concern_key` | REQUIRED | entry-local stable identifier | 1 | Reusable concern identity within the entry. | Always. |
| `category` | REQUIRED | bounded classification string | 1 | Capability category. | Always. |
| `scope` | REQUIRED | bounded string | 1 | What is handled. | Always. |
| `exclusions` | REQUIRED | array of bounded strings | 0..* | What is explicitly not handled. | Always. |
| `authority_refs` | REQUIRED | array of `SourceRef` | 1..* | Authority for the concern boundary. | Always. |

### 12.2 SelectionCondition

| Member | Requirement | Shape | Cardinality | Meaning | Activation condition |
|---|---|---:|---:|---|---|
| `condition_id` | REQUIRED | entry-local stable identifier | 1 | Identity of the condition. | Always. |
| `kind` | REQUIRED | enum `positive` or `negative` | 1 | Whether it permits consideration or prohibits selection. | Always. |
| `predicate` | REQUIRED | structured predicate object | 1 | Deterministic condition over authorized routing inputs. | Always. |
| `concern_refs` | REQUIRED | array of local concern IDs | 1..* | Concerns to which the condition applies. | Always. |
| `authority_refs` | REQUIRED | array of `SourceRef` | 1..* | Authority for the condition. | Always. |

### 12.3 Structured predicate object

| Member | Requirement | Shape | Cardinality | Meaning | Activation condition |
|---|---|---:|---:|---|---|
| `subject_kind` | REQUIRED | enum or governed identifier | 1 | Type of routing input examined. | Always. |
| `operator` | REQUIRED | governed operator string | 1 | Deterministic comparison. | Always. |
| `expected_value` | REQUIRED | scalar or governed list | 1 | Expected value or set. | Always. |
| `source_ref` | REQUIRED | `SourceRef` | 1 | Durable source of the evaluated fact. | Always. |

A matching negative condition MUST prohibit selection even if positive conditions or names match. Name matching alone is insufficient. All required positive conditions and prerequisites must be satisfied. Unresolved overlap MUST NOT be resolved by record order, confidence, proximity, or consumer preference.

## 13. Prerequisite Model

### 13.1 Prerequisite

| Member | Requirement | Shape | Cardinality | Meaning | Activation condition |
|---|---|---:|---:|---|---|
| `prerequisite_id` | REQUIRED | entry-local stable identifier | 1 | Identity of the obligation. | Always. |
| `description` | REQUIRED | bounded string | 1 | Human-readable obligation. | Always. |
| `predicate` | REQUIRED | structured predicate object | 1 | Deterministic satisfaction rule. | Always. |
| `required_for` | REQUIRED | enum set | 1..* | `selection`, `loading`, `use`, or another governed stage. | Always. |
| `failure_escalation_ref` | REQUIRED | local escalation-rule ID | 1 | Escalation used when unmet. | Always. |
| `authority_refs` | REQUIRED | array of `SourceRef` | 1..* | Authority making the prerequisite mandatory. | Always. |

Mandatory prerequisites MUST NOT be reduced to guidance. An unmet prerequisite stops the dependent action and invokes its specific escalation path. A consumer MUST NOT guess around it.

## 14. Workflow-Stage Applicability

### 14.1 WorkflowApplicability

| Member | Requirement | Shape | Cardinality | Meaning | Activation condition |
|---|---|---:|---:|---|---|
| `stage_ref` | REQUIRED | `StableRef` | 1 | Reference to an approved workflow stage or role boundary. | Always. |
| `relevance` | REQUIRED | bounded string | 1 | Why the entry is relevant at this stage. | Always. |
| `allowed_use` | REQUIRED | enum set | 1..* | Permitted use such as routing metadata, authority loading, or review reference. | Always. |
| `limitations` | REQUIRED | array of bounded strings | 0..* | Stage-specific restrictions. | Always. |

An entry MAY apply to several approved stages. Applicability MUST NOT create a workflow, add a transition, grant ownership of the stage, or authorize work assigned to another role.

## 15. Entrypoint and Progressive Disclosure

### 15.1 ContentReference

| Member | Requirement | Shape | Cardinality | Meaning | Activation condition |
|---|---|---:|---:|---|---|
| `content_ref` | REQUIRED | `StableRef` | 1 | Durable identity of the content. | Always. |
| `relationship` | REQUIRED | governed relationship string | 1 | Relationship to the entry, such as `entrypoint` or `supporting-resource`. | Always. |
| `revision` | CONDITIONAL | immutable revision string | 0..1 | Exact content revision. | REQUIRED when meaning or bytes may vary by revision. |
| `locator_ref` | CONDITIONAL | `LocationRef` | 0..1 | Durable retrieval locator. | REQUIRED when identity alone cannot retrieve content. |
| `digest` | CONDITIONAL | SHA-256 string | 0..1 | Exact frozen content digest. | REQUIRED when the referenced content is frozen and digest-governed. |

### 15.2 ProgressiveResource

| Member | Requirement | Shape | Cardinality | Meaning | Activation condition |
|---|---|---:|---:|---|---|
| `resource_id` | REQUIRED | entry-local stable identifier | 1 | Identity of the resource relation. | Always. |
| `reference` | REQUIRED | `ContentReference` | 1 | Durable supporting-content reference. | Always. |
| `purpose` | REQUIRED | bounded string | 1 | Focused reason to load the resource. | Always. |
| `activation_condition` | REQUIRED | structured predicate or `always-after-entrypoint` | 1 | Deterministic loading trigger. | Always. |
| `load_order` | CONDITIONAL | positive integer | 0..1 | Relative order among simultaneously activated resources. | REQUIRED only when order is semantically material. |
| `required_when_activated` | REQUIRED | Boolean | 1 | Whether failure to load blocks dependent work. | Always. |

The entrypoint is distinct from supporting resources. Routing MUST be possible from bounded inventory metadata without loading or copying the complete Skill. After selection, consumers load the entrypoint and only activated resources. Complete Skill bodies MUST NOT be copied into entries.

## 16. Neighbor and Overlap Relationships

### 16.1 SkillRelationship

| Member | Requirement | Shape | Cardinality | Meaning | Activation condition |
|---|---|---:|---:|---|---|
| `relationship_id` | REQUIRED | entry-local stable identifier | 1 | Identity of the relationship assertion. | Always. |
| `type` | REQUIRED | enum `neighbor`, `overlap`, `predecessor`, `replacement` | 1 | Explicit relationship meaning. | Always. |
| `target_ref` | REQUIRED | `StableRef` | 1 | Target entry identity without asserting current existence. | Always. |
| `scope` | REQUIRED | `RelationshipScope` | 1 | Bounded concerns and conditions affected. | Always. |
| `precedence` | CONDITIONAL | enum `self`, `target`, `none` | 0..1 | Authorized precedence. | REQUIRED only when authority explicitly establishes precedence. |
| `precedence_authority_ref` | CONDITIONAL | `SourceRef` | 0..1 | Authority for precedence. | REQUIRED whenever `precedence` is `self` or `target`. |

### 16.2 RelationshipScope

| Member | Requirement | Shape | Cardinality | Meaning | Activation condition |
|---|---|---:|---:|---|---|
| `concern_refs` | REQUIRED | array of local concern IDs or durable target concern refs | 1..* | Overlapping or neighboring concern subset. | Always. |
| `condition_refs` | REQUIRED | array of condition IDs | 0..* | Affected selection conditions. | Always. |
| `exclusions` | REQUIRED | array of bounded strings | 0..* | Explicit limits. | Always. |

If overlapping candidates lack authoritative precedence, the route is ambiguous and MUST be escalated. Record order MUST NOT decide selection.

## 17. Escalation Model

### 17.1 EscalationRule

| Member | Requirement | Shape | Cardinality | Meaning | Activation condition |
|---|---|---:|---:|---|---|
| `escalation_id` | REQUIRED | entry-local stable identifier | 1 | Identity of the escalation rule. | Always. |
| `condition` | REQUIRED | structured predicate | 1 | Deterministic trigger. | Always. |
| `status` | REQUIRED | canonical status enum | 1 | Specific Domain status or governed fallback. | Always. |
| `resolution_authority_ref` | REQUIRED | `SourceRef` | 1 | Authority entitled to resolve the condition. | Always. |
| `blocker_ref` | REQUIRED | `BlockingRef` | 1 | Durable blocker basis. | Always. |
| `required_action` | REQUIRED | bounded enum or string | 1 | Stop, reroute, obtain capability, or human decision as authorized. | Always. |

Required distinctions:

- `DOMAIN_SKILL_NOT_AVAILABLE`: a selected required capability or authorized lookup is unavailable or unverified.
- `DOMAIN_ROUTING_AMBIGUOUS`: an authorized route cannot be uniquely selected.
- `DOMAIN_CONCERN_UNSUPPORTED`: no approved capability or lookup supports the concern.

The specific Domain status takes precedence over generic `BLOCKED`. An unavailable capability MUST NOT be replaced with general knowledge. A consumer MUST NOT resolve ambiguity or substitute another capability locally. Unsupported concern MUST NOT be reported as unavailable.

## 18. Routing-Result Integration

An inventory entry is referenceable from `Domain Skill Routing Result.skill_ref` using a `SourceRef` whose subject resolves to `entry_id` and whose revision or locator is present when required.

The Routing Result remains the sole location for Task-specific:

- selection outcome;
- Task concern identity and mapping;
- Task consumer mapping;
- applicable Task requirements;
- Task blocker or escalation outcome;
- routing observations.

Inventory metadata MUST NOT duplicate that outcome. A material inventory change MAY invalidate an affected frozen Routing Result, but the frozen result remains historical and MUST NOT be rewritten. Rerouting produces a distinguishable replacement result.

## 19. Freeze, Invalidation, and Replacement

Freeze preserves bytes and identity; it does not prove validity or correctness. Consumers are read-only with respect to entries.

A frozen entry MUST NOT be materially edited in place. Material correction creates a distinguishable replacement and preserves predecessor lineage. Deprecated, stale, and invalid entries remain historical but MUST NOT provide active authority. Invalidating change MUST affect only entries, routing results, validation claims, and consumers that depend on the changed subject. Unrelated entries remain active.

Maintenance authority MUST be recorded only when supported. If maintenance authority is external, the entry references it and does not invent a global inventory owner.

## 20. Resume Requirements

Durable state MUST permit reconstruction, without conversation history, of:

- exact inventory and entry identity;
- entry and content revision;
- required locators;
- provenance and source relationships;
- availability state and basis;
- validation state, exact target, scope, and evidence;
- relationship and ambiguity basis;
- predecessor and replacement lineage;
- affected dependents after a material change.

Resume MUST reject ambiguous identity or revision. It MUST NOT infer authority from prior conversation, hidden state, filename matching, or remembered reasoning.

## 21. Adversarial Interpretation Rules

The following interpretations are prohibited:

1. Treating an available entry as validated without applicable evidence.
2. Treating unavailable as unsupported, or ambiguous as unavailable.
3. Selecting by display-name match alone.
4. Resolving overlap by record order.
5. Treating prerequisites as advice.
6. Letting provenance grant authority.
7. Carrying validation to a different material revision.
8. Editing a frozen identity after material change.
9. Inserting a Task-specific Routing Result into inventory metadata.
10. Substituting general knowledge or a consumer-chosen Skill for missing authority.
11. Naming or asserting the existence, non-existence, installation, or availability of an implementation-specific routing capability.
12. Copying complete Skill content into an entry instead of using progressive disclosure.
13. Treating stage applicability as authority to perform stage work.
14. Using age alone to establish validation freshness.
15. Expanding the canonical Agents, statuses, artifacts, Task State, or evidence model.

## 22. Synthetic Conforming Examples

The following names are synthetic labels and do not claim system existence.

### 22.1 Available but unvalidated

```yaml
entry_id: synthetic.entry.alpha.v1
display_name: Synthetic Capability Alpha
entry_revision: "1"
availability:
  state: available
  observed_revision: "1"
  basis_refs:
    - subject: synthetic.content.alpha
      relationship: availability-observation
validation:
  state: unvalidated
  target_ref: synthetic.entry.alpha.v1
  target_revision: "1"
  scope:
    concern_refs: [alpha-concern]
    selection_condition_refs: []
    resource_refs: []
    limitations: [No active validation evidence]
  evidence_refs: []
```

### 22.2 Deprecated entry with replacement

```yaml
entry_id: synthetic.entry.beta.v1
display_name: Synthetic Capability Beta
entry_revision: "1"
availability:
  state: deprecated
  observed_revision: "1"
  basis_refs:
    - subject: synthetic.deprecation.decision
      relationship: deprecation-authority
replacement_ref:
  subject: synthetic.entry.beta.v2
  relationship: replacement
```

The replacement is separately evaluated. It does not inherit validation merely because it is the successor.

### 22.3 Unresolved overlap

Two synthetic entries may declare an overlap relationship over the same concern. If neither contains authority-backed precedence, the routing producer returns `DOMAIN_ROUTING_AMBIGUOUS`. Reversing record order produces the same result.

## 23. Final Invariants

1. This artifact defines a schema only and does not create a live inventory.
2. No named Domain Skill is presumed to exist.
3. Stable identity is distinct from display name.
4. Availability and validation are separate dimensions.
5. `availability.state = available` combined with `validation.state = unvalidated`, `stale`, or `invalid` is not validated authority.
6. `unavailable`, `ambiguous`, and unsupported concern remain distinct.
7. Positive conditions permit consideration; negative conditions prohibit selection when matched.
8. Name matching alone is insufficient.
9. Mandatory prerequisites block dependent work when unmet.
10. Entrypoints and resources use durable references and progressive disclosure.
11. Overlap is resolved only by authority-backed precedence; otherwise it escalates.
12. Validation binds to exact target, revision, scope, context, and evidence.
13. Task-specific routing data remains in `Domain Skill Routing Result`.
14. Frozen material is not materially edited in place.
15. Resume uses durable state and no conversation memory.
16. Consumers are read-only and cannot guess, substitute, or expand authority.
17. Material change causes selective invalidation and distinguishable replacement.
18. Normative rules are expressed once or by an unambiguous reusable reference; token efficiency MUST NOT weaken semantics, authority, evidence, or resume durability.
