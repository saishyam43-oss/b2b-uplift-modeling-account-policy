<p align="center">
  <img src="images/logo_causalyn.png" alt="Causalyn Logo" width="300"/>
</p>

<h1 align="center">Causalyn — Precision Account Targeting with Causal Uplift</h1>  

> *Preventing Admin Churn While Capturing 80% of Value from 40% of Accounts*

---

## ⚡ Executive Snapshot

**What this is**
- A decision system that determines **which B2B accounts should be nudged and which should be left alone**, using causal uplift modeling and explicit safety guardrails.

**How to read this**
- Skim the visuals and bold insights first.
- Read deeper only if you want to understand *how* the decisions were built and validated.

**Key outcome**
- Blind nudging generates **~$56K** short-term value but risks **churning 281 admins**.
- A precision policy targets **~40% of accounts**, captures **~80% of total value**, and **avoids all high-risk admin interventions**.
- Output is a **production-ready account policy**, not a model score.

➡️ **Decision**: Ship the Precision Policy. Do not ship blanket nudges.

---

## 🏢 Company Background

**Causalyn** is a fictional B2B SaaS company that provides a collaboration and workflow platform used by mid-market and enterprise teams.

The product’s success depends on **multi-user adoption within an account**, where:
- Individual users drive engagement
- Admins and power users control configuration, rollout, and renewal decisions
- A single negative admin experience can outweigh dozens of positive user interactions

The company frequently uses **in-product nudges** (emails, banners, feature prompts) to encourage adoption of collaborative features tied to long-term retention.

---

## 📍 Project Context

Causalyn planned to roll out a new collaboration feature and considered **nudging all active users** to accelerate adoption.

However, prior experiments showed mixed results:
- Some users adopted quickly after nudges
- Others would have adopted anyway
- A non-trivial segment reacted negatively, especially admins and power users

The open question was not:
> “Who is likely to convert?”

But rather:
> **“Who should we intervene on, who should we leave alone, and why?”**

This project was built to answer that question **before shipping a risky, account-wide intervention**.

---

## 👥 Stakeholders & Decisions

**VP Product**
- Decide whether to ship a blanket nudge or a targeted rollout
- Balance short-term activation against long-term churn risk

**Growth / Lifecycle Team**
- Identify which accounts should receive interventions
- Prioritize limited rollout and experimentation budget

**Data / Analytics**
- Provide decision logic that is auditable and defensible
- Explain *why* accounts are targeted or suppressed, not just score them

This project’s output is designed to directly support these decisions with **clear policies**, not opaque model predictions.

---

## 🧠 Executive Summary

### 1️⃣ Policy Funnel — How 2,400 Accounts Became 964 Targets
<p align="center">
  <img src="images/01_policy_funnel.png" width="700"/>
</p>

- 60% of accounts are suppressed *before* value optimization.
- Suppression is driven by **risk and economics**, not model uncertainty.

---

### 2️⃣ Risk vs Reward — Why Blind Nudging Is Dangerous
<p align="center">
  <img src="images/02_risk_vs_reward.png" width="700"/>
</p>

- Blind nudging maximizes short-term revenue.
- It exposes **281 admins** to negative interventions, creating asymmetric downside.

---

### 3️⃣ Budget Efficiency — Where the Value Actually Comes From
<p align="center">
  <img src="images/03_budget_efficiency.png" width="700"/>
</p>

- ~80% of net value comes from ~40% of accounts.
- Enables aggressive budget cuts without proportional value loss.

---

### 4️⃣ Uplift Distribution — Guardrails Are Working
<p align="center">
  <img src="images/04_uplift_distribution.png" width="700"/>
</p>

- Targeted accounts skew strongly positive uplift.
- Suppressed accounts cluster near zero or negative uplift.
- Validation does **not** rely on hidden ground truth.

---

### 5️⃣ Failure Mode Audit — How the System Fails
<p align="center">
  <img src="images/05_failure_matrix.png" width="700"/>
</p>

- **0% of Sleeping Dogs** are targeted.
- Errors bias toward **under-targeting**, not harmful over-targeting.

---

## 📌 Consolidated Insights & Recommendations

### Key Insights

