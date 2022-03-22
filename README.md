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
  * [Data Set Dictionary](#data-set-dictionary)
- [Data Preprocessing](#data-preprocessing)
- [Data Splitting](#data-splitting)
- [Model Strategies](#model-strategies)
- [Presentations and Reports](#presentations-and-reports)
- [Code](#code)
- [Data Visualizations](#data-visualizations)
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





## Data Set Dictionary




# Data Preprocessing



# Data Splitting 



# Model Strategies 




# Presentations and Reports

# Code


# Conclusion

# References

Arkenberg, C., Ledger, D., Loucks, J., & Westcott, K. (2021, October 1). Digital Media Trends: How Streaming Video Services Can Tackle subscriber churn. Deloitte Insights. Retrieved March 12, 2022, from https://www2.deloitte.com/us/en/insights/industry/technology/video-streaming-services-churn-rate.html

