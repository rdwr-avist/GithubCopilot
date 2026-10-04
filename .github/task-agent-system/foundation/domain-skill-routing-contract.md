# Domain Skill Routing Contract

## 1. Purpose

This Foundation contract defines deterministic Task-level routing from durable Task concerns to applicable Domain Skills or authoritative lookups. It completes the behavioral contract for the approved Domain Skill Routing Result while preserving the structure, cardinalities, identities, and relationships defined by the Task Package Schema.

Routing identifies applicable professional authority and relevant constraints for exact downstream consumers. It does not perform the governed professional work.

## 2. Authority Order and Normative Conventions

Use the supplied active authority set.

Do not substitute or infer authorities. The following Foundation contracts are binding within their defined scopes:

1. The Task Package Schema governs canonical artifact structure, references, cardinalities, and package relationships.
2. The Task Authority Model governs ownership, consumer authority, freeze, validity, invalidation, replacement, and rerouting.
3. The Minimum Evidence Model governs evidence identity, target and revision binding, scope, freshness, validity, and proof strength.
4. The Task Status Model governs canonical status meanings, emitters, resolution ownership, and specific-status precedence.
5. The Resume Invariant governs durable reconstruction, identity coherence, selective invalidation, revalidation, and continuation without conversation history.
6. The Domain Skill Inventory Schema governs reusable inventory identity, selection conditions, prerequisites, availability, validation, progressive disclosure, relationships, and escalation metadata.

A derived interpretation MUST NOT override a governing contract within that contract's scope. Where binding authorities conflict within the same scope, routing MUST stop and preserve a durable blocker for the authority that owns the disputed decision.

In this contract:

- **MUST** and **MUST NOT** express mandatory behavior.
- **SHOULD** expresses recommended behavior that may be departed from only for a recorded authority-backed reason.
- **MAY** expresses permitted behavior.
- A conditional obligation applies only when its stated activation condition is true.

Conversation history, hidden state, remembered reasoning, filename similarity, and unstored interpretation are not authority.

## 3. Scope

This contract governs:

- routing inputs and preconditions;
- extraction and normalization of in-scope Task concerns;
- candidate discovery and eligibility;
- deterministic selection and authority-backed precedence;
- Routing Result outcomes and field obligations;
- confidence and ambiguity handling;
- unavailable authority and unsupported concerns;
- multi-Skill Tasks;
- stage-specific production and exact consumer binding;
- progressive disclosure and bounded loading;
- freeze, validity, selective invalidation, and rerouting;
- escalation and durable resume.

## 4. Explicit Non-Goals

Routing MUST NOT:

- implement a router, routing service, dispatcher, scheduler, state machine, inventory service, cache, or loader;
- create or populate a live Domain Skill inventory;
- define requirements, acceptance criteria, Design, architecture, Components, infrastructure, APIs, implementation, tests, evidence truth, or Review judgment;
- select an architecture or assign Component ownership or responsibilities;
- perform repository-wide analysis or Component analysis;
- invent infrastructure or replace missing authority with general knowledge;
- replace, restate, merge, or copy a Domain Skill body;
- load unrelated Skills or resources;
- add or redefine an Agent, canonical artifact, package slot, or canonical status;
- prove, approve, validate, or complete itself.

The approved five-Agent architecture, ten canonical artifacts, and 23 canonical statuses remain unchanged.

## 5. Core Concepts

### 5.1 Task concern

A **Task concern** is an independently routable domain question or obligation already present in an exact Task Contract requirement, acceptance criterion, scope item, dependency, or approved decision. A concern does not create new Task scope.

### 5.2 Candidate

A **candidate** is an inventory entry or authoritative lookup whose approved metadata may apply to a Task concern. Candidate status does not imply eligibility or selection.

### 5.3 Eligibility

A candidate is **eligible** only when all applicable positive conditions and mandatory prerequisites for selection are satisfied, no matching negative condition prohibits selection, the workflow stage permits the intended use, and the candidate is not excluded by active lifecycle or validation facts.

### 5.4 Routing outcome

