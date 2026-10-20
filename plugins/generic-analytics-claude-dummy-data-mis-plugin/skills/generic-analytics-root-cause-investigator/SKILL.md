---
name: generic-analytics-root-cause-investigator
description: Systematic investigation of business metric shifts, drops, and anomalies.
  Use when explaining unexpected KPI changes, conducting dimensional drill-downs,
  or building post-mortems.
metadata:
  category: diagnostics
  framework: mece-rca
---

# Root Cause Investigator

Systematically diagnose and explain unexpected metric movements, anomalies, and performance variances.

## 6-Step Root Cause Investigation Protocol

1. **Validate Statistical Significance**:
   - Compare the metric shift against historical standard deviation (Z-score).
   - If the shift is within +/- 1.5 standard deviations, document it as normal variance.

2. **Pinpoint the Timeline**:
   - Determine if the change is a **Step Change** (sudden drop at a specific timestamp, indicating a bug/outage/event) or **Gradual Drift** (structural market shift).

3. **Decompose the Core Formula**:
   - Break the metric into independent mathematical components before running dimensional queries:
     $$\Delta \text{Revenue} = \Delta (\text{Transactions} \times \text{Avg Value})$$

4. **Dimensional Drill-Down**:
   - Segment before vs. after across all available dimensions (channel, product, geography, cohort).
   - Rank segments by **absolute contribution to total variance** (focus on the 80/20 driver).

5. **Simpson's Paradox Safeguard**:
   - Verify that aggregate trends are not confounded by mix shifts across sub-populations.

6. **Synthesize Executive Deliverable**:
   - **BLUF**: What changed and by how much.
   - **Primary Driver**: Quantified share of impact.
   - **Action Plan**: Immediate containment (0-7 days) vs structural improvements (30 days).
