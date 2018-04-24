---
layout: post
comments: true
published: true
title: WIP Crossfit open 2018 WIP
subtitle: Introduction to webscraping
header-img: img/header_img/victor-freitas-546919-unsplash.jpg
---

Hello in this article, I am going to give some leads on how to create a webscraping system that has been used in the data from the [Crossfit games website of Reebok]("https://games.crossfit.com/regionals")


# Introduction to Crossfit

The Crossfit is defined as

```a strength and conditioning program consisting mainly of a mix of aerobic exercise, calisthenics (body weight exercises), and Olympic weightlifting
```

This program seems to have been invited in the 2000's by Greg Glassman and Lauren Jenai, and the sport is licensied under the name of **CrossFit, Inc**.

I invite to take to some videos on the [crossfit inc channel on youtube](https://www.youtube.com/channel/UCtcQ6TPwXAYgZ1Mcl3M1vng) to have a better view of what could be the exercices to do during a session.

In my case I am practicing crossfit since August 2018, three times per week and I liked it honestly when I started I was looking the sport as some brutes that were doing gym exercice at high intensity.

<center>
<img src="https://media.giphy.com/media/iw3QWgT77aC1G/giphy.gif" />
<h6><i>My first impression</i></h6>
</center>

More seriously, I was a little bit afraid by the intensity of the exercices that form point vof view could hhurt people pretty badly, but this sport is made for everybody no need to be the king of the gym or Superman to practice crossfit.

The strenght is that every exercices can be scaled in term of weight, movement in function of your need (physical condition, injuries) but the only goal is to complete the goals that you decide to choose at the end of the training period. Never give up could the moho of crossfit.

Another think that I like is the organisation of the "world cup" call Crossfit Games, they are directed by Dave Castro, have been held every summer since 2007. Athletes at the Games compete in workouts they learn about only hours beforehand, sometimes including surprise elements that are not part of the typical CrossFit regimen. Past examples include a rough-water swim, a softball throw, and a pegboard climb.The Games are styled as a venue for determining the "Fittest on Earth," where competitors should be "ready for anything."

In 2011, the Games adopted an online qualification format, facilitating participation by athletes worldwide. During the 5-week long "CrossFit Open", one new workout is released each week. Athletes have several days to complete the workout and submit their scores online, with either a video or validation by a CrossFit affiliate. Since the Open is available to any level of athlete, many affiliates encourage member participation and the number of worldwide participants can be in the hundreds of thousands.

The top CrossFit Open performers for individuals and teams in each region advance to the regional events, held over the following two months. The Games include divisions for individuals of each gender, co-ed teams, and a number of Masters and Teenage age groups.

So why I want to use this case for my introduction to web scraping:

- I read a  [cool article](https://towardsdatascience.com/my-first-battle-with-web-scraping-77e15954d13b) on the scraping of the Crossfit games website
- i found the presentation of the leaderboard quite limited in term of comparison
- I wanted to make a webscraping exercie since a long time


# Data collection process (webscraping 101)

In this case I decided to scrap the following elements:
- the [leaderboard pages](https://games.crossfit.com/leaderboard/open/2018?division=1&region=0&scaled=0&sort=0&occupation=0&page=1) for this article we will just working on the result for 2018 but if you want the approach for the previous year i invited you to look this [article](https://towardsdatascience.com/my-first-battle-with-web-scraping-77e15954d13b) on the topic
- the [athletes pages](https://games.crossfit.com/athlete/153604), because every athlete have a oage with some interesting informations
- the [gym pages](https://games.crossfit.com/affiliate/3220) that contains some details on the location of the athletes


## the leaderboard pages

There is no proper need to scrap the webpage, the API that is used by the frontend can be called directly by a simple get request. Thanks to @pedro to notice this.There is just a need to mention in the request:
- the code of the division
- if the leaderboard concern the scald or not scalad applications
- the page of the api (athat you can get form the reauest of the first page)

In the next par,there is the request to execute.

{% highlight python %}

request_tosend="https://games.crossfit.com/competitions/api/v1/competitions/open/{}/leaderboards?division={}&region=0&scaled={}&sort=0&occupation=0&page={}".format(year,division,scaled,i+1)
response_data=requests.get(request_tosend).json()

{% endhighlight %}


## Athlete pages

In this case the athlete web page looks the screenshot in the following figure
<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/crossfit_athletespage.png" />
<h6><i>Example of an athlete page</i></h6>
</center>

And at then botoom of the page, there is some benchmarl for some exercices.

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/stat_athlete.png" />
<h6><i>Benchmark of exercices</i></h6>
</center>

So I decided to scrap the page for all the athletes that participate to the open during the last 5 years and that represent more than 700000 pages to scrap, to optimize the collection I decided to parallelise to the process and i use the following code to get the data for one page.


{% highlight python %}

def get_athletesdetails(competitorid):

    url_profile="https://games.crossfit.com/athlete/{}".format(competitorid)
    #print(url_profile)

    response=requests.get(url_profile)
    soup = bs(response.content, features="lxml")

    infobar=soup.find("ul", {"class": "infobar"})
    if not infobar is None:
        # Get link profile picture
        profile_picture=soup.find("img",{"class":"pic img-circle"})
        dict_athlete['profile_picture_link']=profile_picture["src"]

        #Get name
        name=soup.find("h3",{"class":"c-heading-page-cover"})
        firstname=name.find("small").text
        lastname=name.text.replace(firstname,"").replace("\n","").replace(" ","")
        #print(firstname,lastname)
        dict_athlete['name']=firstname+" "+lastname


        for item in infobar.find_all("li"):
            #print(item.find("div",{"class": "item-label"}).text.replace("\n","").replace(" ",""),item.find("div",{"class": "text"}).text.replace("\n","").replace(" ",""))
            dict_athlete[item.find("div",{"class": "item-label"}).text.replace("\n","").replace(" ","").lower()]=item.find("div",{"class": "text"}).text.replace("\n","").replace(" ","")
        # Get benchmark
        stats=soup.find("ul",{"class": "stats-container"})
        if not stats is None:
            for item in stats.find_all("tr"):
                #print(item.find("th",{"class": "stats-header"}).text.replace("\n","").replace(" ",""),item.find("td").text.replace("\n","").replace(" ",""))
                dict_athlete["bs_"+item.find("th",{"class": "stats-header"}).text.replace("\n","").replace(" ","").lower()]=item.find("td").text.replace("\n","").replace(" ","")
    time.sleep(0.5)
    return dict_athlete

{% endhighlight %}


## gym pages

In rthe case of the gym page, the number of informations to collect is less important than for the athlete, in the following figure there is a screenshot of the page of a gym.
<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/gym_details.png" />
<h6><i>Example Athlete page</i></h6>
</center>

The script will focus on the details on the header of the page, that concern the location. The number of pages to scrap in this case is around 10000 pages , and the following code has been used to do that.

{% highlight python %}

def get_affiliatedetails(affiliateid):

    list_param=["country","region","location"]

    url_profile="https://games.crossfit.com/affiliate/{}".format(affiliateid)
    #print(url_profile)
    response=requests.get(url_profile)
    soup = bs(response.content, features="lxml")

    infobar=soup.find("ul", {"class": "infobar"})
    if not infobar is None:
        for i,elt in enumerate(infobar.find_all("li")[:3]):
            dict_affiliate[list_param[i]]=elt.find_all("div",{"class": "text"})[0].text.replace("\n","").lower().replace("      ","")


        dict_affiliate["name"]=soup.find("h3", {"class": "c-heading-page-cover"}).text


    time.sleep(0.5)
    return dict_affiliate

{% endhighlight %}


## Ethic behind the process

As you have read there is a lot of data that have been collected by my system , it's legal or not.

If i refer to the common belief, it's an internet so it's free that's fine well it seesm that it's more complicated than that i refer to [this artcile](https://benbernardblog.com/web-scraping-and-crawling-are-perfectly-legal-right/), it seems that I did something illegal because i didn't respect the term of use of the website so Idecided to contact Crossfit Inc to warn of what I have done and get ther feedback on it (i contacted the organisation by their form, some email related to privacy etc).

I have no feedback from them on this subject so as the englis expression says:
**Silent is consent**

Let's have a look on some global insight of the dataset.

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


# Weight model estimator


<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/linearmodel_example.png" />
<h6><i>Illustration of a linear relation between two exercices</i></h6>
</center>



<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/heatmap_linearmodel.png" />
<h6><i>rscore matrix for the different benchmark</i></h6>
</center>


<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/linearmodel_weight.png" />
<h6><i>rscore matrix for the different benchmark</i></h6>
</center>

<center>
<img src="{{ site.baseurl }}/img/posts/crossfit/matrix_adjust_weight.png" />
<h6><i>Adjustment matrix </i></h6>
</center>


# Next steps

- Create a Kaggle dataset (if Reebok OK)
- Create some kind of API that will used this kind of data to give training advices
- Based on the pictures availalbe on the athletes profile, as the age and the gender are right create a model to determine from the face of someone his gender and age (age range)
- Add more historical data on the table (i scraped the past 5 years but the format of the past data is a little bt different) and myabe add regionals and games data)
