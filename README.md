# UTM FAI FYP Matchmaker

> An explainable, document-grounded AI assistant that matches Artificial Intelligence student project proposals to relevant FAI research supervisors, identifies related past FYPs, and provides evidence-backed project scope refinements.

---

## Problem Statement
Undergraduate AI students at UTM struggle to align their Final Year Project (FYP) ideas with suitable research supervisors and existing academic work. Information regarding staff research domains, publications, and FYP guidelines is scattered across separate portals and handbooks. **UTM FAI FYP Matchmaker** uses AI engineering principles to perform semantic matching and RAG-based document retrieval, returning grounded supervisor recommendations backed by published research evidence.

---

## Evidence of the Problem
- Official faculty research profiles, lab domains, and publication histories are distributed across separate directory pages from the main FYP portal.
- Students often propose project topics that overlap with past completed FYPs or fall outside available faculty supervisor expertise due to a lack of centralized semantic search tools.

---

## Initial Related Work / Existing Approaches
- **Static Faculty Directories:** Authoritative but limited to exact keyword searches; fails to match semantically related research terms.
- **General-Purpose AI Assistants:** Flexible but prone to hallucinating non-existent professors, research labs, or faculty guidelines.
- **Traditional Project Archives:** Difficult to navigate and lacks decision-support features for refining project scopes.

---

## Engineering Gap / Limitation
Existing systems rely on rigid keyword queries or ungrounded general LLMs. There is a practical need for a system that combines vector-based semantic retrieval with explainable recommendation logic grounded in official university documents.

---

## Project Objectives
- **O1:** Design a document-grounded retrieval solution using official UTM FAI faculty profiles and FYP portal guidelines.
- **O2:** Develop a multi-stage matching workflow that recommends supervisors, identifies similar past projects, and suggests project scope improvements.
- **O3:** Evaluate answer groundedness, retrieval precision, recommendation fairness, latency, and safety controls.

---

## Target Users
Undergraduate Artificial Intelligence students preparing for FYP proposal submission and supervisor selection at UTM.

---

## Data Sources
- **UTM FAI Official FYP Portal & Guidelines:** [https://sites.google.com/utm.my/fai-fyp/](https://sites.google.com/utm.my/fai-fyp/)
- **UTM Scholar FAI Research Experts Archive:** [https://utmscholar.utm.my/faculties/fai](https://utmscholar.utm.my/faculties/fai)
- **UTM Faculty of Artificial Intelligence Staff Directory:** [https://fai.utm.my/management/](https://fai.utm.my/management/)

---

## Weekly Progress

| Week | Focus | Deliverable | Status |
| :--- | :--- | :--- | :---: |
| **W1** | System map + project scope | README + `/docs/` artifacts | Completed |