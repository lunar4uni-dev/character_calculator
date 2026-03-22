# Character Count — Translation Estimate

A browser-based tool for counting source-language characters and generating cost estimates for translation outsourcing projects. Produces structured reports exportable as CSV without requiring any installation or server infrastructure.

---

## Overview

`jap_char_estimate.html` is a self-contained, single-file web application that reads text files (`.md`, `.txt`), counts language-specific characters or words, calculates a translation cost estimate at a configurable unit price, and exports a standardized CSV report.

The tool is intended for use by project managers, localization coordinators, and translation requestors who need to produce accurate, reproducible estimates before placing orders with external translation vendors.

**All file processing occurs entirely within the browser. No file content is transmitted to any external server.**

---

## System Requirements

| Requirement | Detail |
|---|---|
| Browser | Google Chrome 80+, Microsoft Edge 80+, or Mozilla Firefox 78+ |
| Internet | Required at launch (loads Tailwind CSS and Chart.js from CDN) |
| Installation | None — open the HTML file directly in a browser |
| Operating System | Any (Windows, macOS, Linux) |

> **Note:** Internet access is only needed to load the visual stylesheet and chart library on first open. Character counting and CSV export work fully offline once the page has loaded.

---

## Quick Start

1. Open `jap_char_estimate.html` in a browser (double-click the file).
2. Select the source language, upload your files, and set the unit price.
3. Click **Export CSV** to download the estimate report.

---

## Features

- **Multi-language support** — Japanese, Korean, Vietnamese, and English source files
- **Drag-and-drop file upload** — accepts `.md` and `.txt` files; multiple files simultaneously
- **Configurable unit price** — edit the price per character or per word in real time; all estimates update instantly
- **Per-file breakdown table** — shows category-level counts, total count, and individual estimate per file
- **Aggregate summary** — total count and total estimate across all loaded files
- **Stacked bar chart** — visual distribution of character categories per file
- **CSV export** — structured report matching the format of the companion Python scripts, with UTF-8 BOM for correct display in Microsoft Excel
- **Non-destructive language switching** — switching the source language re-analyzes all loaded files without requiring re-upload
- **Duplicate prevention** — uploading a file with a name already loaded is silently ignored
- **In-session file management** — individual files can be removed from the session; **Clear All** resets the workspace

---

## Supported Languages

### Japanese

| Category | Unicode Range | Description |
|---|---|---|
| Hiragana | U+3040 – U+309F | Japanese syllabic script |
| Katakana (full-width) | U+30A0 – U+30FF | Katakana standard block |
| Katakana (half-width) | U+FF65 – U+FF9F | Halfwidth Katakana forms |
| Kanji (CJK Unified) | U+4E00 – U+9FFF | CJK Unified Ideographs |
| Kanji (CJK Ext. A) | U+3400 – U+4DBF | CJK Extension A ideographs |

- **Billable unit:** character
- **Total count:** Hiragana + Katakana + Kanji
- **Estimate formula:** Total characters × Price per character

---

### Korean

| Category | Unicode Range(s) | Description |
|---|---|---|
| Syllables | U+AC00 – U+D7A3 | Hangul syllable blocks (precomposed) |
| Jamo | U+1100 – U+11FF | Hangul Jamo |
| Jamo | U+3130 – U+318F | Hangul Compatibility Jamo |
| Jamo | U+A960 – U+A97F | Hangul Jamo Extended-A |
| Jamo | U+D7B0 – U+D7FF | Hangul Jamo Extended-B |

- **Billable unit:** character
- **Total count:** Syllables + Jamo
- **Estimate formula:** Total characters × Price per character

> In modern Korean text, virtually all characters appear as composed syllable blocks (U+AC00–U+D7A3). Jamo characters are included for completeness.

---

### Vietnamese

| Category | Unicode Range / Rule | Description |
|---|---|---|
| Letters (ASCII) | U+0041–U+005A, U+0061–U+007A | Basic Latin letters A–Z, a–z |
| Diacritics | Any Unicode letter with code point > U+007F | Extended Latin characters carrying tone or vowel diacritics (e.g., à, ắ, ộ, ữ) |
| Numbers | U+0030 – U+0039 | Decimal digits 0–9 |

