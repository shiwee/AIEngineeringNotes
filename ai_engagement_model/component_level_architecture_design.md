# Component-Level Architecture Design

## 5-Level AI Engagement Model for Enterprise Platforms

## 1. Purpose and Audience

This document translates the 5-Level AI Engagement Model and logical system architecture into build guidance for product, system, platform, security, and delivery teams. It describes the capabilities and components required at each level, the foundations that should be reused, their logical interactions, and observable acceptance criteria.

The architecture is reusable across traditional enterprise platforms. mNet is a grounding example of a mature cloud platform with established business capabilities, SSO, multiple tenants, and resource-level permissions. Its user-uploaded network data is evaluated within the platform and is not connected to a live network. Other platforms may have different identity, resource, and operational boundaries; their authoritative controls remain controlling.

This is a technology-neutral logical design. It does not select a model, agent framework, retrieval product, protocol, cloud service, deployment topology, or specific service decomposition.

## 2. Design Rules

1. Assess engagement level per capability or scenario, not for a whole product.
2. Treat engagement and authority as separate dimensions. A level never grants permission by itself.
3. Keep business rules, calculations, validation, and state changes in authoritative platform capabilities.
4. Use deterministic workflows for known paths. Use dynamic reasoning where ambiguity or investigation adds value.
5. Enforce identity, tenant, resource, and action authorization at trusted service boundaries, not by model instruction.
6. Keep L3 at investigation and recommendation unless a separate, explicit authorization contract is invoked. L4 owns a bounded goal-pursuit loop, not unrestricted authority.
7. L5 is approved-signal analysis, catalog matching, and proposal generation only. Humans own product decisions and engineering implementation.
8. Build foundations for the scenarios that need them. Do not require every capability to mature through L1 to L5.

## 3. Logical Component Map

```text
User / Enterprise Client
          |
          v
Interaction and Intent Boundary
          |
          +---- Identity, Tenant and Resource Context
          |
          +---- Policy Enforcement and Approval
          |
          +---- Context, Grounding and Evidence <---- Authoritative Platform Data
          |                                             and Approved Knowledge
          +---- Capability Catalog --> Capability Adapters --> Platform Services
          |                                                   APIs, workflows,
          |                                                   calculations, data
          +---- Reasoning and Planning
          |          |
          |          +---- L3 Recommendation
          |          +---- L4 Goal and Autonomy Controller
          |                       |
          |                       +--> Workflow and Execution Coordinator
          |
          +---- Evidence, Audit and Observability
                         |
                         +--> Minimized / Anonymized L5 Signals
                                  |
                                  v
                         Capability Evolution Analysis
                                  |
                                  v
                         Capability Evolution Proposal
                                  |
                                  v
                         Human Product / Platform Governance
```

These are logical responsibilities, not mandatory deployable services. A team may combine responsibilities or split them into services if ownership, authorization, failure isolation, and audit remain clear.

## 4. Shared Foundations

These components support one or more levels. They should be designed as shared platform capabilities rather than reimplemented separately for each level.

| Component | Responsibility | Minimum contract or evidence |
| --- | --- | --- |
| Interaction and Intent Boundary | Capture requests, selected tenant/resource context, constraints, approvals, user clarification, and feedback; present answers, plans, progress, and outcomes. | Correlation ID; authenticated principal; explicit active tenant/resource; interaction mode; user-visible status and outcome. |
| Identity and Context Propagation | Carry verifiable user, tenant, resource, and session context through retrieval and operations. | Context is validated by trusted services and the authoritative platform; client/model-supplied identifiers alone do not establish access. |
| Policy Enforcement and Approval | Decide whether a requested operation is permitted for this user, tenant, resource, capability, action class, and state. | Allow/deny/approval-required decision; policy version; reason; evidence; audit record. Deny by default for unknown or ambiguous operations. |
| Capability Catalog | Describe discoverable, approved operations and their contracts, owners, versions, permissions, side effects, execution modes, preconditions, failure behavior, and lifecycle. | Versioned capability identity and machine-usable input/output contract; owner; status; authority metadata. |
| Capability Adapter | Map a catalog contract to an existing platform API, workflow, calculation, report, or other authoritative operation. | Validated input/output; propagated identity/context; normalized errors; side-effect and correlation evidence. |
| Context, Grounding and Evidence | Retrieve authorized platform facts and approved knowledge with provenance, freshness, and scope. | Evidence item with source, resource scope, timestamp/freshness, and access basis; distinguish source facts from generated interpretation. |
| Execution and Workflow Coordination | Invoke known workflows, maintain operation state, report progress, and support safe failure handling. | Operation ID; accepted/running/waiting/completed/failed/cancelled state; idempotency/cancellation behavior where supported; outcome evidence. |
| Evidence, Audit and Observability | Link requests, decisions, selected capabilities, execution, outcomes, quality measures, and approved derived signals. | Correlated, access-controlled event records; separate operational metadata from sensitive raw interaction content. |
| Evaluation and Release Controls | Evaluate capability quality and safety before and after release. | Versioned test scenarios; acceptance thresholds set by owners; monitored outcomes; rollback/disable path appropriate to the platform. |

