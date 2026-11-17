# Week 3 — Data Inventory

## Project Information

**Project:** UTM FAI FYP & Supervisor Matching Assistant  
**Course:** SAIA 3113 — AI Engineering  
**Selected Pathway:** A — PDF/TXT Documents

## 1. Data Source Inventory

| Source ID | Filename / URL | Type and Purpose | Version / Date / Owner | Access / Rights | Inspection Evidence |
|---|---|---|---|---|---|
| S01 | `fai_all_lecturer_profiles.txt` | TXT; lecturer names, departments, and research expertise for supervisor matching | Compiled 17 November 2026; UTM FAI source webpages | Public faculty webpages; reuse and redistribution rights not independently confirmed | 117 records detected; 33,335 characters; readable TXT |
| S02 | `utm_fai_fyp_guidelines.pdf.pdf` | PDF; official FYP policies and procedures | Academic Session 2026/2027; UTM Faculty of Artificial Intelligence | Institutional handbook; redistribution permission not independently confirmed | 119 pages; 115 pages with extractable text; 4 pages without extractable text |

## 2. Requirements Traceability

- **FR1 — Supervisor Matching:** Requires lecturer names, departments, and research expertise from S01.
- **FR2 — FYP Policy Q&A:** Requires official handbook information from S02.
- **FR3 — Scope Refinement:** Uses relevant expertise and FYP guidance from both sources.
- **FR5 — Citations:** Requires preservation of source URLs and PDF page information.
- **NFR4 — Source Updates:** Requires source metadata and reproducible processing.

## 3. Initial Inspection Observations

### S01 — Lecturer Profiles

- File size: 34,172 bytes.
- Total characters: 33,335.
- Lecturer records detected: 117.
- The TXT contains labeled fields such as Record ID, Name, Department, Research Expertise, and Source URL.
- Some records have research expertise marked as Not provided.
- The dataset was compiled from official UTM FAI department webpages and manually reviewed.
- The TXT header should be updated to reflect its manual review status.

### S02 — FYP Handbook

- File size: 89,523,865 bytes.
- Total PDF pages: 119.
- Pages containing extractable text: 115.
- Pages without extractable text: 4.
- The first page contains repeated headings and duplicated code text in the extraction.
- Page-level metadata should be preserved for future citations.
- Pages without extractable text require further inspection before processing decisions are finalized.

## 4. Storage and Data Handling

**Raw data:** `data/raw/`

**Prepared data:** `data/processed/`

Original files will be retained separately from processed outputs.

The Colab workspace is temporary. Final approved artefacts will be saved to the existing project repository.

## 5. Selected Preparation Method

**Pathway A — PDF/TXT**

Planned processing includes document parsing, justified whitespace cleaning, text chunking, validation, and metadata preservation.

Missing lecturer expertise will not be invented. Supervisor availability will not be inferred from research expertise.