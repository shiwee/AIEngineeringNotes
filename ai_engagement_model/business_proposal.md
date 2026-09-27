Proposal of AI Transformation -- with chatgpt
2026年9月1日
11:07
 
Absolutely. Let's treat this as a formal role-play.
I am now the Business / Solution Architect for the mNet AI Transformation, and my responsibility is not to design the technical architecture yet. I own:
•	Business vision
•	Business problems/opportunities
•	Target business capabilities
•	AI engagement model
•	Solution concept
•	Business value
•	Transformation roadmap
•	Scope and priorities
•	High-level investment rationale
The System Architect will later take my proposal and determine how to technically realize it.
 
mNet AI Transformation
Business & Solution Architecture Proposal
1. Executive Vision
mNet will evolve from a traditional network evaluation and planning tool into an AI-powered network engineering platform.
Today, mNet primarily provides users with capabilities to:
Evaluate → Analyze → Plan
The future mNet should additionally help users:
Understand → Investigate → Diagnose → Recommend → Plan → Execute
and ultimately enable mNet itself to:
Continuously learn, identify capability gaps, and evolve.
The vision can therefore be summarized as:
From a tool that provides network engineering capabilities, to an intelligent platform that collaborates with engineers, performs engineering work, and continuously evolves with customer needs.
 
2. Business Transformation
The transformation is not:
"Add a chatbot to mNet."
It is:
Change the way users interact with, consume, and extend network engineering capabilities.
Today
Engineer
   │
   ▼
Understand requirement
   │
   ▼
Operate mNet
   │
   ▼
Configure parameters
   │
   ▼
Run evaluation
   │
   ▼
Analyze results
   │
   ▼
Design planning solution
   │
   ▼
Run planning/evaluation
   │
   ▼
Generate report
A significant amount of engineering knowledge and manual work remains with the user.
Future
Engineer
   │
   │ Natural language / engineering objective
   ▼
       AI
        │
        ├── Understand
        ├── Investigate
        ├── Diagnose
        ├── Recommend
        ├── Plan
        └── Execute
               │
               ▼
             mNet
               │
               ▼
        Engineering Result
               │
               ▼
          Human Decision
The human moves progressively from operator toward decision maker / supervisor.
 
3. AI Engagement Maturity Model
I propose that mNet adopts the following six-level AI engagement maturity model.
Level	AI Role	AI Responsibility	Human Role
L1	AI Assistant	Information	Do
L2	AI Copilot	Understanding	Do + Decide
L3	AI Analyst	Investigation & Diagnosis	Validate + Decide
L4	AI Planner	Recommendation & Planning	Approve
L5	AI Agent	Execution	Supervise
L6	AI Network Engineer	Continuous Evolution	Govern
The progression is:
Information → Understanding → Investigation → Decision → Action → Evolution
Or from the human perspective:
AI tells me → AI helps me → AI investigates for me → AI recommends → AI acts → AI helps improve itself/mNet
 
4. L1 — AI Assistant
Business objective
Make mNet's existing information and knowledge accessible through natural language.
The user should no longer need to understand every mNet menu, parameter, report, or data structure to obtain basic information.
Typical capabilities
Network Evaluation
•	Explain KPI
•	Explain evaluation rules
•	Summarize evaluation results
•	Query evaluation results
•	Explain charts
•	Explain network status
Network Planning
•	Explain planning parameters
•	Explain planning results
•	Summarize planning proposals
•	Compare basic planning results
Example
"Summarize the evaluation result for this network."
AI:
"The network achieves 94.2% coverage, 2.3% below the target. 127 cells fail the coverage criterion, concentrated primarily in three regions."
Capacity target
AI can provide relevant, trustworthy information from mNet.
Business value
•	Lower learning curve
•	Faster information retrieval
•	Better user experience
•	Reduce repetitive support questions
 
5. L2 — AI Copilot
Business objective
Move from information retrieval to contextual understanding.
AI understands the user's context:
Customer
Project
Network
Region
Evaluation
Scenario
Time
and explains relationships between them.
Example
Instead of:
"Coverage = 92%."
AI says:
"Coverage decreased from 96% to 92% compared with the previous evaluation. The degradation is concentrated in Region A and mainly affects urban cells."
Capabilities
•	Context-aware Q&A
•	Natural-language queries
•	Cross-result comparison
•	Contextual explanations
•	Guided analysis
•	Conversational interaction
•	Intelligent navigation
•	Recommended next steps
Capacity target
AI understands the user's question in the context of the mNet engineering scenario.
Business value
The user spends less time operating the tool and more time solving the engineering problem.
 
