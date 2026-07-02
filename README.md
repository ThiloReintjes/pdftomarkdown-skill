# pdftomarkdown — PDF reading for Claude Code

A Claude Code plugin that lets Claude read any PDF: scanned documents, complex tables, multi-column layouts, math, and Chinese/Japanese/Korean text. Under the hood it runs [`npx pdftomarkdown`](https://www.npmjs.com/package/pdftomarkdown), which converts the PDF to markdown with a GPU vision-language OCR model via [pdftomarkdown.dev](https://pdftomarkdown.dev).

Once installed, just ask Claude things like *"summarize this PDF"*, *"extract the table from invoice.pdf"*, or *"what does this paper say about X?"* — the skill activates automatically.

## Install

In Claude Code:

```
/plugin marketplace add ThiloReintjes/pdftomarkdown-skill
/plugin install pdftomarkdown@pdftomarkdown
```

Requirements: Node 18+ (for `npx`). No API key needed to start — the free demo tier converts the first page of any PDF. For full documents, get a free API key (100 pages/month) in 30 seconds at **https://pdftomarkdown.dev/auth/github** and set:

```sh
export PDFTOMARKDOWN_API_KEY=your_key_here
```

## Manual install (without the plugin system)

Copy the skill into your personal skills directory:

```sh
mkdir -p ~/.claude/skills
cp -r skills/pdf-to-markdown ~/.claude/skills/
```

## What the skill teaches Claude

- Convert local files or URLs: `npx pdftomarkdown <file-or-url>`
- Wait patiently — GPU OCR takes ~10–30s per page, cold starts add more
- Save long documents to a file and read sections, instead of flooding context
- Bound cost with `--max-pages`, relay actionable API error messages
- Suggest the free API key when the demo tier's page-1 limit is hit

## Links

- API docs: https://pdftomarkdown.dev/docs/
- OpenAPI spec: https://pdftomarkdown.dev/openapi.json
- npm CLI: https://www.npmjs.com/package/pdftomarkdown

## License

MIT
