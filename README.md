# Power Outage Analysis

by Neil Dandekar (nmdandekar@ucsd.edu) and Idhant Kumar (ikumar@ucsd.edu)

---

## Introduction

Power outages disrupt millions of lives every year, halting essential services, impacting public safety, and incurring substantial economic costs. In this project, Neil Dandekar and I set out to explore a dataset documenting power outages across the United States, aiming to uncover the key factors influencing outage durations. With climate change intensifying extreme weather events, it's more important than ever to understand the variables that contribute to prolonged outages and how they can be mitigated.

Our project focuses on answering the question: "How do climate regions and outage characteristics influence the length of power outages?"

This question is highly relevant because the duration of an outage affects recovery times, resource allocation, and overall community resilience. By investigating the role of factors such as climate conditions and outage causes, we hope to provide insights that could help strengthen the nation's power grid and improve response strategies in vulnerable regions.

Our dataset contains 1534 rows and 56 columns. Below is a description of the most relevant columns for our project:

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

Description: This histogram displays the distribution of power outages by year from 2000 to 2016. The x-axis represents the year, while the y-axis shows the number of outages that occurred in each year. The graph shows a general upward trend in outages over the years, with a significant peak around 2011-2012, where the number of outages exceeds 250. After this peak, the frequency of outages declines gradually but remains higher than the early 2000s. This suggests an increase in the frequency of power outages over time, with a notable concentration of outages during the early 2010s.

Plot 2: Distribution of Power Outages Across NERC Regions

<iframe src="assets/uni-outage-by-nerc.html" width="800" height="600" frameborder="0"></iframe>

Description: This bar chart illustrates the distribution of power outages across various NERC (North American Electric Reliability Corporation) regions. The x-axis represents the different NERC regions, while the y-axis indicates the number of outages in each region. The WECC (Western Electricity Coordinating Council) and RFC (ReliabilityFirst Corporation) regions experience the highest number of outages, with 451 and 419 outages, respectively. Other regions such as SERC and NPCC have significantly fewer outages, with 205 and 150, respectively. Smaller regions like HECO, PR, and ASCC report minimal outages, each having only one. This distribution suggests that power outages are more concentrated in certain regions, particularly WECC and RFC, which may reflect differences in geographic size, population density, or infrastructure vulnerabilities.

#### Bivariate Analysis

Plot 1: Outage Duration vs Customers Affected

<iframe src="assets/biv-outage-vs-customers.html" width="800" height="600" frameborder="0"></iframe>

Description: This scatter plot visualizes the relationship between outage duration (in minutes) and the number of customers affected, with data points categorized by NERC regions. The x-axis represents the outage duration, while the y-axis shows the number of customers affected, with a zoomed-in view focusing on durations under 20,000 minutes and customer counts under 500,000. Each point corresponds to an outage, and its color indicates the NERC region. Most outages are clustered in the lower-left corner, suggesting that shorter outages tend to affect fewer customers. However, there are scattered points with longer durations and higher customer impacts, indicating occasional large-scale outages. The distribution varies slightly across NERC regions, reflecting regional differences in outage impacts.

Plot 2: Outage Duration by Cause Category

<iframe src="assets/biv-outage-by-cause.html" width="800" height="600" frameborder="0"></iframe>

Description: This box plot illustrates the distribution of outage durations (in minutes) across various cause categories. The x-axis represents the cause categories, while the y-axis indicates the outage durations. The data shows that "severe weather" has a wide range of outage durations, with several significant outliers. "Fuel supply emergency" exhibits the largest variability among categories, with its interquartile range spanning much higher durations and outliers extending even further. Categories such as "intentional attack," "system operability disruption," and "islanding" have much shorter durations with relatively minimal variability. This visualization highlights that certain cause categories, like "severe weather" and "fuel supply emergency," are associated with longer and more variable outages, reflecting their potentially larger impacts on power systems.

#### Interesting Aggregate

Pivot Table: Number of Outages and Average Duration by NERC Region

| NERC.REGION | Number of Outages | Average Duration Length |
| :---------- | ----------------: | ----------------------: |
| ASCC        |                 0 |                       0 |
| ECAR        |                32 |                 5603.31 |
| FRCC        |                43 |                 4271.12 |
| FRCC, SERC  |                 1 |                     372 |
| HECO        |                 3 |                 895.333 |
| HI          |                 1 |                    1367 |
| MRO         |                44 |                 2933.59 |
| NPCC        |               147 |                 3262.17 |
| PR          |                 1 |                     174 |
| RFC         |               416 |                 3477.96 |
| SERC        |               194 |                 1737.99 |
| SPP         |                62 |                 2693.77 |
| TRE         |               108 |                 2960.57 |
| WECC        |               424 |                 1481.49 |

Description: This pivot table summarizes the number of outages and average outage durations across NERC regions. WECC and RFC report the highest number of outages, with 424 and 416, respectively, but their average durations (1481.49 and 3477.96 minutes) are not the longest. ECAR stands out with a significantly high average duration of 5603.31 minutes despite having only 32 outages, indicating fewer but more prolonged events. Regions like FRCC, SERC, and PR have shorter average durations, with FRCC, SERC reporting 372 minutes and PR 174 minutes. Notably, ASCC has no recorded outages. These patterns suggest regional differences in outage frequency and duration, likely influenced by factors such as infrastructure, population, and regional vulnerabilities.

---

## Assessment of Missingness

| Column                | Number of Missing Entries |
| :-------------------- | ------------------------: |
| OUTAGE.DURATION       |                        58 |
| OUTAGE.START.DATETIME |                         9 |
| NERC.REGION           |                         0 |
| CLIMATE.REGION        |                         6 |
| CUSTOMERS.AFFECTED    |                       443 |
| CAUSE.CATEGORY        |                         0 |
| ANOMALY.LEVEL         |                         9 |
| U.S.\_STATE           |                         0 |
| POPULATION            |                         0 |
| DEMAND.LOSS.MW        |                       705 |
| MONTH                 |                         9 |
| YEAR                  |                         0 |

It seems like we have a number of missing columns

---

## Hypothesis Testing

---
