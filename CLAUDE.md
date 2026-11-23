# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Request 11/19/2026 — All Tables.Rmd tables for the bolus + HRS-criteria cohort**

Sub-project of the HARMONY Terlipressin Registry. Regenerates every table from
`/Users/to909/Desktop/Tables/Tables.Rmd` on the cohort restricted to
`terli_route_day0 == 1` (bolus initiation) **and** `HRS_Criteria == 1`
(≥1 of criteria 1–4). N = 118 of 243.

Shared context (master dataset path, derived variables, conventions) lives in the parent
`/Users/to909/Desktop/Terlipressin projects/CLAUDE.md` — do not duplicate that content here.

## Repository Layout

- `Results/` — CSV exports of every table
- `docs/` — Quarto site (`analysis.qmd` holds all tables); symlinks
  `redcap_variables.qmd` and `derived_variables.qmd` back to the shared docs in
  `Terlipressin/docs/`. `docs/tables/*.xlsx` (generated at render time, git-ignored)
  are the downloadable Excel files served by the site.
- `Code/` — placeholder for auxiliary scripts (empty)

## Tables

Four `tableone` families (Table 1, follow-up, hospital course, HRS clinical course),
each stratified by `HRS_reversal_new`, `hrs_responders`, `hrs_responders_cat_2`,
`time_to_death_status_90days`, `HRS_criteria_3`, `HRS_criteria_meld`; AE tables
(overall, `HRS_criteria_3`, `HRS_criteria_meld`, `criteria_2`, MELD 3.0 ≥ 35);
and the MAP day 0–3 change table by `hrs_responders_cat_2`.

`make_table()` in `docs/analysis.qmd` reproduces the original `create_table_one()`
print options, writes CSV + XLSX, and records dropped variables / collapsed
stratifiers in the page's Run notes.

## Rendering & Publishing

**Path note:** the folder is named `Request 11/19/2026` in Finder, which is
`Request 11:19:2026` on disk — and Quarto's Sass theme compiler fails on paths
containing `:`. Always render/publish through the colon-free symlink
`../Request-11-19-2026` (created next to the folder):

```bash
quarto render "/Users/to909/Desktop/Terlipressin projects/Request/Request-11-19-2026/docs"
open "/Users/to909/Desktop/Terlipressin projects/Request/Request-11-19-2026/docs/_site/index.html"
```

Publish to GitHub Pages (gh-pages branch of
https://github.com/Tianqi-Ouyang/Terli-Request-11-19-2026):

```bash
cd "/Users/to909/Desktop/Terlipressin projects/Request/Request-11-19-2026/docs" && quarto publish gh-pages --no-prompt --no-browser
```

The repo is public — never commit the master dataset, raw site CSVs, or any other
patient-level file. Only code and aggregate tables belong in git.
