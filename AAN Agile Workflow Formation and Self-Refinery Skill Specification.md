# AAN Agile Workflow Formation and Self-Refinery Skill Specification

## 1. Skill Identity

**Skill Name:** `AAN-Agile-Workflow-Formation`

**Version:** `1.0`

**Language:** British English

**Purpose:**  
To reproduce the Agile Agentic Neural (AAN) framework described in the paper *Agile AI Agentic Neural Based Framework For Complex Workflow Forming, With Multiple Implementations Upon Upstream And Downstream Scenarios* as a usable, executable agentic system.

**Primary Capability:**  
Given a complex operational intent and heterogeneous business data, autonomously:

1. understand the operational scene;
2. decompose the intent into executable sub-tasks;
3. construct an appropriate agent workflow;
4. dynamically select an agent-network topology;
5. retrieve and reuse historically successful cases;
6. coordinate specialised agents and tools;
7. generate an executable workflow;
8. verify the workflow;
9. collect human and machine feedback;
10. dynamically refine the workflow;
11. retain successful execution cases for future reuse.

The reproduced system SHALL prioritise **agile workflow formation and continuous self-refinement**, rather than merely providing conversational question answering.

---

# 2. Conceptual Foundation

The implementation SHALL reproduce the three-layer AAN architecture described in the source paper:

```text
┌─────────────────────────────────────────────┐
│                 INPUT LAYER                 │
│ Scene Recognition                           │
│ Intent Understanding                        │
│ Task Decomposition                          │
│ Multimodal Fusion                           │
│ Data Quality Observation                    │
└──────────────────────┬──────────────────────┘
                       ↓
┌─────────────────────────────────────────────┐
│             COMPUTATIONAL LAYER             │
│ Supervisor Agent                            │
│ Distributed Sub-Agent Cluster               │
│ Model Selection                              │
│ Dynamic Network Topology                    │
│ Tool Invocation                              │
│ Historical Case Retrieval                   │
│ In-context Refinery                         │
└──────────────────────┬──────────────────────┘
                       ↓
┌─────────────────────────────────────────────┐
│                  OUTPUT LAYER               │
│ Workflow Assembly                           │
│ Workflow Verification                       │
│ Rationality Scoring                          │
│ Human Feedback                              │
│ Machine Feedback                            │
│ Dynamic Graph Modification                  │
│ AIGC-based Workflow Regeneration            │
└──────────────────────┬──────────────────────┘
                       ↑
                 Self-refinement
                       │
                 Historian Log
```

The source architecture explicitly defines an input layer, computational layer and output layer. The computational layer dynamically organises sub-agents, while the output layer verifies the generated workflow and activates dual feedback when the rationality score is below the required threshold.

---

# 3. Core Design Principle

The implementation SHALL NOT treat the workflow as a fixed chain.

Instead:

> **The workflow is an emergent execution structure dynamically formed according to the current intent, scene, available tools, historical cases and feedback.**

Therefore, the system SHALL distinguish between:

- **Intent**
- **Task**
- **Agent**
- **Tool**
- **Model**
- **Workflow**
- **Network topology**
- **Execution case**
- **Feedback**
- **Refined workflow**

A workflow SHALL be regarded as a graph:

```text
G = (V, E)
```

where:

- `V` represents tasks, agents or executable nodes;
- `E` represents execution or information dependencies.

The system SHALL permit the graph to change during refinement.

---

# 4. System Inputs

The Skill SHALL accept heterogeneous operational inputs.

Supported input types SHOULD include:

- text;
- structured business data;
- time-series signals;
- SCADA data;
- production logs;
- maintenance records;
- images;
- speech;
- external knowledge;
- historical execution records;
- user instructions;
- machine observations.

The source implementation explicitly describes multimodal input `X`, including text, speech, image and signals.

Example:

```text
User Intent:
"Determine the likely cause of abnormal gas pressure
and recommend an appropriate operational response."

Context:
- SCADA measurements
- pipeline operating status
- weather information
- historical incidents
- equipment status
- supply and demand information
```

---

# 5. Input Layer Skill

## 5.1 Scene Recognition

