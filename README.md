# User Success Prediction 

---

##  Project Overview

This analysis answers a single critical question: **what separates power users from one-time visitors on the AI platform?** Using a full behavioral event log, I engineered predictive features, trained a Random Forest classifier, and segmented users into meaningful cohorts — all to surface actionable growth levers for the AI tool.

---

## 📊 Dataset Statistics

| Metric | Value |
|---|---|
| **Total Users Analysed** | 4,771 |
| **Total Raw Events** | ~409,300 |
| **Unique Event Types** | 141 |
| **Engineered Features** | 148 (142 base + 6 behavioral) |
| **Success Rate (top-20% by activity)** | ~19.7% |
| **Date Range** | Sep 2025 – Dec 2025 |
| **Success Threshold** | ≥ 16 total events |

---

## 🏆 Top 5 Success Predictors

Ranked by Random Forest feature importance (trained on 4,771 users × 148 features):

| Rank | Feature | Importance | Type |
|---|---|---|---|
| 🥇 1 | `diversity_score` — breadth of unique action types | **17.98%** | ★ Engineered |
| 🥈 2 | `avg_events_per_session` — depth per visit | **16.97%** | ★ Engineered |
| 🥉 3 | `agent_tool_call_get_canvas_summary_tool` — AI agent usage | **5.82%** | Raw Event |
| 4 | `agent_tool_call_get_block_tool` — detailed block inspection | **5.58%** | Raw Event |
| 5 | `agent_tool_call_finish_ticket_tool` — task completion via AI | **5.35%** | Raw Event |

> **Key Insight:** The two most powerful predictors are *engineered* behavioral features — not raw event counts. Users who explore broadly (high diversity) and engage deeply per session are the strongest predictors of success, outperforming all 141 raw event signals. AI agent adoption is the dominant raw-event predictor.

---

##  User Segment Breakdown

K-Means clustering (k=5, silhouette score = **0.688**) identified five distinct user personas:

| Segment | Size | % of Users | Profile |
|---|---|---|---|
| 👀 **One-Time Visitors** (×3 clusters) | 4,452 | **93.3%** | Single session, minimal engagement, no AI usage |
| 🔍 **Active Explorers** | 264 | **5.5%** | 2 sessions, moderate diversity (22), some AI messaging |
| ⚡ **Power Builders** | 55 | **1.2%** | 38 sessions, 89 active days, 154 engagement depth, 392 credits used |

> **Power Builders** are the rarest but most valuable cohort: they run 26+ blocks per session, use the AI agent heavily, and drive the majority of platform credit consumption. Active Explorers represent the highest-leverage conversion opportunity.

---

## Model Performance Metrics

Trained on a stratified 80/20 split with 5-fold cross-validation:

| Metric | Score |
|---|---|
| **ROC-AUC (CV Mean)** | **0.9985** ± 0.0007 |
| **Average Precision (CV)** | **0.9942** |
| **F1-Score (CV)** | **0.9452** |
| **Test ROC-AUC** | **0.9992** |
| **Test Average Precision** | **0.9971** |
| **Test Brier Score** | **0.0132** (near-perfect calibration) |
| **Test Precision** | **93.9%** |
| **Test Recall** | **98.4%** |
| **Test F1** | **96.1%** |

> **Model assessment:** Exceptional discriminative power. A ROC-AUC of 0.9992 on held-out data confirms the behavioral signals are highly reliable. The enriched feature set (+6 engineered features) improved AUC by **+0.20%** over the baseline 142-feature model — a meaningful gain given the already-high ceiling.

---

## 💡 Business Recommendations

### 1.  Activate "Breadth-First" Onboarding
Diversity score is the single strongest predictor of success. Users who try ≥ 5 different feature areas in their first session are significantly more likely to become power users. **Recommendation:** Redesign onboarding to nudge new users toward at least 3–5 distinct product areas (canvas, AI agent, deployment, data connections, assets) within the first session via guided checklists or interactive prompts.

### 2. Double Down on AI Agent Adoption as a Growth Lever
Three of the top-5 success predictors are AI agent tool-call events. Users who invoke the agent — especially to inspect canvases (`get_canvas_summary`) and finish tasks (`finish_ticket`) — are disproportionately successful. **Recommendation:** Proactively surface AI agent entry points to users with ≥ 2 sessions but zero agent interactions. A simple "Try the AI agent on your canvas" prompt could unlock significant conversion in the **Active Explorers** cohort (264 users, 5.5% of base).

### 3. Design a Re-Engagement Trigger for the 93% One-Time Visitor Cohort
The majority of users (4,452 / 93.3%) never return after their first visit. `recency_days` and `days_to_first_key_action` are strong model features, confirming time-to-value is critical. **Recommendation:** Implement a 3-day re-engagement email or in-app notification for users who sign up but don't trigger a "key action" (block run, AI message, canvas open) within 24 hours. Capturing even 5% of this cohort into the Active Explorer segment would represent a **~220-user uplift** — a 83% increase in engaged non-power users.

---

## 🔬 Methodology Summary

```
Raw Data (409K events) → Clean & Pivot → User-Level Features (142)
     → Feature Engineering (+6 behavioral) → 148-Feature Matrix
     → Random Forest (CV AUC: 0.9985) → Feature Importance Ranking
     → K-Means Clustering (k=5, Silhouette: 0.688) → Segment Profiles
```

**Tech Stack:** Python · scikit-learn · pandas · matplotlib
