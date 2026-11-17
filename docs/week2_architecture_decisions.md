# V1 Architecture Decisions & Requirements Traceability — UTM FAI FYP Matchmaker

---

## 1. Requirements-Driven Architecture Needs

| Requirement ID | What does the requirement need? | Architecture Response |
| :---: | :--- | :--- |
| **FR1 / FR2 / RAI4** | Supervisor recommendations and FYP guideline answers must be supported by verified university information and source citations. | **Verified UTM FAI Knowledge Sources** + **Knowledge Retrieval** |
| **FR3 / FR5** | The system must provide understandable supervisor matching explanations and evidence-based project scope refinement suggestions. | **Language Model Service** + **Response / Output** |
| **FR4 / RAI2** | The system must identify out-of-scope questions and refuse or redirect them appropriately. | **Privacy and Scope / Safety Control** |
| **RAI3** | The system must prevent student personal information from being transmitted to external model services. | **Privacy and Scope / Safety Control** |
| **NFR1 / NFR2 / RAI4** | The system must be evaluated for response latency, factual correctness, and citation grounding. | **Evaluation Point** |

---

## 2. Architecture Decision Records (ADR)

### ADR 1: Document-Grounded Knowledge Retrieval

**Requirement / Constraint:** FR1, FR2, and RAI4. Supervisor recommendations and FYP guideline answers must be supported by verified university sources and citations.

**Alternatives Considered:**

1. **Base LLM Only:** Generate answers using the language model's existing knowledge without retrieving university documents.
2. **Static Keyword Search:** Search university documents using exact keywords.
3. **Semantic Knowledge Retrieval:** Retrieve relevant information from verified lecturer profiles and official FYP documents based on the meaning of student queries.

**Selected Option:** Semantic Knowledge Retrieval combined with Retrieval-Augmented Generation (RAG).

**Trade-off Accepted:** This approach requires additional processing and may increase response time. However, it helps the system provide more relevant information and verifiable source citations while reducing the risk of unsupported answers.

### ADR 2: Explicit Privacy and Scope / Safety Control

**Requirement / Constraint:** FR4, RAI2, and RAI3. The system must refuse out-of-scope questions and protect student personal information before sending requests to external model services.

**Alternatives Considered:**

1. **Prompt-Only Control:** Rely on instructions given to the language model to handle privacy and out-of-scope questions.
2. **Explicit Privacy and Scope / Safety Control:** Use a separate control component to check requests for personal information and whether they are within the defined FYP scope.

**Selected Option:** Explicit Privacy and Scope / Safety Control.

**Trade-off Accepted:** This adds some processing time and may occasionally reject valid queries. However, it provides a more testable way to protect privacy and enforce the system's academic scope.

---

## 3. Initial Requirements Traceability Matrix

| Objective | Requirement ID | Evaluation Criterion | Architecture Element | Later Evidence |
| :---: | :---: | :--- | :--- | :--- |
| **O1** | **FR1 / FR2 / RAI4** | Source Citation & Grounding Match | Verified UTM FAI Knowledge Sources + Knowledge Retrieval | Citation presence and citation-source match test results |
| **O2** | **FR3 / FR5** | Scope Refinement & Explanation Clarity | Language Model Service + Response / Output | Average explanation clarity and scope refinement rubric scores |
| **O3** | **NFR1 / NFR2** | Supervisor Recommendation & FYP Answer Correctness; Response Latency | Language Model Service + Evaluation Point | Correctness scores and p90 response latency measurements |
| **O3** | **FR4 / RAI2** | Out-of-Scope & Refusal Handling | Privacy and Scope / Safety Control | Refusal-and-redirect success rate from 20 out-of-scope test queries |
| **O3** | **RAI3** | Privacy Protection | Privacy and Scope / Safety Control | Number of detected unmasked PII transmissions during controlled privacy tests |

---

## Part C Reflection

**Question:** *Which architecture decision involved the most important trade-off? Which requirement drove the decision, and what trade-off did you accept?*

**Reflection:** **ADR 1 (Document-Grounded Knowledge Retrieval)** involved the most important trade-off because the system needs reliable university information. This decision was driven by **FR1, FR2, and RAI4**. We accepted additional processing time and system complexity to provide more accurate information and verifiable source citations.