6. L3 — AI Analyst
Business objective
AI starts performing engineering analysis, not just helping the user perform it.
The key question becomes:
"Why?"
For example:
"Why did coverage decrease?"
AI investigates:
Evaluation
   ↓
Affected cells
   ↓
Topology
   ↓
Configuration
   ↓
Historical changes
   ↓
Neighbor relationships
   ↓
Potential causes
   ↓
Diagnosis
The result should include evidence:
Root cause: interference increase in Region A.
Evidence: 32 affected cells experienced a 5.2 dB SINR degradation following configuration changes to neighboring cells.
Capabilities
•	Automated investigation
•	Root-cause analysis
•	Anomaly detection
•	Cross-domain correlation
•	Historical comparison
•	Problem clustering
•	Impact analysis
•	Evidence-based diagnosis
Capacity target
AI can independently investigate a network problem and produce an evidence-based diagnosis.
Business value
This is where mNet starts capturing engineering expertise, rather than simply exposing engineering algorithms.
 
7. L4 — AI Planner
Business objective
Move from:
"What is wrong?"
to:
"What should we do?"
AI converts a diagnosed problem or business requirement into a planning objective and proposes solutions.
For example:
"Improve coverage to 95% while keeping CAPEX below £2M."
AI produces:
Candidate A
+10 sites
Coverage: 97%
CAPEX: £2.4M
Candidate B
+6 sites
Coverage: 95.8%
CAPEX: £1.8M
Candidate C
+8 sites
Coverage: 96.4%
CAPEX: £2.0M
Then:
Recommendation: Candidate C
with explanation, evidence, expected impact, cost, risk, and alternatives.
Capacity target
AI can generate and evaluate candidate solutions against engineering objectives and constraints, and provide an explainable recommendation.
Business value
The engineer moves from manually constructing solutions toward reviewing and selecting AI-generated solutions.
 
8. L5 — AI Agent
Business objective
Move from recommendation to execution of engineering workflows.
Instead of:
"Here is a recommended plan."
AI can:
"I analyzed the network, generated three candidate plans, evaluated them, selected the best candidate according to your constraints, and prepared the planning result for approval."
The agent can orchestrate:
Understand objective
       ↓
Investigate
       ↓
Diagnose
       ↓
Plan
       ↓
Evaluate
       ↓
Compare
       ↓
Recommend
       ↓
Execute approved action
       ↓
Verify
Important business principle
Execution does not mean unrestricted autonomy.
mNet should define different levels of authorization:
Read
 ↓
Analyze
 ↓
Recommend
 ↓
Simulate
 ↓
Modify
 ↓
Execute
Human approval can remain mandatory for higher-risk operations.
Capacity target
AI can independently orchestrate and execute multi-step mNet workflows toward a user-defined objective, within defined authorization boundaries.
 
9. L6 — AI Network Engineer / Continuous Evolution
This is the most strategic level.
L6 is not about connecting mNet to a live network.
mNet remains a planning/evaluation platform.
Instead:
AI continuously improves the capability of mNet itself.
The AI observes:
Customer requests
User behavior
Support cases
Failed workflows
Feature requests
Usage patterns
Market trends
Network technology evolution
Engineering knowledge
New algorithms / capabilities
and asks:
What does the customer need that mNet cannot currently provide?
Then:
Observe
   ↓
Identify capability gap
   ↓
Define new capability
   ↓
Find / compose / develop capability
   ↓
Validate
   ↓
Test
   ↓
Deploy
   ↓
Observe adoption / outcome
   ↓
Learn
   └──────────────→
This is continuous product evolution.
Example
AI detects:
38 customers repeatedly perform manual comparison of network planning scenarios.
AI proposes:
New capability: AI-assisted Scenario Comparison
It then helps create the capability, validates it against test cases, and proposes it for release.
Capacity target
mNet can continuously identify unmet customer needs and capability gaps, and systematically evolve its capabilities through a controlled AI-assisted development lifecycle.
Business value
The ultimate transformation is:
Customer needs → Capability gaps → New capabilities → Better mNet → New customer needs
This creates a continuous product evolution loop.
 
