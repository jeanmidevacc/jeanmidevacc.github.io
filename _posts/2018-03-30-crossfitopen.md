---
layout: post
comments: true
published: false
title: Crossfit open 2018
subtitle: Introduction to webscraping
header-img: img/header_img/victor-freitas-546919-unsplash.jpg
---

Hello


# Introduction to crossfit

# Data collection process (webscraping 101)

# Insights on the Crossfit open

## Open 2018

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/genderrepartition.png" />
<h6><i>Gender repartition</i></h6>
</center>

It is good to see that there is quite a similar number of  men (56.8%) and the women (43.2%) in the leaderboard for the last open.It is auite a good representation of what I can seee during my sessions where this is quite the same number of men and women.



<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/boxplot_age.png" />
<h6><i>Box plot of the age of the athles (in function of the gender)</i></h6>
</center>

The distribution of the age are quite similar between the gender, the athletes with an age greater than 60 are considered as outliers. Another poitn to notice is that avergae age of the athletes is greater than 30 years old, this is could be the mark of :

- Need of experience to participate to the open
- the price to be member of a box is too high (or you need some fund to be part)

The following distribution graph is a good illustration of this age segmentation.

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/distplot_age.png" />
<h6><i>Distribution of the age of the athletes</i></h6>
</center>

This is the illustration of the age segmentation that can be related to the income


<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/part_leaderboard.png" />
<h6><i>Count of athletes that are did the open in Rx'd/scaled/both</i></h6>
</center>

Most of the athletes are in the scaled ledaerboard

## Athletes

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/jointplot.png" />
<h6><i>Count of athletes that are did the open in Rx'd/scaled/both</i></h6>
</center>


## Gym/Box

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/countaffiliate_countrytop10.png" />
<h6><i>Top 10 count of box</i></h6>
</center>

Theu United States the number of gyms

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/countaffiliate_countrytop9-US.png" />
<h6><i>Top 10 (- USA) count of box</i></h6>
</center>

It is interesting to see that Brazil is the seonc coutnry with the most number of box, follow by three countrues of the commonwealth, Europ is not too far of these country


<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/countcompetitor_countrytop10.png" />
<h6><i>Count of athletes that are did the open in Rx'd/scaled/both</i></h6>
</center>

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/countcompetitor_countrytop9-US.png" />
<h6><i>Count of athletes that are did the open in Rx'd/scaled/both</i></h6>
</center>


Ranking

|country|Rx'd|Scaled|Count of athletes (Rx'd + scaled)|
|:----:|:------:|:----------------:|:------------:|:------------:|
|united states|16|36|220153|
|-|82|50|25627|
|canada|14|32|21152|
|australia|31|58|20870|
|brazil|124|87|19969|
|united kingdom|57|57|18609|
|france|99|81|12272|
|germany|93|65|5333|
|south africa|59|89|4591|
|sweden|12|27|4251|



# Dashboard with Tableau Public


# Next steps

- Create a Kaggle dataset (if Reebok OK)
- Create some kind of API that will used this kind of data to give training advices
- Based on the pictures availalbe on the athletes profile, as the age and the gender are right create a model to determine from the face of someone his gender and age (age range)
- Add more historical data on the table (i scraped the past 5 years but the format of the past data is a little bt different) and myabe add regionals and games data)
