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

### NMAR Analysis

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

It seems like we have a number of missing columns. In particular, the OUTAGE.DURATION column is likely NMAR. It's difficult to assess the reason why it's missing just from observing the data, so we must reason about the data generating process. For example, for a given power outage may have been observed for different lengths, therefore there was no singular value to report for the duration of that outage. This could be due to delays in the powerlines or some other data reporting process, which would make the data NMAR. Therefore, more data and/or analysis is required to assess whether the OUTAGE.DURATION data is NMAR.

### Missingness Dependency

We tested the missingness dependency of OUTAGE.DURATION upon two columns: MONTH and CLIMATE.REGION. In other words, we compared whether the distributions of MONTH and CLIMATE were the same when the duration is missing and when the duration is not missing.

<iframe src="assets/missing-month.html" width="800" height="600" frameborder="0"></iframe>

<iframe src="assets/missing-clireg.html" width="800" height="600" frameborder="0"></iframe>

Above we observed the distributions of MONTH and CLIMATE when the duration is missing vs. when it's not missing. It's difficult to determine any missingness dependency here so we tried the following permutation test:

-   Null Hypothesis: The distribution of the test column is the same regardless of whether duration is missing.

-   Alternative Hypothesis: The distribution of the test column is different when duration is missing compared to when it is not missing.

-   Test Statistic: TVD, because both month and climate region are categorical variables.

-   Cutoff: 0.05

**Results:**

Climate Region

-   Observed TVD: 0.2535212947392274
-   P-value: 0.004
-   Conclusion: Reject the Null Hypothesis

Month

-   Observed TVD: 0.22267850229522704
-   P-value: 0.088
-   Conclusion: Fail to Reject the Null Hypothesis

For the Climate Region, the p-value is less than our cutoff. This means we reject the null hypothesis that the distribution of climate region is the same regardless of whether duration is missing. Therefore, it's likely that the missingness of duration IS dependent upon Climate Region. On the other hand, the Month column did NOT provide significant statistical evidence that the distribution of month is different when duration is missing. Therefore, it's likely that the missingness of duration is NOT dependent upon the Month.

---

## Hypothesis Testing

To understand whether power outage durations are influenced by the region in which they occur, we formulated and tested the following hypotheses:

-   Null Hypothesis (H₀): The NERC region has no effect on the duration of power outages. Any observed differences in mean outage duration across regions are due to random chance.
-   Alternative Hypothesis (H₁): The NERC region does have an effect on the duration of power outages, meaning that the mean outage durations differ significantly across regions.
-   Test Statistic: We chose the difference between the maximum and minimum mean outage durations across NERC regions as our test statistic. This statistic captures the range of variation in outage durations between regions, providing a clear measure of disparity.
-   Significance Level: 0.05

### Results

-   Observed Statistic: difference in means= 4151.860728172499
-   P-value: 0.206

### Conclusion

Since the p-value is greater than the significance level of α=0.05, we fail to reject the null hypothesis. This suggests that there is insufficient evidence to conclude that the NERC region significantly affects the duration of power outages. While differences in mean durations were observed, they are not statistically significant and could reasonably occur due to chance.

---

## Framing a Prediction Problem

We framed a prediction problem to understand the factors contributing to the duration of power outages. The goal of this problem is to build a model that can predict the duration of a power outage based on relevant features, providing insights that could help utility companies improve their response strategies and minimize the impact of outages.

### Prediction Task

Our task is to predict the duration of power outages (OUTAGE.DURATION) based on features related to geography, weather, and infrastructure. This task is framed as a regression problem since the response variable is continuous.

### Response Variable

The response variable for this prediction task is OUTAGE.DURATION, which represents the length of a power outage in minutes. This variable was chosen because accurately predicting outage durations is crucial for optimizing resource allocation during outages and planning effective preventative measures.

### Evaluation Metric

We selected Root Mean Squared Error (RMSE) as our evaluation metric. RMSE was chosen because:

1. It penalizes larger errors more heavily due to the squaring of residuals, which is important for accurately modeling power outage durations that can vary significantly.
2. It provides an intuitive interpretation of the model’s prediction accuracy by being in the same units as the response variable (minutes).
3. Compared to metrics like Mean Absolute Error (MAE), RMSE is better suited for detecting models that systematically underperform on extreme durations.

## Features

The features used in our final model include both baseline and engineered features. These features were carefully chosen to be accessible at the time of prediction and are critical for capturing various aspects of the outage:

