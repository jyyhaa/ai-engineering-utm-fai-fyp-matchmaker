# Data Inventory — UTM FAI FYP Matchmaker (Prototype Pipeline)
Version: 1.2

---

## Source Inventory Table

| Source ID | Filename / Source Path | Data Type & Intended Purpose | Version / Date / Owner | Access / Rights | Ingestion Inspection Evidence |
| :---: | :--- | :--- | :--- | :--- | :--- |
| **S01** | `data/raw/student_handbook_demo.pdf` | Unstructured PDF / Proxy policy handbook for testing section-level text extraction and chunking (FR2, NFR4). | 2026 Demo Edition / Teaching Team | Synthetic Educational Lab Dataset | Verified in Colab (`RAW_DIR`). 1-page PDF with clean selectable text across 4 sections (Registration, Attendance, Graduation, Academic Support). |
| **S02** | `data/raw/faq_demo.txt` | Plain Text (`.txt`) / Proxy Q&A knowledge source for testing multi-format ingestion and citation attribution (FR1, RAI4). | 2026 Demo Edition / Teaching Team | Synthetic Educational Lab Dataset | Verified in Colab (`RAW_DIR`). UTF-8 plain text containing 2 synthetic Q&A pairs separated by blank lines. |

---

## Requirement Traceability Checkpoint

* **Prototype Dataset Justification:**
  * Using the `01_documents` dummy dataset (`student_handbook_demo.pdf` and `faq_demo.txt`) provides a complete, deterministic benchmark for Lab 3 pipeline verification.
  * **FR1 & FR2:** Validates multi-format text extraction (`.pdf` and `.txt`) and paragraph-aware chunking across structured sections and Q&A blocks.
  * **RAI4 & NFR4:** Establishes SHA-256 source hashing and chunk-level metadata tracking (`record_id`, `source`, `source_sha256`) to guarantee 100% citation traceability.
* **Data Storage Strategy:**
  * Raw input documents are stored under `data/raw/`.
  * Cleaned, unified text chunks from both files will be exported to `data/processed/prepared_text.jsonl` in Part B.