The system SHALL first identify the operational scenario.

Example scene classes:

```text
WELLBORE_MAINTENANCE
PIPELINE_SCHEDULING
EQUIPMENT_FAILURE
PRODUCTION_ANOMALY
WEATHER_DISTURBANCE
SUPPLY_DEMAND_DISTURBANCE
UNKNOWN_COMPLEX_SCENARIO
```

The classifier MAY be implemented using:

- an LLM;
- a multimodal model;
- a rule-assisted classifier;
- an ontology;
- a hybrid approach.

The implementation SHOULD retain the original raw input together with the recognised scene.

---

## 5.2 Intent Understanding

Convert the user's operational request into a structured intent:

```json
{
  "intent": "...",
  "scene": "...",
  "object": "...",
  "objective": "...",
  "constraints": [],
  "time_window": "...",
  "required_outputs": []
}
```

The paper represents this as:

```text
z_intent = Φ_intent(X)
```

Intent understanding SHALL be subjected to a self-consistency check where practical.

For example:

```text
Candidate interpretation 1
Candidate interpretation 2
Candidate interpretation 3
        ↓
Consistency / majority evaluation
        ↓
Validated intent
```

---

# 6. Task Decomposition Skill

The validated intent SHALL be decomposed into executable tasks:

```text
{τ1, τ2, ..., τK}
```

These tasks SHALL form a directed task graph:

```text
G = (V, E)
```

Example:

```text
Detect anomaly
      ↓
Characterise anomaly
      ↓
Retrieve historical cases
      ↓
Diagnose possible causes
      ↓
Generate response options
      ↓
Evaluate operational impact
      ↓
Select recommended response
      ↓
Verify recommendation
```

Task decomposition SHALL be context-dependent.

The same intent SHALL be permitted to generate different task graphs under different operational conditions.

---

# 7. Multimodal Fusion Skill

The system SHALL combine available data sources into a task-relevant context:

```text
X_fused = Φ_fuse(X)
```

The fusion process SHALL include data-quality observation.

The system SHOULD detect:

- missing data;
- contradictory observations;
- abnormal values;
- stale information;
- insufficient contextual information;
- incompatible data sources.

A data-quality observer SHALL be capable of triggering:

```text
request additional data
        OR
reduce confidence
        OR
change workflow
        OR
activate human intervention
```

---

# 8. Computational Layer

## 8.1 Supervisor Agent

The Supervisor Agent is the central orchestration component.

Responsibilities:

1. receive the task graph;
2. select appropriate sub-agents;
3. select models;
4. select tools;
5. determine network topology;
6. distribute tasks;
7. monitor execution;
8. retrieve historical cases;
9. compare candidate strategies;
10. trigger refinement;
11. decide when the workflow is sufficiently reliable.

Conceptually:

```text
                Supervisor
              /      |      \
             /       |       \
       Agent A    Agent B    Agent C
         |           |          |
       Tool A      Tool B     Tool C
```

---

# 9. Sub-Agent Formation

For each task `τk`, the Supervisor SHALL select or instantiate an appropriate sub-agent:

```text
τk → Agent Ak
```

The system SHALL maintain an Agent Registry.

Example:

```json
{
  "agent_id": "pipeline_diagnosis_agent",
  "capabilities": [
    "fault_diagnosis",
    "historical_case_retrieval",
    "pipeline_analysis"
  ],
  "tools": [
    "SCADA_query",
    "historical_case_search",
    "simulation"
  ],
  "model": "selected_foundation_model"
}
```

Agents SHALL be capability-oriented rather than permanently assigned to a single fixed workflow.

---

# 10. Dynamic Model Selection

For each sub-task:

```text
θk = Φ_model(τk)
```

The system SHALL select an appropriate model according to:

- task type;
- required reasoning capability;
- latency;
- available tools;
- historical performance;
- data modality;
- task complexity.

The initial reproduction MAY use one foundation model for all agents.

A later implementation SHOULD support a model pool.

Example:

```text
Simple classification → lightweight model
Complex reasoning → reasoning model
Multimodal interpretation → multimodal model
Code generation → coding model
Numerical simulation → specialised model/tool
```