The applicable platform remains the source of truth for identity and business authorization. The AI layer may make an authorization request but cannot replace the platform's enforcement. For a multi-tenant platform, every read, invocation, execution record, and derived signal must retain the correct tenant scope. Where access is further constrained by projects or other resources, those checks also apply.

## 5. Level 1: Knowledge

### Outcome

Answer a user's question with relevant, authorized, grounded platform information or approved domain knowledge. L1 does not need to change business state.

### Components to Build or Configure

* **Question and context handler:** resolves the question and binds it to the active user, tenant, resource, and current platform context.
* **Authorized source connectors:** query existing reports, structured APIs, approved documents, or other information sources. Retrieval technique is an implementation choice.
* **Grounding and evidence assembler:** selects relevant evidence, checks scope and freshness, and provides provenance to response generation.
* **Answer composer:** explains the result in domain language, distinguishes source facts from interpretation, and avoids asserting unsupported conclusions.
* **No-evidence and correction path:** asks a clarifying question, reports insufficient evidence, or directs the user to an appropriate source rather than fabricating an answer.

### Logical Interaction

1. Authenticate the user and establish tenant/resource context.
2. Resolve the question and permitted information sources.
3. Retrieve scoped evidence through authoritative interfaces.
4. Compose an answer tied to source evidence and present relevant limitations.
5. Record a correlated quality/audit event under the applicable data policy.

### Acceptance Criteria

* Answers use only information the user is authorized to access.
* Material factual claims can be traced to source evidence with provenance and freshness where available.
* Missing, stale, conflicting, or out-of-scope evidence leads to clarification or an explicit limitation, not an invented answer.
* An attempt to switch tenant/resource context cannot expose data without authorization.
* The capability does not change platform state.

## 6. Level 2: Action

### Outcome

Execute an existing platform capability through a known workflow. The path and workflow semantics are established before the user request; AI interprets intent and parameters but does not invent the business process.

### Components to Build or Configure

* **Capability registration:** describe the operation, input/output schema, preconditions, permissions, side effects, modes, owner, version, and failures in the catalog.
* **Intent-to-workflow binding:** map a recognized request to an approved workflow and collect required parameters. Ambiguity is resolved with the user.
* **Input validation:** validate parameter types, ranges, resource ownership, preconditions, and business rules before execution.
* **Platform adapter:** call the existing API, service, calculation job, report, or workflow without duplicating its authoritative logic.
* **Deterministic workflow coordinator:** execute the predefined sequence and branches, maintain state, and report progress/results.
* **Policy and confirmation gate:** check user delegation and resource permissions for the requested action; request approval where policy requires it.

### Logical Interaction

1. Resolve intent and bind it to one registered workflow.
2. Collect/confirm parameters and resource scope.
3. Validate preconditions and obtain a policy decision before each material operation.
4. Invoke the platform capability using its contract and correlation context.
5. Report success, partial completion, failure, or approval requirement with evidence.

### Acceptance Criteria

* The executed workflow is a registered, versioned workflow; the model cannot silently substitute a different process.
* Inputs are validated by deterministic controls before invocation.
* Authorization is checked against the initiating user, tenant, resource, and action.
* Retries do not create unintended duplicate effects where idempotency is supported; unsupported retry behavior is explicit.
* The user receives a truthful outcome, including partial failure and effects that completed before failure.

## 7. Level 3: Reasoning

### Outcome

Dynamically determine what to investigate, which authorized evidence and capabilities are relevant, and what the evidence supports. L3 produces an explanation, plan, or recommendation for human validation; it does not become L4 merely because it generated a plan.

### Components to Build or Configure

