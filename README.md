# Scalable Real-time Prediction and Analysis ofSan Francisco Fire Department Response Times

This repository contains the analysis supporting this research paper:

  - Reevesman A, Lian X, Melancon S, Presta JR, Spiering BJ, Woodbridge DM. Leveraging Distributed Computing Systems for Prediction and Analysis of San Francisco Fire Department Response Times. Accepted for presentation at IEEE International Conference on Ubiquitous Intelligence and Computing, Leicester, UK, August 2019

## Abstract

Predicting  the  San  Francisco  Fire  Department(SFFD) emergency response time in real-time is critical for both callers and the SFFD. In this paper, we leverage machine learningand distributed computing to attempt a solution to this problem. While driving time can be estimated using the Google MapsAPI, there are many more factors that affect the response time. We obtained a publicly available SFFD call history dataset, using BigQuery, containing information about the location ofthe station, the location of the incident, and the time the call was received. We combined features from this dataset with driving time estimates from the Google Maps Distance Matrix API. The dataset was over a gigabyte, calling for a distributed framework to efficient training of the machine learning models. We fit Linear Regression, Decision Tree Regression, and RandomForest Regression models in the Apache Spark machine learning library (MLlib) on an Amazon Web Services (AWS) ElasticMapReduce cluster. We benchmarked training time for eachmodel on different cluster sizes.

## Data

The [data](https://www.kaggle.com/datasf/san-francisco) comes from the San Francisco Open Data dataset on Kaggle. Though this dataset is updated regularly, the data for this study was collected on January 24, 2018. At the moment, the only data used was the `sfpd_service_calls` table.

## Results

#### Accuracy

Linear Regression using all of the feautures showed the best results with 

R^2:  0.0382449456338

RMSE: 4.82394317574 (minutes)

MAE: 3.3039737344 (minutes)

[This](/notebooks/sffd_m4.large_big.ipynb) notebook outlines the process.

#### Training time

Linear regression models consistently took longer to train than tree-based models. Moving from m4.large to m4.xlarge instances cut the model training time in half.

![](/images/training_times.png)

## Conclusions

We found that the data from the Google Routes API would be the most useful features when modeling response times. Without this information, we did not have the physical distance between fire stations and incidents. In addition, Google gives an estimate of the time it will take to drive between the two locations (just like if one was to use Google Maps on their phone). 

Using the distances and time estimates, we built a linear regression model using just these two features. This gives interpretable coefficients that measure how one could scale the distance and estimated driving time for a regular driver to figure out what that time looks like for an ambulance or fire truck.

In addition to this simple model, data from the `sffd_service_calls` and `sfpd_incidents` tables were used to make more accurate predictions.