---

# 11. Dynamic Network Topology Skill

This is a core reproduction requirement.

The system SHALL NOT use one permanent topology.

At minimum, the implementation SHOULD support:

### 11.1 Sequential / Layer Topology

```text
A → B → C → D
```

Use when tasks are strongly dependent.

### 11.2 Tree Topology

```text
          Supervisor
         /    |    \
        A     B     C
             / \
            D   E
```

Use when hierarchical decomposition is appropriate.

### 11.3 Mesh Topology

```text
A ↔ B
↕   ↕
C ↔ D
```

Use when agents require iterative mutual communication.

### 11.4 Workflow / Graph Topology

```text
A → B → D
 \       ↑
  → C ───┘
```

Use when the task contains conditional or branching logic.

### 11.5 Code-Generated Topology

The system MAY generate executable orchestration code when a conventional static topology is insufficient.

The topology SHALL therefore be represented as an explicit object:

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

# 12. Historical Case Retrieval Skill

Historical experience is a fundamental component of the reproduced system.

The system SHALL maintain a **Historian Log**.

Each successful execution case SHOULD contain:

```json
{
  "case_id": "...",
  "scene": "...",
  "intent": "...",
  "input_context": "...",
  "task_graph": "...",
  "agents": [],
  "tools": [],
  "prompts": [],
  "execution_trace": [],
  "workflow": "...",
  "result": "...",
  "human_feedback": "...",
  "machine_metrics": "...",
  "final_score": 0.0,
  "success": true
}
```

The paper specifically states that historical prompts, chains of thought, plans and outcomes are retrieved and semantically compared to adapt execution strategies, without fine-tuning or retraining the underlying foundation model.

---

# 13. Semantic Case Matching

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
Successful Cases
      ↓
Reusable Workflow Patterns
```

Retrieval SHOULD consider:

- scene similarity;
- intent similarity;
- entity similarity;
- task similarity;
- temporal similarity;
- tool similarity;
- operational constraints;
- historical success score.

The system SHALL distinguish:

```text
case reuse
```

from:

```text
case copying
```

A historical workflow SHALL be treated as a candidate pattern rather than an unquestionable solution.

---

# 14. In-context Refinery Skill

For every selected historical case:

```text
Historical Case
      ↓
Current Scenario
      ↓
Difference Analysis
      ↓
Workflow Adaptation
      ↓
Candidate Refined Workflow
```

The foundation model weights SHALL NOT be modified during normal operation.

Instead, improvement SHALL occur through:

- historical case retrieval;
- prompt refinement;
- task restructuring;
- agent selection;
- tool selection;
- topology modification;
- feedback incorporation.

This reproduces the paper's central principle of continuous refinement without foundation-model fine-tuning.

---

# 15. Tool Invocation Skill

Every agent SHALL have access only to explicitly registered tools.

Example:

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

Each tool SHALL expose:

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

The Supervisor SHALL verify tool availability before workflow execution.

---

# 16. Workflow Generation Skill

The system SHALL assemble the final workflow:

```text
Y = Φ_output({y1, y2, ..., yK})
```

The workflow SHOULD contain:

```json
{
  "workflow_id": "...",
  "intent": "...",
  "scene": "...",
  "nodes": [],
  "edges": [],
  "agents": [],
  "tools": [],
  "conditions": [],
  "expected_outputs": [],
  "verification_rules": []
}
```

The output SHALL be executable or directly translatable into an executable orchestration graph.

---

# 17. Workflow Rationality Verification

The system SHALL calculate a workflow rationality score:

```text
r = Φ_verify(Y)
```

The verification layer SHOULD evaluate:

1. task completeness;
2. task dependency correctness;
3. agent capability compatibility;
4. tool availability;
5. data sufficiency;
6. logical consistency;
7. operational constraints;
8. safety constraints;
9. expected output completeness.

Define:

```text
γ = minimum acceptable rationality score
```

If:

```text
r ≥ γ
```

the workflow MAY proceed.

If:

```text
r < γ
```

the refinement loop SHALL be activated.

---

# 18. Dual Feedforward Feedback Skill

This is one of the most important components to reproduce.

The system SHALL collect two feedback streams:

```text
Human Feedback       Machine Feedback
       │                    │
       └────────┬───────────┘
                ↓
        Combined Feedback
