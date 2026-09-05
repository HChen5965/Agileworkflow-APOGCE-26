# **AgileWkfl **

**With further details remain confidential at the moment, skill specification document shared, which could allow engineer and colleagues to realise the system by skill guided AIGC.**

# AAN System Reproduction Guide
## How to Reproduce the AAN Agile Workflow Formation and Self-Refinery System from the Skill

**Language:** British English  
**Purpose:** Engineering reproduction guide  
**Basis:** The AAN framework described in the supplied paper and the `AAN-Agile-Workflow-Formation` Skill Specification.

---

## 1. Reproduction Objective

The objective is to reproduce a usable implementation of the AAN system described in the paper:

> *Agile AI Agentic Neural Based Framework For Complex Workflow Forming, With Multiple Implementations Upon Upstream And Downstream Scenarios*

The reproduction should not stop at a conventional multi-agent application. The key capability to reproduce is:

**Intent → Task Decomposition → Dynamic Agent Network Formation → Workflow Execution → Verification → Human/Machine Feedback → Workflow Refinement → Historical Case Storage → Future Workflow Reuse**

The paper describes a three-layer architecture comprising an **Input Layer**, **Computational Layer**, and **Output Layer**. The Computational Layer dynamically organises sub-agents, while the Output Layer verifies generated workflows and supports feedback-driven refinement.

---

# 2. Target System

The target architecture should be implemented as follows:

```text
                    AAN SYSTEM
                        │
        ┌───────────────┴────────────────┐
        │                                │
   INPUT LAYER                    COMPUTATIONAL LAYER
        │                                │
 Scene Recognition                  Supervisor Agent
 Intent Understanding               Agent Registry
 Task Decomposition                 Model Selection
 Multimodal Fusion                  Tool Selection
 Data Observation                   Dynamic Topology
        │                                │
        └───────────────┬────────────────┘
                        ↓
                 WORKFLOW GRAPH
                        ↓
                 EXECUTION ENGINE
                        ↓
                OUTPUT / VERIFIER
                        ↓
              ┌─────────┴─────────┐
              ↓                   ↓
       Human Feedback       Machine Feedback
              └─────────┬─────────┘
                        ↓
                REFINEMENT ENGINE
                        ↓
                   HISTORIAN
                        │
                        └──────→ Future Cases
```

The central engineering principle is:

> **The workflow must be formed dynamically according to the current intent, context, available agents, tools, historical cases and feedback.**

It should not be implemented as one permanently fixed chain.

---

# 3. Minimum Reproducible System

Build the following ten components first:

1. Foundation Model Gateway
2. Supervisor Agent
3. Agent Registry
4. Tool Registry
5. Workflow Graph Engine
6. Workflow Executor
7. Workflow Verifier
8. Historical Case Store
9. Semantic Case Retriever
10. Workflow Refinement Engine

A first reproduction does not require foundation-model fine-tuning. The paper describes improvement through historical case retrieval, contextual adaptation and workflow refinement rather than mandatory modification of the foundation-model weights.

---

# 4. Recommended Development Sequence

Implement the system in the following order:

| Stage | Development Target | Main Deliverable |
|---|---|---|
| 1 | Agent Runtime | Agents can execute tasks |
| 2 | Workflow Graph | Dynamic graph execution |
| 3 | Supervisor | Intent → tasks → agents |
| 4 | Dynamic Topology | Different problems generate different graphs |
| 5 | Tool System | Agents can use operational tools |
| 6 | Historian | Execution cases are persistently stored |
| 7 | Retrieval | Similar historical cases can be recalled |
| 8 | In-context Refinery | Historical workflows can be adapted |
| 9 | Workflow Verifier | Workflow rationality is scored |
| 10 | Dual Feedback | Human + machine feedback is collected |
| 11 | Closed Loop | Failed workflows are automatically refined |
| 12 | Benchmark | Paper-style experiments are reproduced |
| 13 | Industrial Scenario | The framework is tested on a real upstream/downstream scenario |

**Important milestone:** Stages 10–11.

Before these stages, the system is primarily a multi-agent workflow engine. After these stages, it implements the paper's central agile workflow formation and self-refinery mechanism.

