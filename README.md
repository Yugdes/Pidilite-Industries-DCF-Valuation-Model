# Pidilite Industries — DCF Valuation Model

A 10-year FCFF discounted cash flow model for **Pidilite Industries Ltd (NSE: PIDILITIND)**, built in Excel, with a peer-regression WACC, a trading-comps cross-check, a live sensitivity table and a reverse-DCF.

> **Valuation date:** 25-Sep-2026 | **Currency:** INR crore unless stated | **Base year:** FY26 (year ended March 2026)

---

## Headline output

| Method | Value / share (₹) | vs. market (₹1,517.20) |
|---|---:|---:|
| **DCF (base case)** | **508** | −67% |
| EV/EBITDA comps — FY26 EBITDA | 1,150 | −24% |
| EV/EBITDA comps — FY27E EBITDA | 1,228 | −19% |
| P/E comps — FY26 PAT | 1,035 | −32% |

| DCF bridge | ₹ Cr |
|---|---:|
| PV of explicit FCFF (FY27E–FY36E) | 21,604 |
| PV of terminal value | 25,975 |
| **Enterprise value** | **47,579** |
| + Cash & current investments | 4,219 |
| − Debt | 106 |
| **Equity value** | **51,692** |

Terminal value is ~55% of enterprise value. The DCF implies ~11.4x FY26 EBITDA, against a peer median of ~27x.

### How to read the gap

The CAPM-based DCF (12.5% WACC) sits well below both the comps range and the market price. This is a finding, not a bug. At base-case cash flows, the market price of ₹1,517 is consistent with a discount rate of roughly 8–9% or a long growth runway, and the reverse-DCF shows it implies ~8.1% terminal growth, above the 7.12% risk-free rate. The DCF is best read as a **conservative floor**, and the comps as the **market-consistent range**. The inputs were deliberately not tuned to match the market price.

---

## Workbook map

| Tab | Purpose |
|---|---|
| **DCF** | 10-year FCFF build (FY27E–FY36E), discounting, terminal value, equity bridge, DCF-vs-comps spread, sensitivity table, reverse-DCF, Revenue/EBITDA chart |
| **RAW FS** | Hard-coded FY22–FY26 consolidated financials (source data) |
| **Data Sheet** | Market data, historical ratios, and every forecast assumption (growth, margin, D&A, capex, NWC, tax, terminal growth) |
| **WACC** | Peer table, beta unlevering/relevering, CAPM cost of equity, WACC build, beta selection switch |
| **Comps** | Peer EV/EBITDA and P/E, applied to Pidilite for an implied value per share |
| **Astral / Supreme / Asian / Berger / Kansai** | One tab per peer: 24-month regression beta vs NIFTY 50, then Blume adjustment |
| **Intrinsic Growth** | Historical ROIC and reinvestment rate, giving fundamental growth (ROIC × reinvestment) as a sanity check on terminal growth |
| **Rm** | Illustrative long-run market-return series (reference for the ERP assumption; not wired into the WACC) |
| **Sources** | Data sources and model conventions |

**Colour convention:** yellow cells are hard-coded inputs, green cells are key outputs.

---

## Methodology

### 1. Free cash flow

```
FCFF = EBIT × (1 − tax) + D&A − Capex − ΔOperating NWC
```

Consolidated financials are used so that the enterprise-value DCF is consistent with the equity bridge.

### 2. Forecast assumptions (10 years, fading)

| Driver | FY27E | FY31E | FY36E | Rationale |
|---|---:|---:|---:|---|
| Revenue growth | 11.0% | 9.5% | 6.0% | Starts at the FY26 actual (11.1%), then fades toward the 5.5% terminal rate |
| EBITDA margin | 27.5% | 26.8% | 26.0% | Partial normalisation from the 28.5% FY26 print, still above the 5-year average (~25.2%) |
| D&A / revenue | 2.7% | 2.7% | 2.7% | FY26 actual level |
| Capex / revenue | 4.0% | 4.0% | 4.0% | In line with recent history |
| NWC / revenue | 8.0% | 8.0% | 8.0% | Near the FY26 level |
| Tax rate | 25.5% | 25.5% | 25.5% | Consistent with the historical effective rate |

A 10-year explicit period is used because Pidilite is a long-runway compounder. A 5-year horizon truncates growth abruptly and pushes too much value into the terminal value.

### 3. WACC (12.5%)

| Input | Value |
|---|---:|
| Risk-free rate (India 10Y G-sec, ~25-Sep-2026) | 7.12% |
| Equity risk premium | 6.0% |
| Selected unlevered beta (peer average) | 0.91 |
| Relevered beta (target D/E 1%) | ~0.91 |
| Cost of equity | 12.6% |
| Pre-tax cost of debt / marginal tax | 8.4% / 25.5% |
| Target weights | 99% equity / 1% debt |
| **WACC** | **12.5%** |

**Peer betas.** Betas are estimated from **24 months of monthly adjusted-close returns (Oct-2024 to Sep-2026) regressed against the NIFTY 50**, using Excel's `SLOPE()`. Each raw beta is Blume-adjusted (75% raw / 25% market beta), unlevered with `βu = βL / [1 + (1−T) × D/E]`, then relevered at the target capital structure.

| Peer | Regression beta | Unlevered (adjusted) |
|---|---:|---:|
| Asian Paints | 1.18 | 1.12 |
| Berger Paints | 1.19 | 1.13 |
| Kansai Nerolac | 1.21 | 1.14 |
| Astral | 0.44 | 0.57 |
| Supreme Industries | 0.42 | 0.56 |

