# Project Charter — ECO 6810 Final Project

> Need the big picture first? Read the [Final Project brief](./FINAL_PROJECT.md) before you fill this out.
>
> **What this is.** Your short approved project plan. It tells me what you are trying to do, what data you will use, what your main metric is, and what a good result would look like.
>
> **What this is not.** A brainstorm or a long proposal. Keep it short, specific, and concrete.
>
> **Why we use it.** It keeps the project focused. Once this is approved, the milestone and the final submission are judged against this plan, not against shifting expectations later.
>
> **How to fill it.** Copy this file. Answer every field. Keep it under two pages. If a field asks for a number, give a real number with a unit.
>
> **Where this lives.** Fill this out inside your team GitHub repo. That repo is where we will review and approve the charter.
>
> **How approval works.** Revise `CHARTER.md` in the repo until it is approved. Do not treat the charter as a separate detached file living somewhere else.
>
> **Simplest editing path.** Open `CHARTER.md` on GitHub, click the pencil icon, edit the file, and commit the change.
>
> **After approval.** One teammate can freeze the approved version as a PDF with:
> `pandoc CHARTER.md -o charter_approved.pdf`
> Then commit that PDF to the repo as the locked approved copy.

---

## Header

| Field | Value |
|---|---|
| Team members | Hasil Tewari, Parmeet Singh Majethiya, Sandipan Ganguly |
| Project type | Predictive |
| Estimated hours per person | 50 |
| Charter version | v1 |
| Date | 2026-04-28 |

**Project type notes.** Predictive = you are trying to forecast or predict a quantity. Causal = you are trying to estimate the effect of a policy or intervention. Descriptive = you are measuring patterns or disparities without making a causal claim. The success threshold looks different for each type, so pick the one that fits your main question.

---

## 1. Problem and stakeholder
The Reserve Bank of India's Monetary Policy Committee sets the repo rate under a 4% CPI inflation target. During the 2022–23 surge, CPI inflation peaked around 7.8% YoY. The MPC's ex-post communication attributed the surge largely to external supply shocks (oil, wheat, fertiliser). We want to quantify, month-by-month, how much of observed CPI inflation over 2015–2025 is statistically attributable to monetary policy variables, external shocks, and domestic demand — as input to a hypothetical policy retrospective an MPC analyst would present to the board. Particular attention is paid to the 2022–23 inflation episode. Prior work (Dua and Goel, 2021) stops in 2017 and therefore omits COVID and the post-2020 inflation dynamics; our contribution is to extend this accounting to the recent period using flexible ML methods.

---

## 2. Main outcome variable
- **Name**: month-over-month CPI inflation, India, all-items
- **Unit**: percentage points (m/m)
- **Source**: MoSPI CPI release, all-India combined series (CPI_C_GEN)
- **Population / panel**: monthly observations, Jan 2015 – Dec 2025 (~132 months); train on Jan 2015 – Dec 2021, evaluate on Jan 2022 – Dec 2025 (48 months held-out)

---

## 3. Main quantitative success threshold

Out-of-sample RMSE on the 2022-01 to 2025-12 held-out slice must be at least 20% lower than the AR(1) baseline RMSE computed on the same slice, with a minimum absolute reduction of 0.05 percentage points per month.

---

## 4. Baseline to beat

The naive or prior number your threshold is measured against. Examples:

- A previous study's coefficient or error.
- A simple AR(1) or last-value forecast.
- An unadjusted before-after difference.

State **what the baseline produces numerically** if you know it, or how you will compute it before the checkpoint if you do not. You must compute the baseline *before* you build anything fancy.

*Write here:*

---

## 5. Falsifiable hypothesis

One sentence the data can prove wrong. A sign, a threshold, or a rank ordering. Not "we will analyse X" — "X will be greater than Y by at least Z".

*Write here:*

---

## 6. Data sources and access plan

All sources are open-access and require no login or API key.

MoSPI CPI (all-India combined, monthly):
Landing: https://www.mospi.gov.in/cpi
Access: downloaded from the eSankhyiki catalogue (CPI all-items, combined series, base 2012=100).
Login required: no.

EPWRF India Time Series — IIP (general index, monthly):
Landing: https://www.epwrfits.in/
Access: downloaded manually via institutional access (monthly general IIP index, base 2011–12 = 100).
Login required: yes (institutional subscription).
Mitigation: data exported as CSV and committed under data/ for reproducibility.

RBI repo rate (monthly end-of-period):
Access: constructed manually from RBI Monetary Policy press releases (https://www.rbi.org.in/Scripts/BS_PressReleaseDisplay.aspx).
Login required: no.

FRED INR/USD exchange rate (monthly):
Endpoint: https://fred.stlouisfed.org/graph/fredgraph.csv?id=EXINUS
Access: direct CSV download via pandas.
Login required: no.

FRED Brent oil price (daily → monthly average):
Endpoint: https://fred.stlouisfed.org/graph/fredgraph.csv?id=DCOILBRENTEU
Access: direct CSV download via pandas, aggregated to monthly frequency.
Login required: no.

(A probe cell for each source is included in notebooks/00_data_probe.ipynb.)

---

## 7. Scope limits

Bullet list of things you are **not** claiming and **not** responsible for. Examples:

- "We will not estimate a structural causal effect of monetary policy."
- "We will not harmonise district boundaries across NFHS rounds; analysis is at state level."
- "We will not ship a mobile version of the app."

This section protects you at grading time. If you clearly say "we are not doing X," you will not be graded on X.

*Write here:*

---

## 8. Risks and fallback

One named failure mode, and the fallback analysis you will run if it materialises. Examples:

- "If the 2022-23 PPAC data is not released by the checkpoint, we will use the FY 2021-22 panel and document the truncation."
- "If DiD parallel-trends fails visually, we fall back to a state-fixed-effects panel regression with year trends and report both."

One risk is enough. Two is fine. Zero means you have not thought hard enough.

*Write here:*

---

## 9. Reproducibility checklist

Your final repo must satisfy all of these:

- [ ] `uv run main.py` runs end-to-end in under 10 minutes on a clean machine with no manual intervention.
- [ ] It writes `outputs/primary_metric.json` containing a single JSON object with at least `{"metric_name": "...", "value": <number>, "threshold": <number>, "passed": <bool>}`.
- [ ] It writes `outputs/baseline_metric.json` in the same shape.
- [ ] A `README.md` documents the commands and expected outputs in ≤ 20 lines.
- [ ] All data sources are either fetched in-script or committed under `data/` with a licence note.

If you cannot commit to this, your project is probably still too broad. Talk to the instructor before proceeding.

---

## Sign-off

By submitting this charter, the team agrees that this is the plan the project will be graded against. The instructor will not penalize you just because the topic turns out to be difficult, as long as the project stays honest and within the approved scope.

*Signed:* _(team member names)_
