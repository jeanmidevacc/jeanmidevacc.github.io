---
layout: post
comments: true
published: false
title: Make a forecast system of the national energy consumption
---
Hello readers, for this article i am going to explain my approach to create a forecast system of the French (metropolitian) energy consumption. This kind of problematics is more or less related to part of my job at EDF Energy but this works has been done in my free time to complete my machine engineer nanodegree of [Udacity](www.udacity.com).

In this article, there will be description of the dat used for this project, an explanation of the approach used to make a daily forecast of the consumption.

# Exploration of the dataset

In this case, the data to create the model are from:
- RTE, the energy network manager in France they have create an open data platform to give access ti customer at the production and consumption of energy in France.The data are from the
- INSEE data, some informations on the cities in France (mostly i focus my analysis on the number of habitants)
- Weather underground, for the weather data because the consumption is link to the weather condition on France.I have to scrap this data source from the website (i focus my scraping the weather stations on the airport) and i pick the weather stations that have enough data during the period measure by RTE.I focus my data analytics on the outdoor temperature, the outdoor humidity and the wind direction


For the weather condition, I have choose to kind of weather sources:
- First take the weather stations from the center of France, in my case i took Clermont-Ferrand
/giphy
- The second one is a national weather station where basically each region has his weather station associated , these stations are weighted by the number of people that are in this region (this information is coming from the INSEE data)

To make the data analysis, i will have to convert the average power consume form RTE on energy.
equation conversion

In the following figure there is an illustration of the average energy consumed.

After that to make the forecast at a daily scale, i have to aggregated the data to the daily scale.

In the following figures, there is a representation of the daily data aggregated at different scale in a heatmap.

A study of the hourly effect in funcion of the day of the year

<center>
<img src="{{ site.baseurl }}/img/posts/energy_forecast/heatmap_yearday_hour.png" />
</center>

A study of the consumption of the month of the year and the day of the week

This two illustrations are the perfect illustration  that the daily consumption  link to the moment of the year and the day of the week.But htis insight are not enough to create a forecast model, a very popular that is used to make a forecast is the study if the daily consumption in function of the average daily outdoor temperature.This technic is called PTG for Power temperature gradient and you can find a lot of publications that are based on this [approach]("ptgpublivation").
