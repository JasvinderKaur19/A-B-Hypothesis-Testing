# A/B Hypothesis Testing: E-commerce Landing Page Optimization

## 1. Introduction

A/B hypothesis testing is a statistical technique used to compare two versions of a webpage, product, or feature to determine which performs better.

In this project, I analyze an e-commerce website experiment to evaluate whether a **new landing page improves the user conversion rate compared with the existing landing page**. Using Python, statistical hypothesis testing, and logistic regression, I investigate conversion behavior and examine whether the user's country influences the results.

The goal is to make a data-driven recommendation on whether the company should implement the new page, retain the existing page, or conduct further experiments.

## 2. Project Objectives

- Analyze user behavior and overall conversion rates.
- Clean and validate the experimental dataset.
- Compare conversion rates between the control and treatment groups.
- Apply probability concepts and simulate a null distribution.
- Perform hypothesis testing using p-values and a z-test.
- Build a logistic regression model to evaluate the relationship between landing page type and conversion.
- Investigate the effects of country and its interaction with the landing page.
- Translate statistical findings into actionable business recommendations.

## 3. Technologies and Libraries

- **Python** — Data analysis and statistical calculations
- **Pandas** — Data manipulation, cleaning, and merging
- **NumPy** — Numerical calculations and probability simulations
- **Matplotlib** — Visualizing the simulated sampling distribution
- **Statsmodels** — Proportion z-test and logistic regression
- **Jupyter Notebook** — Interactive analysis and documentation

## 4. Dataset Overview

The project uses two datasets:

**`ab_data.csv`**

Contains user-level experimental data with the following columns:

| Column | Description |
|---|---|
| `user_id` | Unique identifier for each user |
| `timestamp` | Time of the user's recorded activity |
| `group` | Control or treatment group |
| `landing_page` | Old or new landing page |
| `converted` | Whether the user converted (1) or did not convert (0) |

**`countries.csv`**

Contains user identifiers and country information used to investigate whether geographical differences are associated with conversion behavior.

The original experiment dataset contains 294,478 rows. After removing mismatched group/page assignments and duplicate user records, the analysis dataset contains **290,584 unique users**.

## 5. Project Workflow and Steps

### Step 1: Import Libraries and Load Data

Import the required Python libraries and load the CSV datasets into Pandas DataFrames.

Inspect the first few rows, dataset dimensions, column types, and summary information to understand the data before analysis.

### Step 2: Data Cleaning and Validation

Prepare the dataset for reliable statistical analysis by:

- Checking for missing values.
- Identifying mismatches between experimental groups and landing pages.
- Removing records where the control group did not receive the old page or the treatment group did not receive the new page.
- Identifying duplicate user IDs and removing duplicate records.
- Verifying that each remaining user has a consistent experimental assignment.

These checks help reduce data-quality issues that could affect the experiment's conclusions.

### Step 3: Exploratory Data Analysis

Calculate the overall conversion rate and the conversion rates for each experimental group.

The initial analysis shows:

- Overall conversion rate: approximately 11.96%.
- Control group conversion rate: approximately 12.04%.
- Treatment group conversion rate: approximately 11.88%.
- Approximately half of the users received each page.

The old page has a slightly higher observed conversion rate, but descriptive statistics alone cannot establish whether this difference is statistically significant.

### Step 4: Formulate the Hypotheses

For the primary directional test, define:

- **Null hypothesis (H₀):** The new page's conversion rate is less than or equal to the old page's conversion rate.
- **Alternative hypothesis (H₁):** The new page's conversion rate is higher than the old page's conversion rate.

Use a significance level of 5% (\(\alpha = 0.05\)).

### Step 5: Simulate the Null Distribution

Use NumPy's binomial distribution to simulate conversion counts under the assumption that both pages have the same true conversion rate.

Repeat the simulation 10,000 times, using the observed sample sizes for the control and treatment groups. Calculate the difference in conversion rates for each simulation and store the results.

Plot a histogram of the simulated differences to visualize the sampling distribution under the null hypothesis.

Compare the observed difference with this simulated distribution to understand how compatible the observed result is with the null hypothesis.