- **Billable unit:** character
- **Total count:** ASCII Letters + Diacritics + Numbers
- **Estimate formula:** Total characters × Price per character

> Vietnamese is written in Latin script with extensive diacritic usage. The Diacritics category captures all non-ASCII Unicode letters, which in Vietnamese source text are predominantly from the Latin Extended Additional block (U+1E00–U+1EFF) and Latin-1 Supplement.

---

### English

| Category | Counting Rule | Description |
|---|---|---|
| Words | Sequences of one or more Unicode letters (`\p{L}+`) | Whitespace- and punctuation-delimited word count |
| Chars (excl. spaces) | All non-whitespace characters (`\S`) | Raw character count excluding whitespace |

- **Billable unit:** word
- **Total count (billable):** Words
- **Estimate formula:** Total words × Price per word
- **Chart:** Displayed as grouped bars (not stacked), as Words and Chars represent different units

> Word count is the standard billing unit used by translation agencies for English-language source content.

---

## Step-by-Step Usage

### 1. Open the Tool

Double-click `jap_char_estimate.html`. The page loads in the default browser. An internet connection is required on first load to retrieve the CSS and charting libraries.

### 2. Select Source Language

Click one of the four language buttons at the top of the page:

```
🇯🇵 Japanese    🇰🇷 Korean    🇻🇳 Vietnamese    🇺🇸 English
```

The active language is highlighted in indigo. The price field label and unit suffix update to reflect the selected language's billing unit.

### 3. Set Unit Price

The price input in the top-right corner defaults to **₩ 35 per character** (or per word for English). Edit the value to match the agreed vendor rate. All estimates update immediately.

### 4. Upload Files

Either:
- **Drag and drop** one or more `.md` or `.txt` files onto the upload area, or
- **Click** the upload area to open a file browser and select files manually.

Multiple files may be uploaded at once or in multiple batches. Files are read as UTF-8. A file with a filename already present in the current session is skipped.

### 5. Review Results

Once files are loaded, the results section appears with:

- **Summary cards** — total count and per-category subtotals across all files
- **Estimate banner** — total estimated cost with the count and unit price shown for reference
- **Results table** — one row per file showing category breakdown, total count, and individual estimate; a bold total row at the bottom aggregates all files
- **Distribution chart** — stacked bar chart (or grouped for English) showing the character/word category breakdown per file

To remove a single file from the session, click the × icon at the right of its table row.

### 6. Export CSV

Click **Export CSV** to download the report. The file is named:

```
character_count_estimate_{language}.csv
```

For example: `character_count_estimate_japanese.csv`

The file is UTF-8 encoded with a byte order mark (BOM) for correct display in Microsoft Excel.

### 7. Clear Session

Click **Clear All** to remove all loaded files and reset the workspace. The language selection and price setting are preserved.

---

## CSV Output Reference

All exported files include a final **total row** summarizing counts across all files.

### Japanese

| Column | Description |
|---|---|
| `file name` | Filename without extension |
| `Hiragana` | Hiragana character count |
| `Katakana` | Katakana character count (full-width + half-width) |
| `Kanji` | Kanji character count (CJK Unified + Extension A) |
| `count` | Total Japanese character count; `"total"` in the summary row |
| `estimate` | Cost in KRW (formatted as `₩1,234`) |
| `price(₩) / character` | Empty (reserved for template compatibility with Python scripts) |

### Korean

| Column | Description |
|---|---|
| `file name` | Filename without extension |
| `Syllables` | Hangul syllable block count |
| `Jamo` | Hangul jamo character count |
| `count` | Total Korean character count; `"total"` in the summary row |
| `estimate` | Cost in KRW |
| `price(₩) / character` | Empty |

### Vietnamese

| Column | Description |
|---|---|
| `file name` | Filename without extension |
| `Letters (ASCII)` | Basic Latin letter count |
| `Diacritics` | Non-ASCII Unicode letter count |
| `Numbers` | Decimal digit count |
| `count` | Total alphanumeric character count; `"total"` in the summary row |
| `estimate` | Cost in KRW |
| `price(₩) / character` | Empty |

### English

