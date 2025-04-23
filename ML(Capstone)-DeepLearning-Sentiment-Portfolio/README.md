# Project: "DeepLearning Sentiment Portfolio (DSP)"

Welcome to project 'DeepLearning Sentiment Portfolio (DSP)' repo page! To use this starter repo simply set up your [git client](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) and clone the repo with the SSH link.

This is initially a group project with multiple contributors. The overall results from all contributors are presented in the presentation pdf. However, for fairness purpose, I only include my portion of code in this repo for my project portfolio demonstration.

## Problem & Motivation

- Each time the market undergoes a major shift—economic event driven or not—a great deal of social wealth vanishes. Think you’re safe if you don’t invest directly? Think again. Your social security, pension, or 401(k) can still suffer heavy losses.

- Navigating investing through such market shifts, or regime changes, is hardly a new challenge. Yet - despite decades of work by practitioners and researchers, truly satisfying solutions remain rare. We believe this is largely due to existing methods’ limited ability to capture the non-linear, intricate, and dynamic relationships on both individual assets and the market level.  

- Fortunately, methods such as neural networks are known for their strength in modeling non-linear relationships. Additionally, natural language processing (NLP) opens the door to handling unstructured input, providing new ways to capture signals related to market dynamics.

- Our goal in this project is to build a machine learning–based portfolio construction framework that could potentially help both retail and institutional investors better capture market shifts and preserve capital—particularly during periods of regime change.

## Key Takeaways and Results

- There are many challenges in this attempt, the key ones include (1). decomposing the loaded project goal into workable components, (2). connecting the highly specialized finance problem to relevant machine learning techniques; (3). collecting and consolidating alternative data (text data), (4). adapting traditional data science techniques to handle time series data; (5). attempting to predict market and security returns which are notoriously intricate, noisy and of low signal to noise ratio.

- On high level, we decomposed the research question, "Can we use machine learning techniques to improve the portfolio risk/return profile, especially during the regime change periods?", in two parts: improving market timing, and improving security selection. We attempted at the first part using news driven sentiment analysis on market level to gauge market timing decisions, and we attempted at the second part using transformer based deep learning models to improve security selection processes.

- Infrastructure wise we use co-lab A100 for text sentiment model training, and AWS SageMaker ml.g5.12xlarge instance (NVIDIA A10Gx4) for deep learning model training. Further, for deployment, we utilize AWS EC2, using a FastAPI application to provide endpoints to host the prediction scripts. The interaction between the Streamlit application and the EC2 instance is facilitated by API Gateway and a Lambda function, pointing to the respective FastAPI endpoint within the instance, with an Elastic IP holding the endpoint constant. 

- With extensive data collecting, cleaning, model infrastructure building, training and fine-tuning, we were able to produce a prototype portfolio construction framework involving sentiment and transformer deep learning model to improve upon statistical benchmark model, providing a valuable window for further research in this area and democratize this concept to more users outside of the highly specilized investment groups. 

- Had we had more time and resources, we would:
  - Spend more time on data collection for sentiment model to improve data representation in early 2000s.
  - Explore reinforcement learning, which could further align the data science metrics (learning loss) with the ultimate portfolio goal (sharpe ratio), replace the traditional optimization method with multi-period reinforcement process, enhancing the model robustness in different environments.
  - Conduct further finance implementation related studies to translate the prototype to a more realistic product.

## Data Sources

- Sentiment Model: We began by assembling a corpus of over 16 million financial news articles spanning from 2000 to 2024, sourced primarily from a Financial News and Stock Price Integration (FNSPID) dataset and platforms such as New York Times. To fill the gap in the early years, we also created custom web scraping tools to collect a reasonable amount of news each day for the first 5-6 years of our dataset. We further screened, cleaned up and merged the massive datasets into a workable size and format. 

- Deep Learning Model: The main data source is WDRS (Wharton Research Data Services). More specifically, we merged data from three data providers: CRSP, Compustat, and JKP, which provided return, accounting ratio and many literature-based return metrics. We chose to focus on stocks in the S&P500 universe historically, from 2000 to 2024 with daily frequency. The market level data (S&P index return) comes from Yahoo Finance. After careful cleaning and joining, the combined dataset is about 1GB in size, with ~3mm rows and 30+ columns.

## Overall Approach

We built two main models – a text sentiment model, and a deep learning model. The text sentiment model aims to find the information in financial news so as to provide guidance in market timing decisions, while the deep learning model aims to use the latest neural networks techniques to improve security selection processes. We evaluated the usefulness of the models by both data science metrics, and investment metrics based on a simple trading simulation, such as Sharpe Ratio (a commonly used metrics in finance to measure the return per unit of risk, the higher the better), and max drawdown (another commonly used metrics in finance to measure the largest sudden loss in the investment period, the less negative the better).

