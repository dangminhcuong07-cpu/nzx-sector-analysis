# NZX Sector Return Analysis

A research notebook investigating which NZX50 sectors have persistently outperformed the cross-sectional median from 2018 to 2026, and how those sectors respond to RBNZ OCR decisions.

**Author:** Michael Dang · Master of Business Analytics, University of Auckland (Dec 2026)
**Stack:** Python · pandas · yfinance · scipy · matplotlib

---

## What's inside

| File | Purpose |
|---|---|
| `analysis.ipynb` | The full research notebook: 13 cells, executed end to end with outputs embedded |
| `build_notebook.py` | Python script that regenerates `analysis.ipynb`, so the notebook can be version-controlled as plain Python |
| `FINDINGS.md` | Investment-memo-format writeup of results and caveats |
| `outputs/sector_spread_timeseries.png` | Sector rolling 12-month return spread vs median (2019 to 2026) |
| `outputs/persistence_bars.png` | One-sample t-test results for each sector |
| `outputs/ocr_event_study.png` | 30-day forward return by sector around OCR hikes and cuts |

---

## How to run

```bash
# one-time setup
pip install pandas numpy yfinance scipy matplotlib jupyter nbconvert nbformat

# regenerate the notebook from source (optional, already committed)
python build_notebook.py

# execute end to end
python -m jupyter nbconvert --to notebook --execute analysis.ipynb \
    --output analysis.ipynb --ExecutePreprocessor.timeout=300
```

Takes roughly 60 to 90 seconds depending on Yahoo Finance response time.

---

## Key finding (one line)

Utilities, Infrastructure, and Financials have shown persistently positive rolling 12-month return spreads vs the cross-sectional median (p < 0.01 each), but an OCR event study does *not* reproduce the textbook rate-sensitivity pattern, which is a finding about the NZX sample, not a finding about the theory. Full interpretation in `FINDINGS.md`.

---

## Methods used

- Data acquisition: yfinance with the NZX `.NZ` ticker suffix; RBNZ OCR decision dates hand-verified against RBNZ Monetary Policy Statement history and embedded in the notebook, because the RBNZ and FRED endpoints were not reliably accessible from a script
- Time-series analysis: rolling-window returns, cross-sectional spreads, one-sample t-tests
- Event study design: forward-return alignment around policy decision dates, with identification limitations reported
- Research communication: investment memo format, caveats stated upfront, statistical claims qualified by sample size and confounding

---

## Approach to findings

The notebook reports results together with the caveats needed to judge which of them can be relied on. It reports persistence signals for three sectors (Utilities, Infrastructure and Financials), and it reports an OCR event-study result that contradicts the textbook pattern. It then explains why that contradiction is more likely an identification problem than a disproof of the theory. See `FINDINGS.md` for the full caveats.