* **Problem-framing service:** identify the requested question, known constraints, unknowns, and clarification needs.
* **Investigation planner:** formulate candidate hypotheses and select an evidence-gathering path dynamically from the problem and available catalog.
* **Reasoning/capability selector:** choose only registered capabilities and allowed read/analyze/simulate modes; policy-filter discovery and invocation.
* **Evidence ledger:** record evidence, source, observations, assumptions, uncertainty, and contradictions used in the conclusion.
* **Recommendation composer:** return findings, confidence/uncertainty, supporting evidence, limitations, and recommended next steps.
* **Human validation boundary:** present the recommendation for acceptance, correction, or rejection. A user decision does not silently convert a recommendation into execution; any execution uses an explicit authorized action path.

### Logical Interaction

1. Frame the problem and ask for missing context where needed.
2. Select an investigation strategy based on evidence and available authorized capabilities.
3. Invoke permitted investigative operations and update hypotheses as evidence arrives.
4. Stop when evidence is sufficient, the investigation budget is reached, or progress is blocked.
5. Present a traceable recommendation and uncertainty for human validation.

### Acceptance Criteria

* The investigation path is responsive to the problem and evidence rather than a fixed workflow disguised as reasoning.
* Every material factual conclusion links to evidence or is explicitly identified as an inference/assumption.
* The service communicates uncertainty, conflicting evidence, and investigation limits.
* Only permitted catalog capabilities are discoverable and invocable within the session context.
* A recommendation or plan cannot directly cause a state-changing operation without a separate policy-authorized execution decision.

## 8. Level 4: Autonomy

### Outcome

Pursue a user-defined objective through a bounded plan/act/observe/evaluate loop under explicit constraints, stopping conditions, and delegated permissions. Human control remains available throughout.

### Components to Build or Configure

* **Goal contract handler:** capture objective, tenant/resource scope, constraints, success measures, completion boundary, allowed action classes, and stop conditions. Ask the user to clarify material ambiguity before acting.
* **Goal and Autonomy Controller:** own the goal-pursuit session state, plan versions, current step, budgets, progress, approvals, and outcome verification.
* **Planning and replanning service:** generate or revise a plan using L3 reasoning and newly observed evidence.
* **Policy enforcement point:** authorize each material action using user delegation, tenant/resource permissions, risk/action class, and current state. Goal creation is not a permission grant.
* **Execution coordinator:** invoke registered capabilities, track effects and failures, and support approval waits, cancellation, pause/resume, and safe retry/compensation where available.
* **Progress and intervention interface:** show objective, current activity, completed/remaining work, blocked decisions, requested approval, and stop controls.
* **Goal verification service:** evaluate success criteria against observable platform outcomes; do not treat successful API invocation as proof that the objective is achieved.

### Logical Interaction

1. Confirm objective, constraints, resource scope, completion test, and authority. Ask the user for missing success boundaries.
2. Create a bounded plan and show the intended action classes and approval points appropriate to policy.
3. For each material step, re-check policy, execute an authorized capability, observe the result, and record evidence.
4. Replan only within the original goal and authority; seek user input or approval if either must change.
5. Stop on verified completion, user cancellation, budget/stopping limit, policy denial, unsafe uncertainty, or inability to progress; report the reason and evidence.

### Acceptance Criteria

* A session cannot start material execution without a defined goal, scope, constraints, completion boundary, stopping conditions, and valid authority.
* Policy is checked before every material action, not only when the goal begins.
* The user can pause/cancel and can respond to approval or clarification requests.
* Actions outside configured delegation are denied or held for approval; destructive/high-risk actions require the explicit allowance defined by domain policy.
* For mNet, deletion of a project or deletion of all records in a foundational device-data table requires explicit user allowance as well as successful authorization checks.
* The controller respects action/resource budgets and stops safely when limits are reached.
* Completion is verified against objective-specific evidence; incomplete or uncertain goals are reported as such.
* All actions and decisions can be correlated to the user, goal, plan, policy decision, capability, and outcome.

For mNet, this goal loop acts on the platform's uploaded project/model data and established services, not on a live network connection. Other platforms must explicitly define their own operational boundary; no live-system permission should be inferred from this example.

## 9. Level 5: Capability Evolution

### Outcome

Analyze approved, anonymized patterns to identify statistically meaningful capability gaps, check the Capability Catalog for existing approved compositions, and create a proposal for human review. L5 does not implement the proposal.

### Components to Build or Configure

