# Salesforce Productivity Analyzer — Claude Skill

Diagnoses why a sales team is underperforming using a structured cause-effect model. Works through five diagnostic layers in sequence — stopping at the first layer where a problem is confirmed — and produces a clear finding with a recommended action.

---

## The model

```
Sales Performance = Effort x Quality of Effort x Environment
```

The model is multiplicative: a rep with high effort but poor conversion quality still underperforms. The skill isolates which lever is broken by working through layers in order.

---

## Diagnostic layers

| Layer | What it checks | Root cause if broken |
|---|---|---|
| 1. Outcome gap | Revenue vs quota, margin vs target, ROI by rep/region | Confirms a gap exists and where |
| 2. Effort volume | Calls, meetings, emails, proposals per rep vs benchmark | Rep is underworking or disengaged |
| 3. Effort structure | ICP ratio, activity mix, pipeline stage distribution | Rep is busy but misdirected |
| 4. Conversion quality | Win rate, pipeline velocity, deal size, discount rate | Skill gap in the sales process |
| 5. Environment | Territory fit, comp plan, management, training, market | Systemic constraint outside the rep's control |

The skill stops at the first layer where a problem is confirmed. If all layers look healthy, it proceeds to environment analysis.

---

## How to use

Provide your sales data in any of these formats:

- **CSV or Excel export** from Salesforce — Claude reads all columns, maps them to the model, and flags missing data
- **Pasted table** — Claude parses as-is and asks for clarification on ambiguous columns
- **Verbal description** — describe what you know; Claude asks for the specific numbers it needs layer by layer
- **Partial data** — Claude completes available steps and clearly notes which steps were skipped

### Example prompts

- "Here's our Q3 Salesforce export. Can you diagnose why attainment dropped to 61%?"
- "Our top rep hits 112% but three others are under 40%. What's going on?"
- "I'll describe our team's activity numbers — can you work through the model with me?"
- "Why is our district growing at 5.9% when the US average is 12.3%?"

---

## Output format

After completing the analysis, Claude produces:

**Per-layer outputs:**
- Outcome gap: summary table by rep/region + one-sentence gap statement
- Effort volume: activity table per rep vs benchmark with outliers flagged
- Effort structure: activity mix breakdown, ICP targeting ratio, stage distribution
- Conversion quality: funnel drop-off analysis, conversion rate table per rep
- Environment: factor scorecard (rating with evidence per factor)

**Final diagnosis summary:**
```
Gap identified:      [description]
Root cause layer:    [Effort Volume / Effort Structure / Quality / Environment]
Primary finding:     [one sentence]
Supporting data:     [2-3 key metrics]
Recommended action:  [specific next step]
Secondary findings:  [any additional issues]
```

---

## Salesforce objects used

| Metric | Salesforce source |
|---|---|
| Calls made | Activity (Type = Call) |
| Meetings held | Event object |
| Emails sent | Activity (Type = Email) |
| Proposals sent | Opportunity stage or custom field |
| Win rate | Closed Won / (Won + Lost) |
| Pipeline velocity | Deals x Avg size x Win rate / cycle length |
| Avg time in stage | Opportunity stage entry/exit dates |

---

## Installation

1. Download `salesforce-productivity-analyzer.skill`
2. In Claude, go to Settings and open the Skills section
3. Upload the `.skill` file
4. The skill activates automatically when you share sales data or ask about rep performance

---

## Important caveat

Performance diagnoses from this skill are analytical starting points based on the data provided. Decisions involving personnel — PIPs, role changes, terminations — should never rest on revenue data alone. Always verify tenure, prior performance trajectory, mitigating circumstances, and follow your organization's HR and legal process before acting.