---

# 5. Stage 1 — Build the Agent Runtime

Create a generic agent interface.

Each agent should contain:

```json
{
  "agent_id": "pipeline_diagnosis_agent",
  "name": "Pipeline Diagnosis Agent",
  "capabilities": [
    "fault diagnosis",
    "pipeline analysis"
  ],
  "tools": [
    "SCADA Query",
    "Historical Case Search",
    "Pipeline Simulator"
  ],
  "model": "foundation_model"
}
```

Each agent should support:

```text
receive task
↓
reason
↓
invoke tools if necessary
↓
produce structured result
↓
return execution trace
```

Do not hard-code an agent to one complete business workflow. Agents should expose reusable capabilities.

---

# 6. Stage 2 — Build the Workflow Graph

Represent each workflow as:

```text
G = (V, E)
```

where:

- `V` = workflow nodes;
- `E` = dependencies or communication relationships.

Example:

```json
{
  "nodes": [
    "anomaly_detection",
    "historical_retrieval",
    "diagnosis",
    "simulation",
    "recommendation"
  ],
  "edges": [
    ["anomaly_detection", "historical_retrieval"],
    ["anomaly_detection", "diagnosis"],
    ["historical_retrieval", "diagnosis"],
    ["diagnosis", "simulation"],
    ["simulation", "recommendation"]
  ]
}
```

The graph engine must support at least:

- sequential execution;
- parallel execution;
- branching;
- conditional execution;
- iterative execution;
- node replacement;
- node addition;
- node deletion;
- edge modification.

This graph becomes the core execution representation of the AAN workflow.

---

# 7. Stage 3 — Implement the Input Layer

Create an Input Engine containing:

```text
Scene Recognition
Intent Understanding
Task Decomposition
Multimodal Fusion
Data Quality Observation
```

The input interface can be:

```python
aan_input(user_request, context)
```

and should return:

```json
{
  "scene": "...",
  "intent": "...",
  "entities": [],
  "constraints": [],
  "time_window": "...",
  "required_outputs": []
}
```

Example:

```text
User:
"Determine why pipeline pressure has become abnormal
and recommend an operational response."
```

Possible structured interpretation:

```text
Scene:
Pipeline operational anomaly

Intent:
Diagnose abnormal pressure and recommend response

Candidate Tasks:
1. Detect anomaly
2. Characterise anomaly
3. Retrieve historical cases
4. Diagnose possible causes
5. Generate response options
6. Evaluate consequences
7. Recommend action
8. Verify recommendation
```

The task list should be generated dynamically.

---

# 8. Stage 4 — Implement the Supervisor Agent

The Supervisor is the central orchestration component.

Its responsibilities are:

1. receive the structured intent;
2. inspect the task graph;
3. retrieve historical cases;
4. select agents;
5. select models;
6. select tools;
7. determine network topology;
8. assemble the workflow;
9. monitor execution;
10. trigger verification;
11. trigger refinement;
12. decide whether the result can be accepted.

Conceptually:

```text
                    Supervisor
                  /      |      \
                 /       |       \
             Agent A   Agent B   Agent C
                |         |         |
              Tool A    Tool B    Tool C
```

The Supervisor should make these decisions dynamically.

---

# 9. Stage 5 — Implement Dynamic Topology

The system must be able to choose different agent-network structures.

## Sequential

```text
A → B → C → D
```

Use when tasks are strongly dependent.

## Parallel

```text
       ┌→ B ─┐
A ─────┤     ├→ D
       └→ C ─┘
```

Use when tasks can be performed independently.

## Hierarchical

```text
          Supervisor
          /        \
       Agent A   Agent B
                  /   \
              Agent C Agent D
```

Use when tasks naturally form levels.

## Iterative

```text
A → B → C
    ↑   ↓
    └───┘
```

Use when the workflow requires repeated reasoning or validation.

## General Graph

```text
A → B → D
 \       ↑
  → C ───┘
```

Use when dependencies are non-linear.

The topology should be represented explicitly:

```json
{
  "topology": "graph",
  "nodes": [],
  "edges": [],
  "conditions": [],
  "parallel_groups": [],
  "termination_condition": ""
}
```

