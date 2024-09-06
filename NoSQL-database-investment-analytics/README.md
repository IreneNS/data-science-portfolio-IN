# Project: Enhancing Portfolio Construction with NoSQL Databases

Welcome to project 'Enhancing Portfolio Construction with NoSQL Databases', a project to explore the potential applications of NoSQL databases to investment analytics area. More specifically Neo4j, the prominent graph database management system, is used to model portfolio construction ideas leveraging on its built in algorithms.

This is initially a group project with more than one contributors. The overall results and other contributors are presented in the presentation pdf under this repo. For the fairness purpose I only include my part of the code here.

## Introduction

In light of the data era, the interest about discovering ‘hidden patterns’ in the data has never been higher. Real time analytics are also increasingly in demand. It is especially so in quantitative trading world. Interestingly NoSQL databases tend to have edges in both areas.  We are familiar with using relational database to analyze stock time series data, but we wonder - can we utilize NoSQL databases to enhance portfolio construction?

## Research Objective

- This project aims to examine two possibilities:
    - Use Neo4j algorithms to discover hidden patterns in stock market information, in order to enhance portfolio construction
   - Use Redis and Mongodb to enhance the data delivery and display timeliness

## Example Data

This study uses a public dataset containing five months of NASDAQ100 data in 2021 from [here]("https://raw.githubusercontent.com/tomasonjo/blog-datasets/main/stocks/stock_prices.csv") and yahoo finance. Note that this is just an example data used to explore the application idea, considering our Neo4j database memory limit. For more serious application, longer history of similar data should be used for testing, and such data is available in public domain.


## High-Level Summary of Data Description

The dataset contains 9,180 records with 8 features, including:

- **Stock name**: the name of the stock
- **Date**: the date of the information 
- **Price information**: including close, Adj close, high, low price per day
- **Volume**: the number of shares traded for a given stock on a given day

Additional information such as P/E ratio, market cap and industry information for the same stocks over the same period were used for one of the algorithms. But the most important features are the price and volume information mentioned above.

## Analysis involved (Neo4j)

- **Preprocessing and data cleansing**: raw data inputs were carefully checked for missing value, data range, and any unreasonable or invalid value. However the sample data is relatively clean.
- **Build the graph in Neo4j**: Given Neo4j stores information in terms of the nodes and relationships, we created a stock information based graph, containing stock nodes, date nodes, various relationship among them, along side with respective object properties, preparing the graph ready for analysis.
- **Neo4j algorithm analysis**: we experimented various relational based analysis leveraging on Neo4j algorithms, in order to enhance stock portfolio construction process:

    - ***KNN similarity and Louvain community detecting algorithm***: we construc stock communities based on price and volume correlation similaries, then choose top performers from different communities as a portfolio
    - ***Page rank algorithm***: we use page rank algorithm to identify most influential stocks globally and within communities to make sure we capture the important movers in our portfolio
    - ***Geodesic / Euclidean distance algorithm***: use the euclidean distance algorithm based on non pricing / volume data such as P/E, industry, market cap to identify different clusters, which can be used to further diversify the portfolio

## Analysis involved (Redis and MongoDB)

We explore the advantages of Redis and MongoDB compared to traditional relational databases, and how they can be used to provide enhancement to portfolio analytics in general

## Overall result

- To create a diversified portfolio with top performers, we identified a few ways that NoSQL databases can meaningfully add value.
- With Neo4j, multiple algorithms can be used to identify clusters / communities, or influential stocks, in order to create a diversified portofio with top performers. Useful algorithms include KNN similarity, Louvain communitiy detecting, page rank and euclidean distance. 
- with redis and mongoDB, given their unique advantages compared to traditional database, they can also be used to potentially enhance the portfolio analytics.

## Project Repository Structure

- **code-and-analysis**
  - the ipynb file supporting the analysis that I conducted is saved here 

- **presentation**
  - a final project presentation is presented here

- **README.md**
  - This file, detailing the project summary and repo structure.
