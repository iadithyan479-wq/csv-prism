# CSV Prism

> Understand a dataset before you model it.

CSV Prism is a privacy-first, browser-only CSV profiler. Drop in a table and get an honest first look at its shape: inferred types, missing values, duplicates, distributions, and a searchable row preview. No upload, account, backend, build step, or dependency is required.

## Features

- Drag-and-drop or file-picker CSV loading
- Clipboard paste support for quick inspections
- Automatic text, number, date, and empty-column inference
- Completeness and duplicate-row checks
- Column dictionary with filled percentage, uniqueness, and examples
- Lightweight categorical value distribution chart
- Searchable preview of the first 100 matching rows
- Sample dataset for an instant demo
- Fully local processing with no telemetry or API calls
- Responsive, dependency-free interface

## Quick start

### Prerequisites

- A modern browser with JavaScript enabled
- Python 3, Node.js, PHP, Ruby, or another static HTTP server
- Git if you want to clone the project

### Clone and run with Python

```bash
git clone https://github.com/iadithyan479-wq/csv-prism.git
cd csv-prism
python3 -m http.server 8000
```

Open [http://localhost:8000](http://localhost:8000).

### Run with Node.js

```bash
cd csv-prism
npx serve .
```

Use the local URL printed by `serve`. The command runs a temporary static server; CSV Prism itself has no npm dependencies.

### Run with other static servers

```bash
# PHP
php -S localhost:8000

# Ruby
ruby -run -e httpd . -p 8000
```

Opening `index.html` directly may work in some browsers, but using a local HTTP server is recommended for consistent behavior.

## Usage

1. Open CSV Prism in your browser.
2. Click **Open CSV**, choose a `.csv` file, and wait for local analysis.
3. Alternatively, drag a CSV onto the drop zone or paste CSV text with `Ctrl+V` / `Cmd+V`.
4. Review the **Overview** tab for dataset health, signals, distributions, and warnings.
5. Open **Columns** to inspect inferred types, completeness, uniqueness, and examples.
6. Open **Rows** to browse the first 100 records and filter them with the search box.
7. Click **Load sample** whenever you want to restore the included example dataset.

### What the health score means

The health score is a quick heuristic, not a statistical quality guarantee. It starts with the percentage of non-empty cells and subtracts a small penalty for exact duplicate rows. Use it as a prompt for investigation rather than as a replacement for domain-specific validation.

### Supported CSV behavior

The parser supports:

- Comma-separated fields
- A header row
- Quoted fields containing commas
- Escaped quotes using doubled quote characters (`""`)
- Windows (`CRLF`) and Unix (`LF`) line endings
- Empty cells

The current parser does not attempt to infer locale-specific separators, repair malformed quoting, or process arbitrary binary spreadsheet formats such as `.xlsx`.

## Privacy and limits

CSV Prism processes data in the browser using the `FileReader` API. It does not send files to a server, persist them to a database, or include analytics. The default page references Google Fonts; remove the font links in `index.html` for a fully offline run.

The whole file is loaded into browser memory. For very large files, memory availability depends on the browser and device. CSV Prism is designed for fast exploratory profiling, not distributed processing or production data quality pipelines. Avoid loading sensitive data on shared or untrusted computers.

## Project structure

```text
csv-prism/
├── index.html          # App structure and controls
├── styles.css          # Visual system and responsive layout
├── app.js              # CSV parser, profiler, views, and interactions
├── sample-orders.csv   # Included demo dataset
├── README.md           # Documentation
└── LICENSE             # MIT license
```

The main flow is:

1. `parseCSV()` converts text into headers and row arrays.
2. `infer()` estimates a type from non-empty values.
3. `profile()` calculates completeness, duplicates, health, and column metadata.
4. The render functions update the Overview, Columns, and Rows views.
5. All state lives in memory and is discarded on refresh.

## Development

CSV Prism has no build tool or dependency installation step. After editing a file, refresh the browser to see changes.

Useful checks:

```bash
# Validate JavaScript syntax
node --check app.js

# Start a local server
python3 -m http.server 8000
```

When changing the parser or profiler, test at least:

- Quoted values containing commas
- Empty cells and entirely empty columns
- Numeric and date-like values
- Duplicate rows
- A file with Windows line endings
- An empty or malformed file
- Mobile-width layout

## Contributing

Contributions are welcome:

1. Fork the repository.
2. Create a focused branch, for example `feat/date-inference`.
3. Make a small, documented change.
4. Run `node --check app.js` and test the app in a browser.
5. Open a pull request describing the user-facing impact.

Please keep the project dependency-free unless a dependency solves a significant problem. Never commit private datasets, credentials, API keys, or generated personal information.

## Roadmap

- [ ] Column sorting and pagination for larger previews
- [ ] Configurable delimiter and quote character
- [ ] Export a profiling report as JSON or Markdown
- [ ] More robust date and boolean inference
- [ ] Optional Web Worker parsing for large files
- [ ] Pluggable data-quality rules
- [ ] Dark theme

## License

CSV Prism is released under the [MIT License](LICENSE).
