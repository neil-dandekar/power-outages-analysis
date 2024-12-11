# Power Outage Analysis

by Neil Dandekar (nmdandekar@ucsd.edu) and Idhant Kumar (ikumar@ucsd.edu)

---

## Introduction

In this project, we explored how various factors, like location, affect power outage duration across the United States.

| Column                  | Description                                                                        |
| ----------------------- | ---------------------------------------------------------------------------------- |
| `OUTAGE.DURATION`       | Duration of power outage (minutes)                                                 |
| `OUTAGE.START.DATETIME` | Date and time of beginning of power outage                                         |
| `NERC.REGION`           | North American Electric Reliability Corporation (NERC) region of the power outage  |
| `CLIMATE.REGION`        | U.S. Climate region as specified by National Centers for Environmental Information |
| `CUSTOMERS.AFFECTED`    | Number of customers affected by the power outage event                             |
| `CAUSE.CATEGORY`        | Categories of all the events causing the major power outages                       |
| `ANOMALY.LEVEL`         | Gravity of natural disaster on power outage                                        |
| `U.S._STATE`            | State where the power outage occured                                               |
| `POPULATION`            | Population of those affected by the power outage                                   |
| `DEMAND.LOSS.MW`        | Demand loss in megawatts                                                           |
| `MONTH`                 | Month in which the outage occurred                                                 |

---

## Cleaning and EDA

### Data Cleaning

1. **Clean File + Load Data**
   The first row of the spreadsheet contained metadata, so we had to remove it and also drop the first row of data which included the units instead of data.

2. **Combine Redundant Columns**
   The `OUTAGE.START.DATE` and `OUTAGE.START.TIME` were redundant information, so we combined them into `OUTAGE.START.DATETIME`, and used Pandas DateTime objects.

3. **Drop Irrelevant Columns**
   We dropped the old redundant columns and selected only the relevant columns described earlier which we deemed as possibly useful features for the rest of the project.

Here is the cleaned dataframe:

| OUTAGE.DURATION | OUTAGE.START.DATETIME | NERC.REGION | CLIMATE.REGION     | CUSTOMERS.AFFECTED | CAUSE.CATEGORY     | ANOMALY.LEVEL | U.S.\_STATE |  POPULATION | DEMAND.LOSS.MW | MONTH | YEAR |
| --------------: | :-------------------- | :---------- | :----------------- | -----------------: | :----------------- | ------------: | :---------- | ----------: | -------------: | ----: | ---: |
|            3060 | 2011-07-01 17:00:00   | MRO         | East North Central |              70000 | severe weather     |          -0.3 | Minnesota   | 5.34812e+06 |            nan |     7 | 2011 |
|               1 | 2014-05-11 18:38:00   | MRO         | East North Central |                nan | intentional attack |          -0.1 | Minnesota   | 5.45712e+06 |            nan |     5 | 2014 |
|            3000 | 2010-10-26 20:00:00   | MRO         | East North Central |              70000 | severe weather     |          -1.5 | Minnesota   |  5.3109e+06 |            nan |    10 | 2010 |
|            2550 | 2012-06-19 04:30:00   | MRO         | East North Central |              68200 | severe weather     |          -0.1 | Minnesota   | 5.38044e+06 |            nan |     6 | 2012 |
|            1740 | 2015-07-18 02:00:00   | MRO         | East North Central |             250000 | severe weather     |           1.2 | Minnesota   | 5.48959e+06 |            250 |     7 | 2015 |

### Exploratory Data Analysis (EDA)

#### Univariate Analysis

Plot 1: Distribution of Power Outages by Year

<iframe src="assets/uni-outage-by-year.html" width="800" height="600" frameborder="0"></iframe>

Description:

Plot 2: Distribution of Power Outages Across NERC Regions

<iframe src="assets/uni-outage-by-nerc.html" width="800" height="600" frameborder="0"></iframe>

Description:

#### Bivariate Analysis

Plot 1: Outage Duration vs Customers Affected

<iframe src="assets/biv-outage-vs-customers.html" width="800" height="600" frameborder="0"></iframe>

Description:

Plot 2: Outage Duration by Cause Category

<iframe src="assets/biv-outage-by-cause.html" width="800" height="600" frameborder="0"></iframe>

Description:

#### Interesting Aggregate

Pivot Table:

| Number_of_Outages | Avg_Outage_Duration | Avg_Customers_Affected | Severe_Weather_Proportion |
| ----------------: | ------------------: | ---------------------: | ------------------------: |
|                 1 |                 nan |                  14273 |                         0 |
|                34 |             5603.31 |                 256354 |                  0.823529 |
|                44 |             4271.12 |                 289778 |                  0.590909 |
|                 1 |                 372 |                    nan |                         0 |
|                 3 |             895.333 |                 126729 |                  0.666667 |

Description:

---

## Assessment of Missingness

Here's what a Markdown table looks like. Note that the code for this table was generated _automatically_ from a DataFrame, using

```py
print(counts[['Quarter', 'Count']].head().to_markdown(index=False))
```

| Quarter     | Count |
| :---------- | ----: |
| Fall 2020   |     3 |
| Winter 2021 |     2 |
| Spring 2021 |     6 |
| Summer 2021 |     4 |
| Fall 2021   |    55 |

---

## Hypothesis Testing

---
