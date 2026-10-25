# Evaluation Criteria — UTM FAI FYP Matchmaker
Version: 1.0

---

## Evaluation Criteria Table

| Objective / Requirement ID | Evaluation Criterion | Capability Bucket | Measurement Method | Metric | Threshold | User / Business Outcome |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- |
| **O3 / NFR2** | Supervisor Match & Policy Correctness | Domain-specific capability | Score a curated set of 30 test queries (student proposal titles/abstracts and FYP rule queries) using a standard rubric. | Correct response rate (%) | $\ge 85\%$ | Students receive reliable supervisor recommendations grounded in actual faculty research profiles and official FYP guidelines. |
| **O1 / FR1 / RAI4** | Source Citation & Grounding Match | Instruction-following capability | Inspect recommendations for citation presence and verify whether cited UTM Scholar profiles support the matched expertise. | Citation presence rate; citation-source match rate | 100% presence; $\ge 95\%$ match rate | Students can directly verify why a professor was recommended by checking original publication source links. |
| **O2 / RAI2** | Out-of-Scope & Refusal Handling | Instruction-following capability | Test the system with a defined safety/scope set containing 20 non-academic or out-of-scope test queries. | Refusal-and-redirect success rate | $\ge 95\%$ | Prevents the system from giving unauthorized advice or making ungrounded academic decisions outside FYP scope. |
| **O3 / NFR1** | Match Generation Latency | Cost and latency | Measure end-to-end response processing time across the defined evaluation workload test set. | p90 latency | < 4,000 ms | Students receive timely, interactive matching results without prolonged system delays. |
| **O2 / FR5** | Scope Refinement & Explanation Clarity | Generation capability | Assess the clarity and readability of supervisor match explanations and scope suggestions using a 5-point rubric. | Clarity rubric score | $\ge 80\%$ (average score $\ge 4/5$) | Students clearly understand the research alignment rationale and actionable advice for refining their FYP scope. |

---

## Part B Reflection

**Question:** *Which evaluation criterion is most important for determining whether your AI application will be useful to its intended users? Why?*

**Reflection:** **Source Citation & Grounding Match (O1 / FR1 / RAI4)** is the most important evaluation criterion for the UTM FAI FYP Matchmaker. Undergraduate students relying on AI to select a Final Year Project supervisor need absolute certainty that recommended staff members actually possess published expertise in that research area. If the system produces ungrounded matches or hallucinated citations, students risk proposing topics to unsuitable supervisors, leading to rejected proposals or administrative delays. Verifiable source links build user trust and enable self-verification on the official UTM Scholar portal.