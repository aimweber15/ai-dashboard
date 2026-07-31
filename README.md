# LMR Rollout — Pilot Dashboard

A self-contained HTML dashboard built for St. Croix Electric Cooperative management to answer one question:

> **Is the contractor's proposal — $625,000 for 2,500 LMRs in 24 months with 2 installers — realistic, measured against our pilot data?**

## Files

| File | Purpose |
|---|---|
| `LMR_Pilot_Dashboard.html` | The dashboard. Open it directly in any browser — no server, no internet connection, no install. |
| `LMR_Pilot_Combined_Dataset_rev2.csv` | Source pilot data (50 installs). **Read-only** — the dashboard embeds a copy of this data at build time; the CSV itself is never modified. |

## How to open it

Double-click `LMR_Pilot_Dashboard.html`, or run:

```bash
start "" "LMR_Pilot_Dashboard.html"
```

Everything — data, styling, logic — is embedded in the single HTML file, so it's safe to email, copy to a USB drive, or drop in a shared folder. It works offline.

## What's on the dashboard

1. **Scenario controls** — adjust installers, proposed timeline, total LMR count, and the LMR type mix to run what-if scenarios. Every other section recalculates live.
2. **Projected outcomes** — completion time, total cost, and a Realistic / At Risk / Not Realistic read on the contractor's proposal, all driven by the pilot's measured performance.
3. **Reconciliation panel** — the core of the dashboard: contractor's proposed timeline/budget/pace shown side by side against the pilot-based projection, plus a reference line for the pilot's actual calendar-throughput pace.
4. **Install accuracy panel** — first-visit and eventual (post-rework) success rates from the pilot. Reacts live to table filters.
5. **Consolidated data table** — all 50 pilot installs, sortable by column and filterable by LMR type, installer, first-visit result, final result, or free-text search.

## Methodology summary

- **Per-installer install rate** is a **labor-hours capacity model**: each installer is assumed to work 40 hrs/week (≈173.9 hrs/month), divided by the pilot's average total labor time per install (install + drive + coordination time, ≈1.90 hrs, averaged across the 48 installs with recorded time) → **≈91.65 installs/installer/month**. This is a theoretical ceiling — full working hours spent installing, no scheduling gaps.
- **Pilot calendar pace** is shown as a **reference line** alongside the capacity rate: what the pilot actually delivered — 50 installs ÷ 2 installers ÷ the pilot's real calendar span (2026-08-04 to 2026-11-09, ≈3.19 months) → **≈7.84 installs/installer/month**. The two rates sit roughly **11.7x apart**, which is the gap between "theoretical capacity" and "what actually happened" — both numbers are shown so that gap stays visible.
- **Cost** is built entirely from the contractor's stated per-unit rate table (LMR-100 $200 … LMR-500 $350) × the LMR mix set in the scenario controls. There is no actual-spend field in the pilot data, so cost is always a roll-up, never an observed figure.
- **Install accuracy**: PILOT-041 and PILOT-047 are excluded from all success-rate math — their final-visit outcome was never recorded in the source data. PILOT-024 is excluded from cost/mix totals only (missing LMR type) but counts normally toward accuracy and the install rate.
- **Realistic / At Risk / Not Realistic**: compares projected timeline to the proposed timeline and projected cost to the proposed budget. Realistic = projected ≤ proposed on both. At Risk = up to 25% over proposed on the worse of the two. Not Realistic = more than 25% over. The underlying ratios are always shown alongside the label so the threshold call can be checked, not just trusted.

Full detail, including exact figures, is in the "Methodology notes" footer at the bottom of the dashboard itself — it's generated live from the embedded data, so it always matches what's on screen.

## Known data caveats

- **PILOT-024**: LMR type not recorded in the source log. Included in install counts and accuracy; excluded from cost/type-mix totals.
- **PILOT-041 / PILOT-047**: final-visit outcome and most time fields were never recorded ("No technician notes row on file"). Excluded from success-rate calculations.
- **PILOT-044**: required a rework revisit (first visit failed, second visit succeeded). Counted once as a single install; both visits' labor hours are summed in the table.
- Two LMR type names were auto-standardized from non-standard source values (PILOT-022, PILOT-033) — flagged in the table's Data Notes column, no numeric impact.

## Status

This is a decision-support tool, not the decision itself. Review the methodology and figures before using them in a real go/no-go call on the contractor's proposal.