---

# 10. Stage 6 — Implement the Tool Registry

Each agent should use only registered tools.

Example tools:

```text
SCADA Query
Production Database
Maintenance Database
Weather API
Pipeline Simulator
Wellbore Diagnostic Tool
Economic Evaluation Tool
Knowledge Retrieval
Code Execution
```

Each tool should expose:

```json
{
  "name": "...",
  "description": "...",
  "input_schema": {},
  "output_schema": {},
  "risk_level": "...",
  "timeout": 0
}
```

The Supervisor should check tool compatibility before execution.

---

# 11. Stage 7 — Implement the Historian

Create a persistent historical case store.

For each workflow execution, store:

```json
{
  "case_id": "CASE_000128",
  "scene": "...",
  "intent": "...",
  "input_context": "...",
  "task_graph": "...",
  "agents": [],
  "tools": [],
  "topology": "...",
  "prompts": [],
  "execution_trace": [],
  "workflow": {},
  "result": {},
  "human_feedback": {},
  "machine_feedback": {},
  "final_score": 0.0,
  "success": true
}
```

Both successful and informative failed cases should be retained.

The Historian is essential because future workflows need access to previous operational experience.

---

# 12. Stage 8 — Implement Semantic Case Retrieval

When a new scenario arrives:

```text
New Scenario
     ↓
Semantic Retrieval
     ↓
Historical Cases
     ↓
Similarity Ranking
     ↓
Successful / Relevant Cases
     ↓
Reusable Workflow Patterns
```

Retrieval should consider:

- scene similarity;
- intent similarity;
- entity similarity;
- task similarity;
- operational constraints;
- tool similarity;
- historical success score.

The retrieved workflow is a candidate pattern, not a solution to be copied blindly.

---

# 13. Stage 9 — Implement the In-context Refinery

The In-context Refinery compares the current problem with retrieved historical cases.

```text
Historical Case
       +
Current Scenario
       ↓
Difference Analysis
       ↓
Workflow Adaptation
       ↓
Candidate Refined Workflow
```

For a first occurrence:

```text
Reason → Plan → Form Workflow → Execute → Evaluate
```

For a repeated or similar occurrence:

```text
Retrieve → Compare → Adapt → Verify → Execute
```

Improvement should be achieved through:

- historical retrieval;
- prompt refinement;
- task restructuring;
- agent selection;
- tool selection;
- topology modification;
- feedback incorporation.

No foundation-model weight update is required for the basic reproduction.

---

# 14. Stage 10 — Implement Workflow Verification

Before execution, verify the generated workflow.

The verifier should assess:

1. task completeness;
2. dependency correctness;
3. agent capability compatibility;
4. tool availability;
5. data sufficiency;
6. logical consistency;
7. operational constraints;
8. safety constraints;
9. expected output completeness.

Return a rationality score:

```json
{
  "score": 0.86,
  "task_completeness": 0.91,
  "dependency_correctness": 0.88,
  "agent_compatibility": 0.83,
  "tool_availability": 1.00,
  "data_sufficiency": 0.77,
  "safety": 0.95
}
```

Define:

```text
γ = minimum acceptable rationality score
```

Then:

```text
score ≥ γ → execute
score < γ → refine
```

---

# 15. Stage 11 — Implement Dual Feedback

The system needs two feedback streams:

```text
Human Feedback
       +
Machine Feedback
       ↓
Combined Feedback
       ↓
Workflow Refinement
```

The combined feedback should follow:

```text
f = αfh + (1 − α)fm
```

where:

- `fh` = human feedback;
- `fm` = machine feedback;
- `α` = human-feedback weighting factor.

Example configuration:

```json
{
  "human_weight": 0.6,
  "machine_weight": 0.4
}
```

Human feedback may include:

```text
wrong diagnosis
wrong agent
missing task
unsafe recommendation
wrong execution order
```

Machine feedback may include:

```text
tool failure
constraint violation
low confidence
simulation failure
data-quality problem
execution failure
```

---

# 16. Stage 12 — Make Feedback Modify the Workflow Graph

This is a critical reproduction step.

Suppose the original workflow is:

```text
A → B → C
```

Machine feedback:

```text
C cannot operate because required data are missing.
```

The Refinery may generate:

```text
A → Data Quality Check → B → C
```

Human feedback:

```text
Historical incidents must be checked before diagnosis.
```

The graph may become:

```text
A → Historical Retrieval → B → C
```

The refinement engine should support:

```text
ADD_NODE
DELETE_NODE
REPLACE_NODE
REORDER
CHANGE_AGENT
CHANGE_TOOL
CHANGE_TOPOLOGY
ADD_VALIDATION
ADD_HUMAN_APPROVAL
SPLIT_TASK
MERGE_TASK
REFINE_PROMPT
RETRIEVE_ANOTHER_CASE
```

This is the point at which the system becomes a genuinely adaptive workflow-forming architecture.

---

# 17. Stage 13 — Close the Full Refinement Loop

The runtime should implement:

```text
Generate Workflow
       ↓
Verify Workflow
       ↓
     score ≥ γ ?
    /          \
  YES           NO
   ↓             ↓
Execute       Collect Feedback
   ↓             ↓
Observe       Modify Graph
   ↓             ↓
Evaluate ←──── Regenerate
   ↓
Store Case
```

A practical control loop is:

```python
workflow = generate_workflow(intent, context)

score = verify_workflow(workflow)

while score < threshold:

    human_feedback = collect_human_feedback(workflow)

    machine_feedback = collect_machine_feedback(workflow)

    feedback = combine_feedback(
        human_feedback,
        machine_feedback,
        alpha=human_weight
    )

    workflow = refine_workflow(
        workflow,
        feedback
    )

    score = verify_workflow(workflow)

result = execute(workflow)

evaluation = evaluate_execution(
    workflow,
    result
)

store_historian_case(
    workflow=workflow,
    result=result,
    evaluation=evaluation
)
```

The important property is that the workflow itself changes as a result of feedback.

---

# 18. Stage 14 — Implement the Complete AAN Runtime

The complete runtime should follow:

```text
INPUT
  ↓
Scene Recognition
  ↓
Intent Understanding
  ↓
Self-consistency Validation
  ↓
Task Decomposition
  ↓
Multimodal Fusion
  ↓
Historical Case Retrieval
  ↓
Candidate Workflow Generation
  ↓
Agent Selection
  ↓
Model Selection
  ↓
Topology Selection
  ↓
Tool Selection
  ↓
Workflow Assembly
  ↓
Workflow Verification
  ↓
Score ≥ γ ?
 ├── YES → Execute
 │           ↓
 │        Observe
 │           ↓
 │        Evaluate
 │           ↓
 │      Store Case
 │
 └── NO → Human Feedback
             +
          Machine Feedback
             ↓
        Combined Feedback
             ↓
        Graph Modification
             ↓
        Workflow Regeneration
             ↓
        Verification
             ↓
           Repeat
```

---

# 19. Stage 15 — Reproduce the Paper's Experimental Design

After the engineering system works, reproduce the experimental methodology.

The paper evaluates:

1. Event sensing
2. Task planning
3. Agent liaison
4. Tool use
5. Agile workflow formation
6. Continuous self-refinery

A suitable evaluation dataset should contain repeated occurrences of the same or similar operational events.

For example:

```text
Event A
 ├── Occurrence 1
 ├── Occurrence 2
 └── Occurrence 3

Event B
 ├── Occurrence 1
 ├── Occurrence 2
 └── Occurrence 3
```

The paper describes a mixed evaluation involving 20 events with three temporal occurrences per event.

---

# 20. Upstream Reproduction Scenario

The paper demonstrates an upstream scenario involving shale-oil wellbores.

A reproduction can use a wellbore maintenance dataset containing events such as:

```text
Pump leakage
Rod breakage
Scaling
Wax deposition
Routine maintenance
```

Create specialised agents such as:

```text
Wellbore Data Agent
Anomaly Detection Agent
Failure Diagnosis Agent
Maintenance Agent
Historical Case Agent
Production Evaluation Agent
Safety Agent
Report Agent
```

The AAN should dynamically select and organise these agents rather than using one fixed chain.

---