* **Signal eligibility and minimization:** admit only approved signal types, currently including anonymized interaction patterns, unfulfilled intent patterns, repeated manual-workaround patterns, and workflow execution-failure telemetry.
* **Anonymization and aggregation boundary:** minimize identifiers and content before cross-tenant aggregation. Cross-tenant anonymized aggregate analysis is allowed under the stated existing policy; implementation controls remain to be specified.
* **Pattern and significance analysis:** detect recurring needs and failures, quantify evidence quality, and avoid treating isolated noise as a product gap.
* **Capability gap assessor:** compare validated patterns with current product capability coverage and distinguish a missing capability from discoverability, training, data-quality, or workflow defects where evidence permits.
* **Catalog composition matcher:** determine whether existing approved catalog capabilities could address the gap through composition. It does not create skills or write to the registry.
* **Business impact estimator:** provide a transparent, evidence-backed estimate with assumptions and uncertainty rather than presenting speculative value as fact.
* **Proposal generator and governance handoff:** create a standard proposal and route it to platform/product owners for accept, reject, or defer disposition.
* **Outcome feedback:** after a human-governed change is released, associate permitted outcome signals with the proposal to assess realized value.

### Capability Evolution Proposal Contract

Each proposal should contain:

* proposal identity, creation time, scope, and owning product/platform area;
* observed pattern and evidence summaries, including sample/time-window and quality limitations;
* affected population at an appropriately anonymized/aggregated level;
* capability gap hypothesis and plausible alternative explanations;
* candidate capability concept and existing catalog capabilities that may be composed;
* estimated business impact with assumptions and uncertainty;
* risks, dependencies, and questions for human review;
* status and human disposition: accepted, rejected, deferred, or returned for refinement.

### Strict AI Boundary

L5 must not generate code, scripts, API definitions, or other implementation assets; modify schemas, platform state, production routing, or the Skill Registry; or access CI/CD deployment triggers. Human engineers own design, implementation, security review, testing, and release of accepted proposals.

### Acceptance Criteria

* Only approved signal types enter the analysis path, with anonymization/minimization before cross-tenant aggregation.
* Each proposed gap is supported by traceable aggregate evidence, an explicit threshold/method version, and stated limitations.
* The analyzer checks existing approved catalog capabilities before suggesting a missing capability.
* Proposal impact claims disclose assumptions and uncertainty.
* Proposals are routed for human disposition; AI cannot accept its own proposal or alter production capability state.
* No path exists from L5 to code generation, schema/state/registry modification, or deployment execution.

## 10. Level and Dependency Map

| Level | Scenario responsibility | Reused foundations | Additional capability | Completion boundary |
| --- | --- | --- | --- | --- |
| L1 Knowledge | Grounded explanation | Identity/context, authorized data access, interaction, evidence/audit | Context assembly and grounded response | Scoped answer with provenance, or explicit limitation |
| L2 Action | Known workflow execution | L1 context foundations, identity/policy, capability catalog/adapters, execution/audit | Intent-to-workflow binding and deterministic coordinator | Workflow outcome reported; no dynamic strategy ownership |
| L3 Reasoning | Dynamic investigation and recommendation | L1 evidence; catalog/adapters; policy; evaluation/audit | Problem framing, investigation planning, evidence ledger | Human-reviewable recommendation; no implicit execution authority |
| L4 Autonomy | Bounded goal pursuit | L3 reasoning; L2 execution; policy, approval, audit, interaction | Goal contract, controller, budgets, stop/verify loop | Goal verified, safely stopped, or escalated |
| L5 Evolution | Governed capability-gap proposal | Catalog metadata; approved signals; observability; governance workflow | Anonymized aggregation, gap analysis, composition matching, proposal | Human disposition; no AI implementation or platform mutation |

This table describes dependency reuse, not a compulsory release sequence. L5 can be developed after its signal governance and catalog inputs are reliable even if unrelated product scenarios remain at L1 or L2.

## 11. Suggested Delivery Roadmap

### Foundation: Establish Trust Boundaries

Agree on identity/context propagation, tenant/resource authorization, audit correlation, data classification, capability ownership, and evaluation responsibility. Implement the minimum shared contracts needed by the chosen scenario before connecting an AI component to business data or actions.

### Increment A: Deliver a Bounded L1 Scenario

Select one high-value information task. Connect a small set of authoritative sources, demonstrate tenant/resource isolation, expose evidence provenance, and establish answer-quality evaluation. This creates a user-facing value slice and a trustworthy context path.

### Increment B: Register and Execute a Small L2 Capability Set

Choose stable, valuable workflows. Register their contracts, preconditions, side effects, authorization needs, and failure semantics. Add deterministic workflow orchestration and verify that AI cannot bypass adapters or policy.

### Increment C: Add L3 Investigation

