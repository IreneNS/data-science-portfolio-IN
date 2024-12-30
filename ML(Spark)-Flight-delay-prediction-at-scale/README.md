# Project: "FlightForesight: Predicting Delays with ML (Parallel Processing and Time Series Data Prediction)"

Welcome to project 'FlightForesight: Predicting Delays with ML' repo page! To use this starter repo simply set up your [git client](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) and clone the repo with the SSH link.

This is initially a group project with more than one contributors. The overall results and other contributors are presented in the presentation pdf, and report html under this repo. However, for fairness purpose, I only include my portion of code in this repo for my project portoflio demonstration.

## Introduction and Project Objective

- Efficiently managing on-time flight arrivals and departures is a critical logistical challenge influenced by various factors such as weather, airports, and aircraft. While considering all potential disruptions is impractical, delay predictions can be made using historical and real-time data, aiding airlines in contingency planning and improving passenger satisfaction.

- To address this, our project aims to predict the likelihood of departure delays two hours before departure using flight, airport and weather data from the U.S. Department of Transportation and the National Oceanic and Atmospheric Administration from 2015 to 2019. 

- To reflect the cost analysis results, we chose F2 score of delay over 15 minutes as the main prediction metric, as delays more than 15 minutes tend to cost more. The F2 score, a measure that balances precision and recall with more focus on recall, is particularly relevant in our context as both false positive and false negative are costly, but high false negative (low recall) costs more to the airlines. In short, our goal is to build a multi-class classification predictor, focusing on achieving balanced precision and recall for the delay group.

## Key Takeaways and Results

- The key challenges of this project lie in (1). the formidable size, (2). imbalanced portion of inputs data, and (3). time series nature of the input and prediction problem. Therefore, parallel processing is required, data imbalance needs to be treated, and data leakage needs to be very carefully prevented throughout the processes. A lot of the traditional cookie-cutter ML processes are no longer applicable here. 

- After careful data pre-processing, we added derived features from airports and flights based on lag, recency, centrality and time series, ensuring there is no data leakage in the process. As it turns out, the feature derived from lag information becomes the most important contributor. 

- Logistic regression (LR) was used as a baseline for modeling. It achieved a 48.4% F2 score for delays over 15 minutes, significantly outperforming random guessing (given original delay group counts for <20% of data). Advanced models such as random forest, XGBoost, and neural networks further improved F2 scores to 53.5%, 55.5%, and 56.4%, respectively. Specifically, for neural networks, we tried MPC (F2 = 40.6%), GRU (F2 = 54.3%), and TCN (F2 = 56.4%).

- We recommend TCN for its superior performance but suggest Random Forest for interpretability. Overall, XGBoost performed efficiently for computation requirements and has comparable F2 scores to our highest-scoring model. Depending on airline priorities, they can choose one of these models.

## Data Sources