A **routing outcome** is the bounded classification recorded for one routing entry. It is distinct from a Task status, although blocking outcomes require integration with the applicable canonical Domain status.

### 5.5 Applicability and consumer binding

**Applicability** bounds the concern, stage, lifecycle point, and permitted use of an entry. **Consumer binding** names the exact downstream roles or artifact types authorized to consume it. A consumer MUST ignore an entry that does not name that consumer or its artifact type.

### 5.6 Routing basis

The **routing basis** is the durable set of Task references, inventory or authority revisions, conditions, prerequisites, relationships, availability, validation, constraints, and evidence used to reach the outcome.

## 6. Producing Roles and Lifecycle Placement

A Domain Skill Routing Result MUST be produced only after a Task Contract exists.

- DP-Task-Planner produces planning-time routing.
- DP-Task-Orchestrator produces orchestration-time routing.

Each result MUST record the actual producing role. A role MAY route only within its assigned lifecycle authority. Producing a result does not grant the producer the professional authority of the selected Skill or lookup.

Consumers are read-only with respect to a published Routing Result. Operational or manual state management may record and route an unresolved condition, but MUST NOT resolve the underlying professional choice unless separately authorized.

Before a Task Contract exists, no Domain Skill Routing Result is produced. Any routing dependency remains a durable planning or Task State blocker until the Task Contract is available.

## 7. Routing Inputs and Preconditions

Routing requires the following durable inputs:

1. **Task Contract identity:** exact Task Contract reference and revision.
2. **Concern sources:** exact references to the requirements, criteria, scope items, dependencies, or approved decisions from which concerns are extracted.
3. **Producing context:** actual producing role and lifecycle stage.
4. **Consumers:** exact downstream roles or artifact types requiring the routed authority.
5. **Inventory basis:** approved inventory identity and revision when inventory metadata is used.
6. **Lookup basis:** exact authoritative-source identity, revision, and locator when direct authoritative lookup is considered.
7. **Fixed decisions and constraints:** durable references to applicable decisions and constraints.
8. **Prerequisite inputs:** facts needed to evaluate mandatory prerequisites for selection, loading, or use.
9. **Availability and validation:** independent current facts when required for the intended use.
10. **Existing blockers:** durable references to unresolved authority, availability, or routing conditions.

Critical linkage MUST use durable identities and revisions. A filename, heading, local path, or textual similarity alone is insufficient.

If a mandatory input is unavailable or revision-mismatched, routing MUST stop with a durable blocker rather than guess.

## 8. Concern Extraction

The producing role MUST extract only in-scope concerns from the Task Contract and its approved decisions.

For each concern it MUST:

1. assign a stable `concern_id` unique within the Routing Result;
2. retain an exact `concern_ref` to its source;
3. preserve sufficient granularity for deterministic eligibility evaluation;
4. normalize duplicate statements without losing any source linkage;
5. retain material distinctions among consumers, stages, constraints, and prerequisites.

A mixed or compound source statement MUST be decomposed into distinct routed sub-concerns only when independent routing is required. Each sub-concern MUST have a unique `concern_id`, MUST retain traceability to the common source, and MUST remain within the source statement's approved meaning.

Concern extraction MUST NOT invent requirements, widen scope, select architecture, explore the repository broadly, or perform Component analysis.

## 9. Candidate Discovery and Eligibility

### 9.1 Candidate sources

Candidates MUST come from:

- reusable metadata in an approved Domain Skill inventory revision; or
- an approved authoritative lookup source.

Task-specific selections MUST NOT be written into reusable inventory entries.

### 9.2 Eligibility evaluation

For every candidate, the producing role MUST evaluate:

1. all applicable positive selection conditions;
2. every applicable negative selection condition;
3. mandatory prerequisites for the specific governed action;
4. workflow-stage applicability and limitations;
5. lifecycle state, including deprecation or replacement;
6. availability for the required content and use;
7. validation state, scope, target, revision, context, and freshness when validated authority is required;
8. relationships, overlap, and explicit precedence authority;
9. consumer and applicability fit.

