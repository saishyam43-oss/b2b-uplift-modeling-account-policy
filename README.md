<p align="center">
  <img src="images/logo_causalyn.png" alt="Causalyn Logo" width="300"/>
</p>

<h1 align="center">Causalyn — Precision Account Targeting with Causal Uplift</h1>  

> *Preventing Admin Churn While Capturing 80% of Value from 40% of Accounts*

---

## ⚡ Executive Snapshot

**The Problem** Blind nudging generates **~$56K** in short-term value but introduces asymmetric risk by exposing **281 Admins** to unwanted interventions. In B2B, irritating an Admin triggers account-level churn.

**My Solution** I designed a **Precision Targeting Policy** using Causal Uplift Modeling (T-Learner) wrapped in explicit safety guardrails.

**Key Outcomes** - **Safety:** I avoided **100%** of high-risk "Sleeping Dog" accounts (0.00% error rate).
- **Efficiency:** I captured **~80% of total upside** by targeting just the top **40%** of accounts.
- **Decision:** ➡️ **Ship the Precision Policy.** I rejected the blanket nudge approach to protect long-term retention.

---

## 🏢 Company Background

**Causalyn** is a fictional B2B SaaS company that provides a collaboration and workflow platform used by mid-market and enterprise teams.

The product’s success depends on **multi-user adoption within an account**, creating a unique B2B dynamic:
- **Users (High Volume):** Drive daily engagement and usage metrics.
- **Admins (High Leverage):** Control configuration, rollout, and **renewal decisions**.
- **The Asymmetry:** A single negative Admin experience ("Rage Churn") can outweigh dozens of positive User interactions.

*The company frequently uses "Nudges" (in-app messages) to drive feature adoption, but recent feedback suggests these nudges are annoying senior stakeholders.*

---

## 🧠 Executive Summary

### 1. The Strategy: "Risk Filters First"
I started with 2,400 accounts. My policy explicitly eliminated **60%** of the population before optimization began, prioritizing safety (Toxic Admins) and economics (Profitability) over raw lift.

![Policy Funnel](images/01_policy_funnel.png)
* **Mass Suppression:** 281 accounts were removed solely due to Admin risk.
* **Economic Filtering:** 220 accounts were suppressed because the cost of intervention exceeded the expected lift.

### 2. The Trade-off: "Revenue vs. Churn Risk"
My analysis proved that while "Blind Nudging" (targeting everyone) maximizes theoretical revenue ($56k), it risks churning **281 Admins**. I chose to sacrifice ~40% of the potential revenue to reduce this risk to **zero**.

![Risk vs Reward](images/02_risk_vs_reward.png)
* **The Sacrifice:** We forego ~$24k in "risky revenue" to ensure zero Admin churn.
* **The Decision:** The cost of replacing 281 enterprise contracts far exceeds the short-term upsell value.

### 3. The Efficiency: "Diminishing Returns"
I identified that value is highly concentrated. By ranking accounts by **Net Expected Value**, we can capture **80% of the total upside** by targeting just the top **40%** of accounts.

![Budget Efficiency](images/03_budget_efficiency.png)
* **Pareto Principle:** The top deciles provide exponential returns; the bottom deciles provide marginal or negative returns.
* **Budget Cap:** I recommend stopping spend at the 40% mark to maximize ROI.

### 4. The Validation: "Signal Separation"
I validated that the model successfully distinguishes between helpful and harmful interventions. Targeted accounts (Green) show a strong positive uplift distribution, while suppressed accounts (Red) cluster around zero or negative lift.

![Uplift Distribution](images/04_uplift_distribution.png)
* **Clear Separation:** The policy successfully shifts the target population to the right (positive impact).
* **Risk Avoidance:** The suppressed population is left-shifted, confirming they would have dragged down performance.

### 5. The Audit: "Zero Sleeping Dogs"
I audited the policy against a hidden ground-truth dataset. The results confirmed that my guardrails successfully suppressed **100%** of the "Sleeping Dog" segment.

![Safety Audit](images/05_failure_matrix.png)
* **0% Error Rate:** No Sleeping Dogs were targeted.
* **Conservative Bias:** The system favors missing a potential opportunity (False Negative) over causing harm (False Positive).

---

## 📌 Consolidated Insights & Recommendations

### Strategic Findings
1.  **The "Active User" Trap:** High-activity users ($\mu > 90$ percentile) showed a **-4.2% lift**. Traditional propensity models would have targeted them because they are "active," causing active harm. My causal model correctly identified them as "Sleeping Dogs."
2.  **The Admin Multiplier:** Accounts with >2 Admins had a **3x sensitivity** to negative interventions. Aggregating user scores to the account level reversed the decision for **~15% of accounts**, saving us from intervening in accounts with mixed sentiment.
3.  **Cost Sensitivity:** **15%** of positive-lift accounts were actually **ROI Negative** because the expected revenue (~$5) didn't cover the intervention cost ($7). Pure lift optimization would have bled budget here.
4.  **The "Momentum" Signal:** Users showing a **deceleration** in activity (dropping 10-20% WoW) were **2x more persuadable** than stable high-activity users. The model identified "at-risk" engagement as a key opportunity signal.
5.  **Guardrails > Thresholds:** Explicit business rules (e.g., "No Toxic Admins") proved more reliable than probability thresholds alone, reducing the false positive rate on "Sleeping Dogs" from **12%** (raw model) to **0%** (policy).

