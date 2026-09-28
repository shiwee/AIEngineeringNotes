# High-Level System Design

## AI Engagement Architecture for Enterprise Agentification

## 1. Purpose

This document translates the **5-Level AI Engagement Model** into a technology-neutral, high-level system design.

The design supports enterprise platforms that already provide business data, domain rules, workflows, calculations, transactions, and operational services. It defines how AI can engage with those capabilities progressively:

> **L1 Knowledge -> L2 Action -> L3 Reasoning -> L4 Autonomy -> L5 Capability Evolution**

This is a logical architecture. It does not mandate a particular large language model, retrieval technology, agent framework, protocol, cloud provider, deployment topology, or software delivery mechanism.

The model is intended to generalize across traditional enterprise platforms. The current mNet context is a mature, cloud-deployed, browser-based platform with SSO, multiple tenants, project-level permissions, and established APIs, services, workflows, calculation jobs, reporting, and audit. It evaluates user-uploaded network data and is not connected to the live network. These are grounding facts for examples, not requirements that every adopting platform share mNet's implementation.

## 2. Design Goals

The architecture shall:

* preserve existing business capabilities as the system of record and execution authority;
* support grounded information access without limiting L1 to one retrieval technique;
* expose existing capabilities for reliable, controlled AI use;
* distinguish predefined workflow execution from dynamic reasoning;
* support autonomous goal pursuit through feedback and verification;
* identify capability gaps without treating L5 as uncontrolled self-modification;
* separate AI engagement maturity from authority and risk;
* enforce tenant, identity, data, policy, and audit boundaries;
* allow scenarios at different maturity levels to coexist in one product;
* provide observable evidence for reliability, governance, and continuous improvement.

## 3. Non-Goals

This document does not:

* select a vendor or model;
* define a detailed microservice or deployment topology;
* prescribe a specific retrieval, tool-calling, or agent protocol;
* define production-grade data retention periods;
* authorize autonomous production code deployment;
* replace domain-specific safety, security, or regulatory assessments;
* treat the maturity levels as mandatory product release phases.

## 4. Architectural Principles

### 4.1 Existing Capabilities Remain Authoritative

AI should understand, orchestrate, reason over, and help extend existing platform capabilities. Business rules, calculations, validation, transactions, and state changes should remain in authoritative platform services wherever practical.

### 4.2 Engagement and Authority Are Separate Dimensions

The engagement level describes what responsibility AI has in solving a problem:

> **Knowledge -> Action -> Reasoning -> Autonomy -> Evolution**

Authority describes what AI is permitted to do:

> **Read -> Analyze -> Recommend -> Simulate -> Modify -> Execute**

An L2 capability may be permitted to create a report but not delete a project. An L4 capability may pursue a goal autonomously while remaining limited to analysis and simulation. Authority is assigned by risk, policy, data sensitivity, reversibility, and accountability.

### 4.3 Deterministic Work Should Remain Deterministic

AI should not replace deterministic platform logic merely because a natural-language interface is present. The architecture should route deterministic operations through deterministic services and use reasoning where ambiguity, investigation, planning, or dynamic selection creates value.

### 4.4 Human Accountability Moves Upward

Human involvement changes from operating individual steps to setting objectives, validating recommendations, governing authority, and steering platform evolution. Accountability is not removed by increasing AI autonomy.

### 4.5 Evidence Before Progression

An AI capability should be classified by observable behavior and evidence, not by whether it uses RAG, tool calling, an Agent framework, or a particular model.

## 5. Logical Context

```text
                         Product / Platform Governance
                          policy, approval, release, audit
                                      |
                                      v
User / Client ---> AI Engagement Platform <---- Platform Capabilities
                       |       |                 data, rules, workflows,
                       |       |                 calculations, transactions
                       |       |
                       |       +---------------> Capability Evolution
                       |                         signals, gaps, proposals
                       v
                 Enterprise Identity,
                 Security and Policy
```

The AI Engagement Platform is an orchestration and intelligence layer. It is not the owner of the enterprise platform's business state.

## 6. Logical Components

### 6.1 Interaction and Intent Boundary

Provides the user-facing entry point for questions, requests, objectives, approvals, and feedback.

Responsibilities:

* authenticate the user and establish tenant and session context;
* capture the request, objective, constraints, and requested outcome;
* present answers, plans, recommendations, progress, approvals, and results;
* make the difference between suggestion, simulation, modification, and execution visible;
* support user correction, cancellation, and escalation.

