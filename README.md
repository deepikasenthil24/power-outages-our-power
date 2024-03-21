# Power Outages: An Investigation into What's Really in Our Power
 This is a project for DSC80 at UCSD in which I will be cleaning my dataset, performing exploratory data analysis, assessing missingness, conducting permutation tests, creating a classification model, and judging fairness.

 **By** Deepika Senthil

### Introduction

Power outages affect everyone and worse, they make people feel powerless. It is through learning more about outages, specifically why they occurred and what to typically expect when one must be endured that people will be able to feel more prepared and more in control. That is what my question which this analysis is based on aims to do, answering the question: **What causes power outages, and who is most affected by them?** The first part will be answered by checking which characteristics (columns) in the dataset can best predict the cause of an outage. The second part of the question will be answered by analyzing whether whenever a power outage occurs certain groups of people (those in more economically robust areas) will have their power restored in a more timely manner and will therefore have to bear less of an effect of the outage in their lives.

The dataset that is going to be analyzed is the "Major Power Outage Risks in the U.S." dataset collected by Purdue University, outlining the major outages that have occurred across different states in the continental US from January 2000 to July 2016 and well as including other characteristics of each outage. It is of these characteristics that we will be deciding which are the most relevant to answering the key exclamations that characterize outages: "How long will it take?" and "WHY?". The dataset contains a total of 1534 observations, 1476 of which we will consider. The dataset also has 56 columns representing the various characteristics and potential features in answering the aforementioned explanations. The columns that are most relevant to my question are the duration of the power outages ("OUTAGE.DURATION"), the peak demand lost during the outage ("DEMAND.LOSS.MW"), ("CLIMATE.CATEGORY"), the NERC region affected by the outage ("NERC.REGION"), the percentage of water area in the state of the outage compared to the water area in the continental US ("PCT_WATER_TOT"), the per capita real GSP of the state in which the outage occurred ("PC.REALGSP.STATE"), and the name of the hurricane if the cause of the outage was a hurricane ("HURRICANE.NAMES"), so these columns will be popping up throughout this analysis.

### Data Cleaning and Exploratory Data Analysis

### Assessment of Missingness

### Hypothesis Testing


### Framing a Prediction Problem


### Baseline Model

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