- **Prediction does not equal decision**
  - Many users with high conversion likelihood would have converted without intervention.
  - Intervening on them adds cost without incremental value.

- **Intervention risk is asymmetric in B2B**
  - A single negative admin experience can outweigh dozens of successful user activations.
  - Risk must be explicitly modeled, not inferred from averages.

- **Uplift effects are non-linear**
  - High activity does not imply positive treatment effect.
  - Some highly engaged users (especially admins) are *Sleeping Dogs* who react negatively to nudges.

- **Economic decisions must operate at the account level**
  - User-level uplift signals must be aggregated into account-level value.
  - Optimizing users independently produces misleading results in B2B contexts.

- **Value is heavily front-loaded**
  - A minority of accounts drive the majority of net value.
  - This enables budget-aware rollouts without material loss.

---

### Actionable Recommendations

**1. Replace blanket nudges with a Precision Account Policy**
- Do not nudge all active users.
- Deploy interventions only to accounts that pass **risk, scale, and profitability gates**.

**2. Rank accounts by expected net value, not uplift alone**
- Convert user-level uplift into **expected dollar value**.
- Subtract intervention cost to prioritize economically justified accounts.

**3. Enforce hard suppression rules**
- Suppress any account if:
  - An admin is predicted to experience negative uplift.
  - More than 10% of users show negative treatment effect.
  - The account size is too small to justify intervention cost.

**4. Treat conservatively when uncertain**
- Bias errors toward under-targeting rather than harmful over-targeting.
- Accept missed upside to eliminate churn risk.

**5. Roll out in budget-constrained phases**
- Start with the top ~40% of ranked accounts.
- Capture ~80% of total value while minimizing exposure.
- Expand only if results remain stable.

**6. Operationalize transparency**
- Provide stakeholders with:
  - A final target list
  - Explicit suppression reasons per account
  - Expected value estimates
- Avoid opaque model-only decisions.

---

### Decision Summary

If implemented in production:
- **964 accounts** should be targeted immediately.
- **~60% of accounts** should be intentionally suppressed.
- **281 admin-level churn risks** are avoided by design.
- The system optimizes for **long-term account health**, not short-term activation spikes.

---

## 🛡️ Policy Design

### Insight → Rule Mapping

**Insight: Prediction ≠ Decision**
- High conversion likelihood does not imply positive intervention impact.  
**Policy**: Rank by **uplift (incremental impact)**, not propensity.

**Insight: Risk Is Non-Linear**
- High-activity users can be harmed by nudges.  
**Policy**: Suppress any account with **admin-level negative uplift**.

**Insight: Accounts Are the Economic Unit**
- User-level wins can destroy account-level value.  
**Policy**: Aggregate uplift → **expected net value at account level**.

**Insight: Value Is Front-Loaded**
- Most value comes from a minority of accounts.  
**Policy**: Support phased rollout using **budget efficiency curves**.

---

## 🧪 Modeling & Validation

**The Confounding Problem**
- High-activity users were **more likely to be treated**.
- High-activity users were **more likely to convert anyway**.
- A naive model would overstate treatment impact.

**How This Was Addressed**
- Treatment bias explicitly injected during data generation.
- Uplift modeling used to isolate **incremental effect**, not correlation.
- Validation focused on:
  - Treatment neutrality
  - Directional alignment
  - Failure-mode bias (false positives vs false negatives)

**Why Accuracy Metrics Were Deprioritized**
- AUC measures prediction quality.
- This system is evaluated on **decision quality and safety**.

---

## 🧬 Data Generation & Ground Truth

- Synthetic B2B SaaS data engineered to replicate:
  - Heavy-tailed activity
  - Selection bias in treatment
  - Non-linear treatment effects
- Latent uplift groups used **only for post-hoc audit**, never training.

---

## 📂 Repository Structure