```

The paper defines:

```text
f = αfh + (1 − α)fm
```

where:

- `fh` = human feedback;
- `fm` = machine feedback;
- `α` = human-feedback weighting factor.

The implementation SHALL make `α` configurable.

Example:

```json
{
  "human_weight": 0.6,
  "machine_weight": 0.4
}
```

---

# 19. Human Feedback Skill

Human feedback SHOULD be collected at meaningful control points.

Example:

```text
[Accept]
[Reject]
[Modify]
[Unsafe]
[Insufficient information]
```

More detailed feedback MAY include:

```text
wrong task
wrong agent
wrong tool
wrong execution order
wrong diagnosis
wrong recommendation
missing constraint
unsafe action
```

Human feedback SHALL be converted into machine-readable workflow corrections.

---

# 20. Machine Feedback Skill

Machine feedback MAY include:

- execution success;
- tool success/failure;
- numerical deviation;
- constraint violation;
- workflow latency;
- data-quality score;
- confidence;
- output consistency;
- simulation result;
- safety-rule violation.

Example:

```json
{
  "execution_success": false,
  "data_quality": 0.81,
  "constraint_violation": true,
  "tool_failure": false,
  "confidence": 0.72
}
```

---

# 21. Dynamic Workflow Refinement

The combined feedback SHALL be used to modify the workflow:

```text
ΔG = Φ_feedback(Y, f)
```

Possible modifications include:

```text
Add node
Delete node
Reorder nodes
Change agent
Change model
Change tool
Change topology
Add verification
Add human approval
Split task
Merge tasks
Repeat task
Change prompt
Retrieve another historical case
```

The paper describes these adjustments as structural modifications to the AAN and indicates that natural-language feedback may be translated into structural changes through AIGC coding.

---

# 22. Refinement Loop

The complete refinement loop SHALL implement:

```text
Generate Workflow
       ↓
Verify Workflow
       ↓
     r ≥ γ ?
    /       \
  YES        NO
   ↓          ↓
Execute    Collect Feedback
   ↓          ↓
Observe    Modify Graph
   ↓          ↓
Evaluate ← Regenerate
```

Pseudo-process:

```text
while rationality_score < threshold:

    human_feedback = collect_human_feedback()

    machine_feedback = collect_machine_feedback()

    combined_feedback =
        α * human_feedback +
        (1 - α) * machine_feedback

    workflow_graph =
        modify_graph(workflow_graph,
                     combined_feedback)

    workflow =
        regenerate(workflow_graph)

    rationality_score =
        verify(workflow)
```

This reproduces the feedback loop specified in Algorithm 1.

---

# 23. Execution Skill

After verification, the workflow SHALL be executable.

Execution SHALL generate a trace:

```text
Workflow
 ↓
Task 1
 ↓
Agent 1
 ↓
Tool
 ↓
Result
 ↓
Task 2
 ↓
Agent 2
 ↓
...
```

The execution trace SHALL be stored for later refinement.

The trace SHOULD contain:

```json
{
  "timestamp": "...",
  "node": "...",
  "agent": "...",
  "tool": "...",
  "input": "...",
  "output": "...",
  "latency": 0,
  "success": true,
  "error": null
}
```

---

# 24. Historian Log Update

After every completed workflow:

```text
Execution
   ↓
Evaluation
   ↓
Human Feedback
   ↓
Machine Feedback
   ↓
Final Score
   ↓
Historian Log
```

Successful workflows SHALL receive a high reuse priority.

Failed workflows SHOULD also be retained when the failure provides useful learning information.

The Historian Log therefore becomes the long-term operational memory of the AAN system.

---

# 25. Self-Refinery Skill

When an identical or similar event occurs again:

```text
Current Event
     ↓
Historical Search
     ↓
Successful Case
     ↓
Difference Analysis
     ↓
Workflow Adaptation
     ↓
Refined Workflow
     ↓
Verification
     ↓
