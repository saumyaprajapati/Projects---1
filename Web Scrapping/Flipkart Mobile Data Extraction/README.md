<p align="center">
  <h1 align="center">📱 Flipkart Mobile Data Extraction</h1>
</p>

<p align="center">
  <b>A Python web scraping project that extracts smartphone listings from a saved Flipkart HTML page into a clean, analysis-ready CSV.</b><br>
  HTML Parsing · BeautifulSoup Scraping · Data Cleaning · Pandas Export
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python"/>
  <img src="https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white" alt="Pandas"/>
  <img src="https://img.shields.io/badge/BeautifulSoup-43B02A?style=for-the-badge&logo=python&logoColor=white" alt="BeautifulSoup"/>
  <img src="https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white" alt="Jupyter"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Records-Under%2050-blue?style=flat" alt="Records"/>
  <img src="https://img.shields.io/badge/Columns-8-blue?style=flat" alt="Columns"/>
  <img src="https://img.shields.io/badge/Source-Flipkart-orange?style=flat" alt="Source"/>
</p>

<p align="center">
  <img src="https://img.shields.io/github/last-commit/saumyaprajapati/Projects---1?style=flat&color=green" alt="Last Commit"/>
  <img src="https://img.shields.io/github/repo-size/saumyaprajapati/Projects---1?style=flat" alt="Repo Size"/>
  <img src="https://img.shields.io/github/stars/saumyaprajapati/Projects---1?style=flat" alt="Stars"/>
  <img src="https://img.shields.io/github/forks/saumyaprajapati/Projects---1?style=flat" alt="Forks"/>
</p>

---

## Table of Contents

