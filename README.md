
# Power Outages: An Investigation into What's Really in Our Power
 This is a project for DSC80 at UCSD in which I will be cleaning my dataset, performing exploratory data analysis, assessing missingness, conducting permutation tests, creating a classification model, and judging fairness.

 **By** Deepika Senthil

### Introduction

Power outages affect everyone and worse, they make people feel powerless. It is through learning more about outages, specifically why they occurred and what to typically expect when one must be endured that people will be able to feel more prepared and more in control. That is what my question which this analysis is based on aims to do, answering the question: **What causes power outages and who are most affected by them?** The first part will be answered by checking which characteristics (columns) in the dataset can best predict the cause of an outage. The second part of the question will be answered by analyzing whether whenever a power outage occurs certain groups of people (those in more economically robust areas) will have their power restored in a more timely manner and will therefore have to bear less of an effect of the outage in their lives.

The dataset that is going to be analyzed is the "Major Power Outage Risks in the U.S." dataset collected by Purdue University, outlining the major outages that have occurred across different states in the continental US from January 2000 to July 2016 and well as including other characteristics of each outage. It is of these characteristics that we will be deciding which are the most relevant to answering the key exclamations that characterize outages: "How long will it take?" and "WHY?". The dataset contains a total of 1534 observations (rows), 1476 of which we will consider. The dataset also has 56 columns representing the various characteristics and potential features in answering the aforementioned explanations. The columns that are most relevant to my question are the duration of the power outages ("OUTAGE.DURATION"), the peak demand lost during the outage ("DEMAND.LOSS.MW"), ("CLIMATE.CATEGORY"), the NERC region affected by the outage ("NERC.REGION"), the percentage of water area in the state of the outage compared to the water area in the continental US ("PCT_WATER_TOT"), the per capita real GSP of the state in which the outage occurred ("PC.REALGSP.STATE"), the electricity consumption in the industrial sector in the area of the outage (IND.SALES), and the name of the hurricane if the cause of the outage was a hurricane ("HURRICANE.NAMES"), so these columns will be popping up throughout this analysis.

### Data Cleaning and Exploratory Data Analysis
The data cleaning process is necessary to ensure a proper analysis. I first removed rows and columns with information other than the data recorded of the characteristics of the outages. I then created two new columns ('OUTAGE.START' and 'OUTAGE.RESTORATION') using the 'OUTAGE.START.DATE' column with the 'OUTAGE.START.TIME' column as well as the 'OUTAGE.RESTORATION.DATE' column with the 'OUTAGE.RESTORATION.TIME' column respectively, converting to Timedelta and TimeStamps to do so. Using these new columns, I then filled in the missing values of the "OUTAGE.DURATION" columns with the difference between the 'OUTAGE.START' and 'OUTAGE.RESTORATION' columns. I then removed columns that were irrelevant to my analysis, keeping ones that might be relevant later on, however. Finally, I created a new column "robust" that contains a value of 1 if the "PC.REALGSP.STATE" is greater than 60000, 0 otherwise, to represent whether a state is economically robust or not. The value of 60000 as a marker of economic robustness was obtained from US News' ranking of the best state economies.
INSERT IN DF HEAD

<iframe
  src="assets/xillis_count.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This plot shows the distrbution of the causes of power outages in the dataset. More than half of all power outages can be attributed to severe weather, with the second most popular cause of outages being intentional attack.

<iframe
  src="assets/xillis_count2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This plot shows the distribution in the durations of power outages. We can see that most power outages last less than 20k minutes (less than 14 days), but this plot also emphasizes the presense of many outliers in outage durations that should be kept in mind and/or investigated further

| CLIMATE.CATEGORY | cold | normal | warm |
|--------|-------|------|-------|
| MONTH |       |      |       |
| 1 | 0.59 | 0.19 | 0.22 |
| 2 | 0.68 | 0.19 | 0.14 |
| 3 | 0.19 | 0.51 | 0.30 |
| ... | ... | ... | ... |
| 10 | 0.29 | 0.38 | 0.34 |
| 11 | 0.36 | 0.40 | 0.24 |
| 12 | 0.41 | 0.23 | 0.36 |

In this pivot table, you can see the distribtion of the number of power outages that occur in each climate region for each month. The number of outages that occur at the start and end of the year is higher for those in the cold climate region than in the normal or warm climate regions. However, a few months in, those in the normal climate region start having a higher porportion of power outages. The number of power outages in the warm climate region vary a lot less. Given that severe weather was the most common cause of power outages, it makes sense that those in colder climate categories might have more power outages in the winter months characterzied by their severe weather while warm climate areas arent characterized by such varying weather and thus have less varying power outage proportions.