Execution
```

The key distinction is:

### First occurrence

```text
Reason → Plan → Form Workflow → Execute → Evaluate
```

### Repeated occurrence

```text
Retrieve → Compare → Adapt → Refine → Verify → Execute
```

This is the operational manifestation of the self-refinery mechanism described in the paper.

---

# 26. Minimum Reproducible System

A practical reproduction SHALL first implement the following Minimum Viable AAN:

```text
1. LLM
2. Supervisor Agent
3. Three or more specialised Agents
4. Tool Registry
5. Workflow Graph Engine
6. Historical Case Store
7. Semantic Retrieval
8. Workflow Verifier
9. Human Feedback Interface
10. Machine Feedback Collector
11. Workflow Refinement Engine
12. Execution Logger
```

The first version DOES NOT require:

- foundation-model fine-tuning;
- training a new neural network;
- reinforcement-learning model training;
- a fully autonomous production deployment;
- a proprietary foundation model.

The paper itself describes AAN training/refinement without fine-tuning the underlying foundation models.

---

# 27. Recommended Implementation Architecture

A practical implementation MAY be organised as:

```text
                    ┌────────────────────┐
                    │    User / System   │
                    └─────────┬──────────┘
                              ↓
                    ┌────────────────────┐
                    │   Intent Engine    │
                    └─────────┬──────────┘
                              ↓
                    ┌────────────────────┐
                    │ Task Decomposition │
                    └─────────┬──────────┘
                              ↓
                    ┌────────────────────┐
                    │ Workflow Planner   │
                    └─────────┬──────────┘
                              ↓
             ┌────────────────────────────────┐
             │       Supervisor Agent         │
             └───────────┬────────────────────┘
                         ↓
       ┌─────────────────────────────────────────┐
       │ Dynamic Agent Network / Graph           │
       │                                         │
       │ Agent A ↔ Agent B → Agent C             │
       │       ↘ Agent D ↗                       │
       └─────────────────┬───────────────────────┘
                         ↓
                 ┌───────────────┐
                 │ Tool Registry │
                 └───────┬───────┘
                         ↓
                 ┌───────────────┐
                 │ Execution     │
                 └───────┬───────┘
                         ↓
             ┌─────────────────────────┐
             │ Workflow Verification   │
             └────────────┬────────────┘
                          ↓
                 ┌─────────────────┐
                 │ Human Feedback  │
                 │ +               │
                 │ Machine Metrics │
                 └────────┬────────┘
                          ↓
                 ┌─────────────────┐
                 │ Refinement      │
                 │ Engine          │
                 └────────┬────────┘
                          ↓
                 ┌─────────────────┐
                 │ Historian Log   │
                 └────────┬────────┘
                          │
                          └──────→ future workflow reuse
```

---

# 28. Reproduction Procedure

## Phase 1 — Build the execution substrate

Implement:

```text
LLM Gateway
Agent Runtime
Tool Runtime
Graph Runtime
Database
Vector Retrieval
Execution Logger
```

Do not implement self-refinement first.

The system must first be able to execute a fixed workflow reliably.

---

## Phase 2 — Implement the Input Layer

Implement:

```text
Scene Recognition
Intent Understanding
Task Decomposition
Multimodal Fusion
Data Observer
```

Validate whether the system can convert an operational request into a valid task graph.

---

## Phase 3 — Implement the Computational Layer

Implement:

```text
Supervisor
Agent Registry
Model Registry
Tool Registry
Dynamic Topology Engine
Agent Execution
```

At this stage, the system SHALL be able to generate different workflows for different intents.

---

## Phase 4 — Implement Historical Case Reuse

Create the Historian Log.

Populate it using historical operational cases.

For each case store:

```text
input
intent
tasks
agents
tools
workflow
execution
result
evaluation
feedback
```

Implement semantic retrieval.

---

## Phase 5 — Implement In-context Refinery

When a similar case is detected:

```text
retrieve historical cases
        ↓
compare current/historical context
        ↓
reuse successful workflow
        ↓
adapt workflow
        ↓
