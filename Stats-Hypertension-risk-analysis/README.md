# Project: "Age-and-Beyond-Investigating-Hypertension-Risk-Factors"

Welcome to project 'Age-and-Beyond-Investigating-Hypertension-Risk-Factors', a statistical analysis on hypertension risk focusing on its association with age and other related factors.

This is initially a group project with more than one contributors. The overall results and other contributors are presented in the presentation pdf under this repo.

## Introduction

Hypertension imposes signficant risk to the health of the general public. Hypertension, associated with serious health complications like heart disease, stroke, and kidney failure, affects an estimated 1.28 billion adults aged 30–79 worldwide. 
This makes it important to understand the factors that meaningfully associated with the occurance of hypertension. 

Various researches have indicated age's connection with blood pressure dynamics, and also suggested that hypertension's complexity also extends to factors beyond age, such as genetics, race, overweight, lifestyle, etc. 

Our analysis aims to delve deeply into the relationship between age and systolic blood pressure (SBP), while considering the multifactorial nature of hypertension. 

## Source Data

This study uses data from the 2017-2018 National Health and Nutrition Examination Survey (NHANES), conducted by the CDC’s National Center for Health Statistics (NCHS).  The sample includes non institutionalized U.S. civilians over 20 years old.

## High-Level Summary of Data Description

The dataset contains 100 features related to the censored individual, including:

- **Blood pressure informatoin**: such as Systolic BP (mm Hg)
- **Demographic details**: such as age, gender, race, etc.
- **General health condition**: such as general health condition indicator, Have diabetes Y-N, Chest pain on level ground Y-N, etc.
- **Life style information**: such as number of alcohol per day, number of cigarettes per day, number of hours of sedentary activity per day, etc.

## Analysis involved

- **Data cleaning**: raw data inputs were carefully merged and cleaned before any exploratory analysis
- **Exploratory data analysis (EDA)**: data are divided with 70/30% split into exploration and validation data sets. Various exploration and visualization analysis were done to examine the data and discover variable relationships, identify key variables ,and perform any transformation as needed
- **Statistical analysis**: examine the model fit, variable significance, and their model contribution in sequential manner. All modeling decisions are made using exploration data set only. Once the model is decided, report the results on validation data set. 

## Overall result

- Our study highlights a significant linear relationship between age and Systolic BP ('SBP'). Furthermore, demographic information such as BMI, race, gender, alcohol consumptions also show statistically significant association with SBP. This analysis will be of interest for healthcare institutions seeking to develop preventitative strategies and promote healthy lifestyles. 

## Project Repository Structure

- **data**
  - **Raw data**: data before preprocessing and split is saved here
  
  - **Processed data**: data after preprocessing and exploration / validation split are saved here

- **code-and-analysis**
  - the rmd file (using langauge R) supporting the analysis is saved here 

- **report**
  - a report generated from the rmd file with Latex format is saved here

- **presentation**
  - a short final project presentation is presented here

- **README.md**
  - This file, detailing the project summary and repo structure.
