# UTM FAI FYP Matchmaker

> An explainable, Retrieval-Augmented Generation (RAG)-based AI assistant that helps UTM Artificial Intelligence students identify potential FYP supervisors, retrieve official FYP information, and improve project scopes using verified university sources and evidence-backed recommendations.

---

## Problem Statement

Undergraduate Artificial Intelligence students at Universiti Teknologi Malaysia (UTM) need to identify potential Final Year Project (FYP) supervisors whose research expertise aligns with their proposed project topics. However, lecturer research information and official FYP guidelines are distributed across separate university platforms, requiring students to manually search and compare multiple sources.

**UTM FAI FYP Matchmaker** aims to address this challenge by combining semantic retrieval with Retrieval-Augmented Generation (RAG) to provide explainable supervisor recommendations, official FYP guidance, and evidence-based project scope suggestions supported by verifiable source citations.

---

## Evidence of the Problem

- **Distributed University Information:** Lecturer research expertise is available through UTM Scholar and FAI departmental webpages, while FYP procedures and guidelines are published separately through the FAI FYP Portal. Students must consult multiple sources when preparing their FYP proposals.

- **Manual Research Expertise Comparison:** Official faculty profiles describe lecturer research interests using specialized terminology. Students may need to manually compare their project ideas with different lecturer expertise areas to identify potential supervisors, creating an opportunity for semantic matching and explainable recommendations.

**Sources:**
- [UTM Scholar – Faculty of Artificial Intelligence](https://utmscholar.utm.my/faculties/fai)
- [UTM FAI Department of Intelligence and Informatics](https://fai.utm.my/department-of-intelligence-informatics-2/)
- [UTM FAI FYP Portal](https://sites.google.com/utm.my/fai-fyp/)

---

## Initial Related Work / Existing Approaches

- **Official Faculty Directories and Research Profiles:** Provide lecturer information and research expertise but do not directly generate semantic supervisor recommendations based on student project descriptions.

- **General-Purpose AI Assistants (e.g., ChatGPT and Claude):** Support conversational question answering but may generate unsupported or outdated university-specific information without access to verified institutional sources.

- **Official FYP Portals and Handbook Documents:** Provide important academic guidelines and requirements but require students to locate and interpret relevant information manually. They do not directly offer personalized supervisor matching or project scope refinement.

---

## Engineering Gap / Limitation

Existing university information sources provide lecturer research expertise and FYP requirements but do not offer an integrated, document-grounded conversational system that semantically matches student project proposals with potential supervisors.

The proposed system addresses this gap by combining semantic retrieval, RAG-based response generation, and explainable recommendation logic supported by verifiable university sources.

---

## Project Objectives

- **O1:** To design and implement a document-grounded retrieval pipeline using verified UTM FAI lecturer research expertise and official FYP guidelines.

- **O2:** To develop an explainable RAG-based conversational assistant that recommends potential FYP supervisors based on student project descriptions and provides evidence-based project scope refinement suggestions.

- **O3:** To evaluate the system in terms of retrieval relevance, supervisor recommendation correctness, citation grounding, response latency, and responsible AI safeguards.

---

## Target Users

Third- and fourth-year Bachelor in Artificial Intelligence with Honours students at UTM FAI who are preparing FYP proposals, exploring potential supervisors, or seeking guidance on official FYP requirements.

---

## Data Sources

- **Source 1: UTM FAI Official FYP Portal and Handbook**
  - URL: https://sites.google.com/utm.my/fai-fyp/
  - Data Type: Official PDF documents and university webpages.
  - Purpose: Retrieve FYP procedures, submission requirements, guidelines, and evaluation information.

- **Source 2: UTM FAI Departmental Lecturer Profiles**
  - URL: https://fai.utm.my/department-of-intelligence-informatics-2/
  - Data Type: Official faculty webpages, with verified lecturer information prepared as TXT or JSON records.
  - Purpose: Obtain lecturer names, research expertise, and source references for supervisor matching.

- **Source 3: UTM Scholar – Faculty of Artificial Intelligence**
  - URL: https://utmscholar.utm.my/faculties/fai
  - Data Type: Online research profiles and publication information, where accessible.
  - Purpose: Provide additional evidence of lecturer research interests and relevant publications.

---

## Weekly Progress

| Week | Focus | Deliverable | Status |
| :--- | :--- | :--- | :---: |
| **W1** | AI system observation, conceptual system map, and project scoping | `README.md`, `docs/week1_observation_sheet.md`, `docs/week1_system_map.png`, `docs/week1_project_scope.md` | Completed |