The interaction boundary must not be treated as the source of authorization. Authorization is evaluated by the policy and capability controls for every material operation.

### 6.2 Context and Grounding Service

Builds the trusted context supplied to reasoning and response generation.

Possible sources include:

* structured platform queries;
* platform reports and analytics;
* domain knowledge repositories;
* semantic or keyword search;
* approved documents and procedures;
* historical results;
* current workflow and transaction state.

Responsibilities:

* apply tenant, identity, and data-access filters;
* identify source provenance and freshness;
* distinguish authoritative platform data from explanatory knowledge;
* return evidence suitable for answer grounding and audit;
* prevent unauthorized cross-tenant or cross-domain context assembly.

L1 is achieved when this service and the AI response flow can reliably answer domain questions with relevant, grounded information. Retrieval-augmented generation may be used, but is not the definition of L1.

### 6.3 Capability Catalog

The catalog is the governed inventory of capabilities that AI may discover and use.

Each registered capability should describe, at a minimum:

* stable capability identity and version;
* business purpose and supported scenarios;
* input and output contracts;
* required permissions and data classifications;
* side effects and reversibility;
* preconditions and validation rules;
* supported execution modes, such as read, simulate, modify, or execute;
* timeout, retry, compensation, and failure behavior;
* owner, lifecycle status, and evidence requirements.

The catalog is the architectural boundary between AI reasoning and platform operations. AI should select only capabilities that are registered, discoverable, compatible with the current context, and permitted by policy.

### 6.4 Capability Adapter Layer

Adapters expose existing platform services to the AI Engagement Platform without moving business logic into prompts or model instructions.

An adapter may wrap an API, workflow, report, calculation service, transaction service, or other approved platform interface.

Responsibilities:

* translate between the capability contract and the platform interface;
* validate inputs before invocation;
* propagate identity, tenant, correlation, and authorization context;
* normalize results and errors;
* record side effects and execution evidence;
* support idempotency, timeout, cancellation, and compensation where applicable.

### 6.5 Workflow and Execution Coordinator

Executes approved operations and maintains execution state.

It supports two distinct modes:

**Predefined execution, used primarily at L2**

* invokes a known sequence of capabilities;
* applies deterministic branching and validation rules;
* reports progress and final outcome;
* does not dynamically redefine the business strategy.

**Dynamic execution, used by L4 within authority**

* executes an approved plan or next action;
* checks policy before each material action;
* observes outcomes and records state;
* supports pause, resume, cancellation, approval waits, retry, and compensation;
* stops when the goal is achieved, a stopping condition is reached, an approval is required, or progress is no longer safe or possible.

### 6.6 Reasoning and Planning Service

Provides dynamic investigation, evidence evaluation, alternative generation, planning, and recommendation.

Responsibilities:

* determine what information is needed;
* select relevant capabilities from the catalog;
* formulate hypotheses and investigation paths;
* correlate evidence and identify uncertainty;
* evaluate alternatives and constraints;
* produce a plan, recommendation, or proposed next step;
* explain supporting evidence, assumptions, and limitations.

At L3, this service owns the reasoning process but not the user's final objective or execution loop. A plan or recommendation is returned for human validation unless separately authorized as part of an L4 goal-pursuit session.

### 6.7 Goal and Autonomy Controller

Manages L4 goal-pursuit sessions.

Responsibilities:

* represent the user-defined objective, constraints, success measures, and stopping conditions;
* identify ambiguity in the requested outcome and ask the user to define or confirm success and completion boundaries before pursuing an underspecified goal;
* maintain a bounded plan and current execution state;
* invoke reasoning for planning and replanning;
* request capabilities through the policy enforcement point;
* observe results and evaluate progress against the goal;
* adapt the plan when evidence changes;
* pause for approval or escalate when authority is insufficient;
* support user pause, cancellation, and resumption under the applicable policy;
* verify completion and produce an outcome report.

The controller must be designed as a bounded control loop, not as unrestricted self-directed execution.

```text
Goal and constraints
        |
        v
Plan -> Authorize -> Act -> Observe -> Evaluate
  ^                                      |
  |-------------- Adapt -----------------|
        |
        v
Verify goal / stop / escalate
```

### 6.8 Policy Enforcement and Approval Service

Centralizes decisions about whether an AI-initiated operation is permitted.

Policy inputs may include:

* user identity and role;
* tenant and data domain;
* capability and version;
* requested action type;
* target resource and data classification;
* business risk and reversibility;
* current workflow state;
* objective constraints;
* approval requirements;
* time, volume, and rate limits.