1. CLIMATE.REGION: The climate region where the outage occurred, capturing geographic and weather-related information.
2. CAUSE.CATEGORY: The primary cause of the outage (e.g., severe weather, equipment failure), providing context on outage drivers.
3. CUSTOMERS.AFFECTED: The number of customers impacted, which reflects the scale of the outage.
4. MONTH: The month in which the outage occurred, revealing seasonal trends in outages.
5. ANOMALY.LEVEL: A quantitative measure of the gravity of the event that caused the outage.
6. OUTAGE.START.HOUR: The hour of the day when the outage began, derived from the OUTAGE.START.DATETIME column, which helps capture patterns related to time-specific behaviors or vulnerabilities.
7. OUTAGE.START.DAYOFWEEK: The day of the week when the outage began (0 = Monday, 6 = Sunday), also derived from OUTAGE.START.DATETIME, providing insights into weekly patterns.

---

## Baseline Model

### Model Description

For the baseline model, we aimed to predict the duration of power outages (`OUTAGE.DURATION`) using a simple linear regression approach. This baseline model provides a reference point for evaluating more advanced models in subsequent steps. By starting with straightforward features and preprocessing steps, we establish an initial benchmark for prediction performance.

### Features and Encoding

The baseline model uses four features:

1. **CLIMATE.REGION (Nominal)**:  
   The climate region where the outage occurred. This categorical feature was encoded using one-hot encoding to transform it into binary columns, allowing the model to process the data effectively.

2. **CAUSE.CATEGORY (Nominal)**:  
   The cause of the outage (e.g., severe weather, equipment failure). This feature was also one-hot encoded for the same reason.

3. **CUSTOMERS.AFFECTED (Quantitative)**:  
   The number of customers impacted by the outage. This feature was left unchanged as it is already numeric.

4. **MONTH (Ordinal)**:  
   The month of the year when the outage occurred. As it is numeric, no transformation was needed.

By combining both nominal and numeric features, the model captures key factors influencing outage durations while ensuring that all features are appropriately preprocessed.

### Data Splitting

The data was split into training and test sets using a 75-25 split ratio:

-   The **training set** was used to fit the model.
-   The **test set** was reserved to evaluate its performance on unseen data.

This approach ensures that the model’s performance reflects its ability to generalize beyond the training data.

### Model Pipeline

To streamline the process, we built a **scikit-learn pipeline** that combines preprocessing and modeling steps:

1. **Preprocessing**:

    - Handled one-hot encoding for `CLIMATE.REGION` and `CAUSE.CATEGORY`.
    - Passed numeric features (`CUSTOMERS.AFFECTED`, `MONTH`) through unchanged.

2. **Model**:
    - A linear regression model was applied to predict outage durations.

This pipeline ensures consistency, reproducibility, and efficiency in training and evaluation.

### Performance Evaluation

The baseline model was evaluated using **Root Mean Squared Error (RMSE)**, which measures the average magnitude of prediction errors:

-   **RMSE of the Baseline Model**: _1324.17_

#### Why RMSE?

-   It penalizes larger errors more heavily, making it suitable for capturing large variations in outage durations.
-   It provides results in the same units as the response variable (minutes), offering intuitive interpretability.

### Model Assessment

The baseline model establishes a solid starting point for prediction. While the RMSE is not exceptionally low, it is expected for a baseline model that uses limited features and lacks advanced tuning or feature engineering. The baseline model is considered “good” for its purpose of providing a benchmark for comparison. It highlights the potential for improvement through:

-   Incorporating additional features,
-   Engineering new variables,
-   Exploring more sophisticated models, and
-   Performing hyperparameter tuning.

This model provides an initial understanding of how key factors like climate region and outage causes influence the duration of power outages. Subsequent steps aim to enhance its performance and provide deeper insights.

---

## Final Model

### New Features

To improve upon the baseline model, we introduced two new features engineered from the original dataset:

1. **OUTAGE.START.HOUR (Quantitative)**:  
   Captures the hour of the day when an outage started. Time of day can significantly influence the response times for restoring power. For example, outages occurring during peak hours or nighttime may have different durations due to accessibility or resource availability.

2. **OUTAGE.START.DAYOFWEEK (Quantitative)**:  
   Represents the day of the week when the outage began. Weekends and weekdays often have different patterns of response and workload, which can influence the time taken to resolve outages.

