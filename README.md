# Predicting Response Times of the San Francisco Fire Department

The goal of this project is to predict the time it will take for the fire department to arrive on the scene of an incident (when applicable).

## Data

The [data](https://www.kaggle.com/datasf/san-francisco) comes from the San Francisco Open Data dataset on Kaggle. Though this dataset is updated regularly, the data for this study was collected on January 24, 2018. At the moment, the only data used was the `sfpd_service_calls` table.

## Progress

Linear regression (regularized and unregularized), decision tree regression and random forest regression models were fit using a few variables from the `sfpd_service_calls` table. They were `battalion`, `station_area`, `call_type`, and `zipcode_of_incident`.


- Features for `day_of_week` and `hour_of_day` were created from the original dataset and features for `distance` and `duration` were created using the [Google Distance Matrix API](https://developers.google.com/maps/documentation/distance-matrix/intro) to get distances and time estimates between each fire station and the addess of the incident. Given a destination and origin, an http request is sent for a json object that contains the variables of interest. A departure time can also be specified (must be "now" or in the future; we used "now" and results were obtained throughout the day on a Saturday).

## Possible Todo's:

- Cluster the fire_stations (`station_area`) in `sfpd_service_calls` and put some `call_type`'s in an "other" category. For example, a table of summary statistics was generated about each of the fire stations, this could be used to do clustering.
- Use the data in the `sfpd_incidents` table to get aggregates of crime based on addresses. The addresses can (hopefully) be used to join this data with the `sffd_service_calls`.
- Generate variables like the year, month, day and hour of that the call was received by the fire department.
- If time allows (lol), look into joining the rest of the tables in a similar fashion as the crime table

## Results

We found that the data from the Google Routes API would be the most useful features when modeling response times. Without this information, we did not have the physical distance between fire stations and incidents. In addition, Google gives an estimate of the time it will take to drive between the two locations (just like if one was to use Google Maps on their phone). 

Using the distances and time estimates, we built a linear regression model using just these two features. This gives interpretable coefficients that measure how one could scale the distance and estimated driving time for a regular driver to figure out what that time looks like for an ambulance or fire truck.

In addition to this simple model, data from the `sffd_service_calls` and `sfpd_incidents` tables were used to make more accurate predictions.

Linear Regression showed the best results with 

$R^2$:  0.0382449456338
$RMSE$: 4.82394317574
$MAE$: 3.3039737344

[This](/notebooks/sffd_m4.large_big.ipynb) outlines our process.
