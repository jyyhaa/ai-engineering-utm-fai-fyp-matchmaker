
# Week 3 — V1 Data Pipeline Decisions

## 1. Project Information

**Project:** UTM FAI FYP & Supervisor Matching Assistant

**Topic:** Topic 3 — Data Engineering for AI

**Pipeline Version:** week3-v1

**Selected Pathway:** Pathway A — PDF/TXT Documents

**Processing Mode:** Batch Processing

## 2. Selected Data Sources

### Source 1: FAI Lecturer Profiles

- File: `data/raw/fai_all_lecturer_profiles.txt`
- Format: TXT
- Records: 117 lecturer profiles
- Content: Lecturer names, departments, research expertise,
  and source URLs.
- Purpose: Provide information about lecturer research
  expertise for the FYP supervisor matching assistant.

### Source 2: UTM FAI FYP Handbook

- File: `data/raw/utm_fai_fyp_guidelines.pdf.pdf`
- Format: PDF
- Pages: 119
- Content: FYP guidelines and academic procedures.
- Purpose: Provide reference information about FYP
  requirements and procedures.

## 3. Part B — Preparation Method Comparison

Two text chunking methods were evaluated:

1. Fixed-size chunking
2. Paragraph-aware chunking

Both methods used a target chunk size of 500 characters.

### Lecturer Profiles

- Fixed-size chunking produced 67 chunks.
- Paragraph-aware chunking produced 101 chunks.
- The source contained 117 lecturer record IDs.
- Fixed-size chunks contained 115 identifiable lecturer IDs.
- Paragraph-aware chunks contained all 117 identifiable IDs.

**Decision:** Select paragraph-aware chunking because
it preserved identifiable lecturer IDs more effectively
in the inspected output.

**Limitation:** Some chunks still contain more than one
lecturer record, so lecturer boundaries are not fully
preserved.

### FYP Handbook

- Fixed-size chunking produced 373 chunks.
- Paragraph-aware chunking produced 438 chunks.
- The PDF contained 119 pages.
- Text was extracted from 115 pages.
- Pages 10, 16, 18 and 94 had no extractable text.

**Decision:** Select paragraph-aware chunking because
it better follows paragraph boundaries in the extracted
text compared with fixed-size chunking.

**Limitation:** Some PDF content contains repeated text
and layout-related extraction artifacts. The prepared
chunks do not preserve page-level references.

## 4. Selected Preparation Configuration

Both document sources use:

- Method: Paragraph-aware chunking
- Chunk size: 500 characters
- Whitespace cleaning: Enabled
- Output format: JSONL
- Processing library: Python and pypdf

The preparation process reads each source, extracts
text, cleans whitespace, divides the text into chunks,
and exports the results as JSONL records.

## 5. ETL or ELT Decision

**Selected approach: ETL (Extract, Transform, Load).**

- Extract: Read TXT or extract text from PDF.
- Transform: Clean whitespace and perform
  paragraph-aware chunking.
- Load: Save prepared chunks into JSONL files.

ETL was selected because text preparation is performed
before the processed data is saved for later AI tasks.

## 6. Batch or Streaming Decision

**Selected approach: Batch processing.**

The lecturer profiles and FYP handbook are stored
documents rather than continuous real-time data.

Batch processing is suitable because the pipeline
processes the available source files when executed.

Streaming is not required for the current V1 scope.

## 7. Executed Pipeline Stages and Contracts

| Stage | Contract / Evidence | Failure Response |
|---|---|---|
| Ingest | Selected source file exists and can be accessed | Stop if the source is missing |
| Inspect | Source is readable and contains non-empty text | Stop if text cannot be extracted |
| Prepare | Paragraph-aware chunking produces text chunks | Stop if no chunks are generated |
| Validate | Prepared output exists, is non-empty, and has SHA-256 fingerprint | Stop if output is missing or empty |
| Record Lineage | Source hashes, output hashes, processing configuration, and mappings are stored in the manifest | Report missing provenance information |

## 8. Part C — Executed V1 Pipeline Outputs

### Lecturer Profiles Pipeline

- Pipeline ID: `documents`
- Output:
  `data/pipeline_v1/documents/prepared_text.jsonl`
- Manifest:
  `data/pipeline_v1/documents/pipeline_manifest.json`

### FYP Handbook Pipeline

- Pipeline ID: `documents_fyp_handbook`
- Output:
  `data/pipeline_v1/documents_fyp_handbook/prepared_text.jsonl`
- Manifest:
  `data/pipeline_v1/documents_fyp_handbook/pipeline_manifest.json`