- [Quick Start](#quick-start)
- [Project Overview](#project-overview)
- [Extraction Workflow](#extraction-workflow)
- [Scraping Logic](#scraping-logic)
- [Dataset Schema](#dataset-schema)
- [Data Cleaning Steps](#data-cleaning-steps)
- [Tech Stack](#tech-stack)
- [Repository Structure](#repository-structure)
- [Data Source](#data-source)
- [Installation Guide](#installation-guide)
- [How to Run the Notebook](#how-to-run-the-notebook)
- [Sample Output](#sample-output)
- [Key Code Snippets](#key-code-snippets)
- [Troubleshooting](#troubleshooting)
- [Limitations](#limitations)
- [Future Improvements](#future-improvements)
- [Contributing](#contributing)
- [Ethical Note on Scraping](#ethical-note-on-scraping)
- [Author](#author)
- [Acknowledgments](#acknowledgments)
- [Footer](#footer)

---

## Quick Start

**Prerequisites:** Python 3.9+, Jupyter Notebook or JupyterLab, ~50 MB free disk space.

```bash
git clone https://github.com/saumyaprajapati/Projects---1.git
cd "Projects---1/Web Scrapping/Flipkart Mobile Data Extraction"
pip install pandas beautifulsoup4 lxml
jupyter notebook Flipkart_SmartPhone_Extraction.ipynb
```

The notebook is self-contained — run all cells in order, and `SmartPhone.csv` will be regenerated in the same folder using `Flipkart_Smartphone.html` as the input source.

---

## Project Overview

**What is this project?**
This project parses a locally saved Flipkart "Mobiles" listing page (saved via the SingleFile browser extension) and extracts structured smartphone data — name, rating, storage, display size, and pricing — into a single CSV file. It demonstrates a complete offline web scraping pipeline: from raw HTML to a clean tabular dataset ready for analysis.

**Problem Statement**

- How do smartphone prices vary across listed models?
- Which phones offer the best discount relative to their original price?
- Is there a relationship between storage (ROM) and price?
- How are customer ratings distributed across different listings?

**Key Metrics at a Glance**

| Metric                     | Value                                    |
| -------------------------- | ---------------------------------------- |
| 📦 Total Records Extracted | Under 50                                 |
| 📊 Columns per Record      | 8                                        |
| 🌐 Source                  | Flipkart.com (Apple Smartphones listing) |
| 🧰 Core Libraries          | pandas, BeautifulSoup4                   |
| 📁 Output Format           | CSV                                      |
| 🗂️ Input Format            | Saved HTML (SingleFile)                  |

**Why Python for this?**

- BeautifulSoup parses nested, inconsistent HTML far more reliably than manual string searching.
- Pandas converts parsed lists/dictionaries into a structured DataFrame and handles type casting in one step.
- A Jupyter Notebook keeps each scraping stage (fetch → parse → clean → export) visible and independently re-runnable.
- The same script can be repointed at any newly saved Flipkart category page with minimal changes.
- Python's text-processing tools (regex, string methods) make cleaning price and rating strings straightforward.

---

## Extraction Workflow

```
Flipkart_Smartphone.html  →  BeautifulSoup Parser  →  Raw Field Lists
        →  Type Cleaning & Casting  →  pandas.DataFrame  →  SmartPhone.csv
```

The notebook works in four stages: it loads the saved HTML file into BeautifulSoup, locates each product card on the page, pulls the relevant text fields out of each card, and finally assembles everything into a DataFrame that gets written to CSV.

---

## Scraping Logic

The source file, `Flipkart_Smartphone.html`, is a page saved with the **SingleFile** browser extension from a Flipkart Apple Smartphones listing URL, with all images and styles inlined so the page renders identically offline.

The extraction follows this approach inside the notebook:

1. **Load and parse** — the HTML file is read and passed to `BeautifulSoup` with the `lxml` (or `html.parser`) parser.
2. **Locate product cards** — each smartphone listing on the page is wrapped in a repeating container `div`. The notebook selects all matching containers using `find_all()` with the relevant class name.
3. **Extract per-card fields** — for every product card, the notebook pulls out the title text, star rating, review count, and price block using nested `find()` calls scoped to that card, so a missing field on one card does not break extraction for the rest.
4. **Parse compound strings** — fields like the product title (which embeds ROM and display size, e.g. _"... 128 GB) ... 6.7 inch ..."_) are split using string slicing and regex to pull out ROM and display size separately.
5. **Append to lists** — each cleaned value is appended to a column-specific Python list, keeping all lists aligned by index across cards.
6. **Build the DataFrame** — the aligned lists are combined into a single `pandas.DataFrame` with the eight target columns.

---

## Dataset Schema

`SmartPhone.csv` contains one row per smartphone listing and the following columns:

| Column         | Data Type | Description                            | Example                          |
| -------------- | --------- | -------------------------------------- | -------------------------------- |
| `Name`         | string    | Full smartphone name/title as listed   | `Apple iPhone 14 (Blue, 128 GB)` |
| `Star`         | float     | Average star rating out of 5           | `4.6`                            |
| `Rating`       | int       | Number of ratings/reviews received     | `16639`                          |
| `ROM`          | int       | Internal storage in GB                 | `128`                            |
| `Display_inch` | float     | Screen size in inches                  | `6.7`                            |
| `New_price`    | int       | Current selling price (₹)              | `9999`                           |
| `old_price`    | float     | Original/MRP price before discount (₹) | `11999.0`                        |
| `Discount`     | float     | Discount percentage applied            | `16.0`                           |

---

## Data Cleaning Steps

The following transformations are applied before the data is written to CSV:

- **Rating string → int**: raw rating strings contain comma separators and the word "ratings" (e.g. `16,639 Ratings`); commas are stripped and the numeric portion is cast to `int`.
- **Price strings → numeric**: prices scraped with a `₹` symbol and thousands separators (e.g. `₹9,999`) have the symbol and commas removed before casting to `int`/`float`.
- **Star rating → float**: the rating value is extracted as plain text and cast directly to `float`.
- **ROM and Display size extraction**: these values are embedded inside the product title string and are isolated using string splitting/regex before being cast to `int` and `float` respectively.
- **Discount percentage → float**: the discount text (e.g. `16% off`) has the `% off` suffix stripped, leaving a clean float value.
- **Missing fields**: cards missing a particular field (e.g. no rating yet) are handled defensively so the scraper does not halt on a single incomplete listing.

---

## Tech Stack

| Tool             | Version | Purpose                                           |
| ---------------- | ------- | ------------------------------------------------- |
| Python           | 3.9+    | Core scripting language                           |
| BeautifulSoup4   | 4.x     | HTML parsing and DOM traversal                    |
| pandas           | 2.x     | Data structuring, cleaning, and CSV export        |
| lxml             | 4.x     | Fast HTML parser backend for BeautifulSoup        |
| Jupyter Notebook | latest  | Interactive development and execution environment |

---

## Repository Structure

```
Web Scrapping/
├── .venv/                                  ← local virtual environment (NOT pushed to GitHub)
└── Flipkart Mobile Data Extraction/
    ├── Flipkart_SmartPhone_Extraction.ipynb  ← main notebook: scraping + cleaning + export
    ├── Flipkart_Smartphone.html               ← saved Flipkart page (scraping source)
    └── SmartPhone.csv                         ← final extracted dataset (output)
```

| File                                   | Description                                                                                                                                                                                               |
| -------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Flipkart_SmartPhone_Extraction.ipynb` | The complete pipeline — loads the HTML, scrapes product fields with BeautifulSoup, cleans each column, and exports the final DataFrame to `SmartPhone.csv`. This is the only script that needs to be run. |
| `Flipkart_Smartphone.html`             | The raw input — a Flipkart Apple Smartphones listing page saved offline with the SingleFile browser extension.                                                                                            |
| `SmartPhone.csv`                       | The structured output dataset, ready for analysis in pandas, Excel, or any BI tool.                                                                                                                       |

> ⚠️ **Note:** the `.venv` folder holds the local Python virtual environment and should be excluded from version control. Add it to `.gitignore` before pushing (see [Installation Guide](#installation-guide)).

---

## Data Source

| Source                                                    | Records  | Key Columns                                                           |
| --------------------------------------------------------- | -------- | --------------------------------------------------------------------- |
| Flipkart.com — Mobiles listing (Apple Smartphones filter) | Under 50 | Name, Star, Rating, ROM, Display_inch, New_price, old_price, Discount |

The page was captured as a static HTML snapshot rather than scraped live, which means the dataset reflects pricing and ratings at the time the page was saved, not real-time values.

---

## Installation Guide

**Prerequisites**

| Requirement                   | Version                 | Notes                                            |
| ----------------------------- | ----------------------- | ------------------------------------------------ |
| Python                        | 3.9 or higher           | Required for f-strings and modern pandas support |
| pip                           | latest                  | Comes bundled with Python                        |
| Jupyter Notebook / JupyterLab | latest                  | To run the `.ipynb` file                         |
| OS                            | Windows / macOS / Linux | No OS-specific dependencies                      |

**Steps**

1. Clone the repository:

   ```bash
   git clone https://github.com/saumyaprajapati/Projects---1.git
   cd "Projects---1/Web Scrapping/Flipkart Mobile Data Extraction"
   ```

2. Create and activate a virtual environment:

   ```bash
   python -m venv .venv
   # Windows
   .venv\Scripts\activate
   # macOS/Linux
   source .venv/bin/activate
   ```

3. Install dependencies:

   ```bash
   pip install pandas beautifulsoup4 lxml jupyter
   ```

4. Exclude the virtual environment from GitHub by adding this to a `.gitignore` file at the repo root:
   ```bash
   echo ".venv/" >> .gitignore
   ```

---

## How to Run the Notebook

1. Launch Jupyter from the project folder:
   ```bash
   jupyter notebook Flipkart_SmartPhone_Extraction.ipynb
   ```
2. Run all cells in order (**Cell → Run All**). The notebook reads `Flipkart_Smartphone.html` from the same folder — no internet connection is required since the page is already saved locally.
3. Confirm `SmartPhone.csv` is created or overwritten in the same directory once the final export cell completes.
4. Open `SmartPhone.csv` in pandas, Excel, or any spreadsheet tool to verify the eight expected columns are populated correctly.

---

## Sample Output

| Name                           | Star | Rating | ROM | Display_inch | New_price | old_price | Discount |
| ------------------------------ | ---- | ------ | --- | ------------ | --------- | --------- | -------- |
| Apple iPhone 14 (Blue, 128 GB) | 4.6  | 16639  | 128 | 6.7          | 9999      | 11999.0   | 16.0     |

_(Illustrative row showing the expected structure and types of `SmartPhone.csv`.)_

---

## Troubleshooting

| Problem                                       | Quick Fix                                                                                                                                                    |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `FileNotFoundError` on the HTML file          | Confirm the notebook's working directory matches the folder containing `Flipkart_Smartphone.html`; use an absolute path if needed.                           |
| `find_all()` returns an empty list            | Flipkart periodically changes its CSS class names — open the HTML file in a browser, inspect a product card, and update the class name used in the notebook. |
| `ValueError` when casting price/rating to int | A card had missing or malformed text (e.g. "Out of Stock"); add a try/except or conditional check before casting.                                            |
| ROM or Display size missing from a row        | The title string format for that listing differs from the expected pattern; check the regex/split logic against that specific title.                         |
| `ModuleNotFoundError: No module named 'bs4'`  | Activate the virtual environment and run `pip install beautifulsoup4`.                                                                                       |
| CSV opens with garbled characters in Excel    | Open `SmartPhone.csv` specifying UTF-8 encoding, or use `df.to_csv(..., encoding="utf-8-sig")` when exporting.                                               |

---

## Limitations

- The dataset is a one-time snapshot of a saved HTML page, not a live or scheduled scrape, so prices and ratings will go stale over time.
- Only Apple smartphone listings were captured in this snapshot; other brands are not represented.
- The scraper is tied to Flipkart's current HTML structure — class names embedded in `Flipkart_Smartphone.html` may break if Flipkart redesigns its listing page in the future.
- With under 50 rows, the dataset is best suited for learning/demo purposes rather than large-scale market analysis.

---

## Future Improvements

**Planned**

- [ ] Add a live scraping mode using `requests` + rotating headers instead of relying on a pre-saved HTML file
- [ ] Extend extraction to additional brands beyond Apple
- [ ] Add unit tests for each cleaning function (`clean_price`, `clean_rating_count`, etc.)
- [ ] Add data validation checks (e.g. `New_price < old_price`) before export
- [ ] Generate basic exploratory charts (price distribution, discount vs. rating) in the same notebook

**Under Consideration**

- [ ] Schedule periodic re-scrapes and track price history over time
- [ ] Store output in SQLite instead of/in addition to CSV
- [ ] Convert the notebook logic into a reusable `.py` module with a CLI
- [ ] Add proxy/rate-limiting support for a future live-scraping version

---

## Contributing

1. Fork the repository and create a feature branch:
   ```bash
   git checkout -b feature/your-feature-name
   ```
2. Make your changes and commit using conventional commit messages:
   ```bash
   git commit -m "feat: add live scraping mode for additional brands"
   git commit -m "fix: handle missing rating field in product card"
   git commit -m "docs: update README with new column description"
   ```
3. Push your branch and open a pull request:
   ```bash
   git push origin feature/your-feature-name
   ```

**Data contribution requirements:**

- Any new saved HTML page must follow the same SingleFile export format used in this project.
- New columns added to the CSV must be documented in the [Dataset Schema](#dataset-schema) table.
- Cleaning functions for new fields must handle missing/malformed values gracefully.

---

## Ethical Note on Scraping

This project scrapes a single, manually saved HTML snapshot rather than making repeated live requests to Flipkart's servers, which keeps it low-impact and offline-friendly. If extending this into a live scraper, be sure to check Flipkart's `robots.txt` and terms of service, add reasonable request delays, and avoid scraping at a volume or frequency that could affect site performance for other users.

---

## Author

**Saumya Prajapati**

[![GitHub](https://img.shields.io/badge/GitHub-saumyaprajapati-181717?style=flat&logo=github)](https://github.com/saumyaprajapati)

---

## Acknowledgments

- Data sourced from publicly viewable listings on [Flipkart.com](https://www.flipkart.com).
- Page captured offline using the **SingleFile** browser extension.
- Built with the **BeautifulSoup** and **pandas** open-source libraries.

---

<p align="center">⭐ If you found this useful, please give it a star!</p>

<p align="center">
  <a href="https://github.com/saumyaprajapati/Projects---1/issues/new">Report Bug</a> ·
  <a href="https://github.com/saumyaprajapati/Projects---1/issues/new">Request Feature</a>
</p>
