# Project: "Predicting Hotel Cancellations with Machine Learning"

Welcome to project 'Predicting Hotel Cancellations with Machine Learning' repo! To use this starter repo simply set up your [git client](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) and clone the repo with the SSH link.

This is initially a group project with more than one contributors. The overall results and other contributors are presented in the presentation pdf under this repo. However, for fairness purpose, I only include my portion of code in this repo for my project portoflio demonstration.

## Introduction and Project Objective

Hotel cancellations can result in significant losses due to lost revenue and reputational risks from overbooking. To address this, our project aims to predict the likelihood of hotel booking cancellations using machine learning. By accurately forecasting cancellations, hotels can develop strategies to minimize last-minute cancellations and create dynamic pricing models to account for potential cancellations.

## Sources of Data

The original source data is from Kaggle.com. We downloaded the dataset from Kaggle via this link: https://www.kaggle.com/datasets/youssefaboelwafa/hotel-booking-cancellation-prediction. The Kaggle dataset is provided in CSV format, ensuring ease of use with popular data science libraries such as Pandas and scikit-learn. A detailed data dictionary is included to guide users through the interpretation of each feature. As cited from Kaggle: “The data is originally from the article Hotel Booking Demand Datasets, written by Nuno Antonio, Ana Almeida, and Luis Nunes for Data in Brief, Volume 22, February 2019.”

Please note that the source data and processed data are not hosted in the GitHub repository. They are stored in a Google Drive folder accessible to everyone with the link. You can access the raw data via this link: [Google Drive Data Link](https://drive.google.com/file/d/1Swl91VKmMYP77eZ_RSg6t7do_qk83VT_/view?usp=drive_link). Pre-processed file links are provided below.

## High-Level Summary of Data Description

The dataset contains various features related to hotel bookings, including:

- **Booking Status**: Whether the booking was canceled or not
- **Booking Details**: Such as lead time, number of adults and children, and number of special requests
- **Customer Information**: Including repeat guest status and required car parking space
- **Booking Timing**: Arrival month and arrival date
- **Market Segment**: The type of market segment the booking belongs to (e.g., Online, Offline, Corporate, Aviation, Complementary)

## Data Dictionary

Here is a list of all data elements in the dataset:

- **Booking_ID**: Unique identifier for each booking.
- **no_of_adults**: Number of adults.
- **no_of_children**: Number of children.
- **no_of_weekend_nights**: Number of weekend nights (Saturday and Sunday) the guest stayed or booked to stay at the hotel.
- **no_of_week_nights**: Number of week nights (Monday to Friday) the guest stayed or booked to stay at the hotel.
- **type_of_meal_plan**: Type of meal plan booked with the reservation.
- **required_car_parking_space**: Binary value indicating if a car parking space was required.
- **room_type_reserved**: Type of room reserved.
- **lead_time**: Number of days between the booking date and the arrival date.
- **arrival_year**: Year of arrival date.
- **arrival_month**: Month of arrival date.
- **arrival_date**: Date of the month of arrival date.
- **market_segment_type**: Market segment designation.
- **repeated_guest**: Binary value indicating if the guest is a repeated guest.
- **no_of_previous_cancellations**: Number of previous bookings that were canceled by the customer prior to the current booking.
- **no_of_previous_bookings_not_canceled**: Number of previous bookings not canceled by the customer prior to the current booking.
- **avg_price_per_room**: Average price per day of the reservation; prices of the rooms are dynamic. (in Euros)
- **no_of_special_requests**: Total number of special requests made by the customer (e.g. high floor, view).
- **booking_status**: Binary value indicating if the booking was canceled or not.

## Analysis involved

- **Data Preprocessing and EDA**: 
    - Data cleansing and evaluation: various data cleasing and evaluation are done to make sure data are in appropriate shape, no missing or invalid data before analysis
    - Data visualization and feature engineering: split variables in numeric and categorial, visualize average or distribution by response (booking status) accordingly; group/bin numeric variables with limited value range as needed; derive new variables as needed; check data imbalance
- **Baseline Model (logistic regression)**: 

    - Given it is a classification problem, we conduct logistic regression as baseline model instead of using average. Raise the bar a little higher. Establish evaluation metrics, including F1 for performance, accuracy for generalization and model faireness.

- **Conduct Various of Advanced Models (applying consistent metrics to compare to baseline)**:
    - KNN
    - Deep Neural Network
    - Random Forest
    - XG Boost

## Overall result

Both random forest and XGboost presents compelling results in terms of model performance, model generalization, and model fairness. XGboost has marginal improve over random forest in terms of overall accuracy and F1 on test data (margainally), while random forest has the most balanced results measured by various of metrics.  


## Repository Structure

- **data/**
    - The data is not hosted in Github.
    - If the users intend to run the code from EDA to models, there is no need to download any data, EDA will generate relative path to store intermediary data in `../data/data_processed`.
    - If the users intend to skip EDA but run the models directly, please download processed data from the links noted above, and adjust the data ingestion path in models to your local data directory.

    - **Raw data**: [Link to raw data](https://drive.google.com/file/d/1wOwHx7T68HTX7V_-5ejhMNqeiPOYz4v3/view?usp=drive_link)
    
    - **Processed data**: 
        - [Link to X_train.csv](https://drive.google.com/file/d/12Y13qajLa4zHGhjWqUj5_YmCYuG983nr/view?usp=drive_link)
        - [Link to X_val.csv](https://drive.google.com/file/d/1OxCsPNP1Co9FSG_QjeH538IRR12QPiPb/view?usp=drive_link)
        - [Link to X_test.csv](https://drive.google.com/file/d/1JSBznI0-O_b5rhyJLjP3FzuagYR4-fOi/view?usp=drive_link)
        - [Link to Y_train.csv](https://drive.google.com/file/d/1ZlhkWkXvN4r6xMugapqYxBAR094kIK3x/view?usp=drive_link)
        - [Link to Y_val.csv](https://drive.google.com/file/d/15McQmxIcCQuA2LWOrwls8YPBqnMMu3kq/view?usp=drive_link)
        - [Link to Y_test.csv](https://drive.google.com/file/d/1zavZSdGN8e7JJfB3Kwbujye-tV8UhGIS/view?usp=drive_link)

- **code-and-analysis/**
  - Contains modularized Jupyter notebooks used for data loading, data preprocessing, EDA, baseline model, and various experiments for ML modeling and evaluation. The folder includes the following Jupyter notebooks (my part of the code):
    - **Data Preprocessing and EDA**: Hotel_Cancellation_Data_Preprocessing_EDA.ipynb
    - **Baseline Model (logistic regression)**: Hotel_Cancellation_Baseline_Logistic_Regression.ipynb
    - **Random Forest**: Hotel_Cancellation_Random_Forest.ipynb


- **README.md**
  - This file, detailing the project summary and repo structure.

