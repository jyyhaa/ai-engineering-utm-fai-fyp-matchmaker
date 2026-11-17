# UTM FAI FYP Matchmaker

> An explainable, Retrieval-Augmented Generation (RAG)-based AI assistant that helps UTM Artificial Intelligence students identify potential FYP supervisors, retrieve official FYP information, and improve project scopes using verified university sources and evidence-backed recommendations.

**Project Status:** In Development — Week 3 Data Pipeline Completed

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

### Source 1: UTM FAI Official FYP Portal and Handbook

- **URL:** https://sites.google.com/utm.my/fai-fyp/
- **Data Type:** Official PDF documents and university webpages.
- **Purpose:** Retrieve FYP procedures, submission requirements, guidelines, and evaluation information.

### Source 2: UTM FAI Departmental Lecturer Profiles

- **URL:** https://fai.utm.my/department-of-intelligence-informatics-2/
- **Data Type:** Official faculty webpages, with lecturer information prepared as TXT records.
- **Purpose:** Obtain lecturer names, research expertise, and source references for supervisor matching.

### Source 3: UTM Scholar – Faculty of Artificial Intelligence

- **URL:** https://utmscholar.utm.my/faculties/fai
- **Data Type:** Online research profiles and publication information, where accessible.
- **Purpose:** Provide additional evidence of lecturer research interests and relevant publications.

---

## Weekly Progress

| Week | Focus | Deliverable | Status |
|---|---|---|---|
| **W1** | AI system observation, conceptual system map, and project scoping | `README.md`, `docs/week1_observation_sheet.md`, `docs/week1_system_map.png`, `docs/week1_project_scope.md` | Completed |
| **W2** | Requirements specification, evaluation criteria, V1 architecture, and architecture decisions | `docs/week2_requirements.md`, `docs/week2_evaluation_criteria.md`, `docs/week2_architecture_v1.png`, `docs/week2_architecture_decisions.md` | Completed |
| **W3** | Data inspection, text preparation, lineage tracking, executable V1 pipelines, and repeatability verification | `notebooks/week3_pipeline.ipynb`, Week 3 documentation, prepared data, and pipeline manifests | Completed |

---

## Week 3 — Data Engineering for AI

### 1. Overview

In Week 3, an executable V1 batch data pipeline was developed for the UTM FAI FYP Matchmaker.

The pipeline prepares lecturer research expertise and FYP handbook information for future AI retrieval and supervisor matching tasks.

The selected approach is **Pathway A — PDF/TXT Documents**.

### 2. Datasets Used

Two datasets were selected:

**Dataset 1: FAI Lecturer Profiles**

- Filename: `fai_all_lecturer_profiles.txt`
- Format: TXT
- Number of lecturer records: 117
- Content: Lecturer names, departments, research expertise, and source URLs.

**Dataset 2: UTM FAI FYP Handbook**

- Filename: `utm_fai_fyp_guidelines.pdf.pdf`
- Format: PDF
- Total pages: 119
- Pages with extractable text: 115
- Pages without extractable text: 10, 16, 18, and 94.

### 3. Data Preparation

Two text chunking methods were compared:

1. Fixed-size chunking.
2. Paragraph-aware chunking.

**Comparison Results**

| Dataset | Fixed-size Chunks | Paragraph-aware Chunks |
|---|---:|---:|
| Lecturer Profiles (TXT) | 67 | 101 |
| FYP Handbook (PDF) | 373 | 438 |

**Selected Method:** Paragraph-aware chunking with a 500-character chunk size.

This method was selected because it better follows paragraph boundaries in the extracted text.

For the lecturer dataset, paragraph-aware chunks contained all 117 identifiable lecturer record IDs, while fixed-size chunks contained 115.

However, some chunks still contain multiple lecturer profiles, so record boundaries are not fully preserved.

### 4. V1 Data Pipeline Workflow

The executed pipeline follows these stages:

**Source → Ingest → Inspect → Prepare/Transform → Validate Output → Record Lineage**

The pipeline performs the following operations:

- Reads TXT and PDF source documents.
- Inspects document readability and extracted text.
- Cleans whitespace.
- Performs paragraph-aware text chunking.
- Exports prepared data as JSONL.
- Checks that output files exist and are non-empty.
- Records source and output SHA-256 fingerprints.
- Saves source-to-output mappings in pipeline manifests.

The pipeline uses **ETL (Extract, Transform, Load)** and **batch processing**.

### 5. Pipeline Outputs

Two independent document pipelines were executed to prevent files from overwriting each other.

