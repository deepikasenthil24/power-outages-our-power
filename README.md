
# Power Outages: An Investigation into What's Really in Our Power
 This is a project for DSC80 at UCSD in which I will be cleaning my dataset, performing exploratory data analysis, assessing missingness, conducting permutation tests, creating a classification model, and judging fairness.

 **By** Deepika Senthil

### Introduction

Power outages affect everyone and worse, they make people feel powerless. It is through learning more about outages, specifically why they occurred and what to typically expect when one must be endured that people will be able to feel more prepared and more in control. That is what my question which this analysis is based on aims to do, answering the question: **What causes power outages and who are most affected by them?** The first part will be answered by checking which characteristics (columns) in the dataset can best predict the cause of an outage. The second part of the question will be answered by analyzing whether whenever a power outage occurs certain groups of people (those in more economically robust areas) will have their power restored in a more timely manner and will therefore have to bear less of an effect of the outage in their lives.

The dataset that is going to be analyzed is the "Major Power Outage Risks in the U.S." dataset collected by Purdue University, outlining the major outages that have occurred across different states in the continental US from January 2000 to July 2016 and well as including other characteristics of each outage. It is of these characteristics that we will be deciding which are the most relevant to answering the key exclamations that characterize outages: "How long will it take?" and "WHY?". The dataset contains a total of 1534 observations (rows), 1476 of which we will consider. The dataset also has 56 columns representing the various characteristics and potential features in answering the aforementioned explanations. The columns that are most relevant to my question are the duration of the power outages ("OUTAGE.DURATION"), the peak demand lost during the outage ("DEMAND.LOSS.MW"), ("CLIMATE.CATEGORY"), the NERC region affected by the outage ("NERC.REGION"), the percentage of water area in the state of the outage compared to the water area in the continental US ("PCT_WATER_TOT"), the per capita real GSP of the state in which the outage occurred ("PC.REALGSP.STATE"), the electricity consumption in the industrial sector in the area of the outage (IND.SALES), and the name of the hurricane if the cause of the outage was a hurricane ("HURRICANE.NAMES"), so these columns will be popping up throughout this analysis.

### Data Cleaning and Exploratory Data Analysis
The data cleaning process is necessary to ensure a proper analysis. I first removed rows and columns with information other than the data recorded of the characteristics of the outages. I then created two new columns ('OUTAGE.START' and 'OUTAGE.RESTORATION') using the 'OUTAGE.START.DATE' column with the 'OUTAGE.START.TIME' column as well as the 'OUTAGE.RESTORATION.DATE' column with the 'OUTAGE.RESTORATION.TIME' column respectively, converting to Timedelta and TimeStamps to do so. Using these new columns, I then filled in the missing values of the "OUTAGE.DURATION" columns with the difference between the 'OUTAGE.START' and 'OUTAGE.RESTORATION' columns. I then removed columns that were irrelevant to my analysis, keeping ones that might be relevant later on, however. Finally, I created a new column "robust" that contains a value of 1 if the "PC.REALGSP.STATE" is greater than 60000, 0 otherwise, to represent whether a state is economically robust or not. The value of 60000 as a marker of economic robustness was obtained from US News' ranking of the best state economies.

