# Project: 'Forecast CO2 Emission Time Series (The "Keeling Curve")'

Welcome to project 'Forecast CO2 Emission Time Series (The "Keeling Curve")', a statistical analysis focusing on forecasting global CO2 emission using different statistical methods, with this fundamental research question: 
*Is the growth of carbon dioxide (CO2) concentrations (partially or entirely) due to a deterministic trend, which can be associated with human activities such as fossil fuel combustion? Or is it mostly due to a stochastic process with a positive drift, thus harder to link to human activities?* 

This is initially a group project with more than one contributors. The overall results and other contributors are presented in the presentation pdf under this repo.

## Introduction

In the 1950s, the geochemist Charles David Keeling observed a seasonal pattern in the amount of carbon dioxide present in air samples collected over the course of several years. In 1958 Keeling began continuous monitoring of atmospheric carbon dioxide concentrations from the Mauna Loa Observatory in Hawaii and soon observed a trend increase carbon dioxide levels in addition to the seasonal cycle. He was able to attribute this trend increase to growth in global rates of fossil fuel combustion. This trend has continued to the present and is known as the "Keeling Curve".

The rising CO2 concentrations and their consequences, including global warming, ocean acidification, and rising sea levels are widely impacting human life around the globe. The repercussions of these impacts are extreme weather events, ecosystem degradation, agricultural disruption, and significant economic costs.

In light of the background, one research question comes to our mind is - *Is the growth of carbon dioxide (CO2) concentrations (partially or entirely) due to a deterministic trend, which can be associated with human activities such as fossil fuel combustion? Or is it mostly due to a stochastic process with a positive drift, thus harder to link to human activities?* 

In this analysis, we aim to apply different statistical methods corresponding to the two different hypotheses of the research question, and see which method is more consistent with reality.

Specifically, we split our data into before (and include) 1997 Dec, and after 1997 Dec, conducts two parts of analysis. The first part serves as a training set where we experiment with different modeling methods. The second part is used as a testing set where we evaluate the models with unseen data, and train new models incorporating the most recent data and evaluation observations. 


## Source of Data

Give the wide public attention on CO2 emmisions, the dataset before 1997 Dec is available in R datasets itself, and the full dataset including data post 1997 Dec is available in public domain with the following links: [monthly_data] (https://gml.noaa.gov/webdata/ccgg/trends/co2/co2_mm_mlo.txt); [weekly_data](https://gml.noaa.gov/webdata/ccgg/trends/co2/co2_weekly_mlo.txt)

## High-Level Summary of Data Description

The focused combined dataset is a monthly time series of CO2 emission from 1958 to 2024. Additional weekly data is available for post 1997 data as well. 

## Analysis involved

- **Analysis from 1958 to 1997**:
  - **EDA**: CO2 level, difference and log differences were assessed based on their respective time series plot, ACF, PACF and distribution. It is revealed that in order to detrend and maintain stable dispersion over time, we should focus on the differences of the log transformation in the stochastic process modeling (ARIMA). Meaningful seasonality is observed in the data.
  - **Regression modeling**: various regression models were applied to fit the data. The model with log transformation as target, linear trend, quadratic trend and seasonality as regressors produced the best in-sample fit. However, the residuals of the best fit regression are not stationary, indicating a certain degree of model misspecification.
  - **ARIMA modeling**: with best educated guess based on EDA, and a few iterations of modeling, checking residual diagnostics to see if the residuals resemble white noises, we found the best fit SARIMA model with (0,1,0)(0,1,1)[12] as parameter. It is not only logically reasonable but also results in well-behaved white noise residuals.
  - **Main takeaways**: Regression model represents the hypothesis that CO2 emission is driven by deterministic force (such as human activities), while ARIMA model represents the hypothesis that CO2 emission is primarily driven by stochastic process. We noted that ARIMA model produces lower RMSE and MAE in-sample, suggesting a better in-sample fit. However, a two-year forecast ahead reveals that ARIMA model tends to produce much larger forecast uncertainty than regression.

- **Analysis from 1987 to 2024**:
  - **EDA**: a similar EDA analysis is done for the period post 1997 as it was done for the period pre 1997. Monthly data patterns are very similar to the analysis in the first half.
  - **Regression model assessment**: The predicted value from the picked regression model and realized CO2 levels track closely together which indicates that our regression model fits the data well even beyond 1997. Both the trend and the seasonal
 pattern (e.g., periodic fluctuations) are well-captured by the model.
  - **ARIMA model assessment**: ARIMA model effectively captures both the upward trend and the seasonal fluctuations of the Keeling Curve. However, it consistently underpredicts actual CO2 levels, particularly in later years. This very observation could be due to either CO2 growth is really
 not a stochastic process, or the model is forecasting too far in the future with dated information, which naturally hinders the performance.
  - **ARIMA model update**: to incorporate more recent information, we analyzed and generated an updated ARIMA model using weekly data (more granular) up to 2022. To understand the model better, we also played around using seasonality adjusted and unadjusted data as inputs to fit ARIMA. Regression model is also retrained with the more recent data with the same structure. A short-term test (2-year) is done to assess their out-of-sample performance.
    - With short-term out-of-sample test, seasonality adjusted ARIMA models outperformed regression and non-seasonality adjusted ARIMA, suggesting its superior performance for short term forecast when the updated information is used. 
    - However, when forecasting in longer term without updated data, regression model outperformed the ARIMA meaningfully.
  

## Overall result and conclusion

- In this analysis, we start with the research question - *Is the growth of carbon dioxide (CO2) concentrations (partially or entirely) due to a deterministic trend, which can be associated with human activities such as fossil fuel combustion? Or is it mostly due to a stochastic process with a positive drift, thus harder to link to human activities?* 

- Overall, our analysis as of 1997 perspective and as of current perspective indicate that although with continuously updated information and model retraining, ARIMA model could do a good job in predicting CO2 concentration in short term future, polynomial regression, which inherently assumes a deterministic trend, does a much better job forecasting CO2 concentration even with dated information. Deterministic trends model outperformed stochastic trends model in out-of-sample meaningfully. 

- These results suggest that the CO2 series are more deterministic than stochastic in nature, which could relate to human activities. However, this also showcases that we have the potential to create change in this growing issue. The results of this analysis highlight an urgency for policy makers and individuals to take initiative in mitigating and reducing CO2 emissions.

## Project Repository Structure

- **data**
  - data directly from the source is saved here

- **code-and-analysis**
  - the rmd file (using langauge R) supporting the analysis is saved here 

- **report**
  - a report generated from the rmd file with Latex format is saved here

- **presentation**
  - a short final project presentation is presented here

- **README.md**
  - This file, detailing the project summary and repo structure.