## Models and High-Level Findings
- Sentiment Model:
  - The text sentiment model aims to utilize finance related news to predict the direction of the next day return of S&P500 index.
  - We utilized the consolidated news data mentioned above, aggregated them into daily frequency leveraging FinBERT’s text embedding and a custom attention-based aggregation method.  Furthermore, we utilized the Hugging Face Trainer class to customize FinBERT’s downstream task from predicting standard sentiments to predict the direction of next day’s S&P500 index return, based on the abstract of news content the day before.
  - Besides the attention-based aggregation plus fine-tune method mentioned above (the ‘Attention-Pooled Fine-Tuned NN’ method), we also tried various other methods for the same task, including random forest, mean-pooling aggregation with fine-tune, prediction without base model fine-tune, and the ensemble method which collectively uses the results from all above models.
  - Notably, our best-performing model is the Attention-Pooled Fine-Tuned NN method, which outpaced the buy-and-hold baseline, and all other alternative methods, achieving 0.44 Sharpe ratio (vs 0.34 for buy-and-hold), and -34% max drawdown (vs -57% in buy-and-hold). 

- Deep Learning Model:
  - In essence, we aimed to find a non-linear and dynamic framework for predicting expected security returns— an area where neural networks are particularly well-suited to capture complex relationships and interactions. More specifically, we adopted a transformer architecture (the same cutting-edge technology used in machine translation) to capture both temporal and cross-asset relationships. We used two transformer structures to capture the relationships subsequently.
  - Our goal was to use similar input features as the benchmark model (a statistical method based model), but apply a different information processing architecture to predict expected returns— ultimately improving security selection.
  - To assess if our machine learning model improves upon the traditional method, we established a statistical based portfolio framework, which utilizes well discussed factors in prior research (i.e. extended Fama-French Factors) as our benchmark. In fact, this choice effectively raises the bar for us, because a well performing statistical model beats the market, and we have to beat the statistical model.
  - After extensive logic and numeric based parameter search, we selected the deep learning model with the most desirable in-sample profile in both data science metrics (validation MSE) and investment metrics (highest sharpe ratio and least negative maximum drawdown). This model beats the benchmark model by both metrics, and delivers consistent in and out of sample performance.

- Deep Learning Model + Sentiment Model
  - Here we want to answer this question: can the sentiment model's probability output (of next day's return being positive) further improve the deep learning model in portfolio risk-return profile, especially in crisis time?
  - We incorporated the sentiment model outputs in deep learning in two ways: (1). as an additional input for deep learning model; (2). as a scaler applied to the initial optimization portfolio weights. Turns out, incorporating sentiment outputs further mitigates the risk of large, sudden losses in our deep learning model, as evidenced by a reduced maximum drawdown (-41% vs 42%). While the improvement is incremental, the model’s performance during crisis periods shows marginal gains—which ultimately aligns with the core objective of our project.


## Conclusion
- Through this project, the team obtained thorough hands-on experience in applying machine learning technologies in the investment space. Specifically, to improve the portfolio risk-return profile through tuning market timing and enhancing security selection compared to a pure statistical method, especially crisis period.

- After diligent striving, we were able to produce a prototype portfolio construction framework involving sentiment and transformer deep learning model to improve upon statistical benchmark model, providing a valuable window for further research in this area and democratize this concept to more users outside of the highly specified investment groups.

- Had we had more time and resources, we would:
  - Spend more time on data collection for sentiment model to improve data representation in early 2000s.
  - Explore reinforcement learning, would could further align the data science metrics (learning loss) with the ultimate portfolio goal (sharpe ratio), replace the traditional optimization method with multi-period reinforcement process, enhancing the model robustness in different environments.
  - Conduct further finance implementation related studies to translate the prototype to a more realistic product.

## Repository Structure

- **data**
    - The data is not hosted in Github.

- **Code-and-analysis**
  - Contains modularized Jupyter notebooks used for data loading, data preprocessing, EDA, feature engineering, model construction, fine-tuning and deployment. The folder includes the following Jupyter notebook(my part of the code):
    - **Initial Data Retrieval & EDA**: dsp_DL_initial_data_EDA.ipynb
    - **Data Retrieval & Preprocessing in OOP Format**: dsp_DL_ml_portf_data_retrieval.py
    - **Benchmark Model For Deep Learning**: dsp_benchmark_model.ipynb
    - **Deep Learning Model For Deployment**: dsp_DL_model_deploy.ipynb

- **Presentation**
  - A power-point based team presentation is attached to outline our key deliveries
 
- **README.md**
  - This file, detailing the project summary and repo structure.