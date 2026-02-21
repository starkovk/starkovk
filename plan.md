# Implementation Plan

## Objective
Deliver an automated n8n workflow that translates scientific PDFs from Japanese to Russian while preserving layout and producing bilingual output, using **PDFMathTranslate-next + BabelDOC** as the mandatory engine.

## Scope and Constraints
- Mandatory engine: PDFMathTranslate-next (BabelDOC backend), no custom replacement pipeline.
- Primary language direction: `--lang-in ja --lang-out ru`.
- Must preserve formulas, tables, positioning, styles, and page structure where possible.
- Requirements and acceptance criteria are defined in `requirements.md`; context and operational constraints are in `context.md`.

## Milestones

### Milestone 1 — MVP Translation Flow (Single PDF)
**Goal:** End-to-end translation for one valid PDF with bilingual output.

**Includes:**
- Input validation for a single PDF.
- n8n execution path calling PDFMathTranslate-next.
- Required language flags for JA→RU.
- Output persistence to configured directory.
- User-facing result (download link/API response).

**Exit Criteria:**
- A valid Japanese PDF is translated into bilingual output.
- Invalid/non-PDF and protected/corrupt file cases return clear errors.

---

### Milestone 2 — Batch and Partial Processing
**Goal:** Support multi-file processing and targeted page translation.

**Includes:**
- Directory/multi-file batch processing.
- Parallel worker tuning (`--pool-max-workers`).
- Partial page processing (`--pages`).
- Per-file status tracking in n8n output.

**Exit Criteria:**
- Batch jobs complete with per-file success/failure reporting.
- Partial-page runs produce expected scoped output.

---

### Milestone 3 — Robustness and Edge Cases
**Goal:** Improve reliability for large/scanned/complex documents.

**Includes:**
- Scanned PDF pre-check + warning/pre-OCR gate.
- Large-document strategy (chunking and memory-safe handling).
- Error logging, fallback provider strategy, and retry orchestration in n8n.
- Safe output strategy preventing corrupted partial files.

**Exit Criteria:**
- Large/scanned inputs are handled predictably with explicit warnings.
- Interrupted or partial failures do not corrupt completed outputs.

---

### Milestone 4 — Layout Fidelity and Embedded Content
**Goal:** Maximize visual fidelity and embedded text handling.

**Includes:**
- Validation of formulas/tables/list/header/footer restoration.
- Font mapping guidance for JA/Kanji and RU/Cyrillic rendering issues.
- Embedded-image text workflow integration (OCR -> translation -> re-embed/annotate).
- Visual comparison checklist for preservation quality.

**Exit Criteria:**
- Document structure is preserved with known limitations documented.
- Embedded-image text flow is testable and produces traceable output.

---

### Milestone 5 — Operationalization and UX
**Goal:** Make workflow maintainable, reviewable, and easy to operate.

**Includes:**
- n8n parameterization (service selection, prompt/glossary, output path).
- Clear logs and run summaries.
- Documentation for runbook/troubleshooting.
- Final acceptance review mapped to all requirement groups.

**Exit Criteria:**
- Team can run and troubleshoot workflow using documented steps.
- Each requirement group has mapped verification evidence.

## Sequencing and Dependencies
1. Milestone 1 is required before all other milestones.
2. Milestone 2 depends on Milestone 1 command path stabilization.
3. Milestone 3 can begin after Milestone 1, but finalization depends on Milestone 2 telemetry.
4. Milestone 4 depends on stable extraction/translation/reconstruction behavior from prior milestones.
5. Milestone 5 finalizes after Milestones 1–4 outputs are validated.

## Deliverables by Stage
- **Stage A:** `plan.md` (this file), then `tasks.md` (next review step).
- **Stage B:** n8n workflow definition + command templates.
- **Stage C:** verification artifacts (test matrix, sample run logs, output checks).

## Definition of Ready for Implementation
Implementation starts only when:
- this plan is approved,
- `tasks.md` is created and reviewed,
- acceptance criteria mapping is agreed,
- test inputs (normal + edge) are selected.
