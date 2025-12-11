


# Mutual Fund Performance Analysis (India) 

## Project overview
This project analyzes a historical NAV dataset of Indian mutual funds (2006–2023) to measure scheme-level performance and risk, identify top and bottom performers, and draw category- and fund-house-level conclusions. The analysis focuses on three core metrics: compound annual growth rate (CAGR), annualized volatility, and a Sharpe-like risk-adjusted ratio (using a 5% assumed risk-free rate). The goal is to produce reliable, interpretable risk–return insights after diagnosing and fixing data-quality issues visible in the notebook screenshots.

## Dataset summary
- Time span: 2006–2023 (as supplied in the dataset)
- Core fields used: scheme identifier, fund house, scheme name, date, NAV, scheme category
- Scope: thousands of schemes spanning equity, debt, hybrid, liquid funds and fixed-maturity plans (some categories labeled by duration, e.g., "1099 Days")

## High-level workflow
- Validate the raw dataset for structural issues and duplicates.
- Convert and standardize date information so each scheme’s NAV history can be ordered chronologically.
- Build a scheme-level summary that captures each scheme’s first and last NAV and the corresponding start and end dates.
- Compute the investment duration (in years) for each scheme and use it to annualize returns.
- Calculate CAGR for each scheme using first and last NAV values and the computed duration.
- Compute daily percentage NAV changes per scheme and convert the standard deviation of those daily returns into an annualized volatility measure (using a standard trading days approximation).
- Derive a Sharpe-like metric by subtracting an assumed 5% risk-free rate from CAGR and dividing by annualized volatility; handle cases where volatility is zero or extremely small carefully.
- Aggregate results at fund-house and scheme-category levels to identify patterns and provide comparative summaries.
- Diagnose and remove outliers that distort visualizations and summary statistics, then replot and re-evaluate.

## Key data-quality issues discovered
- Some NAV histories contained values that were effectively zero at the start of a scheme’s record. These near-zero start values produced astronomically large CAGR values (tens of thousands of percent) and invalid risk metrics.
- A small number of schemes exhibited NAV corrections, resets, or sudden jumps that are inconsistent with normal market behavior; those cases produced extreme volatility and rendered raw visualizations unreadable.
- The initial raw risk–return scatter plot was dominated by a few extreme outliers (examples observed: annualized volatility > 600,000% and CAGR > 40,000%). This made the vast majority of schemes collapse visually near the origin.
- Some short-duration schemes (duration much less than one year) exaggerated CAGR and Sharpe calculations if not properly filtered.

## Exact cleaning and outlier rules applied
- Remove any schemes where the first or last available NAV is zero or negative because these lead to division errors or meaningless CAGR values.
- Exclude schemes with non-positive duration (start date equals end date or invalid dates), since duration must be positive to compute annualized returns.
- Filter out schemes with extreme CAGR values that are clearly not representative of real investor outcomes. The applied rule kept only schemes with CAGR inside a reasonable window (for example, excluding very large positive or negative extremes that indicate data issues).
- Filter out schemes with unrealistically high annualized volatility (these values typically indicate NAV parsing errors or data anomalies).
- After applying the above filters, convert the remaining CAGR and volatility measures into percentage units for human-friendly visualization and reporting.

## What the initial 
- A raw scatter plot of CAGR vs annualized volatility was unreadable: a handful of schemes with extreme values stretched the axes so that the cluster of realistic schemes became invisible.
- A top-schemes table (uncleaned) contained schemes with implausible CAGR values (including fixed-term plans and liquid funds with extremely high computed returns), indicating the presence of parse or data-quality problems rather than genuinely astronomical returns.

## What changed after cleaning
- After removing invalid NAVs, very short durations, and extreme CAGR/volatility outliers, the cleaned dataset retained the majority of schemes (for example, roughly 29,562 schemes remained), while removing the small fraction of problematic records that had disproportionate visual impact.
- The cleaned risk–return scatter plot revealed a sensible cluster: most schemes concentrated in a region consistent with real-world mutual fund behavior (typical annualized volatility in 0–20% and CAGR commonly between –10% and +15%).
- The top-performing schemes became visible and interpretable. The worst-performing schemes also formed a coherent group and were no longer masked by impossible outliers.

## Findings: scheme-level insights
- Top schemes by raw CAGR (before careful filtering) often included fixed-maturity plans or schemes with extremely short observation windows; such results were flagged as unreliable until validated.
- In the cleaned results, the true high performers typically combined strong returns with moderate volatility, making them legitimate targets for further investigation.
- Bottom schemes by CAGR (cleaned) tended to show sustained negative returns. Many of these belonged to fixed-maturity plans or funds that experienced credit-related losses, NAV write-downs, or short life spans that coincided with market stress.

## Findings: fund-house-level insights
- After cleaning and aggregation by fund house, several fund houses appeared near the top by average CAGR. However, caution was required:
  - Large fund houses with many schemes produce averages that are more stable.
  - Small fund houses with few schemes can show extreme averages if one or two schemes dominate performance.
- Some fund houses that appeared to underperform drastically in unfiltered views were shown to include systemic data issues; after cleaning, their aggregated statistics were more reliable but still flagged for further manual review if negative patterns persisted.