The service should enforce the initiating user's configured delegation together with tenant and resource-level permissions. It should support deny-by-default behavior for unregistered or ambiguous operations, explicit approval checkpoints, policy versioning, decision explanations, and an auditable decision record. A goal or plan does not itself grant authority. High-risk or destructive operations require the applicable explicit allowance and authorization; the exact action classes remain domain policy.

### 6.9 Evidence, Audit, and Observability Service

Captures the evidence required to operate, govern, troubleshoot, and improve the system.

It should distinguish at least:

* business outcome and platform execution telemetry;
* AI orchestration and capability-selection telemetry;
* policy and approval decisions;
* quality and reliability measurements;
* derived capability-gap signals;
* sensitive raw interaction content.

Raw prompts, responses, documents, and business data must not automatically be treated as general telemetry. Their collection, access, retention, masking, and deletion are governed separately.

### 6.10 Capability Evolution Service

Supports L5 without granting automatic production modification.

Responsibilities:

* consume approved, anonymized interaction patterns, unfulfilled intent patterns, repeated manual-workaround signals, and workflow-failure telemetry;
* identify statistically significant patterns that suggest unmet platform needs;
* check the governed Capability Catalog for existing approved capabilities that may address the need through composition;
* produce a traceable Capability Evolution Proposal with observed evidence, business context and estimated impact, a capability concept, and candidate composition of existing capabilities;
* route the proposal to the responsible platform, product, and domain stakeholders and record its disposition when an approved governance interface exists.

L5 is an observation and proposal capability, not a platform-change mechanism. It must not synthesize code, scripts, API definitions, or other implementation assets; modify schemas, platform state, production routing, or the Skill Registry; or trigger CI/CD or deployment. If a proposal is accepted, human engineers implement it through the platform's established development, security, testing, and release processes.

Cross-tenant aggregate analysis is permitted under the stated existing policy, subject to anonymization and data minimization. The exact signal schema, anonymization standard, thresholds, retention, and access controls remain governance decisions.

The preferred candidate approach is:

> **Reuse -> Compose**

## 7. Level-to-Architecture Mapping

| Level | Primary behavior | Main components | Completion boundary |
| ----- | ---------------- | --------------- | ------------------- |
| **L1 Knowledge** | Provide grounded information and explanation | Interaction, Context and Grounding, Evidence | Answer is grounded, scoped, and presented with context or evidence. |
| **L2 Action** | Execute an existing capability through a known path | Capability Catalog, Adapters, Workflow Coordinator, Policy | Predefined workflow completes, fails, or is safely stopped. |
| **L3 Reasoning** | Determine an investigation or solution path and recommend | Context and Grounding, Capability Catalog, Reasoning and Planning, Evidence | Recommendation or plan is produced for human validation. |
| **L4 Autonomy** | Pursue a defined objective through iterative control | Reasoning and Planning, Goal Controller, Execution Coordinator, Policy, Evidence | Goal is verified, execution stops, or the session escalates. |
| **L5 Capability Evolution** | Identify and help address capability gaps | Evidence, Evolution Service, Catalog, Governance, controlled delivery process | Proposal is accepted, rejected, refined, or introduced through controlled governance. |

The engagement level is assessed at the product's AI offering boundary, based on observable behavior across its supported user experience. Capability, workflow, and scenario evidence substantiates that product-level assessment; an underlying API or workflow is not independently assigned an engagement level. If a product exposes materially different AI modes, describe a product-level engagement profile that preserves those differences instead of collapsing them into a misleading scalar.

## 8. Critical Architectural Boundaries

### 8.1 L1 to L2: Read to Act

L1 may retrieve and explain information. L2 may invoke a registered capability and change platform state where authorized. The action path remains known before execution.

### 8.2 L2 to L3: Known Path to Dynamic Path

L2 follows a predefined workflow. L3 dynamically determines what information, capabilities, evidence, and reasoning steps are needed. Planning belongs in L3; planning alone does not imply autonomy.

### 8.3 L3 to L4: Recommend to Pursue

L3 determines what should be done and stops at recommendation or human validation. Transition to L4 requires an explicit goal-pursuit context: a user-defined or user-confirmed objective, constraints, success/completion boundary, stopping conditions, and applicable delegated authority. L4 owns the bounded loop for pursuing that objective, but each material action is still checked against policy. A recommendation or plan alone never grants execution authority.

