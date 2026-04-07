---
name: salesforce-productivity-analyzer
description: >
  Analyze Salesforce sales productivity using a Cause-Effect model:
  Sales Performance = Effort × Quality of Effort × Environment.
  Use this skill whenever the user mentions Salesforce analysis, sales productivity,
  sales performance diagnosis, rep performance, pipeline analysis, win rate,
  revenue per account, sales ROI, compensation analysis, territory analysis,
  management effectiveness, or training impact — even if they don't say "analyze".
  Trigger on: salesperson data, quota attainment, sales metrics, deal analysis,
  sales team benchmarking, or any request to understand why sales are up or down.
---

# Salesforce Productivity Analyzer

Analyzes Salesforce data through a structured Cause-Effect model with a
**sequential diagnostic logic** — always start from the outcome gap and drill
down layer by layer until the root cause is found.

---

## The Core Model

```
Sales Performance = Effort × Quality of Effort × Environment
```

The model is **multiplicative**: a rep with high effort but poor quality still
underperforms. Use the sequential diagnosis below to isolate *which lever is broken*.

---

## Sequential Diagnosis Logic

Work through these five steps in order. **Stop at the first layer where a problem
is confirmed** — that is the primary diagnosis. If the layer looks healthy, proceed
to the next.

---

### Step 1 — 结果差异分析 (Outcome Gap)

**Goal**: Establish whether a gap exists, how large it is, and where it appears.

Key questions:
- Revenue vs quota: attainment % by rep, team, region, product line
- Margin vs target: gross margin trend, discount rate trend
- ROI gap: revenue ÷ total sales cost vs benchmark
- Trend: YoY / MoM / QoQ change — is the gap widening or shrinking?
- Segmentation: which reps, regions, or products are below target?

**Decision**: If no meaningful gap exists → analysis complete. If gap confirmed →
proceed to Step 2.

Output format: summary table of gap by segment + one-sentence gap statement.

---

### Step 2 — Effort 数量分析 (Effort Volume)

**Goal**: Determine if reps are doing enough activity.

Metrics to compute per rep (vs team average or benchmark):

| Metric | Salesforce source | Benchmark |
|---|---|---|
| Calls made | Activity (Type = Call) | Defined by sales ops |
| Meetings held | Event object | — |
| Emails sent | Activity (Type = Email) | — |
| Proposals sent | Opportunity stage / custom field | — |
| Follow-ups | Task object linked to Opportunity | — |
| Avg time in stage | Opportunity: stage entry/exit dates | Historical avg |

**Decision**:
- If activity volume is **below benchmark** → root cause is effort quantity.
  Diagnosis: rep is underworking, disengaged, or mis-allocating time.
- If volume is **at or above benchmark** → proceed to Step 3.

Output format: activity volume table per rep vs benchmark, flagging outliers.

---

### Step 3 — Effort 结构分析 (Effort Structure)

**Goal**: Determine if effort is being directed at the right targets and stages.

Even if volume is sufficient, effort can be misdirected.

Key analyses:
- **ICP vs non-ICP ratio**: what % of activities target ideal customer profiles?
- **Activity type mix**: are reps spending time on high-value activities
  (demos, proposals) vs low-value ones (admin, non-ICP calls)?
- **Pipeline stage distribution**: are activities concentrated at the right stages?
  (e.g. too many early-stage touches, neglecting late-stage deals)
- **Account coverage**: are high-potential accounts receiving sufficient coverage?
- **Time allocation**: hours on selling activities vs non-selling tasks

**Decision**:
- If structure is **misaligned** (wrong accounts, wrong stages, wrong mix) →
  root cause is effort structure. Diagnosis: rep is busy but misdirected.
- If structure is **reasonable** → proceed to Step 4.

Output format: activity mix breakdown + ICP targeting ratio + stage distribution.

---

### Step 4 — Quality of Effort 分析 (Conversion Quality)

**Goal**: Determine if effort is converting efficiently into revenue.

Metrics to compute per rep vs team benchmark:

| Metric | Formula | Notes |
|---|---|---|
| Win rate | Closed Won ÷ (Won + Lost) | Core conversion metric |
| Sales per salesperson | Closed Won Amount ÷ rep count | Productivity benchmark |
| Revenue per account | Total revenue ÷ unique accounts | Account penetration |
| Avg deal size | Closed Won Amount ÷ deal count | Pricing / upsell health |
| Proposal-to-close rate | Closed Won ÷ proposals sent | Late-stage conversion |
| Pipeline velocity | (Deals × Avg size × Win rate) ÷ cycle length | Speed of revenue |
| Profit / margin | (Revenue − Cost) ÷ Revenue | May need ERP data |
| Sales ROI | Revenue ÷ total sales cost | Efficiency of investment |
| Discount rate | Avg discount % on closed deals | Pricing discipline |

**Decision**:
- If conversion metrics are **below benchmark** → root cause is quality of effort.
  Sub-diagnose: which stage has the biggest drop-off? (use funnel analysis)
- If conversion is **healthy** → proceed to Step 5.

Output format: funnel drop-off analysis + conversion rate table per rep vs benchmark.

---

### Step 5 — Environment 分析 (Contextual Factors)

**Goal**: Determine if external or internal conditions are limiting rep performance.

Reached only when effort volume, structure, and quality all look reasonable but
performance is still below target. This layer explains systemic constraints.

| Factor | How to assess | Data source |
|---|---|---|
| Territory / ICP fit | Account quality, industry match, deal potential | Salesforce Account fields |
| Pricing competitiveness | Win/loss reason, discount frequency, competitor mentions | Opportunity reason fields |
| Compensation plan | OTE attainment distribution, commission structure vs market | HR / comp data |
| Management support | Coaching logs, manager-to-rep ratio, pipeline review frequency | Activity / custom fields |
| Training | Ramp time vs benchmark, training completion, tenure vs performance | HR / LMS data |
| Market conditions | External signals — use qualitative input, rarely in Salesforce directly | External sources |

**Decision**:
- Identify which environmental factors are below standard.
- Diagnosis: performance constrained by conditions outside the rep's control.
  Recommended action targets management, HR, or strategy — not the rep.

Output format: environment factor scorecard (1–5 rating with evidence per factor).

---

## Diagnosis Output Template

After completing the sequential analysis, summarize findings as:

```
DIAGNOSIS SUMMARY
─────────────────────────────────────────────────────────
Gap identified:     [outcome gap description]
Root cause layer:   [Effort Volume / Effort Structure / Quality / Environment]
Primary finding:    [one sentence description of the problem]
Supporting data:    [2–3 key metrics that confirm the finding]
Recommended action: [specific, actionable next step]
Secondary findings: [any additional issues found in later layers]
```

---

## Data Input Handling

When the user provides data:

1. **CSV / Excel export**: read all columns, map to the model dimensions above,
   compute available metrics, flag missing data.
2. **Pasted table**: parse as-is, request clarification for ambiguous columns.
3. **Verbal description**: ask for specific numbers needed for Step 1 first,
   then proceed layer by layer.
4. **Partial data**: complete available steps, clearly note which steps were
   skipped due to missing data.

Always state which Salesforce objects or fields are needed for metrics you cannot
compute from the provided data.

---

## Output Format Guidelines

- **Step-by-step**: present one step at a time; confirm findings before proceeding.
- **Tables** for metric comparisons (rep vs benchmark).
- **Plain language diagnosis** after each step — avoid jargon.
- **Visualization**: offer to render charts (funnel, bar comparison, heatmap)
  when data is available.
- **Concise**: lead with the finding, support with data. No filler.