|   MONTH | NERC.REGION   | CLIMATE.CATEGORY   | OUTAGE.START.DATE   | OUTAGE.START.TIME   | OUTAGE.RESTORATION.DATE   | OUTAGE.RESTORATION.TIME   | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   HURRICANE.NAMES |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED |   TOTAL.PRICE |   IND.SALES |   TOTAL.SALES |   PC.REALGSP.STATE |   PC.REALGSP.CHANGE |   UTIL.REALGSP |   TOTAL.REALGSP |   PCT_LAND |   PCT_WATER_TOT | OUTAGE.START        | OUTAGE.RESTORATION   |   robust |
|--------:|:--------------|:-------------------|:--------------------|:--------------------|:--------------------------|:--------------------------|:-------------------|:------------------------|------------------:|------------------:|-----------------:|---------------------:|--------------:|------------:|--------------:|-------------------:|--------------------:|---------------:|----------------:|-----------:|----------------:|:--------------------|:---------------------|---------:|
|       7 | MRO           | normal             | 2011-07-01 00:00:00 | 17:00:00            | 2011-07-03 00:00:00       | 20:00:00                  | severe weather     | nan                     |               nan |              3060 |              nan |                70000 |          9.28 |     2113291 |       6562520 |              51268 |                 1.6 |           4802 |          274182 |    91.5927 |         8.40733 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |        0 |
|       5 | MRO           | normal             | 2014-05-11 00:00:00 | 18:38:00            | 2014-05-11 00:00:00       | 18:39:00                  | intentional attack | vandalism               |               nan |                 1 |              nan |                  nan |          9.28 |     1887927 |       5284231 |              53499 |                 1.9 |           5226 |          291955 |    91.5927 |         8.40733 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |        0 |
|      10 | MRO           | cold               | 2010-10-26 00:00:00 | 20:00:00            | 2010-10-28 00:00:00       | 22:00:00                  | severe weather     | heavy wind              |               nan |              3000 |              nan |                70000 |          8.15 |     1951295 |       5222116 |              50447 |                 2.7 |           4571 |          267895 |    91.5927 |         8.40733 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |        0 |
|       6 | MRO           | normal             | 2012-06-19 00:00:00 | 04:30:00            | 2012-06-20 00:00:00       | 23:00:00                  | severe weather     | thunderstorm            |               nan |              2550 |              nan |                68200 |          9.19 |     1993026 |       5787064 |              51598 |                 0.6 |           5364 |          277627 |    91.5927 |         8.40733 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |        0 |
|       7 | MRO           | warm               | 2015-07-18 00:00:00 | 02:00:00            | 2015-07-19 00:00:00       | 07:00:00                  | severe weather     | nan                     |               nan |              1740 |              250 |               250000 |         10.43 |     1777937 |       5970339 |              54431 |                 1.7 |           4873 |          292023 |    91.5927 |         8.40733 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |        0 |



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

|   MONTH |      cold |   normal |      warm |
|--------:|----------:|---------:|----------:|
|       1 | 0.588235  | 0.191176 | 0.220588  |
|       2 | 0.675676  | 0.189189 | 0.135135  |
|       3 | 0.191489  | 0.510638 | 0.297872  |
|       4 | 0.262295  | 0.57377  | 0.163934  |
|       5 | 0.0909091 | 0.792208 | 0.116883  |
|       6 | 0.164835  | 0.769231 | 0.0659341 |
|       7 | 0.0795455 | 0.693182 | 0.227273  |
|       8 | 0.402778  | 0.375    | 0.222222  |
|       9 | 0.262295  | 0.508197 | 0.229508  |
|      10 | 0.285714  | 0.375    | 0.339286  |
|      11 | 0.355556  | 0.4      | 0.244444  |
|      12 | 0.40625   | 0.234375 | 0.359375  |

In this pivot table, you can see the distribtion of the number of power outages that occur in each climate region for each month. The number of outages that occur at the start and end of the year is higher for those in the cold climate region than in the normal or warm climate regions. However, a few months in, those in the normal climate region start having a higher porportion of power outages. The number of power outages in the warm climate region vary a lot less. Given that severe weather was the most common cause of power outages, it makes sense that those in colder climate categories might have more power outages in the winter months characterzied by their severe weather while warm climate areas arent characterized by such varying weather and thus have less varying power outage proportions.


### Assessment of Missingness
I believe that there are columns in this dataset that could be considered NMAR due to this dataset focusing on the collection of power outage data for major power outages, particularly the total number of customers that were affected due to power outages. This is why the missing values in the "CUSTOMERS.AFFECTED" column are NMAR since the values that are missing likely aren't recorded because the actual value itself is too small. Additional data I might want to obtain to explain the missingness (making it MAR) could be a column that details whether the power outage is categorized as a "Major Power Outage" or a "Non-Major Power Outage", since that way you could say the missingness depends on the other column making it MAR.


<iframe
  src="assets/xillis_count6.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


Null hypothesis: Missingness in hurr_name_missing does not depend on the IND.SALES column (MCAR)

Alternative hypothesis: Missingness in hurr_name_missing does depend on the IND.SALES column (MAR)

Significance level: 0.05

After performing a permutation test to see if the missing values in "Hurricanes" depends on "IND.SALES", I got a p-value of 0.114, which is greater than my set significance level of 0.05, so I fail to reject my null hypothesis, which signifies that the missingness does not depend on the "IND.SALES". Above is a KDE plot representing the distrubution of IND.SALES when the hurricane name is missing and the distribution when it's not. Since the two graphs are different shapes, a Kolmogorov–Smirnov test statistic should be used!


### Hypothesis Testing
Null: Power outages are not restored more quickly in economically robust areas

Alternative: Power outages are restored more quickly in economically robust areas