```text
b2b-uplift-modeling-account-policy
│
├── data/
│   ├── documentation/
│   │   ├── cleaning_decisions.md          # Explicit record of what data issues were fixed vs intentionally left untouched
│   │   ├── data_quality_summary.md        # Before/after data quality metrics used to validate cleaning impact
│   │   ├── data_readiness_report.md       # Assessment of whether data is suitable for causal modeling
│   │   ├── generation_report.md           # Design notes and assumptions behind synthetic data generation
│   │   ├── results.txt                    # Sanity-check outputs from validation runs
│   │   └── schema_manifest.txt            # Formal schema definitions for all raw and processed tables
│   │
│   ├── raw/                               # Immutable raw inputs (never modified downstream)
│   │   ├── accounts_raw.csv               # Account-level metadata (plan, size, tier)
│   │   ├── users_raw.csv                  # User-to-account mapping and role information
│   │   ├── user_activity_daily_raw.csv    # Sparse daily activity logs (logins, actions)
│   │   ├── interventions_raw.csv          # Which users were eligible and treated, with timestamps
│   │   ├── outcomes_raw.csv               # Observed outcomes after intervention window
│   │   └── latent_uplift_groups_hidden.csv# Hidden ground truth (used only for audit, never modeling)
│   │
│   ├── processed/
│   │   └── modeling_base_user_level.csv   # Cleaned, temporally valid modeling base table
│   │
│   └── features/
│       └── features_user_level.csv        # Final feature matrix used for uplift modeling
│
├── images/                                # Executive-ready visuals used in README and presentations
│   ├── 01_policy_funnel.png               # Account filtering funnel (risk → scale → value)
│   ├── 02_risk_vs_reward.png              # Blind nudging vs precision policy trade-off
│   ├── 03_budget_efficiency.png           # Cumulative value vs % of accounts targeted
│   ├── 04_uplift_distribution.png         # Predicted uplift split by policy decision
│   ├── 05_failure_matrix.png              # Hidden truth audit of targeting errors
│   └── logo_causalyn.png                  # Project/company logo
│
├── results/                               # Decision artifacts produced by the system
│   ├── user_uplift_scores.csv             # User-level predicted uplift scores
│   ├── account_policy_debug.csv           # Full account-level aggregation and suppression reasons
│   ├── final_target_accounts.csv          # Deployable list of accounts approved for intervention
│   └── failure_mode_analysis.txt          # Explicit accounting of false positives / negatives
│
└── src/
    ├── 01_data_generation/                # Synthetic data generation with causal structure
    │   ├── 01_accounts_users.py            # Generate accounts and users with realistic distributions
    │   ├── 02_generate_user_activity_daily_raw.py  # Create sparse daily activity logs
    │   ├── 03_assign_latent_uplift_groups.py       # Assign hidden uplift archetypes
    │   ├── 04_assign_interventions_raw.py          # Eligibility logic + biased treatment assignment
    │   ├── 05_generate_outcomes_raw.py              # Counterfactual outcome generation
    │   └── validation/
    │       ├── 01_validate_data.py         # Cross-table sanity checks and causal consistency tests
    │       └── 02_record_schema.py          # Schema snapshotting for reproducibility
    │
    ├── 02_data_cleaning/
    │   └── data_cleaning.py                # Cleaning sparse, noisy logs without label leakage
    │
    ├── 03_feature_engineering/
    │   └── feature_engineering.py          # Decision-oriented feature construction (momentum, intensity)
    │
    ├── 04_modeling/
    │   └── train_uplift_model.py            # T-Learner uplift modeling with validation checks
    │
    ├── 05_policy/
    │   └── account_policy.py                # Account-level aggregation, guardrails, and final decisions
    │
    └── 06_visualization/
        └── visualize_impact.py              # Executive visualizations for value, risk, and safety

```

---

## ⚠️ Limitations

- Synthetic environment designed for decision realism, not statistical benchmarking  
- Static policy (no online learning or adaptive feedback loop)  
- No long-term churn or downstream revenue attribution modeled  

These constraints are intentional to keep the project focused on **decision design**, not platform engineering.

---

## 🔮 What I’d Do Next in Production

- Introduce **LTV-weighted uplift** instead of flat value per activation  
- Add **online policy learning** with delayed outcome feedback  
- Extend from single-treatment to **multi-treatment optimization**  
- Integrate with experimentation platforms for controlled rollout  

---

## 🛠️ Tech Stack

Python · pandas · scikit-learn · matplotlib · Git

---

## 📣 Call to Action

This project reflects how I approach **Product Analytics and Decision Science** problems where the goal is not better models, but **better decisions under risk**.