| Column | Description |
|---|---|
| `file name` | Filename without extension |
| `Words` | Word count; `"total"` in the summary row |
| `Chars (excl. spaces)` | Non-whitespace character count; empty in the summary row |
| `estimate` | Cost in KRW |
| `price(₩) / word` | Empty |

### Total Row Format

The final row of every exported CSV summarizes all files:

- **`file name`** column: empty
- **Category columns**: empty (Japanese/Korean/Vietnamese) or `"total"` in Words (English)
- **`count`** column: `"total"` (Japanese/Korean/Vietnamese)
- **`estimate`** column: aggregate cost for all files combined

---

## Technical Notes

### Privacy and Data Handling

All file reading and character counting is performed entirely within the browser using the Web FileReader API. **No file content, metadata, or count results are transmitted to any server.** The tool operates without a backend.

### File Encoding

Input files must be encoded in **UTF-8**. Files in other encodings (e.g., Shift-JIS, EUC-KR) may produce incorrect character counts.

### Duplicate File Handling

If a file with the same filename is added to an existing session, it is silently skipped. To re-process a file (e.g., after modifying it on disk), use **Clear All** and reload.

### Language Switching and Recounting

Switching the source language re-runs the counting logic against all files already in memory using the new language's rules. Files do not need to be re-uploaded.

### Browser Compatibility

The tool requires a browser with ES2018+ support, specifically the Unicode property escape `\p{L}` in regular expressions used for Vietnamese diacritic detection and English word counting.

| Browser | Minimum Version |
|---|---|
| Google Chrome | 64 |
| Microsoft Edge | 79 |
| Mozilla Firefox | 78 |
| Safari | 11.1 |

### External Dependencies (CDN)