Test statistic: Difference in group means <- this test staistic is the best since the two distributions (the restoration times for power outages in economically robust areas and the restoration times for power outages in non-economically robust areas) are numerical. Since the two graphed distributions are roughly the same shape, Kolmogorov-Smirnov test statistic would be an inappropriate choice, and so difference in group means is an appropriate statistic.

Signficance level: a=0.05 <- a good, practical threshold since it ensures that if the null hypothesis was true, there would only be a 5% chance you would see such extreme of a restoration time.

Since the p-value is 0.086, which is greater than the previously set significance level of 0.05, we fail to reject the null hypothesis that power outages are not restored more quickly in economically robust areas.


### Framing a Prediction Problem
Prediction problem: Predict the cause of a major power outage.

Prediction Type: Classification (Multiclass Classification)

Response variable: "CAUSE.CATEGORY" which I chose since when you are experiencing a power outage, the only thing on your mind in the rush of the moment is what's happening and why, and so being able to most accurately predict the cause can be a huge relief to people.

Metric to evaluate your model: Accuracy, since in the heat of a power outage, recall isn't a necessity but rather an accuracy and trying to get the model to predict the correct CAUSE.CATEGORY as often as possible. False positives or false negatives don't have an overweighing impact, simply getting the answer and being as close as possible is a necessity.

At the time of your prediction, you know the "PC.REALGSP.STATE", "TOTAL.SALES", "NERC.REGION", and "PCT_WATER_TOT" since these are all values that stay constant for a longer amount of time (ex: the percentage of water in the area doesn't change much) and so they are all relevant predictors. These values can be contrasted with "OUTAGE.DURATION", which I wanted to use as a predictor but the outage duration is not information that would be known at the time of the prediction so it cannot be used as one of our classification predictors.


### Baseline Model
My model uses a RandomForestClassifier of max_depth=2 and criterion='entropy' in order to make my classification. A ColumnTransformer is used in order to make transformations in a single pipeline. My model has 2 features "NERC.REGION" and "CLIMATE.CATEGORY" which are used in order to make my classifications. Both are nominal, so I OneHotEncode them inside the column transformer and I could just passthrough "PCT_WATER_TOT". My mode did not perform too well, as it had a training accuracy of 0.5664845173041895 and a testing accuracy of 0.5081967213114754. My model is somewhat good in that my testing accuracy wasn't too far off from my training accuracy. However, I believe my current model could be better since it is only based on 2 features and a complex problem like multiclass classification should involve more of a broad spectrum of features.

### Final Model
The two other features I added were "MONTH" and "IND.SALES". I thought these were the best features to add to increase my accuracy since the MONTH it is often has a high effect on the conditions outside and since severe weather was the most common cause of power outages, I predicted that adding in MONTH as a feature could better my model, which it did! The other feature I purposefully decided to include was "IND.SALES". Since the industry sector often hogs up natural resources such as electricity consumption, I predicted that out of the 3 features related to the commercial sector, the residential sector, and the industry sector, the feature for the industry sector would be the most appropriate.

The modeling algorithm I chose is once again using a RandomForestClassifier to perform classification. I utilized a ColumnTransformer to OneHotEncode as well as Binarize my features, my features this time building off of the features in my Baseline model as well as including a few more, one's that I hinted at in the conclusion of my Baseline construction as steps I could take to better my Baseline model. My model has become better after adding my new parameters as well as after being fit with the new hyperparameters. The hyperparameters my GridSearch outputted as being the best performing were a max_depth of 1, a min_samples_split of 5, and an n_estimators of 150. While my baseline model has a testing accuracy of 0.5081967213114754, my new Final Model has a testing accuracy of 0.644808743169399, a more than 20% increase.


### Fairness Analysis
Let's now test this final model for fairness!

Group X: the industrial sector's electricity consumption that is lower (less than 2914897)
Group Y: the industrial sector's electricity consumption that is higher (greater than or equal to 2914897)
Evaluation metric: Accuracy

Null Hypothesis: The model performs the same for the industrial sector's electricity consumption that is higher (greater than or equal to 2914897) and low (less than 2914897).
   
Alternative Hypothesis: The model performs worse for the industrial sector's electricity consumption that is higher (greater than or equal to 2914897) than the industrial sector's electricity consumption that is lower (less than 2914897).

Significance Level: 0.05
Test statistic: Difference of Group Means

Since the resulting p_value of 0.102 is greater than 0.05, we cannot reject the null hypothesis that the model performs the same for higher electricity consumption by the industrial sector and lower electricity consumption by the industrial sector.
