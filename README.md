# NZX Sector Return Analysis

A research notebook investigating which NZX50 sectors have persistently outperformed the cross-sectional median from 2018 to 2026, and how those sectors respond to RBNZ OCR decisions.

**Author:** Michael Dang · MBA Candidate, University of Auckland (Dec 2026)
**Stack:** Python · pandas · yfinance · statsmodels · scipy · matplotlib

---

## What's inside

| File | Purpose |
|---|---|
| `analysis.ipynb` | The full research notebook — 13 cells, executed end-to-end with outputs embedded |
| `build_notebook.py` | Python script that regenerates `analysis.ipynb` — keeps the notebook version-controllable as plain Python |
| `FINDINGS.md` | Investment-memo-format writeup of results and caveats |
| `outputs/sector_spread_timeseries.png` | Sector rolling 12-month return spread vs median (2019–2026) |
| `outputs/persistence_bars.png` | One-sample t-test results for each sector |
| `outputs/ocr_event_study.png` | 30-day forward return by sector around OCR hikes and cuts |

---

## How to run

```bash
# one-time setup
pip install pandas numpy yfinance scipy matplotlib jupyter nbconvert nbformat

# regenerate the notebook from source (optional — already committed)
python build_notebook.py

# execute end-to-end
python -m jupyter nbconvert --to notebook --execute analysis.ipynb \
    --output analysis.ipynb --ExecutePreprocessor.timeout=300
```

Takes roughly 60–90 seconds depending on Yahoo Finance response time.

---

## Key finding (one line)

Utilities, Infrastructure, and Financials have shown persistently positive rolling 12-month return spreads vs the cross-sectional median (p < 0.01 each), but an OCR event study does *not* reproduce the textbook rate-sensitivity pattern — which is a finding about the NZX sample, not a finding about the theory. Full interpretation in `FINDINGS.md`.

---

## Skills demonstrated

- **Data acquisition** — yfinance with NZX ticker quirks handled (`.NZ` suffix); hand-verified macro dataset embedded after public API failures documented as self-improvement events
- **Time-series analysis** — rolling-window returns, cross-sectional spreads, one-sample t-testing
- **Event study design** — forward-return alignment around policy decision dates; honest reporting of identification limitations
- **Research communication** — investment memo format, caveats stated upfront, statistical claims qualified by sample size and confounding

---

## Why it matters for an IB / Markets interview

A capital markets analyst is not paid to produce clean, confident findings. They are paid to produce honest findings with correct caveats, so the senior analyst reading them knows which numbers they can actually use. This notebook follows that rule: it produces defensible persistence signals for three sectors, produces an OCR event-study result that *contradicts* the textbook story, and explains honestly why the contradiction is more likely an identification problem than a disproof of theory.

That's the work. The code just makes the work reproducible.
