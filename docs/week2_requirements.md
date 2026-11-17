# Requirements Specification — UTM FAI FYP Matchmaker

---

## Section 0: Project Framing & Application Scope Checkpoint

| Framing Element | Details |
| :--- | :--- |
| **Problem Evidence** | **1. Distributed university information:** Lecturer research expertise is available through [UTM Scholar](https://utmscholar.utm.my/faculties/fai) and [FAI departmental webpages](https://fai.utm.my/department-of-intelligence-informatics-2/), while FYP procedures and guidelines are provided through the separate [FAI FYP Portal](https://sites.google.com/utm.my/fai-fyp/).<br><br>**2. Manual expertise comparison:** Official lecturer profiles describe research interests using specialized terminology. Students may need to compare their proposed project ideas with multiple lecturer profiles to identify potentially relevant supervisors. |
| **Existing Approaches** | **1. Official Faculty Directories:** Provide lecturer information and research expertise but do not directly generate semantic supervisor recommendations from student project descriptions.<br><br>**2. General-Purpose AI Assistants:** Support conversational question answering but may generate unsupported or outdated university-specific information without verified institutional sources.<br><br>**3. Official FYP Portals and Handbooks:** Provide academic procedures and requirements but require students to locate and interpret relevant information manually. |
| **Project / Engineering Gap** | Existing university information sources provide lecturer expertise and FYP requirements but do not offer an integrated, document-grounded conversational system that semantically matches student project proposals with potential supervisors and explains recommendations using verifiable evidence. |
| **Objective O1** | To design and implement a document-grounded retrieval pipeline using verified UTM FAI lecturer research expertise and official FYP guidelines. |
| **Objective O2** | To develop an explainable RAG-based conversational assistant that recommends potential FYP supervisors based on student project descriptions and provides evidence-based project scope refinement suggestions. |
| **Objective O3** | To evaluate the system in terms of retrieval relevance, supervisor recommendation correctness, citation grounding, response latency, and responsible AI safeguards. |
| **Intended Users** | Third- and fourth-year Bachelor in Artificial Intelligence with Honours students at UTM FAI who are preparing FYP proposals, exploring potential supervisors, or seeking guidance on official FYP requirements. |
| **Supported Tasks** | **1.** Accept student FYP titles, abstracts, and FYP-related questions.<br>**2.** Retrieve relevant information from verified lecturer profiles and official FYP documents.<br>**3.** Generate ranked potential supervisor recommendations with explanations and source citations.<br>**4.** Answer questions about documented FYP procedures, submission requirements, and evaluation guidelines.<br>**5.** Suggest possible project scope improvements based on the student's description and available evidence. |
| **Information / Knowledge Domain** | Verified UTM FAI lecturer research expertise, accessible research publication information, official FYP handbook documents, submission procedures, and evaluation guidelines. The initial knowledge base will use a manageable subset of verified lecturer profiles and available official documents. |
| **Operating Context** | A web-based conversational assistant intended for UTM FAI undergraduate students during FYP topic exploration and proposal preparation. The initial prototype will use a curated knowledge base and may be developed using Python, semantic retrieval, and an LLM-based response generation component. |
| **Important Constraints** | **1. Data Availability:** The system must use accessible and verified university information. Complete lecturer coverage and historical FYP records are not assumed.<br>**2. Privacy:** Student personal information must not be transmitted to external model services without appropriate protection.<br>**3. Performance:** The prototype targets p90 response latency below 4 seconds for standard evaluation queries.<br>**4. Cost:** The prototype should operate within a limited student project budget.<br>**5. Data Maintenance:** Lecturer and FYP information may change; the knowledge base must support updates without modifying core application logic.<br>**6. Source Limitations:** The system must indicate when available documents do not provide sufficient evidence. |
| **Explicit Out-of-Scope Tasks** | The system **SHALL NOT** automatically assign supervisors, guarantee lecturer availability, approve or reject FYP proposals, officially grade student work, override FYP committee decisions, or provide medical, legal, financial, or mental health counselling. Historical FYP duplication detection is excluded from the initial prototype until a suitable verified dataset is available. |
| **Evaluation Focus** | Assess whether the system retrieves relevant evidence, produces correct and explainable supervisor recommendations, answers documented FYP questions accurately, provides valid citations, meets reasonable response-time targets, and handles unsupported or out-of-scope queries appropriately. |

---

## Section 1: Functional Requirements (FR)

| # | Requirement — The System SHALL | Scope Boundary — SHALL NOT (where relevant) |
| :---: | :--- | :--- |
| **FR1** | Accept student FYP titles or abstracts, compare their semantic meaning with verified UTM FAI lecturer research expertise, and return a ranked list of potential supervisors with evidence-based match explanations and source references. | The system **SHALL NOT** invent lecturers, attribute unsupported expertise or publications, or present recommendations as confirmed supervisor assignments. |
| **FR2** | Retrieve relevant information from official UTM FAI FYP documents and answer student questions about documented procedures, submission requirements, and evaluation guidelines, including source references. | The system **SHALL NOT** invent university policies, grant exemptions, or present outdated or unsupported information as current official requirements. |
| **FR3** | Analyze student project descriptions and provide possible scope refinement suggestions based on the proposed topic, relevant lecturer expertise, and available official academic information. | The system **SHALL NOT** claim that suggested improvements are officially approved or that a proposed topic is unique, feasible, or acceptable without appropriate academic review. |
| **FR4** | Identify queries outside the defined FYP academic guidance scope and provide a clear refusal or appropriate redirection. | The system **SHALL NOT** provide medical, legal, financial, or personal mental health counselling or make official academic decisions. |
| **FR5** | Present supervisor recommendations, FYP guidance, and project scope suggestions in clear language appropriate for undergraduate AI students. The system shall provide verifiable source citations for factual university information and indicate when supporting evidence is insufficient. | The system **SHALL NOT** present generated suggestions as verified university facts or fabricate source citations. |

---

## Section 2: Non-Functional Requirements (NFR)

| # | Quality Attribute | Requirement | Measurement Method | Metric | Threshold |
| :---: | :--- | :--- | :--- | :--- | :--- |
| **NFR1** | Performance | The system shall provide interactive response times for standard FYP questions and supervisor recommendation requests. | Measure end-to-end response time across 30 representative queries, including retrieval and response generation. | 90th percentile (p90) latency | < 4,000 ms |
| **NFR2** | Correctness | The system shall produce supervisor recommendations and factual FYP answers that are consistent with verified source information. | Evaluate 30 representative queries using a manually prepared answer key and assessment rubric based on verified lecturer profiles and official FYP documents. | Percentage of responses meeting the correctness rubric | ≥ 85% |
| **NFR3** | Cost | The system shall operate within a defined inference and retrieval budget for the student prototype. | Record billable model, embedding, and retrieval-service usage during evaluation and calculate the estimated cost per 100 queries. | Operational API cost per 100 queries | < US$0.50 |
| **NFR4** | Maintainability | The system shall support updating lecturer profiles and FYP documents without changing the core chatbot application logic. | Add or replace a representative source document, run the ingestion and indexing pipeline, and verify that updated information becomes retrievable. | Time from starting the update to successful retrieval verification | < 15 minutes |

**Note:** These thresholds are initial engineering targets. Their feasibility will be assessed during implementation and evaluation. The cost metric excludes development time and fixed infrastructure costs.

---

## Section 3: Responsible AI Requirements (RAI)

| # | Responsible AI Risk | Requirement | Measurement Method | Metric | Threshold |
| :---: | :--- | :--- | :--- | :--- | :--- |
| **RAI1** | Fairness and Representation Bias | The system shall provide relevant supervisor recommendations without systematically favoring lecturers solely because they have longer publication histories or more detailed online profiles. | Construct a controlled evaluation set containing relevant lecturer profiles with different amounts of available research information. Compare recommendation correctness across profile-coverage groups using manually verified expected matches. | Absolute recommendation correctness-rate gap between profile-coverage groups | ≤ 10 percentage points |
| **RAI2** | Safety / Scope Control | The system shall decline requests outside the defined FYP academic guidance scope and provide an appropriate redirection when necessary. | Test the system using 20 out-of-scope queries covering medical, legal, financial, personal counselling, and unauthorized academic decisions. | Refusal-and-redirect success rate | ≥ 95% |
| **RAI3** | Privacy | The system shall detect and remove or mask student personally identifiable information before transmitting query content to external model APIs. | Run controlled tests containing synthetic student names, identification numbers, email addresses, and other identifiable details. Inspect outgoing API request payloads before transmission. | Number of detected unmasked PII transmissions | 0 |
| **RAI4** | Transparency and Grounding | The system shall provide verifiable source references for document-grounded supervisor recommendations and FYP policy answers and clearly indicate when sufficient supporting evidence is unavailable. | Evaluate source-grounded responses for citation presence and manually verify whether the cited source supports the corresponding factual claim. | Citation presence rate; citation-source match rate | 100% citation presence; ≥ 95% citation-source match rate |

---

## Part A Reflection

**Question:** *Which Responsible AI requirement in your project will be the most difficult to measure or enforce technically? Why?*

**Reflection:** **RAI1 (Fairness and Representation Bias)** will be the most difficult to measure because lecturers have different amounts of research information available online. Some lecturers have many publications, while others have fewer. This may cause the AI to recommend lecturers with more publications, even when other lecturers have equally relevant expertise. To reduce this bias, the system will use consistent lecturer profiles and compare recommendation accuracy across lecturers with different levels of research information.