A positive match permits consideration only. A matching negative condition prohibits selection. Name or text similarity is never sufficient.

A deprecated or replaced entry is historical rather than active authority unless an approved rule explicitly permits the intended historical use. Stale, invalid, or inapplicable validation is not active proof and MUST NOT rewrite the separate availability state.

Inventory record order MUST NOT affect eligibility or selection.

## 10. Decision Rules and Precedence

Routing MUST be deterministic from authorized durable facts.

The producing role MUST:

1. remove candidates excluded by negative conditions, prerequisites, stage, lifecycle, availability, validation, or applicability;
2. apply precedence only when an approved authority explicitly defines it;
3. preserve the exact concern and consumers throughout evaluation;
4. record the selected authority and bounded constraints without adding implementation decisions.

Authoritative exclusions override positive name or concern similarity.

When several eligible candidates overlap and no approved precedence uniquely selects one, the result is ambiguous. Record order, confidence, proximity, convenience, consumer preference, familiarity, or prior conversation MUST NOT break the tie.

If selected constraints conflict and applicable authority does not resolve the conflict, the affected routing MUST stop as ambiguous or otherwise blocked according to the exact cause.

## 11. Domain Skill Routing Result Contract

The Domain Skill Routing Result structure is owned by the Task Package Schema. This section clarifies its routing semantics and MUST NOT be interpreted as an incompatible redefinition.

### 11.1 Common artifact header

Each result MUST preserve the approved common header, including:

- `schema_ref`;
- `artifact_type`;
- `artifact_id`;
- `task_id`;
- `producing_role_or_source`;
- conditional `predecessor_artifact_ref`;
- `source_refs`;
- `created_at`;
- `updated_at`.

### 11.2 Artifact-specific structure

Each result MUST contain exactly one `task_contract_ref` and `routing_entries` with cardinality `1..*`.

Each routing entry MUST contain:

- `concern_id`, unique within the result;
- `concern_ref`, resolving to the exact Task Contract source;
- `routing_result`, one bounded outcome;
- `applicability`;
- `consumer_refs` with cardinality `1..*`;
- `constraint_refs` with cardinality `0..*`.

Conditional fields are:

- `skill_ref` with cardinality `0..1`, required when a Domain Skill is selected;
- `authority_lookup_ref` with cardinality `0..1`, required when an authoritative lookup is selected;
- `blocking_ref` with cardinality `0..1`, required for unresolved availability or routing conflict.

At most one Skill is selected per routing entry. Multiple Skills for one Task require distinct routed concerns or distinct traceable sub-concerns.

### 11.3 Durable binding

The result and its references MUST preserve enough identity and revision data to reconstruct:

- the Task Contract revision;
- inventory and entry revisions;
- selected Skill, lookup, and content references;
- consumers and applicability;
- constraints and prerequisites;
- availability and validation basis;
- blockers and escalation;
- predecessor and replacement lineage.

A route reference selects authority by durable reference. It does not copy the authority body.

## 12. Routing Outcomes

The contract defines these bounded outcome semantics. Their serialized labels MUST remain compatible with the Task Package Schema and MUST NOT create new canonical Task statuses.

### 12.1 Selected Domain Skill

Use when one eligible Domain Skill is uniquely selected for the concern and intended consumers.

Required effects:

- `skill_ref` is present and revision-bound;
- `authority_lookup_ref` is absent unless the Schema and an approved authority require an additional distinct lookup;
- consumers and applicability are exact;
- relevant constraints are referenced;
- no ambiguity blocker is present.

### 12.2 Selected authoritative lookup

Use when the concern is routed to an approved authoritative source rather than a Domain Skill.

Required effects:

- `authority_lookup_ref` is present and revision-bound;
- `skill_ref` is absent;
- consumers, applicability, and constraints are exact;
- no Skill is fabricated.

### 12.3 No routing required

Use only when the in-scope concern is conclusively determined not to require additional Domain Skill or authoritative lookup authority for the declared consumers and stage.

Required effects:

- no `skill_ref` or `authority_lookup_ref` is fabricated;
- the basis for no additional routing is durable and bounded;
- this outcome does not waive another Foundation obligation.