Separate pipeline identifiers were used to prevent
the second document pipeline from overwriting the
first pipeline's prepared output and manifest.

## 9. Repeatability Verification

Cell 9 was executed for both document pipelines.

**Lecturer Profiles Pipeline:** PASS

**FYP Handbook Pipeline:** PASS

The verification compared:

- Source SHA-256 fingerprints
- Prepared output SHA-256 fingerprints
- Output sizes and counts
- Source-to-output mappings

Both pipelines produced matching results when rerun
in the same Google Colab environment.

This is a repeatability smoke test, not proof of
cross-environment reproducibility.

## 10. Data Provenance and Access Limitations

The lecturer profile dataset was compiled from
official UTM FAI webpages with automated extraction
assistance and manual review.

Individual profile details may still require
verification against their original sources.

The FYP handbook was provided as a PDF reference
document.

Source versions and redistribution permissions have
not been fully verified.

Only approved source files and outputs should be
uploaded to the project repository.

## 11. Accepted Limitations and Future Improvements

The current V1 pipeline has the following limitations:

1. Lecturer profiles may be mixed across chunks.
2. Some lecturer expertise fields are not provided.
3. Four PDF pages have no extractable text.
4. PDF extraction may contain duplicated headings
   or other formatting artifacts.
5. Page-level references are not retained in
   the prepared JSONL output.
6. Output repeatability does not guarantee
   semantic correctness.

Future improvements may include record-aware
chunking for lecturer profiles, page-aware PDF
extraction, source verification, and more detailed
data quality checks.

## 12. Traceability to Topic 2 Requirements

The Week 3 V1 data pipeline supports the functional requirements defined in `docs/week2_requirements.md`.

The pipeline prepares two official-document-based data sources for future supervisor matching and FYP academic guidance features. It does not implement the final AI matching or question-answering system.

### 12.1 Functional Requirement Traceability

| Requirement ID | Topic 2 Requirement | Week 3 Pipeline Support |
|---|---|---|
| **FR1** | Match student FYP topics with verified UTM FAI lecturer research expertise and provide evidence-based recommendations. | The lecturer profiles TXT pipeline extracts, cleans, and chunks lecturer information, including research expertise and source URLs. This prepares text for future semantic matching. |
| **FR2** | Retrieve official UTM FAI FYP information and answer questions with source references. | The FYP Handbook PDF pipeline extracts, cleans, and chunks handbook text for future information retrieval. Page-level references are not yet preserved. |
| **FR3** | Suggest possible FYP scope refinements using lecturer expertise and official academic information. | Both prepared datasets provide potential reference material for future AI-generated scope suggestions. |
| **FR4** | Identify out-of-scope questions and provide refusal or redirection. | Not directly implemented by the Week 3 data pipeline. This requirement belongs to the future application-level response and safety controls. |
| **FR5** | Provide clear recommendations and guidance with verifiable citations and indicate insufficient evidence. | The pipeline records source-to-output mappings and SHA-256 fingerprints to support data provenance. However, user-facing citations and evidence verification are not yet implemented. |

### 12.2 Relationship to Topic 2 Architecture

The Week 3 pipeline expands the DATA component of the existing Topic 2 V1 architecture.

The executed workflow is:

**Source → Ingest → Inspect → Prepare/Transform → Validate Output → Record Lineage**

Two independent document pipelines were executed:

1. **Lecturer Profiles Pipeline:** Prepares lecturer research expertise information for future supervisor matching.
2. **FYP Handbook Pipeline:** Prepares academic guideline information for future document retrieval and question answering.

The executed workflow is documented in `docs/week3_pipeline_diagram.png`.

### 12.3 Scope Boundaries

The Week 3 pipeline focuses on document ingestion, text preparation, output validation, and provenance tracking.

The following capabilities are not implemented in the current pipeline:

- Semantic similarity calculation or supervisor ranking.
- Retrieval-augmented generation or chatbot responses.
- Automatic verification of lecturer expertise against original webpages.
- Page-level citations for extracted PDF content.
- Student project scope recommendation generation.
- Application-level out-of-scope query handling.

These capabilities belong to later stages of the project.

### 12.4 Traceability Conclusion

The Week 3 V1 pipeline directly supports the data preparation needs of **FR1** and **FR2** and provides potential reference material for **FR3**.

It provides foundational provenance information relevant to **FR5**, but it does not yet meet the requirement for verifiable user-facing citations.

**FR4** is outside the current data pipeline scope.

Therefore, the Week 3 deliverables establish an initial data-processing foundation for the UTM FAI FYP & Supervisor Matching Assistant without claiming that the complete functional requirements have been implemented.