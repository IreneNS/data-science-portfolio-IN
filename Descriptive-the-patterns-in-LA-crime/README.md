# Project: A-Glimpse-into-LA-Crime-Patterns

Welcome to project 'A Glimpse into the Patterns in Crime in Los Angeles', a data descriptive analysis on the patterns shown in the crime occured between January 2020 to March 2024 in Los Angeles.

This is initially a group project with more than one contributors. The overall results and other contributors are presented in the presentation pdf under this repo.

## Introduction

In recent years, residents of California and elsewhere across the U.S. have expressed concern that the crime rate is increasing, particularly since the outbreak of the COVID-19 pandemic. To investigate the validity of this perception, we will analyze a dataset provided by data.gov containing crime incident data for the City of Los Angeles starting from January, 2020 to March, 2024.

## Research Objective

- Use descriptive method (non-statistical), we would like to Identify and descriptively test the hypotheses that are related to the public perception regarding the changes of crimes in Los Angeles in recent years. 
  - For example, the public observe that petty crimes in Los Angeles, defined as those with an economic loss of less 
than $950, increased meaningfully in recent years. - Does our data support this?  
- Develop outcome based questions that will help describe patterns, trends & insights that can help LA's law enforcement agencies.

## Source Data

This study uses the dataset [City of Los Angeles - Crime Data from 2020 to Present](https://catalog.data.gov/dataset/crime-data-from-2020-to-present) from [Data.gov](https://catalog.data.gov/dataset/).


## High-Level Summary of Data Description

The dataset contains 910,707 records with 28 features, including:

- **Case number**: Division record number (‘DR No.’) 
- **Time related**: date (reported, occurred); time occurred 
- **Area related**: area code, area name, district number, location, latitude, longitude, premises description (type) 
- **Description**: crime description (types, with ‘petty crime’ labeled) 
- **Victim Demographics**: Age, Gender, Ethnicity 

The specific data dictionary can be find [here](https://data.lacity.org/Public-Safety/Crime-Data-from-2020-to-Present/2nrs-mtv8/about_data).

## Analysis involved

- **Preprocessing and data cleansing**: raw data inputs were carefully checked for missing value, data range, and any unreasonable or invalid value. Necessary preprocessing is conducted accordingly.
- **Exploratory data analysis (EDA)**: various exploratory analysis are conducted to understand the potential connections among variables, important variables to answer the descriptive hypothesis questions, and formulate potential outcome based questions to explore further. Necessary data engineering are conducted to ensure analyzable data format, such as date/time format.
- **Descriptive analysis**: examine both the descriptive hypothesis tests and outcome based questions one by one to discern descriptive patterns. \

    - ***Hypetheses (descriptive)***
      1. Does our data support the observation that petty crime increases meaningfully since 2020 in L.A.?
      2. Does our data support the observation that burglaries from vehicels increases meaningfully since 2020 in L.A.?

    - ***Outcome based questions***
      1. Any patterns associated with the time of the crimes?
      2. What are the major crime types and the YoY trend of total crime by area? 
      3. What are the highest crimes and their YoY trend, by area? 
      4. Which areas have seen the highest number of crimes with firearms?

## Overall result

- This study explores LA crime log from 2020 to March, 2024 to assess if the data supports the public perceptions regarding the crime trend in LA. It also contains descriptive analysis that highlights trends,  patterns & insights that could be of help to local law-enforcement. 
- Data supports the argument that petty crime in LA has increased in recent years, especially in 2023. However, it does not support the argument that burglaries from vehicles have risen in the same timeframe.  
- For time associated crime patterns, late afternoon and evening, the first five days of the month, and the months of January to February have a higher crime rate, in a day/month/year respectively.  While the month end tends to have a lower crime rate.  
- Vehicle Theft (Crm Cd 510) is the highest crime in LA. 16% of all crimes reported in Newton & Hollenbeck LAPD divisions is a Stolen Vehicle.  
- Between 2022-2023 total crime has reduced in all LAPD Divisions except seven divisions. 
- Six LAPD Divisions recorded 53% of all firearm crimes in LA.

## Project Repository Structure

- **data**
  - data from data.gov is stored here

- **code-and-analysis**
  - the ipynb file supporting the analysis that I conducted is saved here 

- **report**
  - a report generated detailing the analysis is saved here

- **presentation**
  - a final project presentation is presented here

- **README.md**
  - This file, detailing the project summary and repo structure.