# 21. Downstream Reproduction Scenario

The paper also demonstrates downstream transnational natural-gas pipeline operations.

Relevant event classes include:

```text
Equipment failure
Human factors
Climate / meteorological conditions
Gas supply fluctuations
Gas demand fluctuations
Commodity-price volatility
End-user demand changes
```

Possible agents include:

```text
Pipeline Monitoring Agent
Gas Quality Agent
Forecasting Agent
Weather Agent
Supply-Demand Agent
Fault Diagnosis Agent
Scheduling Agent
Simulation Agent
Safety Agent
Decision Support Agent
```

The same AAN core should dynamically form a workflow for these tasks.

---

# 22. The Most Important Experiment: Repeated Events

The strongest demonstration of self-refinery is repeated-event evaluation.

For each event, measure:

```text
Occurrence 1
- workflow accuracy
- workflow formation time
- execution time
- human intervention
- workflow complexity

Occurrence 2
- workflow accuracy
- workflow formation time
- execution time
- human intervention
- workflow complexity

Occurrence 3
- workflow accuracy
- workflow formation time
- execution time
- human intervention
- workflow complexity
```

The desired pattern is:

```text
Accuracy:
Occurrence 1 → Occurrence 2 → Occurrence 3
       ↗             ↗

Formation Time:
Occurrence 1 → Occurrence 2 → Occurrence 3
       ↘             ↘
```

The purpose is to demonstrate that historical experience improves subsequent workflow formation.

---

# 23. Cross-domain Generalisation Experiment

After validating each scenario independently, combine upstream and downstream cases.

```text
             Common AAN Core
                    │
        ┌───────────┴───────────┐
        ↓                       ↓
 Upstream Scenario       Downstream Scenario
        │                       │
 Wellbore Agents          Pipeline Agents
        │                       │
        └───────────┬───────────┘
                    ↓
             Common Historian
                    ↓
             Common Refinery
```

This demonstrates whether the workflow-forming mechanism generalises across materially different oil-and-gas scenarios.

---

# 24. Feedback Ablation Experiment

Reproduce four configurations:

### A — Global Dual Feedback

```text
Human + Agent
      ↓
Input + Computational layers
```

### B — Global Agentic Feedback

```text
Agent feedback
      ↓
Input + Computational layers
```

### C — Input-layer Dual Feedback

```text
Human + Agent
      ↓
Input layer
```

### D — Input-layer Agentic Feedback

```text
Agent feedback
      ↓
Input layer
```

Compare:

```text
Accuracy
Standard deviation
Workflow formation
Continuous refinement
Robustness
```

The paper reports the global dual-feedback configuration as the strongest overall configuration.

---

# 25. Final Evaluation Matrix

Use the following final matrix:

| Capability | Test | Evidence |
|---|---|---|
| Scene Recognition | New scenario classification | Correct scene |
| Intent Understanding | Complex request | Structured intent |
| Task Planning | Unseen task | Valid task graph |
| Agent Liaison | Multiple agents | Correct coordination |
| Tool Use | Tool-enabled task | Correct tool selection |
| Dynamic Topology | Different scenarios | Different graphs |
| Workflow Formation | Unseen problem | Executable workflow |
| Verification | Invalid workflow | Rejection/refinement |
| Human Feedback | Human correction | Graph modification |
| Machine Feedback | Execution failure | Graph modification |
| Historical Reuse | Repeated event | Case retrieval |
| Self-Refinery | Repeated event | Improved workflow |
| Cross-domain Generalisation | Upstream + downstream | Common AAN engine |

---

# 26. Definition of Done

The reproduction should only be considered complete when the following demonstration works:

```text
Give the system a previously unseen complex operational intent
        ↓
The system recognises the scenario
        ↓
The system understands the intent
        ↓
The system decomposes the task
        ↓
The system retrieves relevant historical experience
        ↓
The system selects agents, models and tools
        ↓
The system dynamically constructs an agent network
        ↓
The system generates an executable workflow
        ↓
The system verifies the workflow
        ↓
The system executes the workflow
        ↓
Human + machine feedback is collected
        ↓
The system modifies the workflow when necessary
        ↓
The successful case is stored
        ↓
A similar event occurs later
        ↓
The system retrieves the previous case
        ↓
The system adapts rather than blindly copies it
        ↓
The new workflow is executed
        ↓
The new result is evaluated
        ↓
The system demonstrates measurable improvement
```