### 8.4 L4 to L5: Solve a Problem to Improve the Capability Set

L4 uses the platform to achieve a business objective. L5 examines signals about the platform and user needs to determine what capability the platform should gain or improve. L5 is not simply a more autonomous L4.

## 9. Security, Privacy, and Tenant Isolation

### 9.1 Identity and Tenant Context

Every request, retrieval, capability invocation, execution session, telemetry event, and proposal must carry verifiable identity and tenant context. Where the platform has finer-grained resource authorization, such as project permissions, that authorization must also be enforced by the authoritative platform boundary. Context must be revalidated at service boundaries; it must not be trusted solely because it was supplied by a model or client. A user's access to one tenant or resource must not imply access to another.

### 9.2 Data Minimization and Isolation

The architecture should use the minimum data needed for each operation. Retrieval indexes, caches, prompts, outputs, traces, and derived signals must enforce tenant and authorization boundaries. Cross-tenant anonymized aggregate analysis is permitted under the stated existing policy; signal minimization, anonymization standards, re-identification controls, access, and retention must be made explicit in the implementation governance.

### 9.3 Sensitive Interaction Data

Prompt and response data may contain personal, confidential, regulated, or customer-specific information. The system should define:

* what raw content is collected;
* what is redacted or tokenized before storage;
* who may access it and for what purpose;
* whether only derived signals are retained for L5;
* retention and deletion behavior;
* customer isolation and export requirements;
* whether content may be used for evaluation, training, or capability discovery.

### 9.4 Capability and Tool Security

Capabilities must be registered, versioned, authenticated, authorized, and observable. The system must protect against prompt injection, unauthorized capability discovery, parameter manipulation, replay, excessive invocation, data exfiltration, and unsafe chaining of individually permitted actions.

## 10. L4 Control Requirements

An L4 implementation should provide, at minimum:

* explicit objective, constraints, success criteria, and stopping conditions;
* a bounded action and resource budget;
* policy evaluation before each material action;
* approval checkpoints for defined risk classes;
* cancellation and emergency stop;
* user-visible progress and a means to pause or stop an active goal-pursuit session;
* idempotency and compensation for repeatable or reversible operations;
* progress, uncertainty, and failure reporting;
* complete correlation from goal to plan, action, result, and decision;
* verification that the stated objective was achieved rather than assuming successful invocation means success.

## 11. L5 Capability Evolution Flow

```text
Platform / interaction signals
              |
              v
      Minimize and govern data
              |
              v
      Detect repeated patterns
              |
              v
      Validate capability gap
              |
              v
 Check approved Capability Catalog
              |
              v
         Reuse / Compose candidates
              |
              v
       Capability Evolution Proposal
              |
              v
 Product, architecture, security and
 platform-owner governance
              |
       +------+------+
       |             |
       v             v
 Controlled      Reject / refine
 introduction          |
       |               |
       +-------<-------+
              |
              v
       Observe outcome and value
```

The proposal should include the observed pattern, evidence quality, affected users or tenants, business value, risks, candidate approaches, dependencies, ownership, and validation plan.

## 12. Definition of a Skill or Capability

For this architecture, a **capability** is a governed business or platform operation that has a stable contract, owner, authorization model, lifecycle, and observable outcome.

A **skill** is a reusable way of applying one or more capabilities to a recognized business task. A skill may include instructions, decision logic, a workflow, or a composition of registered capabilities, but it does not bypass capability contracts or policy enforcement.

This definition is intentionally logical. The implementation may represent a skill as a workflow, service, configuration, prompt asset, code component, or combination, provided that it remains governed, versioned, testable, and attributable to an owner.

## 13. Reliability and Quality Measures

Measures should be collected by scenario and level. Examples include:

* grounded-answer accuracy, evidence coverage, and freshness at L1;
* workflow completion, validation failure, and recovery rates at L2;
* recommendation quality, evidence traceability, and human acceptance at L3;
* goal completion, policy violations, intervention rate, and unsafe-stop rate at L4;
* capability-gap precision, proposal acceptance, time to controlled introduction, and realized value at L5;
* latency, cost, availability, and failure rates across all levels;
* tenant-isolation, security, privacy, and audit findings across all levels.

No single metric should be used as proof of maturity. Business outcome, safety, reliability, and governance evidence must be considered together.

## 14. Incremental Adoption Approach

The architecture supports incremental adoption:

