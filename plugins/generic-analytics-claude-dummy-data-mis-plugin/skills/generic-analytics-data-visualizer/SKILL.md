---
name: generic-analytics-data-visualizer
description: Publication-quality data visualization guide. Use when designing charts,
  selecting visual types, creating presentation graphics, or formatting charts for
  executive stakeholders.
metadata:
  category: visualization
  standard: tufte-anthropic
---

# Data Visualizer

Design publication-grade, accessible, and high-signal data visualizations based on Edward Tufte and Anthropic visual standards.

## Chart Selection Decision Tree

Select chart types based strictly on the analytical relationship being communicated:
- **Temporal Trends (Time Series)**: Use **Line Charts** with time strictly on the horizontal (X) axis.
- **Discrete Category Comparison**: Use **Horizontal or Vertical Bar Charts** sorted by magnitude.
- **Distribution & Skew**: Use **Histograms** (for continuous variables) or **Box Plots** (for multi-group dispersion).
- **Part-to-Whole Composition**: Use **Stacked Bar Charts** or **100% Stacked Area**. Use Donut/Pie charts *only* if categories <= 3.
- **Correlation / Relationship**: Use **Scatter Plots** with clearly labeled axes and trendlines.

## Tufte Visual Hierarchy & Data-Ink Ratio

1. **Action-Oriented Headlines**: Never write passive titles like `'Revenue by Region'`. Write the conclusion: `'North Region drives 54% of total Q2 revenue growth'`.
2. **Eliminate Chartjunk**: Remove top and right box spines. Use soft, muted gridlines (`alpha=0.15`) or eliminate background grids entirely.
3. **Direct Data Labeling**: Label key data points directly rather than forcing readers to cross-reference distant legends.
4. **Accessibility & Color**: Use colorblind-safe palettes (Viridis, ColorBrewer). Use color purposefully for emphasis, keeping non-focal series in neutral grays.