---

# 27. What Counts as a True Reproduction?

A system should NOT be called a successful reproduction merely because it has:

- multiple agents;
- an LLM;
- tool calling;
- a workflow engine;
- a vector database;
- a chat interface.

The reproduction must demonstrate the combination of:

```text
Dynamic Workflow Formation
        +
Dynamic Agent Organisation
        +
Historical Case Reuse
        +
Workflow Verification
        +
Human/Machine Dual Feedback
        +
Dynamic Workflow Refinement
        +
Repeated-case Improvement
```

The defining feature is therefore:

> **The system learns operationally by refining how it forms workflows, rather than merely improving a model's text-generation capability.**

---

# 28. Recommended First Prototype

For the fastest technically meaningful prototype, implement one complete scenario first:

```text
Pipeline Operational Anomaly
        ↓
Intent Engine
        ↓
Task Decomposition
        ↓
Supervisor
        ↓
5–8 Specialised Agents
        ↓
Dynamic Graph
        ↓
SCADA / Historical / Simulation Tools
        ↓
Workflow Verifier
        ↓
Human + Machine Feedback
        ↓
Graph Refinement
        ↓
Historian
        ↓
Repeated Event
        ↓
Workflow Reuse + Adaptation
```

Once this loop is stable, add the wellbore scenario.

Then test whether the same AAN engine can handle both.

This gives a much stronger reproduction than building two independent application-specific multi-agent systems.

---

# 29. Recommended Engineering Principle

Keep the following separation throughout implementation:

```text
Foundation Model
        ↓
General reasoning capability

Agent
        ↓
Task-specific capability + tools

AAN
        ↓
Dynamic organisation + workflow formation

Historian
        ↓
Operational experience

Refinery
        ↓
Continuous workflow improvement
```

The AAN layer should therefore remain independent from any single foundation model or individual business application.

---

# 30. Final Reproduction Architecture

The completed system should look like:

```text
                         USER / SYSTEM
                               │
                               ↓
                    ┌────────────────────┐
                    │    INPUT ENGINE    │
                    │ Scene Recognition  │
                    │ Intent Understanding│
                    │ Task Decomposition │
                    │ Multimodal Fusion  │
                    └─────────┬──────────┘
                              ↓
                    ┌────────────────────┐
                    │  AAN SUPERVISOR    │
                    │ Agent Selection    │
                    │ Model Selection    │
                    │ Tool Selection     │
                    │ Topology Selection │
                    └─────────┬──────────┘
                              ↓
                 ┌──────────────────────────┐
                 │     DYNAMIC GRAPH       │
                 │                          │
                 │ Agent A ↔ Agent B       │
                 │     ↘ Agent C → Agent D │
                 │          ↖ Agent E      │
                 └────────────┬─────────────┘
                              ↓
                    ┌────────────────────┐
                    │ EXECUTION ENGINE   │
                    └─────────┬──────────┘
                              ↓
                    ┌────────────────────┐
                    │ WORKFLOW VERIFIER  │
                    └─────────┬──────────┘
                              ↓
                    ┌────────────────────┐
                    │ HUMAN + MACHINE    │
                    │ FEEDBACK           │
                    └─────────┬──────────┘
                              ↓
                    ┌────────────────────┐
                    │ REFINEMENT ENGINE  │
                    │ Graph Modification │
                    │ Prompt Refinement  │
                    │ Workflow Rebuild   │
                    └─────────┬──────────┘
                              ↓
                    ┌────────────────────┐
                    │     HISTORIAN      │
                    │ Cases + Traces +   │
                    │ Feedback + Scores  │
                    └─────────┬──────────┘
                              │
                              └────→ Future Workflow Formation
```

**End state:**

The reproduced system should be capable of dynamically forming, executing, evaluating and refining complex agentic workflows across different upstream and downstream oil-and-gas scenarios, with historical experience continuously feeding future workflow formation.