These features were added because they directly correspond to temporal factors that can logically affect outage durations, aligning with our understanding of the data-generating process. By incorporating these features, the model gains the ability to capture patterns related to the timing of outages.

### Model Architecture

For the final model, we used a **Random Forest Regressor**, which is well-suited for regression tasks involving both categorical and numerical features. Random Forests handle complex interactions between features and are robust against overfitting when properly tuned.

### Hyperparameter Tuning

To optimize the model, we used **GridSearchCV** with 5-fold cross-validation to tune the following hyperparameters:

-   **n_estimators (Number of Trees)**: [100, 200, 300]  
    Larger values allow the model to learn more patterns from the data but increase computational cost.
-   **max_depth (Maximum Depth of Trees)**: [10, 20, None]  
    Controls the complexity of each tree to prevent overfitting.
-   **min_samples_split (Minimum Samples per Split)**: [2, 5, 10]  
    Ensures that splits occur only when nodes contain a minimum number of samples, which helps generalization.

#### Best Hyperparameters:

After running GridSearchCV, the best-performing hyperparameters were:

-   **n_estimators**: 200
-   **max_depth**: 10
-   **min_samples_split**: 10

These values were chosen based on their ability to minimize Root Mean Squared Error (RMSE) during cross-validation.

### Model Training and Evaluation

The final model was trained using the same train-test split as the baseline model (75% training, 25% testing) to ensure a fair comparison. The performance metric remained **Root Mean Squared Error (RMSE)**.

-   **Final Model RMSE**: _1276.2_
-   **Baseline Model RMSE**: _1324.17_

The improvement in RMSE demonstrates the impact of incorporating the new features and tuning the hyperparameters.

### Methodology

1. **Feature Engineering**:  
   Extracted temporal features (`OUTAGE.START.HOUR` and `OUTAGE.START.DAYOFWEEK`) from the dataset to capture additional patterns.

2. **Pipeline Construction**:  
   Built a pipeline using scikit-learn to preprocess features and train the Random Forest Regressor.

3. **Hyperparameter Selection**:  
   Used GridSearchCV to systematically explore combinations of hyperparameter values and identify the optimal settings.

### Comparison with Baseline

The final model outperforms the baseline model by reducing RMSE from _1324.17_ to _1276.2_. This improvement highlights the importance of thoughtful feature engineering and hyperparameter tuning:

-   The new temporal features allowed the model to account for variations in outage duration based on timing.
-   The optimized hyperparameters enhanced the model’s ability to generalize to unseen data.

While the improvement is modest, it demonstrates that these additional steps lead to a better understanding of the factors influencing outage durations and provide a more accurate predictive model.

---

## Fairness Analysis

### Evaluation Metric

The evaluation metric used for this fairness analysis is **Root Mean Squared Error (RMSE)**. RMSE is a widely accepted measure for assessing the accuracy of regression models, capturing the magnitude of prediction errors while penalizing larger deviations more heavily.

### Group Definition

-   **Group X**: Power outages occurring in the Central NERC region.
-   **Group Y**: Power outages occurring in the Northeast NERC region.

These groups were selected to evaluate potential disparities in the model’s predictive performance across different geographic areas.

### Hypotheses

-   **Null Hypothesis (H₀)**: The model is fair. The RMSE for Group X (Central) and Group Y (Northeast) are approximately the same, and any observed differences are due to random chance.
-   **Alternative Hypothesis (H₁)**: The model is unfair. There is a significant difference in RMSE between Group X (Central) and Group Y (Northeast).

### Test Statistic and Significance Level

-   **Test Statistic**: The absolute difference in RMSE between the two groups.
-   **Significance Level**: 0.05, a standard threshold for assessing statistical significance.
-   **Method**: A permutation test was performed by randomly shuffling the `CLIMATE.REGION` labels and recalculating the test statistic for 1,000 iterations.

### Results

-   **Observed RMSE for Group X (Central)**: 1681.77
-   **Observed RMSE for Group Y (Northeast)**: 1325.49
-   **Observed Difference in RMSE**: 356.28
-   **P-Value from Permutation Test**: 0.550

### Conclusion

With a p-value of 0.550, which is greater than the significance level of 0.05, we do not have sufficient evidence to reject the null hypothesis. This suggests that the observed difference in RMSE between the Central and Northeast regions is not statistically significant and could be attributed to random chance. Therefore, the model appears to perform fairly across these two groups, with no strong evidence of bias.

---

## Report Conclusion

cqbvx