### Assessment of Missingness
I believe that there are columns in this dataset that could be considered NMAR due to this dataset focusing on the collection of power outage data for major power outages, particularly the total number of customers that were affected due to power outages. This is why the missing values in the "CUSTOMERS.AFFECTED" column are NMAR since the values that are missing likely aren't recorded because the actual value itself is too small. Additional data I might want to obtain to explain the missingness (making it MAR) could be a column that details whether the power outage is categorized as a "Major Power Outage" or a "Non-Major Power Outage", since that way you could say the missingness depends on the other column making it MAR.


### Hypothesis Testing
Null: Power outages are not restored more quickly in economically robust areas

Alternative: Power outages are restored more quickly in economically robust areas

Test statistic: Difference in group means
 This test staistic is the best since the two distributions (the restoration times for power outages in economically robust areas and the restoration times for power outages in non-economically robust areas) are numerical. Since the two graphed distributions are roughly the same shape, Kolmogorov-Smirnov test statistic would be an inappropriate choice, and so difference in group means is an appropriate statistic.

Signficance level: a=0.05, a good, practical threshold since it ensures that if the null hypothesis was true, there would only be a 5% chance you would see such extreme of a restoration time.

Since the p-value is 0.086, which is greater than the previously set significance level of 0.05, we fail to reject the null hypothesis that power outages are not restored more quickly in economically robust areas.


### Framing a Prediction Problem
Prediction problem: Predict the cause of a major power outage.
Prediction Type: Classification (Multiclass Classification)
Response variable: "CAUSE.CATEGORY" which I chose since when you are experiencing a power outage, the only thing on your mind in the rush of the moment is what's happeneing and why, and so being able to most accurately predict the cause can be a huge relief to people.

The metric you are using to evaluate your model: Accuracy, since in the heat of a power outage, recall isn't a necessity but rather accurracy and trying to get the model to predict the correct CAUSE.CATEGORTY as often as possible.False positives or false negatives don't have an overweighing impact, simply getting the answer and the being as close as possible is the necessty.

At the time of your prediction, you know the "PC.REALGSP.STATE","TOTAL.SALES","NERC.REGION","PCT_WATER_TOT," so these are all relevant predictors. I wanted to use "OUTAGE.DURATION" as a predictor but the outage duration is not information that would be known at the time of the prediction so it cannot be used as one of our classification predictor


### Baseline Model
My model uses a RandomForestClassifier of max_depth=2 and criterion='entropy' in order to make my classification. A ColumnTransformer is used in order to make transformations in a single pipeline. My model has 2 features "PCT_WATER_TOT" and "NERC.REGION" which are used in order to make my classifications. "PCT_WATER_TOT" is quantitatuve, while "NERC.REGION" is nominal, while is why I had to HotOneEncode it inside the column transformer and I could just passthrough "PCT_WATER_TOT". My mode did not perform too well, as it had a training accuracy of 0.5664845173041895 and a testing accuracy of 0.5382513661202186. My model is good in that my testing acciuracy wasn't too far off from my training accuracy. However, I believe my current model could be better since it is only based on 2 features and a complex problem like multiclass classification should involve more of a broad spectrum of features.

### Final Model


### Fairness Analysis

<!-- # My Project Title

by Suraj Rampure (rampure@ucsd.edu)

***Note***: If you choose a repo name and title as uninspired as the ones here, I will be quite sad.

---

## Introduction

In this project, we studied the effectiveness of spice challenges in building team morale.

---

## Cleaning and EDA

<iframe src="assets/10-80-enrollment.html" width=800 height=600 frameBorder=0></iframe>

---

## Assessment of Missingness

Here's what a Markdown table looks like. Note that the code for this table was generated _automatically_ from a DataFrame, using

```py
print(counts[['Quarter', 'Count']].head().to_markdown(index=False))
```

| Quarter     |   Count |
|:------------|--------:|
| Fall 2020   |       3 |
| Winter 2021 |       2 |
| Spring 2021 |       6 |
| Summer 2021 |       4 |
| Fall 2021   |      55 |

---

## Hypothesis Testing

' px.scatter(x=inputd["POPDEN_RURAL"], y=inputd["OUTAGE.DURATION"], title='OUTAGE DURATION for Rural Populatin Densities', labels={'x': 'Rural Population Density', 'y': 'Power Outage Duration'})''
'fig.write_html('file-name.html', include_plotlyjs='cdn')'


--- -->
