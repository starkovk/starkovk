# Task Backlog

This backlog translates `plan.md` milestones into actionable implementation tasks.

## Status Legend
- `todo` — not started
- `in_progress` — actively being worked on
- `blocked` — waiting on dependency/input
- `done` — completed and verified

## Milestone 1 — MVP Translation Flow (Single PDF)

| ID | Task | Linked Requirements | Priority | Dependencies | Definition of Done | Status |
|---|---|---|---|---|---|---|
| M1-T1 | Define n8n input contract for single file upload/path | G1 | High | none | Input schema documented and accepted by workflow entry node | done |
| M1-T2 | Implement PDF validation (type/corruption/password checks) | G1 AC4-AC5 | High | M1-T1 | Invalid files are rejected with clear error payload/messages | done |
| M1-T3 | Implement JA→RU command template using PDFMathTranslate-next | G3, Context constraints | High | M1-T1 | Exec node command finalized with `--lang-in ja --lang-out ru` and output directory | in_progress |
| M1-T4 | Generate bilingual output and persist artifact location | G4 AC2, AC9 | High | M1-T3 | `dual.pdf` result is stored and surfaced to caller/UI | todo |
| M1-T5 | Return workflow response with status + download path | G4 AC9, G7 | Medium | M1-T4 | API/GUI response includes success state and output link/path | todo |
| M1-T6 | Create smoke test cases for valid/invalid single-file runs | G1/G4 | Medium | M1-T2, M1-T4 | Test checklist and expected outcomes documented with run evidence | todo |

## Milestone 2 — Batch and Partial Processing

| ID | Task | Linked Requirements | Priority | Dependencies | Definition of Done | Status |
|---|---|---|---|---|---|---|
| M2-T1 | Add batch input mode (directory/multiple files) | G1 AC2 | High | M1 complete | Multiple files accepted and queued per run | todo |
| M2-T2 | Configure controlled parallel processing | G1 AC2 | High | M2-T1 | Worker parameter (`--pool-max-workers`) exposed and used | todo |
| M2-T3 | Add partial-page processing option | G7 AC3 | Medium | M1-T3 | `--pages` path implemented and validated | todo |
| M2-T4 | Implement per-file job result summary | G5 AC4 | Medium | M2-T1 | Output includes file-level success/failure and error details | todo |
| M2-T5 | Add batch test scenarios (mixed valid/invalid set) | G1/G5 | Medium | M2-T4 | Batch behavior verified without data loss or cross-file corruption | todo |

## Milestone 3 — Robustness and Edge Cases

| ID | Task | Linked Requirements | Priority | Dependencies | Definition of Done | Status |
|---|---|---|---|---|---|---|
| M3-T1 | Add scanned-PDF detection gate and warning path | G1 AC3, G6 | High | M1-T2 | Scanned inputs trigger warning + pre-OCR requirement | todo |
| M3-T2 | Implement large-file handling strategy | G2 AC3, G5 AC1 | High | M2 complete | Chunking/memory controls documented and wired into run config | todo |
| M3-T3 | Add retry/fallback orchestration for translation providers | G3 AC3, G5 AC2 | High | M1-T3 | Provider fallback path works; logs capture retry/failure chain | todo |
| M3-T4 | Ensure atomic output writing and interruption safety | G5 AC3-AC4 | High | M1-T4 | Interrupted runs do not leave corrupted final PDFs | todo |
| M3-T5 | Add structured logging and debug mode controls | G5 AC2 | Medium | M3-T3 | Logs include request IDs, file IDs, and failure reasons | todo |

## Milestone 4 — Layout Fidelity and Embedded Content

| ID | Task | Linked Requirements | Priority | Dependencies | Definition of Done | Status |
|---|---|---|---|---|---|---|
| M4-T1 | Validate formula and table preservation workflow | G2 AC1, G4 AC5 | High | M3 complete | QA checklist confirms formulas/tables retain structure | todo |
| M4-T2 | Validate headers/footers/page-number restoration | G2 AC5, G4 AC6 | Medium | M4-T1 | Page elements preserved with position/format checks | todo |
| M4-T3 | Add font-family override guidance for JA/RU rendering | G3 AC2 | Medium | M1-T3 | Font override is parameterized and documented for operators | todo |
| M4-T4 | Implement image text pipeline (OCR -> translation -> embed/annotate) | G6 AC1-AC4, G4 AC7 | High | M3 complete | Image text is translated and placed with traceable method | todo |
| M4-T5 | Add low-OCR-quality warning and reporting | G6 AC5 | Medium | M4-T4 | Low-confidence OCR surfaces warning in output summary | todo |

## Milestone 5 — Operationalization and UX

| ID | Task | Linked Requirements | Priority | Dependencies | Definition of Done | Status |
|---|---|---|---|---|---|---|
| M5-T1 | Expose runtime configuration in n8n (service/prompt/glossary/output) | G3, G7 | High | M2/M3 | Operators can adjust runtime params without editing code | todo |
| M5-T2 | Finalize user delivery path (web/API/direct output link) | G4 AC9, G7 | High | M1-T5 | All supported invocation modes can retrieve translated files | todo |
| M5-T3 | Build end-to-end acceptance checklist mapping all groups | G1-G7 | High | M1-M4 | Every acceptance criterion mapped to test evidence | todo |
| M5-T4 | Publish runbook and troubleshooting guide | G5, G7 | Medium | M5-T3 | Operators can diagnose common failures via documented steps | todo |

## Cross-Cutting Tasks

| ID | Task | Linked Requirements | Priority | Dependencies | Definition of Done | Status |
|---|---|---|---|---|---|---|
| X-T1 | Maintain requirements traceability matrix (`requirement -> tasks -> evidence`) | all | High | ongoing | Matrix updated on every milestone completion | todo |
| X-T2 | Maintain change log for workflow behavior and flags | all | Medium | ongoing | Versioned notes added for each released milestone | todo |
| X-T3 | Define sample dataset set (normal + edge documents) | G1-G6 | High | none | Dataset catalog approved for regression checks | todo |

## Review Cadence
- Review task status after each milestone demo.
- Keep exactly one milestone marked `in_progress` at a time.
- Promote tasks to `done` only with linked verification evidence.