## Findings: category-level insights
- Equity-oriented categories (Small Cap, Mid Cap, Multi Cap, ELSS, Dividend Yield) dominated the top categories by average CAGR. These categories had higher returns but also higher volatility — the usual risk–return tradeoff.
- Debt, liquid, and duration-labeled categories (fixed maturity durations) tended to have lower average returns and much lower volatility. Some debt categories showed negative average returns in the observed period, suggesting credit stress or rate-cycle effects.
- Category counts varied widely; categories with large counts provide more stable averages, while small categories are sensitive to single-scheme outcomes.

## Risk-adjusted performance
- The Sharpe-like ranking highlighted many short-duration debt products, overnight funds, and fixed-maturity plans. These schemes had extremely small volatility, which mathematically produced very large Sharpe values.
- Interpreting Sharpe-like ratios requires context: very large Sharpe values often mean the scheme is ultra-stable (low volatility) rather than being an exceptional long-term growth vehicle.
- For investor-facing recommendations, pair Sharpe results with absolute return and duration context (for example, overnight funds are not comparable to long-term equity funds despite high Sharpe).

## Top and bottom lists
- Top performers (cleaned): mostly equity and hybrid schemes with high CAGR and moderate volatility. These are plausible long-term performers for growth-focused investors.
- Bottom performers (cleaned): included several fixed-maturity or debt-oriented schemes that lost value over their lifetimes or suffered credit events; negative CAGRs ranged in the sample roughly from –40% to –46% among the worst cases.

## Visualizations produced and their interpretation
- Raw risk–return scatter: used to detect extreme anomalies. Not interpretable without cleaning.
- Cleaned risk–return scatter with highlighted top schemes: shows the bulk distribution and makes strong performers easy to identify.
- Bar charts of top/bottom 10 schemes by CAGR: facilitate comparison of absolute returns among extremes.
- Fund-house and category bar charts: reveal aggregated performance and help identify structural patterns.
- Sharpe ranking charts: highlight stability-dominant schemes (useful for income/stability investors).

## Concrete actions taken based on the analysis
- Implemented a reproducible cleaning workflow to remove invalid NAVs, extremely short-duration schemes, and mathematically implausible CAGR/volatility values.
- Recomputed scheme-level metrics and re-aggregated at fund-house and category levels to produce robust comparison tables and visualizations.
- Flagged schemes and fund houses for manual review when averages or extremes suggested potential data issues or real credit events.

## Limitations and caveats
- CAGR is sensitive to near-zero starting NAVs and to very short observation periods. Results for short-lived schemes must be interpreted with caution.
- Sharpe-like ratios are distorted by extremely low volatility; very high Sharpe values often indicate low volatility rather than exceptional equity-like growth.
- Dataset provenance matters: parsing errors (for example, missing decimal points), NAV corrections, or unrecorded corporate actions can produce misleading metrics. The notebook screenshots show many such anomalies that were handled by filtering, but some cases may require manual re-parsing or domain knowledge.
- Different schemes have different active periods; comparing long-established schemes to very recent ones requires additional normalization beyond simple annualization.
- No external market benchmark (e.g., NIFTY 50) was included in the current analysis; adding benchmarks would improve context.

## Practical recommendations
- Add automated validation rules to detect suspicious NAV patterns (e.g., start NAV < 0.1, sudden multiple-order-of-magnitude jumps, duplicate dates).
- Where possible, enrich the data with scheme event logs (mergers, splits, closures, corporate actions) to explain sudden NAV changes.
- Use robust summary statistics (median, trimmed mean, winsorized mean) when aggregating across schemes in a fund house to reduce sensitivity to remaining outliers.
- Present both absolute and risk-adjusted metrics together for investor-facing reports. For example, show CAGR, volatility, and Sharpe-like ratio side by side with scheme duration and category.
- When communicating results, emphasize categories and durations so readers understand the investment horizon (for example, overnight funds vs. small-cap funds).

## What to include in the repository
- A copy of the notebook containing the original screenshots with captions that point to the cleaning decisions and the precise photographic evidence that motivated each rule.
- A clear data dictionary describing each column and any preprocessing assumptions (date format, NAV decimal conventions).
- A short executive summary (1–2 paragraphs) highlighting the most robust conclusions: equity categories outperform on average but at higher volatility; some debt categories faced real negative returns; cleaning is essential to avoid misleading visuals.
- A “next steps” document listing improvements (benchmarking, recalculating using median-based aggregation, interactive dashboard for dynamic filtering).

## Summary paragraph for repository front page
This repository documents a careful, evidence-based analysis of Indian mutual fund NAV histories (2006–2023). The work focuses on producing reliable scheme-level CAGR and volatility metrics, diagnosing data-quality issues visible in the notebook screenshots, applying precise cleaning rules to remove distortions, and then presenting defensible risk–return findings at the scheme, fund-house, and category levels. Results show consistent equity outperformance with higher volatility, debt categories with lower and sometimes negative returns during the observed period, and the critical need for robust data validation before reporting financial performance.

## Contact and attribution
Provide your name, contact information, or project affiliation here so collaborators or reviewers can ask for clarifications or request the raw data for deeper manual inspection.