1. Establish identity, tenant context, policy, audit, and capability ownership.
2. Deliver grounded L1 scenarios using authoritative platform information.
3. Register and expose a small set of high-value, low-risk L2 capabilities.
4. Add L3 investigation and recommendation with evidence and human validation.
5. Introduce L4 only for objectives with measurable success criteria, bounded actions, and clear approval rules.
6. Establish governed signals and proposal handling before enabling L5 capability evolution.

These steps are adoption guidance, not a requirement that every scenario follow the same sequence. A scenario can remain at a lower level when its value, risk, or evidence does not justify progression.

## 15. Open Architecture Decisions

The following decisions require System Architecture, Security, Privacy, Product, and platform-owner agreement.

### 15.1 L5 Telemetry

The baseline scope permits anonymized interaction patterns, unfulfilled user-intent patterns, repeated manual-workaround patterns, and workflow execution-failure telemetry, including cross-tenant aggregates under existing policy. Define the approved signal schema, anonymization and minimization controls, evidence thresholds, retention, access, and deletion. Do not assume raw interaction content is required or retained.

### 15.2 Prompt and Response Handling

Define content classification, redaction, encryption, customer isolation, access control, retention, deletion, export, legal hold, evaluation use, and any prohibition on using customer content for model training or cross-customer capability discovery.

### 15.3 L5 Proposal Versus Autonomous Implementation

The L5 AI boundary is fixed for this design:

> **Analyze Approved Signals -> Identify Gap -> Check Existing Catalog Composition -> Propose -> Human Decision and Engineering**

L5 must not synthesize code, scripts, API definitions, or other implementation assets; modify schemas, platform state, production routing, or the Skill Registry; or trigger CI/CD or deployment. Human engineers own implementation, security review, testing, and release under existing governance.

### 15.4 Skill Lifecycle

Define registration, ownership, versioning, compatibility, testing, approval, deprecation, rollback, and discoverability requirements for skills and capabilities.

### 15.5 L3 and L4 Responsibility Boundary

Define the domain-specific handoff contract: how an objective and plan are confirmed, which user delegation applies, how success/completion boundaries and approvals are represented, and how the system prevents an L3 recommendation from becoming unauthorized execution.

### 15.6 Goal and Risk Taxonomy

Define objective types, risk classes, approval authorities, stopping conditions, resource budgets, and unacceptable actions for each domain.

### 15.7 Evaluation and Release Governance

Define offline evaluation, scenario test suites, red-team testing, shadow or simulation modes, production monitoring, rollback, incident response, and acceptance thresholds for new capabilities and skills.

## 16. Architecture Risks and Mitigations

| Risk | Consequence | High-level mitigation |
| ---- | ----------- | --------------------- |
| AI bypasses authoritative business logic | Incorrect or inconsistent state | Keep state changes in platform capabilities and enforce contracts at adapters. |
| Dynamic reasoning selects unsafe capabilities | Unauthorized or harmful action | Catalog, policy enforcement, approval gates, budgets, and per-action audit. |
| L3 recommendation is mistaken for L4 authority | Accidental execution | Explicit mode, lifecycle, and handoff between recommendation and goal pursuit. |
| L4 loop does not verify outcomes | False completion or repeated damage | Observable success criteria, result evaluation, stopping conditions, compensation, and human escalation. |
| Raw interactions leak sensitive information | Privacy, regulatory, or customer trust impact | Data minimization, classification, redaction, isolation, access control, and retention policy. |
| L5 detects noise as a capability gap | Wasted product effort or harmful change | Evidence thresholds, human review, duplicate detection, value assessment, and controlled introduction. |
| Skills become ungoverned prompt or code fragments | Unpredictable behavior and supply-chain risk | Governed catalog, ownership, versioning, testing, provenance, and rollback. |
| Platform is labeled with one maturity level | Misleading roadmap and governance | Assess individual capabilities and scenarios across both engagement and authority dimensions. |

## 17. Conclusion

The proposed architecture treats Agentification as a progressive change in how AI engages with existing enterprise capabilities:

> **L1 knows. L2 acts through known workflows. L3 determines what should be done. L4 pursues a defined goal. L5 helps evolve the capability set.**

The central architectural boundary is not the choice of model or Agent framework. It is the ownership of work:

* L2 follows a known path.
* L3 determines a path and recommends.
* L4 owns a bounded goal-pursuit loop.
* L5 helps determine what the platform should be able to do next.

The system should therefore be built as a governed engagement layer around authoritative platform capabilities, with explicit controls for identity, data, authority, execution, evidence, and capability evolution.
