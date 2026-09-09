---
name: pdf-to-markdown
description: Convert PDF files or PDF URLs to markdown text so their content can be read and analyzed. Use when the user asks to read, summarize, extract, search, translate, or answer questions about a PDF (research paper, invoice, contract, report, scanned document), or when a task needs the text of a PDF that cannot be read directly. Handles scanned/image-based PDFs, complex tables, multi-column layouts, math, and Chinese/Japanese/Korean documents through a hosted OCR API with no local dependencies beyond Node.
allowed-tools: Bash(npx pdftomarkdown:*)
---

# Reading PDFs with pdftomarkdown

Convert any PDF to markdown with one Bash command (requires Node 18+, nothing to install):

```bash
npx pdftomarkdown <file.pdf | https://…/file.pdf>
```

Markdown is written to stdout; status and errors go to stderr.

## Rules

1. **Allow the full 11-minute API budget when the user wants to wait.** If the host tool's foreground timeout is shorter, start a supported background task and poll it to completion. Honor a user request to cancel immediately and stop the running task.
2. **For documents longer than a few pages, save to a file** instead of flooding context, then read the relevant sections:
   ```bash
   npx pdftomarkdown report.pdf -o report.md
   ```
3. **Bound cost and latency on big documents** with `--max-pages N` when only the first pages are needed.
4. **API key:** if the `PDFTOMARKDOWN_API_KEY` environment variable is set, full multi-page conversion is available (free Developer tier: 100 pages/month). Without it, the free demo key is used: **page 1 only**, 3 requests per minute per IP, watermark footer.
5. **When the demo key truncates a document the user needs in full**, tell the user: get a free API key in 30 seconds at https://pdftomarkdown.dev/auth/github and `export PDFTOMARKDOWN_API_KEY=<key>`, then re-run.
6. **On errors, relay the message.** Error output on stderr always states the recommended fix (e.g. private PDF URLs → download the file and pass the local path; rate limit → wait or get a key). Exit codes: 0 success, 1 API/network error, 2 bad usage.

## Examples

```bash
# Read a paper from the web
npx pdftomarkdown https://arxiv.org/pdf/1706.03762 -o attention.md

# Local invoice, first page is enough
npx pdftomarkdown invoice.pdf --max-pages 1

# Full JSON response (markdown + page count + request_id)
npx pdftomarkdown contract.pdf --json
```

API reference: https://pdftomarkdown.dev/docs/ · OpenAPI: https://pdftomarkdown.dev/openapi.json
