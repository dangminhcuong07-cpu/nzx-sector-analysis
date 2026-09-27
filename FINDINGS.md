# NZX Sector Return Analysis — Investment Memo

**Author:** Michael Dang
**Date:** April 2026
**Period analysed:** January 2018 – April 2026
**Repository:** `nzx-sector-analysis/`

---

## Research Question

Which NZX50 sectors have shown persistent return above the benchmark over rolling 12-month windows (2018–2026), and how do RBNZ OCR change events correlate with cross-sectional sector performance?

## Method (one paragraph)

Daily adjusted close for 15 current NZX50 constituents across 8 sectors, fetched from Yahoo Finance. Rolling 12-month total return per ticker, averaged within sector. Spread computed as each sector's rolling return minus the cross-sectional median. Persistence tested via one-sample t-test against zero; event study computed 30-day forward returns by sector around RBNZ OCR hike and cut decisions (12 of each over the sample). OCR decisions were hand-verified from RBNZ MPS release history and committed to the notebook as source data after the public RBNZ endpoints returned HTTP 403 during the analysis run.

---

## Key Findings

### 1. Three sectors have been persistently above the median

| Sector | Mean spread | p-value | Interpretation |
|---|---|---|---|
| **Utilities** | +7.1% | p < 0.01 *** | Most persistent outperformer |
| **Infrastructure** | +6.6% | p < 0.01 *** | Close second — highly persistent |
| **Financials** | +4.9% | p < 0.01 *** | Third, with lower but still statistically strong signal |

Healthcare registers +0.8% with p ≈ 0.05, which in the presence of overlapping rolling windows is not a reliable signal. Consumer is indistinguishable from zero.

### 2. Transport is the consistent underperformer

Transport (driven almost entirely by AIR.NZ) shows a mean spread of **−16.5%**, the largest negative spread in the sample, with t = −38.7 (p < 0.01). The COVID shock is the dominant cause; a broader transport universe would probably reduce but not eliminate the effect.

### 3. The OCR event study produces a result that *does not* match textbook theory — and that's worth stating honestly

Standard rate-sensitivity theory says:
- **Financials should do well after hikes** (higher net interest margin)
- **Utilities and Infrastructure should do poorly after hikes** (long-duration cash flows get discounted harder)

What the data actually shows (average 30-day forward return):

| Sector | After HIKE | After CUT | HIKE − CUT |
|---|---|---|---|
| Financials | −2.2% | **+3.3%** | **−5.5%** |
| Infrastructure | +0.8% | +3.2% | −2.4% |
| Utilities | −0.1% | +1.1% | −1.2% |
| Transport | −3.4% | −0.5% | −3.0% |
| Healthcare | −1.5% | +0.6% | −2.2% |
| Consumer | −2.2% | −0.6% | −1.6% |
| Telecom | +1.2% | −1.9% | +3.1% |
| Materials | −0.7% | −0.5% | −0.2% |

**Financials did better after cuts than after hikes** — the opposite of the net-interest-income story. The most plausible reading is that in the 2018–2026 sample, OCR hikes were concentrated in the 2022 tightening cycle which also coincided with broad equity drawdowns, and OCR cuts were concentrated in COVID 2020 and the 2024–2025 easing cycle, both periods of broad equity rallies. In other words: **the "hike vs cut" variable here is acting as a proxy for the broader macro regime, not as a clean interest-rate shock.**

This is the textbook event-study identification problem. See *Caveats* below.

---

## Business Implication

For a capital markets analyst in Auckland today, three takeaways:

1. **Utilities and Infrastructure have been structurally cheap on a returns basis, not expensive.** The standard assumption "utilities are boring and underperform" is not what the NZX data shows over 2018–2026. Any recommendation that assumes utilities lag should be challenged.

2. **Standard sector rotation rules imported from larger markets should not be applied blindly to NZX.** The sample is too small, sector concentration is too high (1–4 constituents per sector), and idiosyncratic company events dominate sector averages. "Buy financials on hikes" is not a defensible rule in this market.

3. **The cross-sectional median is a useful benchmark.** It is less sensitive to the survivorship bias that plagues the NZX50 index, because it measures *relative* performance within a fixed universe. For internal research, reporting spread-vs-median is more informative than reporting absolute return.

---

## Caveats (read before acting on anything)

- **Survivorship bias.** The 15 tickers used are today's NZX50 constituents. Any company that was dropped from the index because it underperformed is absent. Historical mean spreads are upward biased by this.
- **Tiny sector N.** Telecom = one ticker (SPK). Transport = one ticker (AIR). Consumer = one ticker (ATM). "Sector outperformance" at this sample size is largely company-specific news.
- **Overlapping rolling windows.** The persistence t-statistics are inflated because each observation is not independent. The real effective sample size is roughly 8 years ÷ 1-year window = 8, not the 1,819 rows shown. Read the p-values as "directional" not "definitive."
- **Event-study confounding.** The 2018–2026 OCR hikes cluster in the 2022 tightening; the cuts cluster in COVID 2020 and 2024–2025 easing. Both clusters are correlated with broader equity regimes. The pure interest-rate effect cannot be identified from this data.
- **Total return, not price return.** Using adjusted close (`auto_adjust=True`) bundles dividends into the price. A utilities-vs-financials comparison would benefit from separating the two; that's a next-step extension.

---

## What I'd do differently with a richer dataset

1. **Point-in-time index constituents** from Bloomberg or Refinitiv to eliminate survivorship bias.
2. **OIS surprise at the announcement time** instead of the raw OCR change — the announcement effect is the *surprise*, not the decision, because most of the move is priced in beforehand.
3. **A factor decomposition** (market / size / value / quality) so "sector outperformed" can be split into "sector has more of factor X" vs "sector has idiosyncratic alpha."
4. **Expand to the dual-listed ASX/NZX universe** so every sector has at least five constituents — this alone would remove most of the single-name noise.
5. **Cross-asset context** — layer in NZD/USD, 10-year yields, and global equity beta to check whether the sector effects survive controlling for macro drivers.

---

## Reproducibility

Rerun:
```bash
cd nzx-sector-analysis
python build_notebook.py          # regenerates analysis.ipynb from source
python -m jupyter nbconvert --to notebook --execute analysis.ipynb \
    --output analysis.ipynb --ExecutePreprocessor.timeout=300
```

Outputs:
- `outputs/sector_spread_timeseries.png`
- `outputs/persistence_bars.png`
- `outputs/ocr_event_study.png`

The OCR decisions list is committed inside the notebook (Cell 7 "OCR data load"). The API fallback path is retained as commented-out code for when RBNZ's public pages become accessible again.