| Library | Version | Purpose |
|---|---|---|
| [Tailwind CSS](https://tailwindcss.com) | CDN (latest) | UI layout and styling |
| [Chart.js](https://www.chartjs.org) | 4.4.0 | Distribution bar chart |

Both libraries are loaded from public CDNs at page load. Internet access is required for the initial load. If operating in a restricted network environment, the CDN URLs in the `<head>` section of `jap_char_estimate.html` may be replaced with locally hosted copies.

---

## Known Issues & Planned Improvements

The following issues have been identified in the current version. Each entry describes the problem, its root cause, the scope of impact, and the planned resolution.

---

### Issue 1 — Character Overcounting in Mixed-Content and Technical Documents

#### Vietnamese and English: Embedded Code Is Counted as Source Text

**Problem:** Both Vietnamese and English counting modes do not distinguish between natural-language text and programming code or markup present in the same file. In Vietnamese mode, all ASCII letters (a–z, A–Z) and all non-ASCII Unicode letters are counted — including identifiers, keywords, and variable names from any embedded code. In English mode, all Unicode letter-sequences are counted as words, including code tokens. For technical documents such as `.md` files that contain code examples, this produces a material overcount relative to the actual translatable text volume.

The effect is most pronounced in Vietnamese mode, where "Letters (ASCII)" accumulates every Latin letter regardless of context, causing estimates that can be a significant multiple of the true translatable character count.

**Root cause:** Neither counting function applies any pre-processing to strip Markdown fenced code blocks (` ```...``` `), indented code blocks, or inline backtick spans before iterating over characters. All characters in the raw file are counted uniformly.

**Impact:** Translation cost estimates for technical or mixed-content documents are overstated. The degree of overcount scales with code density in the source file.

**Planned fix:** Pre-process `.md` files to remove fenced code blocks, indented code blocks, and inline code spans prior to character counting. Add an optional UI toggle — "Exclude code blocks" — to allow users to control this behavior, since some projects may legitimately require code translation.

---

#### Japanese + Future Chinese: Shared CJK Unified Ideographs Block

**Problem:** The Unicode blocks used for Japanese Kanji counting — CJK Unified Ideographs (U+4E00–U+9FFF) and CJK Extension A (U+3400–U+4DBF) — are shared across Japanese, Chinese (Simplified and Traditional), and other East Asian writing systems. If a Chinese language mode were added using the same Unicode ranges, Japanese text processed in Chinese mode would be counted as Chinese characters, and vice versa. There is no distinction at the Unicode range level between a character used exclusively in Japanese versus one used exclusively in Chinese.

**Current status:** This issue does not affect the current release because Chinese is not yet a supported language. It is documented here to inform the design of any future Chinese language addition.

**Planned fix:** Adding Chinese support will require a strategy beyond simple code point range comparison. Options under evaluation include character-frequency filtering based on language-specific corpora, or restricting the character set to blocks with less cross-language overlap. Full disambiguation from Unicode ranges alone is not achievable without statistical language detection.

---

#### Korean + Future Hanja

**Problem:** The current Korean counting mode counts only Hangul characters (syllable blocks U+AC00–U+D7A3, and Jamo ranges) and does not count any CJK ideographs. As a result, this issue does not currently affect Korean counts.

However, if a Hanja counting option were added for Korean (Hanja are Chinese-derived characters historically used in Korean writing, falling in the same CJK Unified Ideographs block), the same cross-language overlap described above for Japanese and Chinese would apply to Korean as well.

**Planned fix:** Hanja support for Korean will be deferred until the disambiguation strategy for CJK ranges has been resolved in the context of Chinese language support.

---

### Issue 2 — CSV Export Filename Collision

**Problem:** All CSV exports for a given language are saved under a fixed filename pattern:

```
character_count_estimate_{language}.csv
```

For example: `character_count_estimate_japanese.csv`

If multiple reports are exported within the same browser session or on the same calendar day, the browser either silently overwrites the previous download or appends a numeric suffix (`(1)`, `(2)`, etc.) depending on the browser's download behavior. The filename also provides no information about which source files were analyzed in the session, making it difficult to match a report to its inputs after the fact.

**Impact:** Report provenance is unclear in multi-session or multi-project workflows. Manual file renaming is required to maintain a traceable archive.

**Planned fix:** Append an ISO-format timestamp and an abbreviated identifier derived from the loaded source file names to the export filename. Example output:

```
character_count_estimate_japanese_20260319_143022.csv
character_count_estimate_japanese_20260319_143022_python_beginner_JAP.csv
```

The exact format will be finalized to balance filename length with traceability.

---

### Issue 3 — Currency Fixed to Korean Won (₩)

**Problem:** The price input field, all estimate displays in the UI, and all CSV export values are hardcoded to Korean Won (₩). The currency symbol appears in the column header (`price(₩) / character`), estimate cells (e.g., `₩35,000`), and in the estimate banner. Users who bill in other currencies — such as US Dollars ($), Japanese Yen (¥), or Euros (€) — must manually convert estimates after export. The exported CSV will always display ₩ regardless of the actual currency agreed with the vendor.

**Impact:** The tool is not directly usable for international vendor contracts denominated in currencies other than KRW without post-export manual adjustment.

**Planned fix:** Add a currency selector to the header settings area. Initially planned currencies: KRW (₩), USD ($), JPY (¥), EUR (€). Selecting a currency will update the symbol in all UI elements and CSV output accordingly. Number formatting (decimal separator, thousands separator) will also adapt to the selected locale.

---

### Issue 4 — Fixed Language Set; No User-Defined Language Support

**Problem:** The tool supports exactly four languages — Japanese, Korean, Vietnamese, and English — defined as hardcoded configuration within the application source. Users who need to count characters for other languages (e.g., Simplified Chinese, Traditional Chinese, Thai, Arabic, Hindi, or European languages) cannot do so without directly editing the HTML source file.

**Impact:** The tool cannot be extended to new language pairs without developer intervention, limiting its utility in multilingual outsourcing workflows.

**Planned fix:** Expose a "Custom Language" configuration interface within the UI that allows users to define:
- A language name and display label
- One or more Unicode code point ranges to count
- A billable unit (character or word)
- A label for each counting category

Custom language definitions entered through this interface will be applied immediately and persist within the browser session. A future iteration may support saving custom definitions to browser local storage for reuse across sessions.

---

## File Structure

```
character_calculator/
├── jap_char_estimate.html          # Main tool — open this in a browser
└── README.md                       # This document
```

---

## Dev History
I was originally devloping this with Python, but changed the approach to web-based layouts as it would be prettier. The character-counting logic in `jap_char_estimate.html` is derived from `write_estimate_csv.py` and uses the same Unicode ranges. The Python scripts are not included in Repository.