Reuse L1 grounding and the governed catalog. Start with read/analyze operations and an explicit human-validation boundary. Evaluate evidence traceability, uncertainty communication, and recommendation usefulness before expanding capability selection.

### Increment D: Pilot L4 for a Bounded Objective

Select an objective with measurable completion evidence and manageable consequences. Define delegated action classes, approval gates, resource/action budgets, stop behavior, cancellation, and recovery before enabling execution. Start in a restricted project/model or simulation boundary when available.

### Increment E: Establish L5 Proposal Analysis

First approve signal schemas, anonymization, aggregation, significance criteria, retention, and access. Ensure catalog metadata is sufficient for composition matching. Then generate proposals for human review only and measure precision and realized value after approved changes.

Each increment may be selected based on business value and risk. Building an L4 scenario does not require every unrelated feature to reach L3, and L5 does not require autonomous implementation.

## 12. Cross-Cutting Quality and Safety Requirements

* **Isolation:** prove tenant and resource separation across retrieval, caches, capability discovery, invocation, logs, and derived signals.
* **Authorization:** enforce principal, tenant, resource, capability, action, and current-state policy at authoritative boundaries.
* **Traceability:** correlate user intent, context, evidence, plan, policy decisions, capability calls, results, and human approvals.
* **Privacy:** minimize and classify data; govern raw interaction collection separately from operational metadata and derived L5 signals.
* **Reliability:** define timeout, partial failure, retry, idempotency, cancellation, compensation, and degraded-mode behavior per capability.
* **Evaluation:** version scenario suites and evaluate quality, safety, cost, latency, and business outcomes by scenario/level.
* **Human control:** make current mode and operation status visible; provide correction, approval, pause, cancel, and escalation paths as applicable.
* **Change governance:** version capability contracts and policy; use established review, testing, rollout, monitoring, and rollback controls.

## 13. Open Decisions and Owners

The following details are deliberately not inferred by this logical design. Assign owners and record decisions before implementation requires them.

| Decision | Suggested owner(s) | Why it matters |
| --- | --- | --- |
| Action/risk taxonomy and which actions require explicit confirmation | Product/domain owner, Security, platform owner | Determines L2/L3/L4 authority and approval behavior. |
| L4 goal contract, success evidence, budgets, and stop conditions per domain | Product/domain owner, System Architecture | Prevents ambiguous goals and unverifiable completion. |
| L3-to-L4 handoff and whether/how a user confirms a plan | Product, UX, Security, platform owner | Prevents recommendation from becoming implicit execution. |
| L5 signal schema, anonymization standard, significance thresholds, retention, access, and deletion | Privacy, Security, Data Governance, Product | Controls cross-tenant analysis and proposal evidence. |
| Capability catalog ownership, contract schema, versioning, lifecycle, and composition compatibility | Platform Architecture, capability owners | Enables safe discovery, invocation, and L5 matching. |
| Source data freshness, provenance, and conflict handling | Data/platform owners | Affects grounding and recommendation reliability. |
| Evaluation datasets, quality thresholds, and release gates | Product, domain experts, AI quality, Security | Defines observable readiness at each level. |
| Deployment topology, model/provider, retrieval technology, operational targets, and cost limits | System Architecture, SRE/Platform, Product | These are implementation decisions outside this technology-neutral design. |

## 14. Definition of Ready by Level

| Level | Minimum readiness evidence |
| --- | --- |
| L1 | Authorized source access, scoped context, evidence provenance, no-evidence behavior, and a scenario evaluation set. |
| L2 | Registered capability contract, deterministic validation, user/resource authorization, side-effect semantics, failure handling, and workflow test evidence. |
| L3 | Sufficient catalog/read capabilities, evidence ledger, uncertainty handling, policy-filtered selection, human-validation UX, and recommendation evaluation. |
| L4 | Explicit goal contract, success and stop criteria, delegated action policy, per-action enforcement, budget, pause/cancel, audit correlation, and outcome verification. |
| L5 | Approved signal inventory, privacy controls, aggregation method and thresholds, adequate catalog metadata, proposal schema, human governance owner, and proof that prohibited write/deploy paths are absent. |

## 15. Conclusion

Build the engagement layer around existing authoritative capabilities. Establish shared identity, authorization, catalog, evidence, execution, and audit foundations, then add the level-specific behavior justified by each scenario. L1 grounds understanding; L2 executes known workflows; L3 dynamically investigates and recommends; L4 pursues a bounded, authorized goal; L5 identifies capability gaps and submits evidence-backed proposals for human decision and engineering.