**Lecturer Profiles Pipeline**

- Pipeline ID: `documents`
- Prepared output: `data/pipeline_v1/documents/prepared_text.jsonl`
- Manifest: `data/pipeline_v1/documents/pipeline_manifest.json`

**FYP Handbook Pipeline**

- Pipeline ID: `documents_fyp_handbook`
- Prepared output: `data/pipeline_v1/documents_fyp_handbook/prepared_text.jsonl`
- Manifest: `data/pipeline_v1/documents_fyp_handbook/pipeline_manifest.json`

The Part B prepared outputs and common lineage manifest are stored separately under `data/processed/` and `data/week3_manifest.json`.

### 6. Repeatability Verification

Cell 9 was executed to verify whether the pipelines produced consistent results when rerun.

| Verification | Lecturer Profiles | FYP Handbook |
|---|---|---|
| Source fingerprints match | PASS | PASS |
| Prepared output fingerprints match | PASS | PASS |
| Output sizes and counts match | PASS | PASS |
| Source-to-output mappings match | PASS | PASS |
| Overall repeatability smoke test | PASS | PASS |

**Result:** Both pipelines passed the repeatability verification.

This confirms consistent outputs within the current Google Colab environment. It does not establish full cross-environment reproducibility or guarantee semantic data correctness.

### 7. Known Limitations

The current V1 pipeline has several limitations:

- Some lecturer profiles may be mixed within the same text chunk.
- Some lecturer research expertise fields are missing.
- Four handbook pages contain no extractable text.
- PDF extraction may produce repeated headings or formatting artifacts.
- Page-level references are not preserved in the prepared JSONL output.
- Source versions and redistribution permissions require further verification.

Future improvements may include record-aware lecturer chunking, page-aware PDF extraction, and more detailed data quality validation.

### 8. Week 3 Deliverables

| Deliverable | File |
|---|---|
| Executable V1 Pipeline Notebook | [week3_pipeline.ipynb](notebooks/week3_pipeline.ipynb) |
| Data Inventory | [week3_data_inventory.md](docs/week3_data_inventory.md) |
| Pipeline Diagram | [week3_pipeline_diagram.png](docs/week3_pipeline_diagram.png) |
| Pipeline Decisions | [week3_pipeline_decisions.md](docs/week3_pipeline_decisions.md) |
| Part B Prepared Outputs | `data/processed/` |
| Part B Lineage Manifest | `data/week3_manifest.json` |
| Part C Pipeline Outputs and Manifests | `data/pipeline_v1/` |

Prepared outputs and manifests should only be published when redistribution is permitted.

---

## Current Development Status

**Completed:**
- Week 1: Project scoping and conceptual system mapping.
- Week 2: Requirements, evaluation criteria, and V1 architecture.
- Week 3: Data preparation, lineage documentation, executable document pipelines, and repeatability verification.

**Not Yet Implemented:**
- Semantic retrieval and similarity matching.
- Supervisor recommendation ranking.
- RAG-based conversational responses.
- User-facing evidence citations.
- End-to-end system evaluation.

The current repository documents the project's engineering progress. It does not yet represent a completed AI assistant.

---

## Responsible AI Considerations

The proposed system will follow these principles:

- **Grounded Information:** Use verifiable university sources rather than unsupported claims.
- **Explainability:** Provide evidence supporting potential supervisor recommendations.
- **Human Oversight:** Recommendations do not represent official supervisor assignments or academic approval.
- **Privacy:** Avoid exposing unnecessary personal or sensitive information.
- **Source Verification:** Indicate missing or insufficient evidence instead of fabricating citations.
- **Data Access:** Respect source permissions and document provenance limitations.

---

## Project Repository Structure

```text
ai-engineering-utm-fai-fyp-matchmaker/
├── data/
│   ├── raw/
│   ├── processed/
│   ├── pipeline_v1/
│   │   ├── documents/
│   │   └── documents_fyp_handbook/
│   └── week3_manifest.json
├── docs/
│   ├── week1_observation_sheet.md
│   ├── week1_system_map.png
│   ├── week1_project_scope.md
│   ├── week2_requirements.md
│   ├── week2_evaluation_criteria.md
│   ├── week2_architecture_v1.png
│   ├── week2_architecture_decisions.md
│   ├── week3_data_inventory.md
│   ├── week3_pipeline_diagram.png
│   └── week3_pipeline_decisions.md
├── evaluation/
├── notebooks/
│   └── week3_pipeline.ipynb
├── prompts/
├── src/
└── README.md
```
