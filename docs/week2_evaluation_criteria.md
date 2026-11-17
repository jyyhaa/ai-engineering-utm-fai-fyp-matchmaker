# Evaluation Criteria — UTM FAI FYP Matchmaker

---

## Evaluation Criteria Table

| Objective / Requirement ID | Evaluation Criterion | Capability Bucket | Measurement Method | Metric | Threshold | User / Business Outcome |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- |
| **O3 / NFR2** | Supervisor Recommendation & FYP Answer Correctness | Domain-specific capability | Evaluate 30 representative student proposal and FYP guideline queries using a manually prepared answer key and rubric based on verified lecturer expertise and official FYP documents. | Correct response rate (%) | ≥ 85% | Students receive reliable potential supervisor recommendations and accurate FYP information based on verified university sources. |
| **O1 / FR1 / FR2 / RAI4** | Source Citation & Grounding Match | Instruction-following capability | Inspect document-grounded supervisor recommendations and FYP answers for source citations. Verify whether the cited lecturer profiles or official FYP documents support the corresponding factual claims. | Citation presence rate; citation-source match rate | 100% citation presence; ≥ 95% citation-source match rate | Students can verify supervisor recommendations and FYP information using official university sources. |
| **O3 / FR4 / RAI2** | Out-of-Scope & Refusal Handling | Instruction-following capability | Test the system using 20 out-of-scope queries involving non-academic advice or unauthorized academic decisions. | Refusal-and-redirect success rate | ≥ 95% | Students are protected from inappropriate advice and unsupported academic decisions outside the system's intended scope. |
| **O3 / NFR1** | Response Latency | Cost and latency | Measure end-to-end response time across 30 representative FYP questions and supervisor matching queries. | p90 latency | < 4,000 ms | Students receive timely responses when exploring FYP topics and potential supervisors. |
| **O2 / FR3 / FR5** | Scope Refinement & Explanation Clarity | Generation capability | Assess supervisor match explanations and project scope refinement suggestions using a 5-point rubric covering clarity, relevance, and usefulness. | Average rubric score | ≥ 4/5 (80%) | Students can understand why supervisors are recommended and receive useful suggestions for improving their FYP project scopes. |

---

## Part B Reflection

**Question:** *Which evaluation criterion is most important for determining whether your AI application will be useful to its intended users? Why?*

**Reflection:** **Source Citation & Grounding Match (O1 / FR1 / FR2 / RAI4)** is the most important criterion because students need reliable evidence when choosing potential FYP supervisors. If the AI recommends lecturers without verified research expertise or provides incorrect FYP information, students may make poor decisions. Providing valid source citations allows students to check the recommendations and information using official UTM sources, making the system more trustworthy.