### 12.4 Unresolved ambiguity

Use when the concern is known but authorized facts cannot uniquely select a route.

Required effects:

- candidate basis and unresolved distinction are durable;
- `blocking_ref` is present;
- Task status is `DOMAIN_ROUTING_AMBIGUOUS`;
- consumers MUST NOT choose locally.

### 12.5 Selected capability unavailable or unverified

Use when a specifically required Skill, lookup, entrypoint, or activated required resource cannot be retrieved or cannot be treated as valid authority for the intended use.

Required effects:

- the selected or required authority identity remains exact;
- `blocking_ref` is present;
- Task status is `DOMAIN_SKILL_NOT_AVAILABLE`;
- availability and validation remain distinct;
- no general-knowledge or neighboring-Skill substitution occurs.

### 12.6 Unsupported in-scope concern

Use after complete approved inspection finds no supporting Skill or authoritative lookup.

Required effects:

- `blocking_ref` is present;
- Task status is `DOMAIN_CONCERN_UNSUPPORTED`;
- human authority decides whether to add authority, change scope, split the Task, or remain blocked;
- no candidate is invented.

## 13. Confidence and Ambiguity

Confidence metadata is optional. If represented, it MUST be explanatory, evidence-bounded, and non-authoritative. It MUST identify the basis and limits of the assessment.

Confidence MUST NOT:

- override positive or negative conditions;
- waive a prerequisite;
- transform unavailable or unvalidated content into valid authority;
- create precedence;
- convert ambiguity or unsupported concern into selection;
- replace evidence or source authority.

Low confidence MUST NOT silently become selection. High confidence MUST NOT create authority.

Ambiguity records MUST preserve the candidate set, applicable facts, unresolved distinction, affected concerns and consumers, blocker, resolution authority, pending action, and resume target.

## 14. Missing Authority and Escalation

The following conditions are distinct:

- `DOMAIN_SKILL_NOT_AVAILABLE`: a specifically selected or required capability or authoritative lookup is unavailable, unverified, or unloadable for valid use.
- `DOMAIN_ROUTING_AMBIGUOUS`: the concern is known but an authorized route cannot be uniquely selected.
- `DOMAIN_CONCERN_UNSUPPORTED`: no approved capability or authoritative lookup supports the in-scope concern.

The most specific applicable Domain status MUST be used instead of generic `BLOCKED`. Historical blocker lineage MUST remain durable when a generic blocker is later refined.

Every escalation MUST identify:

- exact concern and consumers;
- selected, required, or candidate authority identities and revisions;
- exact cause;
- durable blocker;
- resolution owner with authority over the cause;
- pending action;
- resume target;
- required rerouting, reinspection, or revalidation.

Consumers, state management, and unrelated roles MUST NOT resolve the issue locally. General knowledge, guessed APIs, invented infrastructure, and neighboring Skills are prohibited substitutes.

## 15. Multi-Skill Tasks

A Task MAY contain several routing entries for distinct concerns.

A compound source concern that requires several capabilities MUST be decomposed into distinct routed sub-concerns when the approved source supports independent obligations. Each sub-concern:

- has a unique `concern_id`;
- retains exact source traceability;
- selects at most one Skill;
- has entry-specific consumers, applicability, constraints, prerequisites, and blockers.

The decomposition MUST NOT invent scope or duplicate an unchanged concern merely to bypass `skill_ref` cardinality.

Ordering among routed capabilities is recorded only when semantically material and established by authority. Overlapping or conflicting constraints are reconciled only by the applicable authority. Unresolved conflict blocks affected routing.

Multi-Skill routing MUST NOT merge Skill bodies, create an implementation plan, duplicate authority, or load unrelated Skills or resources.

## 16. Stage-Specific Routing and Consumer Binding

Planning-time and orchestration-time routing remain distinct producing contexts. Every result records its actual producer.

Every routing entry MUST name exact consumer roles or artifact types. A consumer may use only entries naming it or its artifact type, and only for the declared applicability and stage.

