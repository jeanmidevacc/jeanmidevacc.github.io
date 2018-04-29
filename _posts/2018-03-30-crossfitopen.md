---
layout: post
comments: true
published: true
title: Analysis of the Crossfit Open 2018
subtitle: Introduction to webscraping and Tableau Public
header-img: img/header_img/victor-freitas-546919-unsplash.jpg
---

Hello in this article, I am going to give some leads on how to create a webscraping system that has been used to collect some data from the [Crossfit games website of Reebok]("https://games.crossfit.com/regionals")


# Introduction to Crossfit

The Crossfit is defined as

```a strength and conditioning program consisting mainly of a mix of aerobic exercise, calisthenics (body weight exercises), and Olympic weightlifting
```

This program seems to have been invited in the 2000's by Greg Glassman and Lauren Jenai, and the sport is licensied under the name of **CrossFit, Inc**.

I invite you to take a look to some videos on the [Crossfit Inc channel on youtube](https://www.youtube.com/channel/UCtcQ6TPwXAYgZ1Mcl3M1vng) to have a better view of what could be the exercices to do during a session.

In my case I am practicing crossfit since August 2018, three times per week and I liked it honestly when I started I was looking the sport as some brutes that were doing gym exercice at high intensity.

More seriously, I was a little bit afraid by the intensity of the exercices that from my point of view could hurt people pretty badly, but this sport is made for everybody no need to be the king of the gym or Superman to practice crossfit.

The strenght is that every exercices can be scaled in term of weight, movement in function of your need (physical condition, injuries) but the only goal is to complete the exercice. Never give up could be the moho of crossfit.

The selection for the world cup championship are quite simple:

- The **Open**, everybody can participate to this qalifacation the disivion are defined by age and gender and if you have not an affiliate gym that can validate your performance you can film it and send it to the organisers.
- The **Regionals**, where the best from the Open will compete to be selected for the **Games**
- The **Games**, the world cup

For this article, the data collection will be only the open 2018 data that can be found at this [address](https://games.crossfit.com/open).The Open are defined by:

- a period of 5 weeks, where every week a new wod (workout of the day) is announced
- there is 4 days to try to make the best score to the wod


So why I want to use this case for my introduction to web scraping:

- I read a [cool article](https://towardsdatascience.com/my-first-battle-with-web-scraping-77e15954d13b) on the scraping of the Crossfit games website
- i found the presentation of the leaderboard quite limited in term of comparison
- I wanted to make a webscraping exercice since a long time

So let's go.


# Webscraping 101

In this case I decided to scrap the following elements:

- the [leaderboard pages](https://games.crossfit.com/leaderboard/open/2018?division=1&region=0&scaled=0&sort=0&occupation=0&page=1) for this article we will just working on the result for 2018 but if you want the approach for the previous year I invite you to look this [article](https://towardsdatascience.com/my-first-battle-with-web-scraping-77e15954d13b) on the topic
- the [athletes pages](https://games.crossfit.com/athlete/153604), because every athlete have a oage with some interesting informations
- the [gym pages](https://games.crossfit.com/affiliate/3220) that contains some details on the location of the athletes

To collect the data form this website I used the package called [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/), that is quite popular for the webscraping in Python.In the following sections there will be a description of the data collected and the code associated.


**You can find all functions explained in this part in this [GitHub repository](https://github.com/jeanmidevacc/crossfit_webscraping)**.

## The leaderboard

There is no proper need to scrap the webpage, the API that is used by the frontend can be called directly by a simple get request. Thanks to [@pedro](https://twitter.com/PedroPuros) to notice this.There is just a need to mention in the request:

- the code of the division
- if the leaderboard concern the scaled or not scaled leaderboard
- the page of the api (that you can get form the request of the first page)

This is the request to execute.

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/carbon_getleaderboard.png" />
<h6><i>Function to collect the leaderboard data</i></h6>
</center>

## The athlete pages

In this case the athlete web page looks like the screenshot in the following figure
<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/crossfit_athletespage.png" />
<h6><i>Example of an athlete page</i></h6>
</center>

And at then bottom of the page, there is some benchmark for some exercices.

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/stat_athlete.png" />
<h6><i>Benchmark of exercices</i></h6>
</center>

So I decided to scrap the page for all the athletes that participated to the open during the last 5 years and that represent more than 700000 pages to scrap, to optimize the collection I decided to parallelise the process and I use the following code to get the data for one page.

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/carbon_getathletes.png" />
<h6><i>Function to collect the athlete data</i></h6>
</center>



## The gym pages

In the case of the gym page, the number of informations to collect is less important than for the athletes, in the following figure there is a screenshot of the page of a gym.
<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/gym_details.png" />
<h6><i>Example Athlete page</i></h6>
</center>

The script will focus on the details on the header of the page, that concern the location. The number of pages to scrap in this case is around 10000 pages, and the following code has been used to do that.

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/carbon_getgym.png" />
<h6><i>Function to collect the athlete data</i></h6>
</center>



## Ethic behind the process

As you have read there is a lot of data that have been collected by my system , it's legal or not ?.

If I refer to the common belief, it's on internet so it's free that's fine well it seems that it's more complicated than that if I refer to [this artcile](https://benbernardblog.com/web-scraping-and-crawling-are-perfectly-legal-right/), it seems that I did something illegal because I didn't respect the term of use of the website so I decided to contact Crossfit Inc to warn them of what I have done and get their feedback on it (I contacted the organisation by their form, and some email address related to privacy etc).

**29 April 2018**: I have no feedback from them on this subject.

Let's have a look on some global insights of the dataset.

**You can find all functions explained in this part in this [GitHub repository](https://github.com/jeanmidevacc/crossfit_webscraping)**.

# Insights on the Open 2018

## Overview

In this part, it will be mostly a very general overview on the Open. The analysis will start by the gender repartition.


<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/genderrepartition.png" />
<h6><i>Gender repartition</i></h6>
</center>

It is good to see that there is quite a similar number of  men (56.8%) and the women (43.2%) (Similar as what I can see during my training) that was engaged in the Open. Let's see now the repartition of the age.

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/boxplot_age.png" />
<h6><i>Box plot of the age of the athletes (in function of the gender)</i></h6>
</center>

The distribution of the age are quite similar between the gender, the athletes with an age greater than 60 are considered as outliers. Another point to notice is that the average age of the athletes is greater than 30 years old, this is could be maybe the mark of :

- Need of experience to participate to the Open (but I will not bet on that)
- The price to be member of a box is too high (or you need some fund to be part) and the video rating is not very well promoted

The following distribution graph is a good illustration of this age segmentation.

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/distplot_age.png" />
<h6><i>Distribution of the age of the athletes</i></h6>
</center>

This is the illustration of the age segmentation that can be related to the income.Let's see now the morphology of the athletes.

## Analysis of the athletes

I used for this part the data from the athletes pages, if filter the outlier data that doesn't respect the BMI (Body Mass Index) that are not between 13 and 80, and some wrong weight and height. There is a visualisation of the morphology.

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/jointplot.png" />
<h6><i> Morphology of the athletes </i></h6>
</center>

The general physic of the athlete seems to be :

- a weight around 80 kg
- a height around 180 cm

In term f coutnry repartition , the USA is leading the way. In the following figure there is a coutn of the number of athletes engaged in the event in the USA and the top 10 other countries.

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/countcompetitor_countrytop10.png" />
<h6><i>Count of athletes that are did the open in Rx'd/scaled/both</i></h6>
</center>

I think there is no comment to do on the popularity of the crossfit in the USA, if i zoom more on the other countries there is some interesting insights. In the following there is more details on the top 10 countries (in term of number of athletes) without the USA.

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/countcompetitor_countrytop9-US.png" />
<h6><i>Count of athletes that are did the open in Rx'd/scaled/both</i></h6>
</center>

As we can see there is:

- there is a huge gap between the USA and the second country (like 200 000 athletes)
- the second country with the most important number of athletes it's not a country it's theassociation of all the athletes that was just filming their exercices
- n interesitng interest from athlete in Brazil and part of the commonwealth
- the number of athlete in Europe is less important


Let's see now some details on the gym that was scoring the athletes.

## Analysis of the Gym/Box

So to be clear the USA have some important number of gym/athletes engaged in the event. In the following there is comparison between the number of gyms in the USA and the number of gyms and the 9 others countries with the more gym engaged.

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/countaffiliate_countrytop10.png" />
<h6><i>Top 10 count of gym</i></h6>
</center>

The USA is crushing  the other countries litteraly. In the following figure, there is an illustration of the number of gym in the others countries.

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/countaffiliate_countrytop9-US.png" />
<h6><i>Number of gyms in the others top10 countries</i></h6>
</center>

It's interesitng to see that (there is a lot of similarity between the atlhete number and the gym number, that's normal):

- Brazil has an important number of gyms
- some other members of the Commonwealth (Canada,UK,Australia) are present
- France leading Europe in the ranking

But I can continue to make a lot of graph with this data, so I decided to make an interactive dashbaord that I can make evolved easily at any time and for that I will use [Tableau](https://www.tableau.com/)

# Dashboard on Tableau online


There is the dashboard, I invite you to use it in full screen.

<center>
<div class='tableauPlaceholder' id='viz1525029086914' style='position: relative'><noscript><a href='#'><img alt='Story 1 ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;cr&#47;crossfitopen&#47;Story1&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='crossfitopen&#47;Story1' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;cr&#47;crossfitopen&#47;Story1&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /></object></div>                <script type='text/javascript'>                    var divElement = document.getElementById('viz1525029086914');                    var vizElement = divElement.getElementsByTagName('object')[0];                    vizElement.style.width='100%';vizElement.style.height=(divElement.offsetWidth*0.75)+'px';                    var scriptElement = document.createElement('script');                    scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    vizElement.parentNode.insertBefore(scriptElement, vizElement);                </script>
<h6><i>Tableau dashboard</i></h6>
</center>

To finish I wanted to go furhter on the data and just focused on the benchmark exercices, to try to find a connection between them

## Relationship between the exercices

To analyse the data i had to eliminate outlier value so to do that I add the choice:

- use the dbscan to dtect the outlier after normalisation of the data (efficient byt long to apply it on all the athlete with data)
- use a statisitc approach based on the quantile, and delte the value that were below the 5% quantile limit and abve the 95% quantile

I foudn visually somme correaltion between the exercice, as the following figure illustrates the relation.

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/linearmodel_example.png" />
<h6><i>Illustration of a linear relation between two exercices</i></h6>
</center>

So I wanted to apply the research of correlation (a linear relation) to all the exercices, I applied a linear model on 1000 athletes , and i tested the model on 250 athletes to see if the model was good enough. I used the r score as an index to evaluate the efficiency of the models.

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/heatmap_linearmodel.png" />
<h6><i>rscore matrix for the different benchmark</i></h6>
</center>

On the training set, the exercices that involved to carry some weight was highly correlated to each other, the exerices that involved showed a correlation that was less important. When the models have been applied on the test set, the corealtion for the weight exercices was still good but the exercices with time was overfitted on the training set.In the following figure there is an illustration of the linear model for the exercices associated to weight.

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/linearmodel_weight.png" />
<h6><i>Linear relation for the exercices with weight</i></h6>
</center>

To have a better idea of the impact of a weight modification on one exercice, I created a table that make the conversion of for a weight modification on one exercice to a another.

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/matrix_adjust_weight.png" />
<h6><i>Adjustment matrix </i></h6>
</center>

# Conclusion and next steps

Yhis project was super interesting, the scraping of a website is definetly super interesting to collect data ,and there was some insights to get from this dataset (and quick analysis).


The next steps for this project are:

- Create a Kaggle dataset (if Reebok OK)
- Create some kind of API that will used this kind of data to give training advices
- Based on the pictures available on the athletes profile, as the age and the gender are right create a model to determine from the face of someone his gender and age (age range)
- Add more historical data on the table (i scraped the past 5 years but the format of the past data is a little bt different) and myabe add regionals and games data)
- Improve and build other dashboards