- The flight dataset was downloaded from the [US Department of Transportation](https://www.transtats.bts.gov/homepage.asp) to an external site. and contains flight information from 2015 to 2021
(Note flight data for the period [2015-2019] has the following dimensionality  31,746,841 x 109). 

- The weather dataset was downloaded from the [National Oceanic and Atmospheric Administration repository](https://www.ncei.noaa.gov/access/metadata/landing-page/bin/iso?id=gov.noaa.ncdc:C00679) and contains weather information from 2015 to 2021. The dimensionality of the weather data for the period [2015-2019] is 630,904,436 x 177.

- The airport dataset was downloaded from the [US Department of Transportation](https://www.transtats.bts.gov/homepage.asp) and has the following dimensionality: 18,097 x 10.

- We utilize the carefully curated 5-year (2015-2019) OTPW dataset, which is a result of merging the airport dataset with the weather dataset. The original data was in CSV format, but for the sake of efficiency, we converted it to Parquet for the subsequent processing steps.

## The Key Challenges

The first and foremost challange of this project lies in the formidable size of input data (31.6mm x 214 in size, ~5.5GB after joining flight and weather data). Thereofore python/pandas itself would not be possible to process such input, we have to utilize parallel processing enabled by Apache Spark environement and infrastructure. 

Secondly, the group that matters to us, the delay group, is the minority group in data with only ~20% of data presentation, making the prediction problem much harder for this group than other groups. Careful data imbalance treatment is needed in our project.

Thirdly, the time series nature of input data and prediction makes it extremely important to understand, process and analyze data thoroughly and appropriately to avoid leakage (i.e. using future information to predict the past event) and prevent artificially inflated prediction results. This is also a inplicit test for our team's intellectual honesty throughout the project. 

## Pipeline Design
- Step 0: Preprocessing 
- Step 1: Post processing EDA
- Step 2: Feature Selection and Feature Engineering
  - Careful feature screening and engineering are done to keep the most relevant features, derive new featues while avoid leakage 
- Step 3: Scaling, and Encoding
  - Scale numerical and label encoded features using min-max scaler to combat skewness;
  - Encode categorical variables to numerical representation, adapt binary encoding when dimension reduction is needed.
- Step 4: Cross Validation and Grid Search
  - Time series aware cross validation (block CV) is used to prevent leakage
- Step 5: Model Training and Evaluation
- Step 6: Find optimal algorithm and fine-tune

## High-Level Summary of Data Description (pre-processing and EDA)
- **Data Preprocessing and EDA**: 
    - Data cleansing: various data cleansing and evaluation are done to make sure data are in appropriate shape, no missing, duplication or invalid data before analysis
    - Data visualization and evaluation: use extensive yet selective examinations to sort and describe the data clearly and purposefully
- **Key Takeaways**:
    - This data has 31.6 million rows and 214 columns (~5.53GB). The columns are features about flight information and weather at origin airport. The flight and weather hourly data are joined two hours ahead of the planned departure time, which is considered an appropriate time to make the delay prediction.
    - We note that the dataset is highly imbalanced with a majority of flights (~82%) recording on-time departures (on-time is defined as no more than 15 minutes' delay). Therefore we decide to regroup the label into three groups, which gives the best balance of distribution without losing business meaning. Further, we will treat this imbalance before the training by way of undersampling.
    - Nearly 50% of the columns have >90% of missing values. After preprocessing, we kept ~100 columns and most of the original entries.
    - Duplicated entries are limited. We eliminated all duplicated entries.
    - Out of remaining 100+ columns, 19 are numerical, and the rest being categorical.
    - Spearman correlation analysis shows that none of the native ex-ante available numerical variables have more than 10% correlation with target variable; all the highly correlated variables are only ex-post available (i.e. only available after our prediction time).
    - Many categorical variables show promising class-wise differentiation across different delay groups, indicating good predicting power. Such variables include time related variables (time of the day, quarter, month of the year, day of the month), entity related (carrier), and airport related (airport types).

## Further Analysis Involved 
    
- **Feature Selection and Engineering**:
    - In feature selection, we went through the feature throughly and select most relevant features in three main ways (1). remove highly correlated numerical variables; (2). remove variables that are repetitive in meaning or only for joining purpose; (3). exclude ex-ante unavailable features but keep them for later feature engineering purpose.
    - Given the native usable (ex-ante) numeric features are quite limited, feature engineering becomes a key part of the project to imporove model performance.
    - Feature engineering: we managed to create twelve newly derived features, utilizing those highly relevant but not directly available featuers, based on lag, centrality, recency, and time series fattens, which collectively improved the prediction performance by 3-15% depending on the model.  

- **Baseline Model (logistic regression)**: 

    - Given it is a classification problem, we conduct logistic regression as baseline model instead of using average. Raise the bar for later models to a much higher level. 
    - We decide to use F2 for the delay group (minority group) as the key performance metrics, to strive for a balanced performance measure between recall and precision, with priority tilted to recall, based on cost analysis for airlines (out main audience). 
    - For all our models, we train the model with under-sampled data to further counter the data imbalance issue.
    - It achieved a 48.4% F2 score for delays over 15 minutes, significantly outperforming random guessing (given original delay group counts for <20% of data), taking only 2.3 minutes to run the last model.
    - Feature importance analysis reveals that the most important two features are the new features derived based on lag and recency.

- **Conduct Various of Advanced Models (applying consistent metrics to compare to baseline)**:
    - Neural Network
        - Three architectures are explored: Multilayer Perceptron Classifier (MPC), Temporal Convolutional Network (TCN), and Gated Recurrent Unit (GRU), which achieved target group F2 of 40.6%, 56.4%, and 54.3% respectively, with 21 to 600+ mins in last model training.
        - The main takeaways are that TCN is the most appropriate application for current case, which achieved superior results. It is great for capturing long-range dependencies in time-series data efficiently and good for companies which can afford the higher computational cost
    - Random Forest
        - Careful grid searches are done to search for the best performing parameters with feasible training time. Given the long time to train, two hyperparameters are focused on for grid search. The best parameter combination resulted in 53.5% in test, slightly higher than train (52.3%), taking 15 minutes to train last model.
        - Feature importance analysis showed that ~80% of the importance were attributed to top 10 features (out of 150 features), 8 out of the top 10 features are new features derived by us.
        - The main takeaways are that random forest could deliver decent performance with good interpretable results. And it takes less time to train compared to neural network. 
    - XG Boost
        - Multi-stage grid searches are done to narrow down most relevant parameter combinations. In the final stage, 36 combinations of parameters on 5-year data are used to find the best combination of hyper parameters. The best combination resulted in 55.5% F2 in test, only 2% lower than train results (57.5%), with only 5.4 minutes in last model training.
        - Feature importance analysis revealed that the most importance features are the new features derived based on lag, centrality, and native features such as distance and elevation.
        - The key takeaways are that XGBoost does not only deliver superior performance (only <1% lower than the best performing TCN), but also parallels very well for large dataset, which takes comparable time to train as the baseline, and <10% of the time as TCN.

## Overall result

Optimizing models for the F2 score for the class more than 15-minute delay, our baseline Logistic Regression model has a score of 48.4% (which is better than a random guess for this minority class), and our best scoring TCN model has a score of 56.38%. Based on score, we suggest airlines use TCN as the model, which takes a lot of computing and time to train. Still, if computing and time are the limiting concerns, we recommend using a model that has optimal mix of performance and score XGBoost for further training and inference, which, in comparison, has a 1% less F2 score but completes training much faster. 

## Repository Structure

- **data/**
    - The data is not hosted in Github.

- **analysis-combined/**
  - Contains modularized Jupyter notebooks used for data loading, data preprocessing, EDA, feature engineering, encoding and scaling, baseline model, and various experiments for ML modeling and evaluation. The folder includes the following Jupyter notebooks and related htmls (my part of the code):
    - **Data EDA**: IN- post-processing-EDA-w261_final_project.ipynb
    - **Feature engineering, Encoding and Scaling**: IN- feature-engineering-encoding-notebook-version.ipynb
    - **Baseline Model (logistic regression)**: IN-Logistic-Regression-w261_final_project.ipynb
    - **XG Boost**: IN-XGBoost -w261_final_project.ipynb


- **README.md**
  - This file, detailing the project summary and repo structure.