Stage applicability constrains use but does not authorize one role to perform another role's work.

Later-stage routing MUST NOT rewrite an earlier frozen result. If later routing changes meaning, selection, consumers, applicability, constraints, or blockers, the producer MUST create a distinguishable replacement result with required predecessor lineage.

The Routing Result MUST supply each named consumer with the exact selected content and revision required for governed work. Consumers MUST use that supplied selection and MUST NOT independently reselect it. The Routing Result does not perform the governed work for them.

## 17. Progressive Disclosure and Loading

Routing MUST use bounded inventory metadata before loading complete Skill content.

After selection:

1. the named consumer uses the selected entry and revision supplied by the Routing Result;
2. the consumer loads the approved entrypoint;
3. the consumer loads only resources whose activation conditions apply to the concern, stage, and intended use;
4. a required activated resource that cannot be loaded blocks dependent work;
5. load order is applied only when authority records it as semantically material.

The routing producer MUST NOT load unrelated Skills merely to increase confidence or gather background. The Routing Result carries durable references, applicability, consumers, and bounded constraints rather than copied source bodies.

A valid entrypoint does not compensate for an unavailable required activated resource. Partial loading does not authorize continuation when required authority is missing.

## 18. Constraints and Evidence Boundaries

`constraint_refs` select relevant constraints or required lookups by reference. They MUST NOT copy complete Skill or Foundation content.

Where routing relies on inventory inspection, availability, validation, authoritative lookup, relationship precedence, or rerouting basis, the supporting source or evidence MUST:

- resolve to the exact target and revision;
- identify the performed inspection or action;
- preserve scope, context, result, and provenance;
- be fresh and valid for the intended claim;
- avoid aggregation across incompatible identities or revisions.

Presence, producer identity, confidence, or assertion alone does not prove validity. Broken, stale, target-mismatched, or insufficient evidence MUST be refreshed by the proper authority. Routing cannot relabel defective evidence as sufficient.

A Routing Result cannot validate, approve, or prove itself.

## 19. Freeze, Validity, Invalidation, and Rerouting

### 19.1 Freeze

A Routing Result freezes when published for its named consumers. Consumers are read-only. Freeze preserves bytes and meaning but does not guarantee continuing validity.

### 19.2 Validity

An entry remains valid only while its Task concern, source authority, inventory and entry revisions, selected content, consumers, applicability, prerequisites, availability, validation, constraints, and precedence basis remain materially valid for the intended use.

### 19.3 Material invalidation triggers

Affected routing MUST be re-evaluated after a material change to:

- Task concern or source requirement;
- Task scope or dependency;
- intended consumers or lifecycle stage;
- inventory or entry revision;
- selected Skill or authoritative lookup revision;
- availability or validation;
- mandatory prerequisite;
- relationship or precedence authority;
- relevant constraint source;
- discovered routing conflict.

### 19.4 Selective effect

Invalidation is selective. Only affected entries and dependents become inactive. Unaffected entries and consumers remain valid when their basis remains valid.

An invalidated or superseded result remains immutable historical material but MUST NOT be used for unsupported active work.

### 19.5 Replacement and ownership

A frozen result MUST NOT be materially edited in place. Rerouting creates a distinguishable replacement. `predecessor_artifact_ref` MUST be present when needed to prevent mixing or preserve resume lineage.

The applicable producing role owns rerouting. If resolution requires requirement, Design, inventory-maintenance, validation, or human source authority, that authority resolves its portion before the producer issues a replacement.

## 20. Durable Resume

Resume MUST reconstruct correct continuation without conversation history, hidden state, or remembered interpretation.

For routing, durable state MUST identify:

- exact Task and Routing Result identities;
- actual producing role;
- Task Contract reference and revision;
- concern identities and source references;
- active inventory and entry identities and revisions;
- selected Skill or lookup references and revisions;
- entrypoint and activated resource requirements where applicable;
- consumer mappings and applicability;
- constraints and prerequisites;
- availability and validation basis;
- evidence or source references required for the routing claim;
- blocker and canonical Domain status;
- resolution authority, pending action, and resume target;
- predecessor and replacement lineage;
- selective invalidation impact and required revalidation;
- authorized next action.