### Operational Recommendations
1.  **Immediate Action:** Roll out the intervention **only** to the **964 accounts** identified in the final target list.
2.  **CRM Integration:** Tag the 281 "Toxic Admin" accounts as **"Do Not Disturb"** in the CRM to prevent manual CSM outreach for this campaign.
3.  **Phased Rollout:** Do not ship the full list at once. Deploy the top decile first to validate the "Toxic Admin" hypothesis with a smaller blast radius.
4.  **Budget Cap:** Cap the campaign at **40% penetration**. Spending beyond this point yields diminishing returns and begins to tap into "neutral" accounts where ROI is negligible.
5.  **Channel Strategy:** For the **220 accounts** suppressed due to cost (positive lift but < $7 value), switch to a **lower-cost channel** (e.g., automated email vs. CSM call) to restore profitability and recapture that segment.

---

## 🔍 The Challenge: Overcoming Selection Bias

The raw data contained a critical trap: **Selection Bias.**
Historically, CSMs targeted "Active Users" more often. However, these users were often "Sleeping Dogs" (annoyed by interruptions).

* **The Neutrality Gap:** This created a bias where the Treated group appeared to have a **lower average conversion** (-0.04 lift) than Control.
* **The Fix:** I implemented a **Calibrated T-Learner** (Two-Model approach).
* **The Result:** By calibrating the probabilities, I isolated the *incremental* effect of the intervention, separating "likely to buy" (Correlation) from "likely to be persuaded" (Causality).

---

## 🛡️ Policy Design & Safety Mechanisms

Prediction is not a decision. I engineered a policy layer to translate scores into actions.

| Protocol | The Logic | My Implementation |
| :--- | :--- | :--- |
| **Toxic Admin Protocol** | Admins control the contract. | If *any* Admin has negative predicted lift, suppress the *entire* account. |
| **Toxic User Threshold** | Users talk to each other. | Suppress accounts where >10% of users are predicted to react negatively. |
| **Profitability Constraint** | Support costs are real. | Suppress accounts where `(Exp. Revenue - Cost) <= 0`. |

---

## 📂 Project Structure & Code

This repository mirrors a production data science workflow. Click the links to view the source code.

| Directory | Description | Key Files |
| :--- | :--- | :--- |
| [📂 **data**](data/) | Synthetic B2B SaaS data (Users, Activity, Outcomes) | [`documentation/`](data/documentation/) |
| [📂 **src/01_data_generation**](src/01_data_generation/) | Confounding engine and ground truth generation | [`main_generation_pipeline.py`](src/01_data_generation/main_generation_pipeline.py) |
| [📂 **src/02_data_processing**](src/02_data_processing/) | Cleaning and Feature Engineering | [`02_feature_engineering.py`](src/02_data_processing/02_feature_engineering.py) |
| [📂 **src/03_models**](src/03_models/) | T-Learner training & Policy Logic | [`train_uplift_model.py`](src/03_models/train_uplift_model.py), [`account_policy.py`](src/03_models/account_policy.py) |
| [📂 **src/06_visualization**](src/04_visualization/) | Impact Analysis & Chart Generation | [`visualize_impact.py`](src/06_visualization/visualize_impact.py) |
| [📂 **results**](results/) | Final outputs and audit logs | [`final_target_accounts.csv`](results/final_target_accounts.csv) |

---

## ⚠️ Limitations

* **Synthetic Environment:** Designed for decision realism, not statistical benchmarking.
* **Static Policy:** No online learning or adaptive feedback loop.
* **Attribution:** No long-term churn or downstream revenue attribution modeled.

These constraints are intentional to keep the project focused on **decision design**, not platform engineering.

---

## 🔮 What I’d Do Next in Production

1.  **LTV-Weighted Uplift:** Optimize for Lifetime Value rather than one-time activation.
2.  **Online Policy Learning:** Implement a Contextual Bandit to adapt the policy in real-time.
3.  **Experimentation:** Integrate with A/B testing infrastructure to validate the "Toxic Admin" hypothesis in the wild.

---

## 🛠️ Tech Stack

![Python](https://img.shields.io/badge/Python-3.9+-blue?style=flat-square&logo=python)
![Pandas](https://img.shields.io/badge/pandas-Data_Processing-150458?style=flat-square&logo=pandas)
![Scikit-Learn](https://img.shields.io/badge/scikit--learn-Causal_Modeling-F7931E?style=flat-square&logo=scikit-learn)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-11557c?style=flat-square&logo=python)

---

## 📣 Call to Action

This project reflects how I approach **Product Analytics and Decision Science** problems where the goal is not better models, but **better decisions under risk**.
