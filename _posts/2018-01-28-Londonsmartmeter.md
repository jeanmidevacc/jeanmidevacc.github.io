---
layout: post
comments: true
published: true
title: Smart meters in London (Part1) - Description and first insights  
subtitle: Description of the data and first insights on the electrical consumption
header-img: img/header_img/london_image.jpg
---
Hello, the goal of this article is to offer a clear description of the dataset that i uploaded in November 2017 on [Kaggle]("https://www.kaggle.com/jeanmidev/smart-meters-in-london") followed by some insights on the dataset.

# Description of the dataset

To better follow the energy consumption, the government wants energy suppliers to install smart meters in every home in England, Wales and Scotland. There are more than 26 million homes for the energy suppliers to get to, with the goal of every home having a smart meter by 2020.

This roll out of meter is lead by the European Union who asked all member governments to look at smart meters as part of measures to upgrade our energy supply and tackle climate change. After an initial study, the British government decided to adopt smart meters as part of their plan to update our ageing energy system.

In this dataset, you will find a [refacted version of the data]("https://data.london.gov.uk/dataset/smartmeter-energy-use-data-in-london-households") from the London data store, that contains the energy consumption readings for a sample of 5,567 London Households that took part in the UK Power Networks led Low Carbon London project between November 2011 and February 2014. The data from the smart meters seems associated only to the electrical consumption.

To have an easier dataset to manipulate, different transformations have been applied on the dataset:

- Collection of all the data from a specific household in the same file (that was not the case in the original dataset)
- People from the same ACORN group are on the same file

The original and clean dataset can be find in the halfhourly_dataset zip file and one file looks like this snapshot.

<center>
<img src="{{ site.baseurl }}/img/posts/london_sm/hhdata_preview.png" />
<h6><i>Preview of the half hourly data</i></h6>
</center>

As you can see the dataset is quite easy to manipulate with:

- **LCLid** that corresponds to the household id
- **tstp** the timestamp of the measure
- **energy(kWh/hh)** the energy consumes in the past 30 minutes in kWh

But to make the life easier for the user of my dataset, I created two others zip files that contains some pre-process data:

- the *daily_dataset* that contains daily informations on the consumption of the households
<center>
<img src="{{ site.baseurl }}/img/posts/london_sm/dailydata_preview.png" />
<h6><i>Preview of a file in the daily_dataset folder</i></h6>
</center>
- the *hhblock_dataset* that contains the transpose data of a day for one household (as an array) with for example the hh_0 column is the consumption between 00:00 and 00:30
<center>
<img src="{{ site.baseurl }}/img/posts/london_sm/hhblock_preview.png" />
<h6><i>Preview of a file in the hhblock_dataset folder</i></h6>
</center>

This is an overview of  all the data from the smart meter, but to facilitate the exploration there is a table that stored all the households and their associated files (informations_households.csv).
<center>
<img src="{{ site.baseurl }}/img/posts/london_sm/tableref_preview.png" />
<h6><i>Preview of the reference table</i></h6>
</center>

In this table, there is:

- **LCLid** that correspond to the household id
- **stdorToU** the kind of tariff applied (*ToU* the dynamic tariff in function of the days or *Std* the classic fixed tariff)
- **Acorn** the [ACORN group]("https://acorn.caci.co.uk/") associated, that categorises the household
- **Acorn_grouped** this is another more global classification of the ACORN (fusion of different ACORN groups)
- **file** name of the file in the different zip files where you can find the data of the household

All these informations are from the original dataset but to complete the informations available to make other study there is an addition of some new datasets:

- *acorn_details.csv* : that contains the index for multiple parameters in comparison of the national (that have an index of 100)
<center>
<img src="{{ site.baseurl }}/img/posts/london_sm/accorndetails_preview.png" />
<h6><i>Preview of the details on the ACORN groups</i></h6>
</center>
- *uk_bank_holidays.csv* : the bank holidays for the period of the study
<center>
<img src="{{ site.baseurl }}/img/posts/london_sm/ukbankholidays_preview.png" />
<h6><i>Preview of the details on the bank holidays</i></h6>
</center>
- *weather_daily_darksky.csv* : the daily informations on the weather from [darksky]("https://darksky.net/dev/docs") at London during the study
<center>
<img src="{{ site.baseurl }}/img/posts/london_sm/weatherdaily.png" />
<h6><i>Preview of the daily weather informations</i></h6>
</center>
- *weather_hourly_darksky.csv* : the hourly informations on the weather from [darksky]("https://darksky.net/dev/docs") at London during the study
<center>
<img src="{{ site.baseurl }}/img/posts/london_sm/weatherhourly.png" />
<h6><i>Preview of the hourly weather informations</i></h6>
</center>

This first part offers a general overview of the content of the dataset, it's time now to obtain a clearer vision on the data from the smart meter.

# Exploration of the dataset

## Selection of the households

First step on this study is to find the best period to make the comparison. In my previous article on the electrical consumption in France there was a seasonal effect so a great period to study will be at least one year of data. In the next figure there is an illustration of the count of households with data (the 48 timestamps in the day) per day of the study.
<center>
<img src="{{ site.baseurl }}/img/posts/london_sm/count_householdday.png" />
<h6><i>Count of households per day</i></h6>
</center>

**Notes**: There is clearly an increase of the number of available households since the start of the study in end 2011, the peak is reach in 2013. A good period for our study could be 2013 (and i choose this one). But it's now important to know the distribution of the available days for this period in the households of the experiment, in the following figure there is a representation of this distribution in a boxplot.

<center>
<img src="{{ site.baseurl }}/img/posts/london_sm/boxplot_availalbedayhousehold.png" />
<h6><i>Boxplot of the number of available days for the study in 2013</i></h6>
</center>

The decision has been done to use the Households that possess at least 357 days, so on the original dataset that represents a **lost of 38 households** on the 5566 available in the dataset that's totally acceptable (less than 1%).

## Overview of the panel

One of the first realisation is to display the global consumption of these households during the year of 2013, in the following figure there is the daily global consumption of these households during the period.

<center>
<img src="{{ site.baseurl }}/img/posts/london_sm/timeseries_sumall.png" />
<h6><i>Daily consumption of the households</i></h6>
</center>

**Notes**: This is obvious that there is a link between the electrical consumption and the day of the year (same result than in my [previous article]("http://jmdaignan.com/2017/10/20/Make-a-forecast-system-of-the-national-energy-consumption/")). The seasonal effect is very clear so in this panel there is a lot of people that are using the electricity as an heating source. If the average daily outdoor temperature and the total daily consumption of the panel are crossed, the following figure display the relation:
<center>
<img src="{{ site.baseurl }}/img/posts/london_sm/ptg_like.png" />
<h6><i>Daily consumption X Outdoor temperature</i></h6>
</center>

# Next steps
As you can see this first exploration of the dataset has highlighted a lot of characteristic of the electrical consumption in London like the influence of the weather in this consumption.

Some ideas for the next analytics:
- Cross the ACORN data and the smart meter data
- Try to forecast the consumption of the different households
- Add new datasets like:
  - [EPC]("https://epc.opendatacommunities.org/docs/api") data from London
  - extra data on London like some underground or train strike during the period
