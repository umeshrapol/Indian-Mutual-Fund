# Indian Mutual Fund Analysis (2006-2023)

## Project Overview
This project performs a comprehensive data analysis of the Indian Mutual Fund industry using a historical dataset spanning from 2006 to 2023. The goal is to evaluate scheme performance, assess risk, and identify the best risk-adjusted performers.

## Dataset
* **Source:** `Mutual_Funds.csv`
* **Volume:** Approximately 29 million rows.
* **Key Features:**
    * `Fund_House`: The asset management company.
    * `Scheme_Type`: Open-ended, close-ended, etc.
    * `Scheme_Category`: Large Cap, Small Cap, Liquid, etc.
    * `Scheme_Code` & `Scheme_Name`: Unique identifiers.
    * `Date` & `NAV`: Historical Net Asset Value records.

## Prerequisites
The analysis requires the following Python libraries:
* pandas
* numpy
* matplotlib
* seaborn
* missingno

## Analysis Workflow

### 1. Data Loading & Validation
* Loaded the raw CSV dataset.
* Verified data integrity:
    * Confirmed 0 missing values.
    * Confirmed 0 duplicate rows.
    * Validated data types (Integers for codes, Floats for NAV).

### 2. Preprocessing
* **Date Conversion:** Parsed the `Date` column to datetime objects.
* **Sorting:** Sorted data by `Scheme_Code` and `Date` to allow for accurate time-series calculations.
* **De-duplication:** Removed duplicate entries post-sorting.

### 3. Feature Engineering
* **Duration:** Calculated the investment duration for each scheme in years.
* **CAGR (Compound Annual Growth Rate):** Computed using the formula:
    $$\text{CAGR} = (\frac{\text{End NAV}}{\text{Start NAV}})^{\frac{1}{\text{Years}}} - 1$$
* **Volatility:** Calculated daily percentage changes in NAV and derived the **Annualized Volatility** (Standard Deviation of daily returns $\times \sqrt{252}$).

### 4. Data Cleaning (Outlier Removal)
To ensure statistical significance, the data was rigorously cleaned:
* **Investment Duration:** Only schemes with valid positive duration were kept.
* **Extreme Values:** Removed schemes with unrealistic CAGRs (filtered to kept values between -50% and +50%) and extreme Volatility (filtered to keep 0% to 80%) to handle data entry errors in the raw source.

### 5. Performance Metrics
* **Fund House Ranking:** Aggregated performance by Fund House to identify Top 10 and Bottom 10 performers.
* **Category Analysis:** Analyzed `Scheme_Category` to visualize the Risk vs. Return trade-off.
* **Sharpe Ratio:** Calculated the risk-adjusted return:
    $$\text{Sharpe Ratio} = \frac{\text{CAGR} - R_f}{\text{Volatility}}$$
    *(Risk-Free Rate $R_f$ assumed at 5%)*

## Key Findings
* **Risk vs. Return:** Equity schemes (Small/Mid Cap) showed higher returns but significantly higher volatility.
* **Stability:** Debt and Liquid funds demonstrated low volatility, resulting in high Sharpe Ratios.
* **Top Performers:** Schemes like "DSP Mutual Fund" showed strong historical average returns (before outlier removal).

## Visualizations
The notebook includes:
* Bar charts for Top/Bottom 10 Fund Houses by CAGR.
* Scatter plots for Risk vs. Return analysis by Category.
* Charts for Top 20 Schemes by Sharpe Ratio.
