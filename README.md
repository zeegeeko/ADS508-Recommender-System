# ADS 508 Project: Building an Anime Recomendation system

# Table of Contents

--------

- [Authors and Collaborators](#authors-and-collaborators)
- [Installation](#installation)
- [Technologies](#technologies)
- [About the Project](#about-the-project)
  * [Problem Statement](#problem-statement)
  * [Justification](#justification)
- [Data Exploration](#data-exploration)
  * [Original Data](#original-data)
   - [Data Set Files](#data-set-files)
 - [Data Preparation](#data-preparation)
- [Model Training](#Model-training)
- [Model Strategies](#model-strategies)
- [Presentations and Reports](#presentations-and-reports)
- [Code](#code)
- [Performance Results](#performance-results)
- [Conclusion](#conclusion)
- [References](#references)

-------

# Authors and Collaborators
- Luke Awino
- Emanuel Lucban
- Tristan Demond




# Installation

Data Exploration and Modeling done in Amazon SageMaker

To clone this repository onto your device, use the commands below:

	1. git init
	2. git clone git@github.com:https://github.com/zeegeeko/ADS508-Recommender-System

## Technologies

- Amazon SageMaker
- Amazon Athena
- AWS S3


# About the Project

This project aims specifically to create and continue to build upon an Anime recommendation algorithm. We hope to gain new viewers and build upon attrition for historical ones. This will in turn gain revenue and watch time. The algorithm itself will not be able to measure the extrapolated results of revenue and watch time, so we will have to measure two things that are critical for the algorithm and investment success: 
The accuracy of anime rating predictions
We can predict how good anime are before recommending them. This will make sure we are recommending the best anime even if we don’t have ratings for it and we know our algorithm will work with new data.
 Personalization
It is important to understand if the recommendation system recommends personalized anime recommendations to users. Will will need to measure the dissimilarity (1- cosine similarity) between user’s lists of recommendations.



## Problem Statement

No one foresaw the global pandemic of 2020, which resulted in the wide-scale shutdown of the world as we know it. Due to its highly infectious nature, people had to isolate quarantine to slow the spread; for a while, most public events were shut down or severely limited virtually all entertainment venues were shut down; the only available outlets for social interaction were social media, social media played a massive part in our growth as customers took part in our campaign that offered two free months of service. This shutdown fueled our growth; from March 2020 till December 2021, our paid subscriber base increased 300%.We have two subscription models: a completely add free model and a less priced model with ads.
Our goal is to retain our subscriber base “Paid subscriptions are the dominant business model for streaming video services in the United States” (Arkenberg et al., 2021); with more networks turning to Streaming and creating Streaming outlets, reducing churn and increasing retention is paramount to our survival. Increasing the effectiveness of our recommendation model will extrapolate to also increasing the duration of watch time by our users. This will in turn increase ad revenue because more people will have to watch those ads, thus increasing profitability.We will improve our chances of meeting our goal by building a recommender system using the data to recommend viewing options to our existing customers to increase customer engagement and retention



## Justification

Many anime watchers typically watch many different anime. However, there are many large corporations that have paid partnerships, which allows them to monopolize the big names in anime. We want to compete with these companies by creating a recommendation system that promotes retention from our views and attracts new users from those companies


# Data Exploration

The data will be stored in an s3 bucket. The data was downloaded from Kaggle as a zipped file containing five files and then uploaded to the s3 bucket as five individual files.
We will use Sagemaker to ingest the data using Python. The csv files in the S3 bucket will be queried directly into a table using Amazon Athena. Amazon Sagemaker will then be used to load the Amazon Athena tables into Pandas dataframes for data exploration.
We will assess data quality by looking for missing fields, null values, correlation, inconsistent data, evaluate data types, outliers and potentially which fields may require feature engineering.

## Original Data 

The dataset will be sourced from Kaggle (https://www.kaggle.com/hernan4444/anime-recommendation-database-2020?select=anime.csv).The publisher of the dataset, Hernan Valdivieso originally sourced the data from the site MyAnimeList (https://myanimelist.net/). The dataset includes data such as titles, synopsis, genres, user reviews, aggregated ratings by title and user assigned scores. The data is split along five .csv files and a folder of .html files.





## Data Set Files
We will used Sagemaker to ingest the data using Python. We created five separate folders in the s3 bucket one folder for each bucket; The csv files were loaded into the S3 bucket  queried using Athena to create a table; these are the file paths for the five separate folders with the corresponding  table’s created
s3://ads508-raw-data/animelist/ = animelist_table
s3://ads508-raw-data/anime/ = anime_table
s3://ads508-raw-data/synopsis/ = synopsis_table
s3://ads508-raw-data/ratings/ = rating_table
s3://ads508-raw-data/watching = watchlist_table





# Data Preparation
The current state of the raw dataset is inadequate for model training purposes and several transformations must be performed in order to have the dataset in a consumable state for model training. The first step is to join the multiple tables into a single dataframe with the necessary variables. The transformed dataset will be stored in an Amazon S3 bucket specific for modeling and training

ratings table is already part of the animelist table and is not necessary to include
synopsis table contains summaries of the anime table, however due to time and model training constraints, Natural Language Processing (NLP) tasks will not be included as part of the recommender system and this table will be excluded.
animelist table contains individual user ratings for titles and will be joined to the anime table which includes anime title names, genres and aggregated user scores.
watching_status table contains individual user watch status for each rated title. This will be joined to the animelist table since we will want to exclude already watched titles from recommendations.
Through the data exploration phase of the project, very minor issues were discovered with the dataset. The data was mostly consistent, however some data cleaning was required.
From the raw csv files, there are apparent null values. However, null values are not blank in the csv files but a string “Unknown” was used. In fields such as Score in the anime table, this prevented typing coercion to float. “Unknown” values were used only in the anime table.
Using Pandas, all “Unknown” values were globally replaced with numpy NaN (Not a Number) values.
After replacing “Unknown” values with NaN, numeric columns such as Score, Members, Ranked, Popularity were converted into the appropriate float or integer data types.
5141 Score values in the anime table were null which is 29% of the anime dataset. Mean imputation will be used to replace the missing values or the Score variable will be excluded as a feature depending on variable importance during model training.




# Model Training
For model training, after joining all necessary tables, the following fields will be used as an index: user_id, anime_id. The following fields will be included as variables for the model: rating, watching_status, score, genres, type, episode, aired, rating, ranked, popularity, members and Scores-10 to Scores-1. The following transformations will need to be performed.
The variable aired, represents the date of first airing. This will be used to create a new feature called days_since_aired which will be a timedelta from first airing until today.
Genres is a variable consisting of a list of associated genres (each anime can have multiple genres). 36 unique genres have been identified and will be one-hot-encoded adding 36 new binary variables (or 35 if dummy encoded). 
Variable type is also a categorical variable that takes on 3 values and will be one-hot-encoded.
Variables watching_status and rating are ordinal valued with watching_status taking on values from 1 to 5 and rating taking on values from 1 to 5.
All other real or integer valued variables will be normalized.
Since the collaborative filtering system is neither a classification nor a regression task, k-nearest neighbors will be used to create recommendations based on similarity measures against other titles based on genre, scores, rating and other variables and will be an unsupervised task. In other words, the k-nearest neighbors are the recommendations. Therefore there are no labels or classes to take into consideration in balancing the dataset. Since k-nearest neighbors algorithm does not have a distinct training step (lazy-learner algorithm), there will be no splitting of the dataset into training and test sets. 




# Model Strategies 
Three distinct recommender systems will be evaluated
First is a Naive Recommender, an unskilled system that performs a uniformly random selection (no replacement) of k titles per user. 
Second is a Simple Recommender that uses the feature set outlined in the Data Preparation section and clusters generated by the K-Means model.
Third is a more complex, Comprehensive Recommender that uses the Simple Recommender’s feature set, clusters generated by K-Means and BERT model embeddings of the synopsis data.




# Presentations and Reports

# Code
https://github.com/zeegeeko/ADS508-Recommender-System/blob/main/Team3_Modeling.ipynb


# Conclusion
![image](https://user-images.githubusercontent.com/70973371/163028803-3d1fd71e-c4f9-4cc8-ace4-264d8c883d4d.png)

Based on the average intra-list similarity metric by user, the comprehensive recommender had the highest average intra-list similarity measurement of 0.92. This means the recommended titles from the comprehensive recommender system gives more relevant and related content recommendations.

# References

Arkenberg, C., Ledger, D., Loucks, J., & Westcott, K. (2021, October 1). Digital Media Trends: How Streaming Video Services Can Tackle subscriber churn. Deloitte Insights. Retrieved March 12, 2022, from https://www2.deloitte.com/us/en/insights/industry/technology/video-streaming-services-churn-rate.html

