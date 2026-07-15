# CodeChef ZHCET Chapter — Faculty Briefing PDF Builder

![Node.js](https://img.shields.io/badge/Node.js-v18%2B-339933?logo=node.js&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-blue)
![Status](https://img.shields.io/badge/Status-Stable-brightgreen)
![Platform](https://img.shields.io/badge/Platform-macOS%20%7C%20Linux%20%7C%20Windows-lightgrey)

This repository is a self-contained, lightweight tool designed for the **Editorial Lead** of the CodeChef ZHCET Chapter. It allows you to maintain faculty briefing documents in Markdown and automatically compile them into print-ready, professional A4 PDFs.

> [!IMPORTANT]
> **🎉 Congratulations on becoming the Editorial Lead!**
>
> Welcome to the team! You have inherited a key leadership role in managing our chapter's publications, contest editorials, and event reporting. To help you succeed and save you time, this project was built to automate the tedious process of writing, formatting, and rendering faculty briefings. You can focus on the content — the tool handles the typesetting for you.

---

## 📋 Table of Contents

- [Features](#-features)
- [How It Works](#-how-it-works)
- [Getting Started](#-getting-started-cloning--offline-use)
- [Prerequisites](#-prerequisites)
- [File Structure](#-file-structure)
- [Step-by-Step Guide](#-step-by-step-guide-for-the-next-editorial-lead)
- [Supported Markdown](#-supported-markdown-syntax)
- [Minimal Example](#-minimal-markdown-example)
- [Troubleshooting](#-troubleshooting)
- [FAQ](#-faq)
- [Contributing](#-contributing)
- [Security](#-security)
- [License](#-license)

---

## ✨ Features

- **Zero dependencies** — uses only Node.js standard library and your installed Chrome.
- **Markdown as source of truth** — write in plain text, get a professional PDF output.
- **Validation mode** — check your Markdown structure without generating a PDF (`--check` flag).
- **Offline capable** — no internet connection required after initial clone.
- **Cross-platform** — works on macOS, Linux, and Windows (see [Prerequisites](#-prerequisites)).
- **A4 print-ready** — output is correctly sized, paginated, and styled for faculty distribution.

---

## ⚙️ How It Works

Understanding the pipeline helps you debug problems and extend the tool in future years.

```
Your .md file
     │
     ▼
 Custom Markdown Parser (build-brief.mjs)
 • Reads the .md file line by line
 • Extracts title, preamble metadata, and section blocks
 • Handles: headings, paragraphs, bullet lists, tables, bold, italic, inline code, links
     │
     ▼
 HTML Renderer (build-brief.mjs)
 • Converts parsed blocks into structured HTML
 • Injects print-ready CSS (A4 page size, Calibri font stack, styled tables)
 • Writes output as codechef_zhcet_faculty_brief_YYYY_YY.html
     │
     ▼
 Chrome Headless (via execFileSync)
 • Loads the generated HTML file locally
 • Prints it to PDF using --print-to-pdf flag
 • Writes output as codechef_zhcet_faculty_brief_YYYY_YY.pdf
```

> [!NOTE]
> The Markdown parser is **custom-built**, not a general-purpose library. It supports a specific subset of Markdown. See [Supported Markdown Syntax](#-supported-markdown-syntax) for what is and is not supported.

---

## 📥 Getting Started (Cloning & Offline Use)

There are two ways to get this project onto your local system:

### Option A: Clone Using Git (Recommended)

Cloning sets you up for easy updates, tracking changes, and committing edits.

1. **Open your Terminal** (macOS/Linux) or **Command Prompt / PowerShell** (Windows).
2. **Clone the repository:**
   ```bash
   git clone https://github.com/AhmadxFaraz/codechef-brief-builder.git
   ```
3. **Navigate into the project directory:**
   ```bash
   cd codechef-brief-builder
   ```

### Option B: Download as ZIP (No Git Required)

1. Go to the GitHub repository page in your browser.
2. Click the green **Code** button at the top right.
3. Click **Download ZIP**.
4. Extract the downloaded ZIP file to a folder on your system.
5. Open your terminal and navigate to the extracted directory:
   ```bash
   cd /path/to/extracted/codechef-brief-builder
   ```

> [!NOTE]
> Once cloned or downloaded, the project runs **completely offline**. No `npm install` or internet connection is needed to compile your PDFs.

---

## 🛠️ Prerequisites

To use this tool, you need the following installed on your system:

### 1. Node.js (v18 or higher)

Download from [nodejs.org](https://nodejs.org). Verify your version:

```bash
node --version
```

### 2. Google Chrome

Chrome is used headlessly to render and print the A4 PDF. Install it from [google.com/chrome](https://www.google.com/chrome/).

The script looks for Chrome at the following **default paths** per operating system:

| OS | Default Chrome Path |
|---|---|
| macOS | `/Applications/Google Chrome.app/Contents/MacOS/Google Chrome` |
| Linux | `/usr/bin/google-chrome` or `/usr/bin/chromium-browser` |
| Windows | `C:\Program Files\Google\Chrome\Application\chrome.exe` |

> [!WARNING]
> The script's **built-in default is macOS only**. If you are on Linux or Windows, you must set the `CHROME_PATH` environment variable as described in the [Troubleshooting](#-troubleshooting) section before running the build. This is the most common source of errors for non-macOS users.

### 3. Environment Variable (Optional but Recommended)

If your Chrome is not at the macOS default path, set `CHROME_PATH` before running any build command. Examples:

```bash
# macOS / Linux (custom path)
export CHROME_PATH="/usr/bin/google-chrome"

# Windows (Command Prompt)
set CHROME_PATH=C:\Program Files\Google\Chrome\Application\chrome.exe

# Windows (PowerShell)
$env:CHROME_PATH = "C:\Program Files\Google\Chrome\Application\chrome.exe"
```

---

## 📁 File Structure

```
codechef-brief-builder/
├── build-brief.mjs                          # Core build script (parser + renderer + PDF export)
├── package.json                             # npm scripts: build and check
├── codechef_zhcet_faculty_brief_2025_26.md  # Markdown source — Academic Year 2025-26
├── codechef_zhcet_faculty_brief_2024_25.md  # Markdown source — Academic Year 2024-25
├── codechef_zhcet_faculty_brief_2025_26.html  # Generated HTML (do not edit manually)
├── codechef_zhcet_faculty_brief_2024_25.html  # Generated HTML (do not edit manually)
├── codechef_zhcet_faculty_brief_2025_26.pdf   # Generated PDF output
└── codechef_zhcet_faculty_brief_2024_25.pdf   # Generated PDF output
```

- **Edit only the `.md` files.** The `.html` and `.pdf` files are generated outputs and will be overwritten on every build.
- **`build-brief.mjs`** is the only script. It handles parsing, rendering, and PDF export in one file.
- **`package.json`** provides two convenient npm shortcuts: `build` and `check`.

---

## 🚀 Step-by-Step Guide for the Next Editorial Lead

### Step 1: Create or Edit the Markdown Briefing

#### Editing an existing year

Open the relevant `.md` file (e.g., `codechef_zhcet_faculty_brief_2025_26.md`) and update the content.

#### Starting a new academic year (e.g., 2026-27)

> [!IMPORTANT]
> **You must also update `build-brief.mjs` when adding a new year.** The script maintains a list of allowed year keys. Open `build-brief.mjs` and add your new year to the `allowedBriefs` set on **line 9**:
>
> ```js
> // Before
> const allowedBriefs = new Set(['2025_26', '2024_25']);
>
> // After (adding 2026-27)
> const allowedBriefs = new Set(['2026_27', '2025_26', '2024_25']);
> ```
>
> If you skip this step, the build will fail with: `Error: Unsupported brief key "2026_27". Use "2025_26" or "2024_25".`

Once `allowedBriefs` is updated:

1. Copy an existing briefing file:
   ```bash
   cp codechef_zhcet_faculty_brief_2025_26.md codechef_zhcet_faculty_brief_2026_27.md
   ```
2. Edit the new file. Follow the required Markdown structure precisely:
   - **Title line:** Must start with `#` followed by a space. Example:
     ```
     # Brief of Events Organised by CodeChef ZHCET Chapter in Academic Year 2026-27
     ```
   - **Metadata line:** Immediately after the title, one line per field using bold-label format:
     ```
     **Faculty Advisor:** Professor Name Here
     ```
   - **Sections:** Use `##` headings for each section (e.g., `## Session Overview`, `## Summary of Events`, `## Detailed Event Notes`, `## Office Bearers`).
   - **Events:** Under `## Detailed Event Notes`, each event uses a `###` heading, followed by `**Date:**`, `**Type:**`, and `**Audience:**` paragraphs.

---

### Step 2: Build the Briefing (HTML & PDF)

Open your terminal in the project folder and run:

**For the default year (2025-26):**
```bash
npm run build
```

**For a specific year:**
```bash
node build-brief.mjs 2025_26
node build-brief.mjs 2024_25
```

This generates both `.html` and `.pdf` files in the same directory. Example output:

```
build: wrote codechef_zhcet_faculty_brief_2025_26.html and codechef_zhcet_faculty_brief_2025_26.pdf
```

---

### Step 3: Validate the Markdown (Optional but Recommended)

Use validation mode to check that your Markdown parses correctly **without** generating a PDF. This is faster and useful while drafting content.

```bash
# Validate the default year (2025-26)
npm run check

# Validate a specific year
node build-brief.mjs 2025_26 --check
node build-brief.mjs 2024_25 --check
```

Example output on success:

```
check: codechef_zhcet_faculty_brief_2025_26.md parsed and HTML regenerated.
```

If there is a structural error in your Markdown, the script will throw an error message explaining what is missing (e.g., `Missing top-level title`).

---

## ✍️ Supported Markdown Syntax

The parser is **custom-built** and intentionally supports a focused subset of Markdown. Using unsupported syntax will either be ignored silently or cause unexpected output.

### ✅ Supported

| Syntax | Example |
|---|---|
| Level-1 heading (title) | `# Document Title` |
| Level-2 section headings | `## Section Name` |
| Level-3 event headings | `### Event Name` |
| Bold text | `**bold**` |
| Italic text | `*italic*` |
| Inline code | `` `code` `` |
| Hyperlinks | `[label](https://example.com)` |
| Unordered bullet lists | `- Item text` |
| Markdown tables | `\| Col1 \| Col2 \|` with divider row |
| Bold-label metadata lines | `**Label:** Value` |
| Plain paragraphs | Any text not matching the above |

### ❌ Not Supported

- Nested bullet lists (only one level deep)
- Blockquotes (`>`)
- Horizontal rules (`---` outside preamble)
- Raw HTML inside Markdown
- Strikethrough (`~~text~~`)
- Ordered lists (`1. item`)
- Footnotes or references
- Images (`![alt](url)`)

> [!NOTE]
> Hyperlinks only accept `http://` or `https://` URLs. Other protocols are not rendered as links.

---

## 📄 Minimal Markdown Example

The following is the smallest valid Markdown file this tool will accept. Use it as a starting template when creating a new year from scratch.

```markdown
# Brief of Events Organised by CodeChef ZHCET Chapter in Academic Year 2026-27

**Faculty Advisor:** Professor Name Here

## Session Overview

A brief summary of this academic year's activities.

## Summary of Events

| Event | Date | Type | Audience | Key Outcome |
| --- | --- | --- | --- | --- |
| Sample Event | January 2027 | Workshop | All years | Introduced X |

## Detailed Event Notes

### 1. Sample Event
**Date:** January 2027
**Type:** Workshop
**Audience:** All years

Description of the event goes here.

## Office Bearers

- **President:** Name
- **Vice President:** Name
- **Editorial Lead:** Name
```

---

## 🔍 Troubleshooting

### Chrome binary not found

**Error:** `Error: Chrome binary not found: /Applications/Google Chrome.app/...`

**Cause:** The script defaults to the macOS Chrome path. On Linux or Windows, Chrome is installed elsewhere.

**Fix:** Set the `CHROME_PATH` environment variable to your actual Chrome executable:

```bash
# macOS (non-default install)
CHROME_PATH="/path/to/Google Chrome" npm run build

# Linux (common paths)
CHROME_PATH="/usr/bin/google-chrome" npm run build
CHROME_PATH="/usr/bin/chromium-browser" npm run build
CHROME_PATH="/snap/bin/chromium" npm run build

# Windows (Command Prompt)
set CHROME_PATH=C:\Program Files\Google\Chrome\Application\chrome.exe
npm run build

# Windows (PowerShell)
$env:CHROME_PATH="C:\Program Files\Google\Chrome\Application\chrome.exe"
npm run build
```

To find where Chrome is installed on Linux:
```bash
which google-chrome
which chromium-browser
```

---

### Unsupported brief key error

**Error:** `Error: Unsupported brief key "2026_27". Use "2025_26" or "2024_25".`

**Cause:** The year key you passed has not been added to the `allowedBriefs` set in `build-brief.mjs`.

**Fix:** Open `build-brief.mjs`, find line 9, and add your new year key to the set:

```js
const allowedBriefs = new Set(['2026_27', '2025_26', '2024_25']);
```

---

### Missing top-level title error

**Error:** `Error: Missing top-level title. Expected a line starting with "# ".`

**Cause:** Your `.md` file does not have a valid `# Title` as its first non-empty content line, or the `#` is followed by no space.

**Fix:** Make sure line 1 of your `.md` file is exactly:
```markdown
# Brief of Events Organised by CodeChef ZHCET Chapter in Academic Year YYYY-YY
```

---

### Missing section headings error

**Error:** `Error: Missing section headings. Expected at least one "##" heading.`

**Cause:** Your `.md` file has no `## Section` headings after the title and metadata.

**Fix:** Add at least one `## Section Name` line after the `**Faculty Advisor:**` line.

---

### Tables not rendering correctly

**Cause:** The table divider row (the `| --- | --- |` line) is missing or malformed.

**Fix:** Ensure every table has exactly three rows minimum: header, divider, and at least one data row:

```markdown
| Column A | Column B |
| --- | --- |
| Data 1 | Data 2 |
```

---

### PDF output looks different across computers

**Cause:** The CSS font stack uses Calibri as the primary font. Calibri is bundled with Windows and macOS but may not be installed on some Linux systems. The fallback chain is: `Calibri → Aptos → Arial → sans-serif`.

**Fix:** Install the Microsoft core fonts on Linux if exact font match is needed:
```bash
sudo apt install ttf-mscorefonts-installer   # Debian/Ubuntu
```

---

### Node.js version error

**Error:** Syntax errors or `SyntaxError: Cannot use import statement` on Node older than v18.

**Fix:** Upgrade Node.js to v18 or higher. Check your current version:
```bash
node --version
```

---

## ❓ FAQ

**Q: Can I change the PDF styling (fonts, colours, page margins)?**

A: Yes. All styling is inline CSS inside the `renderHtml()` function in `build-brief.mjs`. Look for the `<style>` block starting around line 336. Fonts, colours, and margins are all defined there as CSS custom properties (e.g., `--ink`, `--heading`, `--rule`).

---

**Q: Can I add new sections beyond the defaults?**

A: Yes. Add any `## New Section Name` heading to your `.md` file and the parser will automatically create a `<section>` block for it in the HTML. No code changes needed for new `##` sections.

---

**Q: Why does the script reject my new year key (e.g., `2026_27`)?**

A: The `allowedBriefs` set in `build-brief.mjs` line 9 is hardcoded to known years. Add your new key to that set before building. See [Step 1](#step-1-create-or-edit-the-markdown-briefing) for exact instructions.

---

**Q: What does `npm run check` actually do?**

A: It runs the parser and HTML renderer on your `.md` file but skips the Chrome PDF step. It overwrites the `.html` file but does not produce a new `.pdf`. It is faster and useful for catching Markdown structure errors while drafting.

---

**Q: Can I add images to the PDF?**

A: The custom parser does not support `![alt](url)` image syntax. Images will not render. If you need images, you would need to extend the `renderDocumentBody()` function in `build-brief.mjs` to handle image blocks.

---

**Q: The `.html` file looks different from the `.pdf`. Is that normal?**

A: Slightly. The HTML includes a page shadow and background for browser preview. The PDF strips these via `@media print` CSS rules and renders the raw A4 content. Both are generated from the same source.

---

**Q: Can I run this on a server or in CI/CD?**

A: Yes. Chrome headless works in most CI environments. Ensure Chrome is installed and `CHROME_PATH` is set. For GitHub Actions, use a step like:

```yaml
- name: Install Chrome
  run: sudo apt-get install -y google-chrome-stable

- name: Build PDF
  run: CHROME_PATH=$(which google-chrome) npm run build
```

---

## 🤝 Contributing

Contributions are welcome from Chapter members and future Editorial Leads. To contribute:

1. **Fork** this repository on GitHub.
2. **Create a branch** for your change:
   ```bash
   git checkout -b fix/chrome-path-detection
   ```
3. **Make your changes** and test them locally with `npm run build` and `npm run check`.
4. **Commit** with a clear message:
   ```bash
   git commit -m "fix: auto-detect Chrome path on Linux and Windows"
   ```
5. **Open a Pull Request** against the `main` branch with a description of what you changed and why.

### Suggested Improvements

If you want to contribute but are not sure where to start, these are known areas for improvement:

- Auto-detect Chrome path across macOS, Linux, and Windows without requiring `CHROME_PATH` to be set manually.
- Remove the hardcoded `allowedBriefs` restriction so new year keys are accepted automatically when the corresponding `.md` file exists.
- Add GitHub Actions CI to run `npm run check` on every push and pull request.

---

## 🔒 Security

This tool is intended for **internal use** by the CodeChef ZHCET Chapter editorial team and processes only locally authored Markdown files. There is no network-facing component.

A few notes for awareness:

- **Hyperlink URLs** in Markdown are restricted to `http://` and `https://` protocols by the parser. Other URI schemes (e.g., `javascript:`) are not rendered as links.
- **HTML content** inside `.md` files is escaped before rendering and is not executed. The parser does not support raw HTML in Markdown.
- **Chrome headless** is invoked via `execFileSync` with a fixed path and fixed flags. No user input is passed as shell arguments.

If you discover a security concern, please open a GitHub Issue or contact the repository owner directly.

---

## 📄 License

This project is licensed under the **MIT License**.

```
MIT License

Copyright (c) 2025 AhmadxFaraz

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

*Built with ❤️ for the CodeChef ZHCET Chapter editorial team.*