execute
```

No foundation-model weight update is required.

---

## Phase 6 — Implement Workflow Verification

Implement:

```text
Workflow Completeness Checker
Dependency Checker
Tool Compatibility Checker
Data Sufficiency Checker
Constraint Checker
Safety Checker
```

Generate:

```text
r = workflow rationality score
```

---

## Phase 7 — Implement Dual Feedback

Add:

```text
Human Feedback
Machine Feedback
```

Implement:

```text
f = αfh + (1−α)fm
```

Then connect feedback to the workflow graph.

---

## Phase 8 — Implement Dynamic Refinement

Enable:

```text
node addition
node deletion
node replacement
agent replacement
tool replacement
topology modification
task reordering
prompt refinement
```

The system now becomes genuinely **agentic-neural-like**, rather than merely a conventional multi-agent workflow.

---

## Phase 9 — Implement Self-Refinery

Run repeated occurrences of the same or similar scenario.

Measure:

```text
T1 = first occurrence reasoning time
T2 = subsequent occurrence reasoning time

A1 = first occurrence accuracy
A2 = subsequent occurrence accuracy
```

The reproduced system SHOULD demonstrate:

```text
T2 < T1
```

and ideally:

```text
A2 ≥ A1
```

while maintaining operational safety.

---

# 29. Reproduction Test Scenarios

The paper uses two cross-value-chain scenarios:

### Upstream

60 shale-oil wellbores involving:

- pump leakage;
- rod breakage;
- scaling;
- wax deposition;
- routine maintenance.

### Downstream

Three transnational natural-gas pipelines covering more than 6,500 km, involving:

- equipment failures;
- human factors;
- climate and meteorological conditions;
- gas supply/demand fluctuations;
- commodity-price volatility;
- end-user demand changes.

A reproduction SHOULD begin with one scenario and subsequently demonstrate cross-scenario generalisation.

---

# 30. Evaluation Protocol

The reproduction SHALL evaluate at least:

```text
1. Event sensing
2. Task planning
3. Agent liaison
4. Tool use
5. Agile workflow formation
6. Continuous self-refinery
```

These are the principal evaluation dimensions used in the source paper.

Each dimension SHOULD be scored from:

```text
0 → 1
```

and evaluated by:

```text
Human evaluator
+
Agent evaluator
```

The reproduction SHOULD report:

```text
Average accuracy
Standard deviation
```

---

# 31. Cross-scenario Generalisation Test

Randomly select cases from at least two substantially different scenarios.

Example:

```text
Wellbore maintenance
        +
Pipeline scheduling
        ↓
Mixed evaluation set
        ↓
AAN workflow generation
```

Measure:

- scene recognition;
- task planning;
- agent coordination;
- tool selection;
- workflow agility;
- self-refinement.

The purpose is to determine whether the workflow-forming mechanism generalises rather than merely memorising one application.

The source paper reports a mixed-scenario evaluation using 20 events with three temporal occurrences per event.

---

# 32. Ablation Test

The reproduction SHALL implement four feedback configurations:

### Configuration A — Global Dual Feedback

```text
Human + Agent
       ↓
Input + Computational layers
```

### Configuration B — Global Feedback Without Human

```text
Agent only
       ↓
Input + Computational layers
```

### Configuration C — Dual Feedback to Input Only

```text
Human + Agent
       ↓
Input layer only
```

### Configuration D — Agentic Feedback to Input

```text
Agent only
       ↓
Input layer only
```

Compare:

```text
accuracy
standard deviation
workflow generation
continuous refinement
```

The original study reports that the global dual-feedback configuration produced the strongest overall workflow-generation performance and robustness.

---

# 33. Success Criteria

A successful reproduction SHALL demonstrate all of the following:

### S1 — Dynamic workflow formation

The system generates workflows rather than simply following predefined chains.

### S2 — Dynamic topology

Different scenarios can produce different agent-network structures.

### S3 — Historical reuse

Previous successful cases can influence future workflows.

### S4 — No mandatory fine-tuning

The system can improve through context, memory, workflow and feedback mechanisms without updating foundation-model weights.

### S5 — Human-machine dual feedback

Human and machine feedback both influence workflow refinement.

### S6 — Closed-loop refinement

A failed or sub-optimal workflow can be regenerated.

### S7 — Repeated-event improvement

Repeated scenarios demonstrate faster and/or more accurate workflow formation.

### S8 — Cross-scenario generalisation

The same framework can operate across materially different upstream and downstream tasks.

---

# 34. Safety and Industrial Deployment Rules

The reproduced Skill SHALL NOT equate a high LLM confidence score with operational safety.

For industrial deployment:

```text
AI recommendation
       ↓
