# Requirements Specification — UTM FAI FYP Matchmaker
Version: 1.0

---

## Section 0: Project Framing & Application Scope Checkpoint

| Framing Element | Details |
| :--- | :--- |
| **Problem Evidence** | **1.** Academic staff research profiles and publication histories on UTM Scholar (`https://utmscholar.utm.my/faculties/fai`) are separate from official FYP guidelines on the FAI FYP portal (`https://sites.google.com/utm.my/fai-fyp/`).<br>**2.** Students frequently propose topics with high overlap or outside available supervisor expertise due to a lack of centralized semantic search tools. |
| **Existing Approaches** | **1. Static Faculty Directories:** Limited to exact keyword matching; cannot evaluate semantic overlap between student proposals and publications.<br>**2. General-Purpose AI Assistants:** Prone to hallucinating non-existent professors, research labs, or course policies.<br>**3. Traditional PDF Handbooks:** Manual, time-consuming search without automated decision support for proposal refining. |
| **Project / Engineering Gap** | Existing approaches either rely on rigid keyword search or ungrounded general LLMs, creating a practical need for a document-grounded, explainable recommendation system with verified citations. |
| **Objective O1** | To design a document-grounded retrieval system using official UTM FAI faculty profiles and FYP portal guidelines. |
| **Objective O2** | To develop an explainable recommendation pipeline that matches student project proposals with supervisor profiles, related past projects, and scope refinement suggestions. |
| **Objective O3** | To evaluate system performance across retrieval relevance, recommendation faithfulness, groundedness, latency, and safety controls. |
| **Intended Users** | 3rd and 4th-year Undergraduate Artificial Intelligence students at UTM preparing FYP proposals and seeking supervisor assignment. |
| **Supported Tasks** | Analyzing student project titles/abstracts, generating ranked supervisor recommendations with publication citations, retrieving official FYP rules, flagging topic overlaps, and suggesting scope refinements. |
| **Information / Knowledge Domain** | UTM FAI academic staff directories, research publications, Google Scholar profiles, official FYP portal guidelines, evaluation rubrics, and past project proceedings. |
| **Operating Context** | Web-based conversational and recommendation tool used by undergraduate students during initial FYP topic formulation and registration periods. |
| **Important Constraints** | Must operate over public data sources, comply with student data privacy (zero PII leakage), keep API response latency under 4 seconds, and run within standard API budget limits. |
| **Explicit Out-of-Scope Tasks** | The system **SHALL NOT** automatically assign supervisors to students, grade/assess student proposals, override committee decisions, or offer non-academic counseling. |
| **Evaluation Focus** | Determine whether the retrieval-augmented recommendation system provides accurate, grounded supervisor matches and rule answers without hallucination or bias. |

---

## Section 1: Functional Requirements (FR)

| # | Requirement — The system SHALL | Scope Boundary — SHALL NOT (where relevant) |
| :---: | :--- | :--- |
| **FR1** | Match student proposal text against FAI staff publication profiles and return a ranked list of relevant supervisor recommendations with citation evidence. | Invent non-existent faculty members or attribute research expertise not supported by source documents. |
| **FR2** | Retrieve and answer student queries regarding official UTM FAI FYP submission rules, evaluation rubrics, and committee timelines based on official portal documents. | Grant official policy exemptions, approve project proposals, or override committee decisions. |
| **FR3** | Analyze student proposal abstracts against past FYP proceedings to identify potential topic overlaps and suggest scope refinements. | Automatically reject or disqualify student proposals without human supervisor review. |
| **FR4** | Decline queries that fall outside the defined academic and FYP scope and provide an appropriate redirect. | Provide medical, legal, financial, or personal mental health advice. |
| **FR5** | Present recommendations, match explanations, and source citations in clear language appropriate for undergraduate AI students. | Present unverified or ungrounded assumptions as official university policy. |

---

## Section 2: Non-Functional Requirements (NFR)

| # | Quality Attribute | Requirement | Measurement Method | Metric | Threshold |
| :---: | :--- | :--- | :--- | :--- | :--- |
| **NFR1** | Performance | The system shall maintain interactive response speeds for proposal matching queries. | Measure end-to-end latency over a test set of 30 standard queries. | p90 latency | < 4,000 ms |
| **NFR2** | Correctness | Supervisor recommendations and rule answers shall be factually consistent with source documents. | Score a curated test evaluation set against ground-truth faculty profiles using a rubric. | Correct response rate (%) | $\ge 85\%$ |
| **NFR3** | Cost | Inference and embedding operational costs shall remain within project budget boundaries. | Track token and API usage logs across evaluation test runs. | Cost per 100 queries | < US\$0.50 per 100 queries |
| **NFR4** | Maintainability | Updated staff publication profiles or FYP portal rules shall be integrated without rebuilding core logic. | Run a controlled document-ingestion and vector update test. | Database update time | < 15 minutes |

---

## Section 3: Responsible AI Requirements (RAI)

| # | Responsible AI Risk | Requirement | Measurement Method | Metric | Threshold |
| :---: | :--- | :--- | :--- | :--- | :--- |
| **RAI1** | Fairness & Bias | System recommendation accuracy shall not unfairly penalize niche or newer research sub-fields. | Compare recommendation accuracy across distinct AI domain categories (e.g., Computer Vision vs. NLP). | Accuracy gap between domains | $\le 10$ percentage points |
| **RAI2** | Safety / Scope | The system shall refuse out-of-scope queries (e.g., non-academic advice) and redirect users. | Test system using a safety benchmark set of 20 out-of-scope test queries. | Refusal-and-redirect success rate | $\ge 95\%$ |
| **RAI3** | Privacy | The system shall not transmit student personally identifiable information (PII) to external APIs. | Inspect outgoing API payload logs during student input processing. | Number of detected PII transmissions | 0 |
| **RAI4** | Transparency & Grounding | The system shall provide verifiable source citations for all supervisor matches and policy answers. | Inspect recommendations for citation presence and verify accuracy against source profile URLs. | Citation presence rate; citation-source match rate | 100% citation presence; $\ge 95\%$ match rate |

---

## Part A Reflection

**Question:** *Which Responsible AI requirement in your project will be the most difficult to measure or enforce technically? Why?*

**Reflection:** **RAI1 (Fairness & Bias)** will be the most difficult to enforce technically. Academic research profiles naturally vary: senior professors have extensive publication histories across multiple domains on UTM Scholar, whereas newer faculty members or specialized researchers may have fewer indexed papers in the vector store. Preventing the vector retrieval model from disproportionately favoring senior, heavily published staff over equally qualified junior supervisors requires careful chunking, embedding normalization, and balanced retrieval strategies.