**Beta selection switch (`WACC!I18`).** The peers split into two clusters (paints ~1.1, pipes ~0.57), so the choice of central tendency matters. The switch lets you choose the median (1), the average (2, default) or the paints-only median (3). The average is the default because Pidilite sells into both decorative/DIY and construction channels. The WACC moves by roughly 1.3 percentage points depending on the choice.

### 4. Terminal value

Gordon Growth at **5.5%** (below the 7.12% risk-free rate). Explicit cash flows use **mid-year discounting**, and the terminal value is discounted with the final year's mid-year factor, which is consistent with the mid-year convention.

### 5. Trading comps cross-check

Peer EV/EBITDA and P/E multiples (Asian Paints, Berger, Kansai Nerolac, Astral, Supreme) are applied to Pidilite's FY26 and FY27E figures. Peer market caps are the Sep-2026 close × shares outstanding, and LTM financials are trailing twelve months to Jun-2026. The peer EV/EBITDA range is wide (~12x to ~35x), so the median is a rough anchor rather than a precise target.

### 6. Sensitivity and reverse-DCF

- **Sensitivity table:** implied value per share across WACC (±1.5% steps) and terminal growth (±0.5% steps). Both axes are centred on the live base case, so they update automatically.
- **Reverse-DCF:** solves for the terminal growth rate needed to reconcile the current market price with the explicit-period cash flows.

---

## Model integrity

The workbook recalculates with **0 formula errors across 740 formulas**. The model went through a line-by-line audit; the corrections below are documented for transparency.

- Corrected the historical tax-rate formula (was PBT ÷ EBIT; now (PBT − PAT) ÷ PBT), which had produced tax rates above 100% and negative historical NOPAT.
- Removed 195 `#DIV/0!` errors in the peer-beta tabs by completing them with real price data.
- Linked the WACC beta inputs to the peer tabs instead of duplicating hard-coded values.
- Made WACC weights consistent with the target D/E used to relever beta.
- Aligned the sensitivity table's terminal-value discounting with the main model (mid-year exponent).
- Extended the explicit forecast from 5 to 10 years, and re-centred the sensitivity table on the base case.
- Refreshed stale peer market caps.

---

## Data sources

| Data | Source |
|---|---|
| Pidilite FY26 financials | Pidilite Annual Report FY2025-26 and investor relations filings |
| Peer LTM financials (EBITDA, PAT, EPS, investments) | screener.in, consolidated, TTM to Jun-2026 |
| Peer and NIFTY 50 prices | Yahoo Finance, monthly adjusted close, Oct-2024 to Sep-2026 (raw CSVs in `/data`) |
| India 10Y government bond yield | Public market data, ~7.12% around 25-Sep-2026 |

---

## Limitations

- **Share counts and market caps** for peers are derived (price × shares outstanding), not pulled from a market-data terminal. Refresh before relying on the comps.
- **Beta regressions use only 24 monthly observations**, so standard errors are wide, and the five-peer sample is small and bimodal. The beta switch exists for this reason.
- **The equity risk premium (6.0%) is an assumption** within the commonly cited 5–7% range for India, not a computed figure. The `Rm` tab is an illustrative reference and is not wired into the WACC.
- **Forecast assumptions are analyst judgement**, not management guidance. The margin and growth fades are documented in cell notes on `Data Sheet!H30` and `H31`.
- The stub period is not modelled: the valuation date (Sep-2026) falls partway through FY27, but the model uses standard mid-year exponents starting at 0.5.
- Peer LTM multiples are applied to FY27E EBITDA in one comps variant, which is an indicative rather than like-for-like comparison.

---

## Repository structure

```
pidilite-dcf-valuation-model/
├── README.md
├── model/
│   └── Pidilite_DCF.xlsx
├── data/
│   ├── ASTRAL_NS_Monthly_Historical_Prices_Sep2024-Sep2026.csv
│   ├── SUPREMEIND_NS_Monthly_Historical_Prices_Oct2024-Sep2026.csv
│   ├── ASIANPAINT_NS_Monthly_Historical_Prices_Oct2024-Sep2026.csv
│   ├── BERGEPAINT_NS_Monthly_Historical_Prices_Oct2024-Sep2026.csv
│   ├── KANSAINER_NS_Monthly_Historical_Prices_Oct2024-Sep2026.csv
│   └── NSEI_Monthly_Historical_Prices_Oct2024-Sep2026.csv
└── images/
    └── (screenshots of the DCF, WACC and Comps tabs)
```

## How to use

1. Open `model/Pidilite_DCF.xlsx` in Excel (formulas recalculate automatically).
2. Change assumptions only in **yellow** cells. Forecast drivers are on `Data Sheet` rows 30–35, and terminal growth is `Data Sheet!C39`.
3. Switch the peer-beta method with `WACC!I18` (1 = median, 2 = average, 3 = paints-only).
4. Read results on the `DCF` tab (green cells) and the DCF-vs-comps spread in `DCF!N58:N60`.

---

## Disclaimer

This model is an educational project. It is not investment advice, and nothing here is a recommendation to buy or sell any security. Inputs and outputs reflect the author's assumptions as of the valuation date and may be out of date.

## Author

**Yug** — Mechanical Engineering, IIT Gandhinagar (Class of 2027). Interested in investment banking and financial analytics.
[LinkedIn](#) · [Email](#)
