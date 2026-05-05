# Rocket Fuel: Ad Effectiveness Analysis

**Did TaskaBella's display ad campaign actually drive purchases — or just reach people who were going to buy anyway?**

This project uses a randomized controlled experiment run by Rocket Fuel Inc. to quantify the causal effect of online advertising on conversions, calculate campaign ROI, and identify efficiency opportunities in timing and ad frequency.

---

## TL;DR

The campaign worked. Users shown the TaskaBella handbag ad converted at **2.55%** vs. **1.79%** for the control group — a statistically significant **43% relative lift** (p < 0.001). The campaign generated **$173,719 in incremental revenue** against **$131,375 in ad spend**, for a **32% ROI**. The bigger opportunity: ads are most effective in the **81–120 impression window**, and users receiving **200+ impressions show near-zero lift** — pointing to a clear frequency cap strategy that could sharpen ROI on future campaigns.

---

## Business Problem

TaskaBella Inc., a luxury accessories manufacturer, partnered with Rocket Fuel to run a pilot digital ad campaign for a new handbag. The core challenge was attribution: given a strong social media presence and organic word-of-mouth, how much of any observed purchase activity was actually caused by the ads?

Rocket Fuel addressed this with a **randomized holdout design** — 4% of targeted users were randomly assigned to see a public service announcement (PSA) instead of the real ad, creating a clean control group for causal inference.

---

## Dataset Overview

| Field | Description |
|---|---|
| `user_id` | Unique user identifier |
| `test` | 1 = saw the real ad, 0 = saw PSA (control) |
| `converted` | 1 = purchased the handbag during campaign |
| `tot_impr` | Total impressions served to the user |
| `mode_impr_day` | Day of week with most impressions (1=Mon, 7=Sun) |
| `mode_impr_hour` | Hour of day (0–23) with most impressions |

- **588,101 total users** | **Treatment: 564,577** | **Control: 23,524**
- Campaign period: November 2015 – February 2016
- Overall conversion rate: ~2.5% (expected for display advertising)
- Impression distribution is heavily right-skewed — median 13, mean 25, max 2,065

---

## Methodology

**Causal identification:** The experiment design (random assignment to test vs. control) supports causal inference. Randomization was verified via a t-test on impression counts across groups — no statistically significant difference (p = 0.83), confirming balance.

**Treatment effect estimation:**
- Two-sample t-test on conversion rates (difference in proportions)
- Logistic regression with `test` as the binary predictor — consistent with t-test directionally, adds log-odds interpretation and extensibility to subgroup models

**Subgroup analyses:**
- Day-of-week effectiveness: t-test and logistic regression per day
- Hour-of-day effectiveness: t-test per hour (hours with < 15 control users excluded)
- Frequency analysis: users binned into 6 impression bands ([0–40], [41–80], [81–120], [121–160], [161–200], [200+]); t-test per bin

**Business translation:** Lift multiplied by treatment group size to estimate incremental conversions; multiplied by $40 margin to get incremental revenue. Total impressions × $9 CPM for cost.

---

## Key Findings

**Treatment effect**
- Absolute lift: **+0.77 percentage points**
- Relative lift: **+43%**
- Logistic regression coefficient: **0.366** (log-odds), odds ratio **~1.44**
- Both methods agree: the ad caused a significant, measurable increase in purchases

**Campaign economics**
- Total impressions: 14.6M | Ad cost: $131,375
- Incremental conversions: ~4,343 | Incremental revenue: $173,719
- Net profit: $42,344 | **ROI: 32%**
- Opportunity cost of control group (foregone conversions): ~$6,720

**Timing**
- Most effective days: **Mon, Tue, Wed** (Tue has highest relative lift at ~111%)
- Least effective: **Thu and Sun** (not statistically significant)
- Most effective hours: **9am–2pm** and a secondary spike at **8pm**
- Afternoon/evening hours (3pm–7pm) show weaker, mostly non-significant lift

**Ad frequency**
- **Sweet spot: 81–120 impressions** → 9.26 pp absolute lift (127% relative), p < 0.001
- 41–80 impressions: solid secondary band (3.79 pp lift, 81% relative)
- **200+ impressions: near-zero lift** (0.07 pp, p = 0.97) — complete ad saturation
- 84% of users fall in the 0–40 bin where no significant lift was detected — largest untapped volume segment

---

## Strategic Recommendations

1. **Implement a frequency cap at ~120 impressions.** Above that, spend has no measurable return.
2. **Reallocate budget from 200+ segment to expand reach** among the 0–40 group — but with care, as low-frequency users have lower baseline intent.
3. **Concentrate spend on Mon–Wed and the 9am–2pm window** for maximum conversion impact.
4. **Design the next campaign as a frequency randomization experiment** — randomly assign users to impression caps (40 / 80 / 120 / 200) to get clean causal estimates of the optimal frequency, removing browsing-intensity confounding.

---

## Tools & Libraries

| Tool | Use |
|---|---|
| Python 3 | Core language |
| pandas | Data manipulation |
| numpy | Numerical operations |
| scipy.stats | t-tests |
| statsmodels | Logistic regression |
| matplotlib / seaborn | Visualization |

---

## How to Run

```bash
# Clone the repo
git clone https://github.com/DanielSantiagoGuzman/rocketfuel-ad-effectiveness-analysis.git
cd rocketfuel-ad-effectiveness-analysis

# Install dependencies
pip install -r requirements.txt

# Launch the notebook
jupyter notebook notebooks/rocketfuel_analysis.ipynb
```

The notebook is fully self-contained — all outputs are pre-rendered inline. No additional data downloads required.

---

---

## Dataset Availability

The dataset used in this analysis (`rocketfuel.csv`) is not included in this repository. It was distributed as part of a proprietary case study and cannot be shared per course policy.

**Source:** Berkeley-Haas Case Series — Katona & Bell (2017), *"Rocket Fuel: Measuring the Effectiveness of Online Advertising"* (Case B5894). Published by the Haas School of Business, University of California, Berkeley. Dataset anonymized; certain figures altered per case study guidelines.