10. The target business model
The resulting mNet evolution looks like:
                 ┌──────────────────────┐
                 │      CUSTOMER        │
                 └──────────┬───────────┘
                            │
                       Needs / Goals
                            │
                            ▼
                  ┌───────────────────┐
                  │   AI-Powered mNet │
                  └─────────┬─────────┘
                            │
          ┌─────────────────┼─────────────────┐
          ▼                 ▼                 ▼
     Evaluation          Planning        Knowledge
          │                 │                 │
          └─────────────────┼─────────────────┘
                            │
                            ▼
                    Engineering Result
                            │
                            ▼
                       User Decision
                            │
                            ▼
                         Outcome
                            │
                            ▼
                    Feedback / Learning
                            │
                            └──────────────┐
                                           ▼
                                  Continuous Evolution
 
11. What should we build at each level?
From the business/solution perspective, I would define the deliverables as:
Level	Primary capability	What we build
L1	Information	AI assistant, knowledge access, natural-language query
L2	Understanding	Context, semantic understanding, conversational interaction
L3	Investigation	Analytical skills, diagnosis, evidence, investigation workflows
L4	Planning	Planning intelligence, candidate generation, recommendation
L5	Execution	Agent workflows, controlled execution, human approval
L6	Evolution	Capability discovery, skill acquisition/development, AI-assisted CI/CD
Notice the progression:
L1  AI uses information
L2  AI understands information
L3  AI reasons over information
L4  AI makes recommendations
L5  AI takes action
L6  AI improves the capability of the platform
 
12. Business transformation shouldn't be a waterfall
I would explicitly state in the proposal:
The six levels define the target maturity path, not six sequential product releases.
We can develop high-value scenarios in parallel.
For example:
                    Common AI Foundation
                            │
             ┌──────────────┼──────────────┐
             ▼              ▼              ▼
        Evaluation       Planning       Reporting
             │              │              │
          L1/L2          L1/L2            L1
             │              │
             ▼              ▼
            L3             L3
             │              │
             └───────┬──────┘
                     ▼
                    L4
                     │
                     ▼
                    L5
                     │
                     ▼
                    L6
This lets the business get value early without waiting years for L6.
 
13. Business architecture principles
I would put these principles into the proposal.
Principle 1 — AI augments, then progressively assumes responsibility
We don't start by trying to replace engineers.
Assist → Collaborate → Analyze → Recommend → Execute → Evolve
 
Principle 2 — Existing mNet capabilities remain the foundation
AI should leverage and orchestrate existing network engineering capabilities, rather than recreate them unnecessarily.
 
Principle 3 — Human remains accountable
As AI responsibility increases, governance and human control must evolve accordingly.
 
Principle 4 — Every level prepares the next
L1 should not be throw-away chatbot work.
Each level should create reusable capabilities for the next level.
 
Principle 5 — Domain expertise is the core asset
The differentiation of AI-powered mNet is not the underlying LLM.
It is:
Network engineering knowledge + mNet capabilities + customer experience + AI.
 
Principle 6 — L6 is controlled continuous evolution
mNet may use AI to discover, acquire, compose, or generate capabilities, but new capabilities must pass appropriate validation, security, quality, and release controls before becoming part of the product.
 
14. The business architecture conclusion
I would summarize the proposal to management in one diagram:
                     AI-POWERED mNet
                           │
                           ▼
              ┌─────────────────────────┐
              │  AI Engagement Maturity │
              └─────────────────────────┘
                           │
       ┌───────────────────┼───────────────────┐
       ▼                   ▼                   ▼
  INFORMATION        UNDERSTANDING        INVESTIGATION
      L1                   L2                   L3
       │                   │                   │
       └───────────────────┼───────────────────┘
                           ▼
                    RECOMMENDATION
                         / PLANNING
                            L4
                           │
                           ▼
                       EXECUTION
                           L5
                           │
                           ▼
                 CONTINUOUS EVOLUTION
                           L6
                           │
                           ▼
              ┌─────────────────────────┐
              │   Better mNet          │
              │   More capabilities     │
              │   Better user outcome   │
              └────────────┬────────────┘
                           │
                           └───────► Feedback
The business vision in one sentence:
Transform mNet from a traditional network evaluation and planning tool into an AI-powered engineering platform that progressively moves from providing information, to understanding and diagnosing network problems, to recommending and executing engineering workflows, and ultimately to continuously evolving its own capabilities based on customer needs.
That would be my Business/Solution Architecture baseline.
I would deliberately stop here before discussing Java, Python, LLMs, RAG, MCP, agents, databases, deployment, etc. Those belong to the System Architecture proposal, which should be derived from these business capabilities and maturity targets rather than driving them.
 