### Step 6: Calculate the P-value and Perform a Z-test

Calculate the observed difference between the treatment and control conversion rates.

Use the simulated null distribution to estimate a p-value, then apply `statsmodels`' proportion z-test to obtain a test statistic and p-value.

The z-test reported in the notebook produces:

- Z-score: approximately 1.311.
- One-sided p-value: approximately 0.905.

Because the p-value is greater than 0.05, the analysis does not provide sufficient evidence that the new page improves conversions.

### Step 7: Build a Logistic Regression Model

Fit a logistic regression model using `statsmodels` to examine the relationship between landing page assignment and conversion.

Create an intercept column and a binary indicator for the treatment group. Fit the model with conversion as the dependent variable and page assignment as the predictor.

The model reports a p-value of approximately 0.190 for the page-assignment coefficient. This is not statistically significant at the 5% level.

The regression approach therefore supports the conclusion that the experiment does not establish a statistically significant difference in conversion rates. Its two-sided hypothesis test is different from the one-sided test used in the earlier analysis.

### Step 8: Investigate Country Effects and Interactions

Merge the country dataset with the cleaned experimental data using `user_id`.

Create country indicator variables and interaction terms between country and landing page assignment. Fit an additional logistic regression model to investigate whether conversion behavior varies by country or whether the new page has different effects across countries.

The reported model has p-values above 0.05 for the country indicators, page assignment, and country-by-page interaction terms. These results do not establish statistically significant country effects or interactions.

*Implementation note:* Before relying on the country-level results, ensure that the country dummy-variable columns are correctly mapped to their country labels. The original notebook assigns the output of `pd.get_dummies()` to manually named columns, which can mislabel the indicators depending on column order.

## 6. Key Findings

- The cleaned dataset contains 290,584 unique users.
- The observed conversion rate is approximately 12.04% for the control group and 11.88% for the treatment group.
- The observed conversion rate of the new page is slightly lower than that of the old page.
- The proportion z-test does not provide evidence that the new page improves conversions.
- The logistic regression model also finds no statistically significant page-assignment effect.
- The reported country-interaction model does not identify statistically significant country-by-page effects, subject to validating the dummy-variable implementation.

## 7. Conclusion and Business Recommendations

This project demonstrates how statistical analysis can support e-commerce product decisions. The conversion rates of the old and new landing pages were compared using data cleaning, exploratory analysis, simulation, hypothesis testing, and logistic regression.

The results do not provide sufficient statistical evidence that the new landing page improves conversion rates. The treatment group also has a slightly lower observed conversion rate than the control group.

**Based on this experiment, I would not recommend implementing the new page solely on the expectation of improved conversions.** The company should review the experiment's duration, check for potential data or assignment issues, and consider additional experiments or segmentation analysis before making a final decision.

The project highlights the importance of validating data, selecting appropriate statistical tests, interpreting p-values correctly, and translating analytical results into practical business recommendations.

## 8. Skills Demonstrated

- Data cleaning and quality validation
- Exploratory data analysis
- Conversion-rate analysis and probability
- Hypothesis formulation and statistical inference
- Null-distribution simulation and p-value estimation
- Proportion z-tests
- Logistic regression
- Feature encoding and interaction terms
- Data merging and segmentation analysis
- Statistical interpretation and business decision-making

## 9. How to Run the Project

1. Clone or download this GitHub repository.
2. Ensure Python and Jupyter Notebook are installed.
3. Install the required libraries:

   ```bash
   pip install pandas numpy matplotlib statsmodels jupyter
   ```

4. Place `ab_data.csv` and `countries.csv` in the project directory, or update the file paths in the notebook.
5. Open the notebook in Jupyter:

   ```bash
   jupyter notebook
   ```

6. Open the A/B testing notebook and run the cells sequentially to reproduce the analysis.

**Note:** The notebook currently uses a local Windows file path. Update it to a relative path, such as `pd.read_csv("ab_data.csv")`, before publishing so other users can run the project. Ensure you have permission to redistribute the datasets before including them in the repository.
