# Milestone 1 Input Contract (n8n Single-PDF Entry)

## Purpose
Define a stable input contract for the initial single-file translation flow.

## Input Modes
1. Binary upload (multipart/form-data)
2. Local/attached path reference (trusted runtime path)

## Required Fields
- `request_id` (string): unique request correlation ID.
- `source_lang` (string): must be `ja` for Milestone 1.
- `target_lang` (string): must be `ru` for Milestone 1.
- One of:
  - `file_binary` (binary PDF content), or
  - `file_path` (string path to PDF).

## Optional Fields
- `output_dir` (string): output directory path.
- `service` (string): translation backend selector (if exposed in n8n).
- `custom_system_prompt_file` (string): path to prompt file.
- `glossaries` (string): glossary CSV path.
- `primary_font_family` (string): rendering override for JA/RU typography.
- `pages` (string): reserved for later milestone partial runs.
- `pool_max_workers` (number): reserved for batch/parallel milestone.

## Validation Rules
- Exactly one of `file_binary` or `file_path` must be provided.
- File must be PDF by extension + MIME check.
- Reject password-protected or corrupted PDFs with explicit error code/message.
- Reject non-`ja -> ru` in Milestone 1 with a compatibility error.

## Normalized Internal Payload
```json
{
  "request_id": "uuid",
  "input_pdf": "/runtime/input/xxx.pdf",
  "lang_in": "ja",
  "lang_out": "ru",
  "output_dir": "/runtime/output",
  "service": "auto",
  "custom_system_prompt_file": null,
  "glossaries": null,
  "primary_font_family": null
}
```

## Output Contract (MVP)
- `status`: `success` | `error`
- `request_id`: string
- `message`: human-readable result
- `artifacts`:
  - `dual_pdf_path` (if success)
  - `mono_pdf_path` (if available)
- `errors`: array of structured errors (if failure)

## Structured Error Codes
- `ERR_INPUT_MISSING`
- `ERR_INPUT_CONFLICT`
- `ERR_FILE_NOT_PDF`
- `ERR_FILE_CORRUPT`
- `ERR_FILE_PROTECTED`
- `ERR_LANG_UNSUPPORTED_M1`
