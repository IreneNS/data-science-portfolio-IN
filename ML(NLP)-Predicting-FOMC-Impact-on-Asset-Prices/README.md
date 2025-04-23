# Project: "Predicting The Impact of FOMC Communications on Market Returns"

Welcome to project 'Predicting The Impact of FOMC Communications on Market Returns' repo page! To use this starter repo simply set up your [git client](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) and clone the repo with the SSH link.

This is initially a group project with two contributors. The overall results from all contributors are presented in the presentation pdf. However, for fairness purpose, I only include my portion of code in this repo for my project portfolio demonstration.

## Introduction and Project Objective

- In any country, the central bank plays a pivotal role in economic policy formulation, establishing the tone of the economic environment in the short and medium run. Therefore, economic and financial market participants closely watch every move of their decisions, making Federal Open Market Committee (FOMC) communication one of the most market moving events in the U.S..  

- However, most of the current analysis focus solely on numerical or a blend of numerical and a particular aspect of non-numerical information (such as sentiment, or tone) to analyze the impact on the financial market.

- Our goal is to make use of the latest NLP techniques to extract predictive and tradable information from the FOMC documents directly. Specifically, we will analyze the FOMC meeting, conference call, and press conference transcripts with transformer-based models to extract relevant embedding representations. These representations will be explored to predict financial market reactions post monetary policy decisions, including changes in stock market returns and bond yields. 

## Key Takeaways and Results

- The key challenges of this project reside in a few areas, including (1). selecting the most relevant documents, extracting the relevant contents; (2). connecting the NLP technologies with our end goal - assets returns appropriately; (3). evaluation using both data science and investment metrics to prove the usefulness of the model outputs; (4). the scarcity of the events and therefore analyzable data. 

- We carefully sorted through all available FOMC related documents and decided to use those documents which are off-script in nature and released timelier after FOMC events. We gathered data from 1980 to 2024 to have meaningful amount of data and performed careful data cleaning and organizing. 

- Overall, we used both BERT and ModernBERT to perform two prediction tasks, predicting the direction of the following days' S&P returns (classification task), and predicting both the direction and magnitude of the following days' S&P returns (regression / prediction task). We used both data science metrics (accuracy for classification, MSE for regression), and investment metrics (Sharpe and maximum drawdown) to measure the effectiveness of our predictions.

- Our analysis showed that both models could produce meaningful predictions on S&P returns based on FOMC documents directly, especially judged by data science metrics. ModernBERT could improve the result of BERT. This effectively shows that FOMC documents, such as transcripts, have meaningful predictive ability to major asset returns (e.g. S&P Index), both directionally and with magnitude, which could assist asset managers and individual investors to manage their investment risk & return more appropriately.  

## Data Sources

- The practice of FOMC documents release is not consistent over time, for instance, conference calls were often held prior to FOMC meetings before 2011, and post FOMC press conferences only started from 2011; and meeting transcripts are stopped to be released from 2018. Given the more ‘off the script’ nature and timely release, we decide to focus on the transcripts of FOMC meetings, conference calls and press conferences as the main data source (the ‘FOMC Transcripts’). 

- There are typically eight FOMC per year, and we opt to use the past 45 years’ FOMC data for research (1980-2024), with the last 6 years as test dataset, and preceding 6 years as validation dataset. The Federal Reserve government website has detailed historical text based information related to the FOMC. The data is downloaded from [Federal Reserve government website](https://www.federalreserve.gov/monetarypolicy/fomc_historical_year.htm)


## Overall Methodologies and Evaluation Metrics

We used different transformer based encoder models as foundation to perform both classification and prediction tasks on FOMC Transcripts. For the classification, we will use FOMC Transcripts to predict the direction of the next 2-day S&P returns.  For the regression / prediction, we will predict the direction and magnitude of the next 2-day S&P returns. For evaluation metrics, we will use both typical data science metrics (accuracy for classification, MSE for prediction), and simple trading simulation’s performance based on the model predicted values. For trading simulations (the ‘Backtests’), we use Sharpe (a widely utilized metric in finance to measure return per unit of risk, the higher the better), and maximum drawdown (another widely recognized metric in finance to measure the max sudden loss occurred over the concerned period, the less negative the better) as main metrics.

## Models and High-Level Findings
- BERT (Baseline):
  - Classification Task: 
    - Goal: Use BERT to predict the directional change in S&P 2-day return using FOMC statements.
    - Key Findings: BERT achieves very modest improvements in directional accuracy (vs naive baseline), but deeper models struggle with generalization.
  - Regression Task:
    - Goal: Use BERT to predict the magnitude of S&P 2-day return using FOMC statements.
    - Key Findings: Smaller models outperform deeper ones in robustness (generalization in out of sample).

- ModernBERT:
  - Classification Task:
    - Goal: see if MordernBERT could improve the directional prediction of S&P next 2-day return using FOMC transcripts post release. Various parameter / structural searching experiments were done. 
    - Key Findings: ModernBERT is able to improve both the accuracy and backtest Sharpe over BERT (in and out of sample). BUT, the improvement is not from extra input length.
  - Regression Task:
    - Goal: see if MordenBERT could improve the regression prediction of S&P next 2-day return using the FOMC transcripts post release. 40+ parameter / structural searching experiments were done.
    - Key Findings: ModernBERT can improve out-of-sample loss and backtest Sharpe over BERT, but struggles to surpass the classification backtest. The change of Fed communication practice makes the more nuanced prediction harder to perform.

## Conclusion
- We experimented both BERT and ModernBERT to conduct directional and magnitude prediction tasks for S&P return using FOMC transcripts following FOMC events.
- Both models demonstrated the ability to make meaningful predictions, especially judging by data science metrics. ModernBERT is able to improve BERT results.
- This allows us to conclude that there is meaningful information in FOMC communication, which can be extracted to predict the market return directly, with proper LLM technologies application.
- However, the change of Fed's communication could introduce challenges. Therefore we need to stay informed and adjust the model accordingly.

## Repository Structure

- **data**
    - The data is not hosted in Github.

- **Code-and-analysis**
  - Contains modularized Jupyter notebooks used for data loading, data preprocessing, EDA, feature engineering, text encoding, and modeling. The folder includes the following Jupyter notebook(my part of the code):
    - **Data Preprocessing and Organization**: data_preprocessing_WL_IN_clean.ipynb
    - **ModernBERT Based Modeling**: MBert_model_IN_clean.ipynb

- **Presentation**
  - A power-point based team presentation is attached to outline our key deliveries - a final paper which was not included in this repo.
 
- **README.md**
  - This file, detailing the project summary and repo structure.