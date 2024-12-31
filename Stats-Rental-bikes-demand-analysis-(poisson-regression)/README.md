# Project: "Rental-Bikes-Demand-Analysis"

Welcome to project 'Rental-Bikes-Demand-Analysis', a statistical analysis focusing on predicting rental bikes in South Korea using weather, seasonality and other related factors. 

This is a classic problem with target variable being non-negative, discrete count-like variables. Although traditional OLS regression is often thought as the first model to try, we took a different route of Poisson regression model to fully consider the non-negative and discrete nature of the target variable. 

We further took the note that even Poisson regression may not be the best fit for the data on hand. More advanced and nuanced model such as negative binomial model should be applied to this problem to overcome overdispersion issue, and to be statistically rigorous.

This is initially a group project with more than one contributors. The overall results and other contributors are presented in the presentation pdf under this repo.

## Introduction

Rental bikes are an essential transportation service in many cities, supporting both work commutes and leisure activities. One city in which rental bikes are a key public service is Seoul, South Korea. It is important for the city to ensure rental bike availability and accessibility, lessening wait times during peak demand. To do so, the city needs to be able to predict how many rental bikes need to be available on different days and at different times. 

In this analysis, we aim to provide decision makers with insights into the factors influencing rental bike usage and demand, facilitating availability for the public by controlling for relevant factors such as weather conditions, seasonality, and time of day. We hypothesize that these factors, particularly temperature, season, and time of day, significantly impact rental bike demand, with specific conditions like higher temperatures and rush hours leading to increased usage.

## Source of Data

The dataset contains the count of public bicycles rented per hour in the Seoul Bike Sharing System, with corresponding weather data and holiday information. Unfortunately, the data collection and sampling process is not explicitly detailed in the provided dataset documentation, which limits our understanding of potential biases or limitations in the data.

## High-Level Summary of Data Description

The data consists of 8760 entries, and 13 features in total, including:

- **Rented bike count**: hourly rented bike count in Seoul from Jan 2017 to Nov 2018.
- **Weather related information**: such as temperature, dew point temperature, humidity, windspeed, visibility, solar radiation, snowfall, rainfall, etc.
- **Seasonality related information**: such as the season of the year, hour of the day, holiday or not.
- **Status related information**: such as if the rental bikes stations are functional or not.

## Analysis involved

- **Exploratory data analysis (EDA)**: Variables are first divided into numerical and categorical. Then the correlation among numerical variables and their scatter plot relationship with target variable were explored to identify most relavant numerical features. For categorical variables, per class target variable distribution for each categorical variable were explored to identify most important categorical variables.  

- **Model development (Poisson regression)**: 
  - We believe that Poisson regression is the more theoretically correct model here compared to OLS, given the non-negative and discrete nature of the target variable. Three progressive models were developed step by step to examine the impact of additional features. AIC, BIC and anova tests are used to select the most appropriate model.
  - Model1: we first built a Poisson regression model with the top five numerical variables and top three categorical variables, considering only linear representation of features
  - Model2: We then added in variables which may not show strong correlation with target variable, but logically have important impact on target variable, such as wind speed, snow fall and rain fall.
  - Model3: We lastly added in quadratic presentation of features which displayed non-linear relationship with target variable in EDA, and likely relevant interaction terms between variables.
  - The results of the incremental anova tests indicate that each increase in complexity, from Model 1 to Model 2, and from Model 2 to Model 3, adds
 a statistically significant improvement to the overall fit of the model. This is consistent with the monotonously decreasing AIC/BIC from Model1 to Model3, suggesting Model 3 as the best model among the all.

- **Model Assessment and Alternative Model Exploration**:
 - In order to be statistically rigorous, we conducted overdispersion test on the Poisson Model 3 residuals. Interestingly, the test shows significant sign of overdispersion, suggesting that the Poisson regression is likely not a great fit for the provided data.
 - With the purpose of finding the statistically appropriate model for the provided data, we went on to explore two more alternatives
  - OLS: despite the overall lower AIC/BIC than Poisson model, OLS couldn't overcome its inherent issue of producing negative prediction for non-negative target variable, and overestimating at high level of input features. This is the reason why we avoided OLS in the beginning.
  - Negative Binomial: as it turns out, the Negative Binomial model is better suited for predicting bike rental counts as it handles overdispersion in the data more effectively than Poisson regression.

## Overall result and conclusion

- In this analysis, we aimed to investigate the factors influencing bike rental demand using the SeoulBikeData dataset. Our primary hypothesis was that various weather conditions—such as temperature, humidity, and rainfall—along with time-related factors like the hour, seasons, and holidays, significantly impact bike rental counts. In the process we started with Poisson regressions given its more appropriate theoretical application for the problem on hand. However, the Poisson model revealed significant overdispersion in residuals. 

- Our key findings show that the Negative Binomial model is more appropriate for this dataset as it handles non-negative count data and accounts for the overdispersion observed in the Poisson model. The OLS model produces some negative fitted values and overpredicts for larger values, making it unreliable for this type of data. 

- The Negative Binomial regression, particularly derived from our Model 3 Poisson, indicates that temperature, humidity, rainfall, and wind speed, along with interactions between time and holiday variables, are important factors in predicting bike rentals. The implications of these findings suggest that bike-sharing systems can optimize bike availability by considering these factors in real-time demand forecasts.  

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