Resume identifies the next authorized action but does not perform another role's governed work.

## 21. Adversarial Interpretation Rules

The following interpretations are invalid:

- selecting a Skill because its name resembles the concern;
- allowing a positive match to override a negative condition;
- treating prerequisites as guidance;
- using inventory order or confidence as precedence;
- treating availability as validation or validation as availability;
- using stale validation as current proof;
- letting a consumer choose among ambiguous candidates;
- substituting a neighboring Skill locally;
- using general knowledge when authority is missing;
- loading unrelated Skills or resources;
- copying a Skill body into a Routing Result;
- selecting architecture or performing Component analysis during routing;
- allowing provenance to grant authority;
- overwriting a frozen result after material change;
- invalidating unrelated entries without a dependency basis;
- assuming an artifact from another conversation or a future F-series task exists.

## 22. Synthetic Non-Normative Examples

These examples illustrate the rules and do not assert that any implementation-specific Skill is installed or available.

### 22.1 Unique Skill selection

Synthetic concern `SYN-C-1` has exactly one eligible, available, and validly validated synthetic Skill entry. All selection prerequisites are satisfied and no negative condition matches. The entry selects that Skill by exact revision for the named consumer and references applicable constraints. It does not select architecture.

### 22.2 Authoritative lookup

Synthetic concern `SYN-C-2` has no applicable Skill entry, but an approved authoritative lookup uniquely governs it. The entry records `authority_lookup_ref`, omits `skill_ref`, and binds the lookup to exact consumers.

### 22.3 Compound concern

One synthetic source statement contains two independently routable obligations. It is decomposed into `SYN-C-3A` and `SYN-C-3B`, both referencing the same approved source. Each entry has one distinct Skill reference and entry-specific consumers.

### 22.4 Ambiguity

Two synthetic Skills satisfy the same concern and have no authority-backed precedence. Reversing inventory order does not change the outcome. Routing records the candidate basis, `DOMAIN_ROUTING_AMBIGUOUS`, and a durable blocker.

### 22.5 Selective invalidation

A frozen synthetic result contains independent entries A and B. A Task scope change affects only A. Entry A and its dependents are invalidated and rerouted through a replacement result; B remains active if its basis is unchanged.

## 23. Token Efficiency Invariants

The contract and each Routing Result MUST minimize unnecessary context without weakening semantics:

1. State each authoritative rule once or reuse it through an unambiguous reference.
2. Do not duplicate complete Foundation contract sections or Domain Skill bodies.
3. Use bounded inventory metadata for routing.
4. Load only selected entrypoints and activated resources.
5. Carry durable references and bounded constraints instead of copied source bodies.
6. Keep examples and repeated summaries proportionate to semantic value.
7. Never trade away authority, ambiguity handling, evidence validity, consumer binding, invalidation, or resume correctness for concision.

## 24. Final Invariants

A conforming routing process and result satisfy all of the following:

- A Task Contract exists before routing output.
- Every concern has stable identity and exact source traceability.
- Candidate selection relies only on approved inventory metadata or authoritative lookups.
- Negative conditions and mandatory prerequisites are enforced.
- Precedence is authority-backed and record order is irrelevant.
- Availability and validation remain separate.
- Each routing entry selects at most one Skill and names exact consumers.
- Multi-Skill Tasks use distinct routed concerns or traceable sub-concerns.
- Routing never selects architecture, invents infrastructure, replaces a Skill, or becomes Component analysis.
- Progressive disclosure excludes unrelated content.
- Ambiguity, unavailable authority, and unsupported concern remain distinct and use their specific canonical Domain statuses.
- Frozen results are immutable; material changes create distinguishable replacements.
- Invalidation is selective and historical lineage is durable.
- Resume works without conversation history.
- The approved five Agents, ten canonical artifacts, 23 canonical statuses, Task Package Schema, Domain Skill Inventory Schema, evidence authority, Task State semantics, and resume authority remain unchanged.
