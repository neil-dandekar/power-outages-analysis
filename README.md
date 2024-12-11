# Power Outage Analysis

by Neil Dandekar (nmdandekar@ucsd.edu) and Idhant Kumar (ikumar@ucsd.edu)

---

## Introduction

In this project, we explored how various factors, like location, affect power outage duration across the United States.

---

## Cleaning and EDA

### Data Cleaning

Here is the cleaned dataframe:

| OUTAGE.DURATION | OUTAGE.START.DATETIME | NERC.REGION | CLIMATE.REGION     | CUSTOMERS.AFFECTED | CAUSE.CATEGORY     | ANOMALY.LEVEL | U.S.\_STATE |  POPULATION | DEMAND.LOSS.MW | MONTH | YEAR |
| --------------: | :-------------------- | :---------- | :----------------- | -----------------: | :----------------- | ------------: | :---------- | ----------: | -------------: | ----: | ---: |
|            3060 | 2011-07-01 17:00:00   | MRO         | East North Central |              70000 | severe weather     |          -0.3 | Minnesota   | 5.34812e+06 |            nan |     7 | 2011 |
|               1 | 2014-05-11 18:38:00   | MRO         | East North Central |                nan | intentional attack |          -0.1 | Minnesota   | 5.45712e+06 |            nan |     5 | 2014 |
|            3000 | 2010-10-26 20:00:00   | MRO         | East North Central |              70000 | severe weather     |          -1.5 | Minnesota   |  5.3109e+06 |            nan |    10 | 2010 |
|            2550 | 2012-06-19 04:30:00   | MRO         | East North Central |              68200 | severe weather     |          -0.1 | Minnesota   | 5.38044e+06 |            nan |     6 | 2012 |
|            1740 | 2015-07-18 02:00:00   | MRO         | East North Central |             250000 | severe weather     |           1.2 | Minnesota   | 5.48959e+06 |            250 |     7 | 2015 |

### Univariate Analysis

Plot 1: Distribution of Power Outages by Year

Pivot Table:

| Number_of_Outages | Avg_Outage_Duration | Avg_Customers_Affected | Severe_Weather_Proportion |
| ----------------: | ------------------: | ---------------------: | ------------------------: |
|                 1 |                 nan |                  14273 |                         0 |
|                34 |             5603.31 |                 256354 |                  0.823529 |
|                44 |             4271.12 |                 289778 |                  0.590909 |
|                 1 |                 372 |                    nan |                         0 |
|                 3 |             895.333 |                 126729 |                  0.666667 |

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