Validation
       ↓
Safety constraints
       ↓
Human approval where required
       ↓
Execution
```

High-risk actions SHOULD require human approval.

The system SHALL support:

```text
read-only mode
recommendation mode
simulation mode
human-approved execution
```

before autonomous execution is enabled.

---

# 35. Canonical End-to-End Skill

The complete Skill SHALL execute the following sequence:

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
r ≥ γ ?
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
          repeat
```

---

# 36. Pseudocode

```python
def aan_agile_workflow(intent, multimodal_data):

    scene = recognise_scene(multimodal_data)

    z_intent = understand_intent(
        intent,
        multimodal_data,
        scene
    )

    z_intent = self_consistency_validate(z_intent)

    tasks = decompose_task(z_intent)

    fused_context = multimodal_fusion(
        multimodal_data
    )

    historical_cases = retrieve_cases(
        scene=scene,
        intent=z_intent,
        tasks=tasks
    )

    workflow = generate_candidate_workflow(
        tasks=tasks,
        historical_cases=historical_cases
    )

    topology = select_topology(
        workflow,
        scene,
        historical_cases
    )

    agents = select_agents(
        tasks,
        topology
    )

    models = select_models(
        tasks,
        agents
    )

    tools = select_tools(
        tasks,
        agents
    )

    workflow = assemble_workflow(
        workflow,
        topology,
        agents,
        models,
        tools
    )

    score = verify_workflow(workflow)

    while score < RATIONALITY_THRESHOLD:

        human_feedback = collect_human_feedback(
            workflow
        )

        machine_feedback = collect_machine_feedback(
            workflow
        )

        feedback = combine_feedback(
            human_feedback,
            machine_feedback,
            alpha=HUMAN_FEEDBACK_WEIGHT
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
        intent=z_intent,
        scene=scene,
        tasks=tasks,
        workflow=workflow,
        result=result,
        evaluation=evaluation,
        feedback=feedback
    )

    return {
        "workflow": workflow,
        "result": result,
        "evaluation": evaluation
    }
```

---

# 37. Engineering Interpretation

The reproduction SHALL preserve the distinction between three levels:

```text
Foundation Model
       ↓
Agent
       ↓
Agentic Neural Workflow
```

The foundation model provides general reasoning capability.

The agent provides task-specific capability and tool access.

The AAN provides the **dynamic organisational structure through which multiple agents cooperate, execute and continuously refine complex workflows**.

Therefore:

> **AAN is not simply a multi-agent system. Its essential contribution is the dynamic formation and refinement of the agentic workflow itself.**

---

# 38. Final Definition of Done

The reproduction can be considered complete only when the system can perform the following demonstration:

```text
Give the system a previously unseen complex operational intent.

        ↓

The system understands the scenario.

        ↓

It decomposes the intent.

        ↓

It retrieves relevant historical experience.

        ↓

It selects agents, models and tools.

        ↓

It dynamically constructs an agent network.

        ↓

It generates an executable workflow.

        ↓

It verifies the workflow.

        ↓

It executes the workflow.

        ↓

Human + machine feedback is collected.

        ↓

The workflow is automatically modified if necessary.

        ↓

The successful execution is stored.

        ↓

A similar event occurs later.

        ↓

The system retrieves the previous case.

        ↓

It adapts rather than blindly copies the old workflow.

        ↓

The new workflow is executed and refined.

        ↓

The system demonstrates measurable improvement.
```

If this complete loop can be demonstrated experimentally, the implementation can reasonably be described as a **reproduction of the AAN agile workflow formation and self-refinery mechanism**, rather than merely an LLM-based multi-agent application.