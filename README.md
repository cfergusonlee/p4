# Exploratory Analysis of Uber Pickup Data in R

![Uber Logo](https://github.com/spacecadet84/p4/blob/master/photos/uber-logo.jpg "Uber Logo")

This project explores the variables, structure, patterns, oddities, and underlying relationships of New York City Uber pickups from January through June of 2015.  The Taxi & Limousine Commission (TLC) released the data after receiving a Freedom of Information Law (FOIL) request from FiveThirtyEight on June 22, 2015. I pulled the data from the FiveThirtyEight github repo found [here](https://github.com/fivethirtyeight/uber-tlc-foil-response).  Each observation represents a single pickup with the following columns:

Variable | Definition
------------------- | --------------------------------------------------
Dispatching_base_num | The TLC base company code that dispatched the Uber
Pickup_date | The full date of the pickup in yyyy-mm-dd h:min:s format
Affiliated_base_num | The TLC base company code of the Uber pickup
locationID | The pickup location ID of the Uber pickup
Borough | The New York City Borough where the pickup took place
Zone | The neighborhood in the New York City Borough where the pickup took place
lat | The latitude of the pickup Zone
lon | The longitude of the pickup Zone
month | The month that the pickup took place
date | The date (1-31) that the pickup took place
day | The day (Sunday-Saturday) that the pickup took place
hour | The hour that the pickup took place
