# Milestone 1 PDF Validation Specification (M1-T2)

## Purpose
Define concrete validation behavior for single-file PDF intake in Milestone 1.

## Validation Pipeline Order
1. Request shape validation
2. Input source resolution (`file_binary` xor `file_path`)
3. File existence/readability checks (for `file_path`)
4. MIME + extension checks
5. PDF integrity check
6. Password/encryption check
7. Milestone language guard (`ja -> ru` only)

## Validation Rules

### R1 — Request Shape
- `request_id` is required and non-empty.
- `source_lang` and `target_lang` are required.

### R2 — Input Source Exclusivity
- Exactly one input source is allowed:
  - `file_binary`, or
  - `file_path`
- If both or neither are present, return input conflict/missing errors.

### R3 — File Path Checks
- `file_path` must point to an existing readable file.
- Directories are rejected.

### R4 — PDF Type Gate
- Extension must be `.pdf` (case-insensitive).
- MIME should be `application/pdf` where available.
- If extension and MIME disagree, reject unless explicit override is configured (not enabled in M1).

### R5 — PDF Integrity
- Parser must be able to open and iterate basic document metadata/page count.
- Corrupted/truncated files are rejected.

### R6 — Password Protection
- If encrypted/password-protected and not decryptable with provided runtime context (none in M1), reject as protected.

### R7 — Language Compatibility (M1)
- Only `source_lang=ja` and `target_lang=ru` are accepted.
- Any other pair returns `ERR_LANG_UNSUPPORTED_M1`.

## Error Contract
All validation failures must return:
- `status = "error"`
- `request_id`
- `errors[]` containing objects:
  - `code`
  - `message`
  - `stage`
  - `details` (optional)

### Error Code Mapping
- `ERR_INPUT_MISSING` — required field/source missing
- `ERR_INPUT_CONFLICT` — both binary and path provided
- `ERR_FILE_NOT_FOUND` — `file_path` not found
- `ERR_FILE_UNREADABLE` — no read access / path invalid
- `ERR_FILE_NOT_PDF` — extension/MIME gate failed
- `ERR_FILE_CORRUPT` — integrity parsing failed
- `ERR_FILE_PROTECTED` — encrypted/password-protected
- `ERR_LANG_UNSUPPORTED_M1` — pair is not `ja -> ru`

## Example Failure Payload
```json
{
  "status": "error",
  "request_id": "a6c0f4a4-58d9-4fda-b7b3-8fdb7e9f6ca8",
  "message": "Input validation failed",
  "errors": [
    {
      "code": "ERR_FILE_NOT_PDF",
      "stage": "pdf_type_gate",
      "message": "Uploaded file is not a valid PDF by MIME/extension checks"
    }
  ]
}
```

## Acceptance Evidence Checklist (M1-T2)
- Non-PDF file rejected with `ERR_FILE_NOT_PDF`
- Corrupted PDF rejected with `ERR_FILE_CORRUPT`
- Password-protected PDF rejected with `ERR_FILE_PROTECTED`
- Valid JA->RU request passes validation
- Non-JA->RU request rejected with `ERR_LANG_UNSUPPORTED_M1`
