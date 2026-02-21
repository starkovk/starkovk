# Persistent Context for PDF Translation Workflow Project (Updated Feb 21, 2026)

## Core Tool (Design Constraint)
PDFMathTranslate-next (GitHub: https://github.com/PDFMathTranslate/PDFMathTranslate-next) — use **ALWAYS** as the main engine; **NO alternatives** (e.g., no custom pdf→text→translate→pdf pipeline). Backend: BabelDOC (https://github.com/funstory-ai/BabelDOC).

## Supported Languages
Multi-language (e.g., en→zh, ja→en/zh/tr/ru, multi via `--lang-in --lang-out`). Japanese (ja) to Russian (ru) is supported via `--lang-in ja --lang-out ru`.

## Installation / Setup
- `pip install pdf2zh-next`
- Docker image: `byaidu/pdf2zh`
- WebUI / GUI via Gradio app
- Windows ZIP releases (e.g., `pdf2zh-v2.8.2-BabelDOC-with-assets-win64.zip`)
- Zotero plugin (`guaguastandup/zotero-pdf2zh`) for direct translate

## CLI Flags / Parameters (from `USAGE_commandline.html` and `ADVANCED.md`)
- **Input**: `input-files` (local files or multiple for batch, e.g., `pdf2zh file1.pdf file2.pdf`)
- **Languages**: `--lang-in [source] --lang-out [target]` (e.g., `--lang-in ja --lang-out ru`)
- **Service**: selection via individual CLI flags, e.g., `--openai`, `--deepseek`, `--deepl`, `--google`; list available services via `pdf2zh_next -h`
- **Partial**: `--pages "1,3,5-7"` (comma-separated with ranges)
- **Multi-thread**: `--pool-max-workers [num]` (workers for parallel execution)
- **Output**: `--output /path/ [dir]` (saves translated PDFs); bilingual `dual.pdf` (side-by-side) vs monolingual `mono.pdf` (default: both)
- **Filters / tuning**:
  - `--custom-system-prompt [file]` (prompt for jargon)
  - `--glossaries "glossary.csv"` (term glossary)
  - `--primary-font-family [font]` (font override)
  - `--ignore-cache` (ignore cache)
  - Not documented: no `-f`/`-c` regex, `--skip-subset-fonts`, `--compatible`, `--onnx`, `--mcp`, `--sse`

## Key Features (from README / ADVANCED.md / arXiv)
- **Layout parsing**: DocLayout-YOLO (ONNX) for detecting blocks/formulas (LaTeX)/tables/figures/multi-column/TOC/annotations/cross-page elements; chunking with metadata (bbox, font/size/baseline/positioning/style)
- **Text extraction**: automatic with Pdfminer.six / PyMuPDF; MinerU for advanced mode (in 2.7+); handles rare cases (insets) via layout-aware extraction
- **Translation**: middleware for chunks via services (Google/DeepL/Ollama/OpenAI); contextual handling for scientific jargon, partial translation for tables/formulas; keep LaTeX/formulas intact and translate descriptions
- **Reconstruction**: re-render translated text in bbox; dynamic scaling/resizing fonts/panels for length differences; preserve graphics/styles without distortion; bilingual side-by-side output
- **Batch processing**: loop over files/dirs (multiple inputs); intended for academic papers/reviewer workflows (including Zotero batch usage)
- **Plugins**: Zotero for direct translation; Gradio PDF preview workflow

## Gotchas / Limitations / Error Handling (from README / issues / X posts)
- **Scanned PDFs**: require pre-OCR (no built-in OCR by default); fallback in word-conversion may merge paragraphs; use `--ocr-workaround` (black-on-white force), `--auto-enable-ocr-workaround` (auto for heavily scanned docs)
- **Large PDFs**: memory issues possible; chunk pages with `--max-pages-per-part 50`; use `--pool-max-workers` for controlled parallelism; large pages may be skipped
- **Japanese Kanji / Russian Cyrillic**: supported, but font mapping/resizing may be required (`--primary-font-family`); idiom/accuracy issues may need `--custom-system-prompt` or `--glossaries`
- **Errors**: no explicit retries; exceptions are propagated; use `--ignore-cache`; for restricted networks set `HF_ENDPOINT=https://hf-mirror.com` for model downloads; use `--debug` for logs; rate limits on DeepL/Google can be mitigated by fallback to Ollama
- **Updates (2025–2026)**: BabelDOC backend is integrated by default (no `--babeldoc`, always on); v2.0 released May 2025; current v2.8.2 (Dec 2025) improved consistency (font/term fixes), experimental table text translation, and cache cleanup (Aug 2025)

## Requirements Alignment Rule
All requirements **MUST** tie to these features and must cover:
- normal and edge cases (e.g., scanned PDFs, large files, Japanese Kanji issues)
- error handling and fallback behavior
- persistence of output PDFs
- UI pathways (GUI and n8n integration)
