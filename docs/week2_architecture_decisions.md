# V1 Architecture Decisions & Requirements Traceability — UTM FAI FYP Matchmaker
Version: 1.0

---

## 1. Requirements-Driven Architecture Needs

| Requirement ID | What does the requirement need? | Architecture Response |
| :---: | :--- | :--- |
| **FR1 / RAI4** | Recommendations must be grounded in verified staff research expertise with source links. | **Knowledge Source** (UTM Scholar & FYP Portal Data) + **Retrieval Component** |
| **RAI2** | Out-of-scope queries (e.g., non-academic or counseling advice) must be blocked and redirected. | **Scope / Safety Control** (Input/Output Guardrail) |
| **FR3 / FR5** | System must analyze proposal abstracts, identify topic overlap, and generate match rationale. | **Language Model Service** (Reasoning & Synthesis Engine) |
| **NFR1 / NFR2 / RAI4** | Responses must meet latency (< 4s), accuracy ($\ge 85\%$), and 100% citation presence thresholds. | **Evaluation Point** (Telemetry & Response Inspection) |

---

## 2. Architecture Decision Records (ADR)

### ADR 1: Dynamic Knowledge Access via Vector Retrieval
* **Requirement / Constraint:** FR1, RAI4 (Recommendations must be grounded in official UTM Scholar profiles and FYP portal guidelines with 100% verifiable citations).
* **Alternatives Considered:** 
  1. *Base LLM only* (Rely solely on standard model internal knowledge).
  2. *Static Keyword Search* (Exact matching against faculty names).
  3. *Dynamic Knowledge Access* (Retrieval-Augmented Generation over structured vector store).
* **Selected Option:** Dynamic Knowledge Access via Vector Retrieval.
* **Trade-off Accepted:** Increases architectural complexity and introduces slight retrieval latency, but ensures high factual groundedness, eliminates staff profile hallucinations, and enables source citations.

### ADR 2: Explicit Scope & Safety Control Component
* **Requirement / Constraint:** RAI2 (The system must refuse non-academic or out-of-scope queries with $\ge 95\%$ success).
* **Alternatives Considered:** 
  1. *Prompt-only instruction* (Instructing the main LLM via system prompt to decline out-of-scope queries).
  2. *Explicit Scope / Safety Control Component* (Dedicated pre-processing filter component).
* **Selected Option:** Explicit Scope / Safety Control Component.
* **Trade-off Accepted:** Slightly increases request processing overhead and risks occasional false-positive refusals, but provides predictable, testable safety guardrails preventing misuse.

---

## 3. Initial Requirements Traceability Matrix

| Objective | Requirement ID | Evaluation Criterion | Architecture Element | Later Evidence |
| :---: | :---: | :--- | :--- | :--- |
| **O1** | **FR1 / RAI4** | Source Citation & Grounding Match | Knowledge Source + Retrieval Component | Automated citation verification test across 30 sample proposals. |
| **O2** | **FR3 / RAI2** | Out-of-Scope & Refusal Handling | Scope / Safety Control | Refusal success rate logged on 20 out-of-scope benchmark queries. |
| **O3** | **NFR1 / NFR2** | Match Correctness & Response Latency | Language Model Service + Evaluation Point | Latency p90 logs and correctness rubric evaluation results. |

---

## Part C Reflection

**Question:** *Which architecture decision involved the most important trade-off? Which requirement drove the decision, and what trade-off did you accept?*

**Reflection:** The decision to incorporate **Dynamic Knowledge Access via Vector Retrieval (ADR 1)** involved the most important trade-off. This decision was driven by **RAI4 (Transparency & Grounding)** and **FR1 (Supervisor Matching Accuracy)**. We accepted higher system design complexity and additional retrieval latency in exchange for eliminating hallucinated faculty expertise and providing verifiable UTM Scholar citations for every recommendation.