---
layout: post
comments: true
title: How to deploy a Messenger bot in Python
---

Hello reader, in this article I will explain my approach to deploy a chatbot in Python on the Messenger platform.

# Genesis of the project
This idea to deploy a chatbot comes from different articles that I read on how non developer create a resume bot to assist them during their job search and have a vey nice digital and original front page.

One of the nicest example that I saw was the HireElibot.
<center>
<img src="{{ site.baseurl }}/img/posts/zappa_messenger/content/HireEliBot_messenger_code_1196843487068415.png" style="width: 200px;"/>
</center>

This chatbot that you can find on Messenger, is quite cool and I wanted to make one to assist me in my future job search (or at least a skeleton for a future chatbot).

I am definetly not a proper developer; but I am a coder (as a data scientist/maker can be) and a Python lover (a lot of colleagues try to push me to use node.js for this project but I refused) so I decided to try to make my own chatbot in Python.

I tried different approaches to complete this project that I am going to explain right now.

# First approach : Telegram + Python

My first approach was to try to find how to deploy a chatbot in a platform and make it available on different devices.

My research leads me to the course of Gareth Dwyer on [Codementor](https://www.codementor.io/garethdwyer/building-a-telegram-bot-using-python-part-1-goi5fncay).

<center>
 <img src="https://process.filestackapi.com/cache=expiry:max/resize=width:1050/compress/8xBGD3aKTDuWnCiubdc3" style="width: 300px;">
 </center>

**Opinion**: The course is really good and it gives a nice overview of the usage of the Telegram api to make a very simple chatbot. I used this approach to create a weather chatbot that will use the [Google maps api](https://developers.google.com/maps/?hl=fr) and the [dark sky api](https://darksky.net/dev/), you can find in this [Github repository](https://github.com/jeanmidevacc/weatherforecast_telegrambot) the chatbot.

But it's a very simple approach (with a while loop), and the Telegram platform is not the most popular as you can see.

<center><img src="https://media.giphy.com/media/xT39D1rKL92e8SQY3m/giphy.gif"></center>

So I decided to use the Messenger platform and try to find the way to deploy the bot on a server or at least avoid the "dirty" while loop with a webhook approach.

# Second approach : Messenger + Heroku + Python

In this part my research leads me to the blog of [Hartley Brody](https://blog.hartleybrody.com/) and his post ["Facebook Messenger Bot Tutorial: Step-by-Step Instructions for Building a Basic Facebook Chat Bot"](https://blog.hartleybrody.com/fb-messenger-bot/).

The article and the code are very clear, but basically the idea is to deploy a Flask application that can receive the data send by the user (by the use of webhook) and answer in consequence at the interactions.

**Opinion**:The course is really clear and I am a Flask defender so that's cool but the usage of Heroku in his free version is quite limited, I have to ping my Heroku app every 10 minutes to avoid that she fall asleep. I can pay to continue to use a proper Heroku application but I found the pricing quite high for just a chatbot.

Like most of my backend is stored in the Amazon Web Services services (dynamodb, RDS, S3), I try to find an approach that could help me to use the AWS structure and be not too expensive and I found the perfect solution for me.

# My approach
My options on AWS to deploy the Flask application were:
- EC2 instance
- Elastic beanstalk
- Lambda

The two first options were still in the same range of price that Heroku and I found the Elastic beanstalk approach very restricted. So my choice went on the AWS Lambda service.

 To deploy the Flask application on a lambda I have to find a dependency that permit that. Amazon has one it's call [Chalice](https://github.com/aws/chalice).I tested this library for some other api projects, it's good but I have to translate all my code from the Flask Framework to the Chalice framework so ...

<center><img src="https://media.giphy.com/media/ToMjGpx9F5ktZw8qPUQ/giphy.gif"></center>

But my research leads me to a super alternative at chalice called [Zappa](https://www.zappa.io/) and guess what : it's working perfectly.

To deploy a Messenger chatbot in a AWS Lambda you can follow the following process (that you can find in this [github repository](https://github.com/jeanmidevacc/messenger-bot-python-flask-zappa-amazon)).

# Process

## Step 1 : Setup a Facebook application setup
- Setup a [Facebook application](https://developers.facebook.com) in the developer portal
- Add a messenger product in the application
<center>
<img src="{{ site.baseurl }}/img/posts/zappa_messenger/pictures/setup_app.png" />
</center>
- Create a Facebook page
- Add a "send message" button to the page
<center>
<img src="{{ site.baseurl }}/img/posts/zappa_messenger/pictures/addmessagebutton.png" />
</center>
- Select the page that you have created (and accept the profile have access blah blah blah)
<center>
<img src="{{ site.baseurl }}/img/posts/zappa_messenger/pictures/settings_messenger_product.png" />
</center>
- Keep the access token that has been assigned for the application

## Step 2 : Create your Flask application
- Create a virtual environment with the command virtualenv in my case **zappa_env**
- Activate the environment with `activate zappa_env`
- Install the dependencies `pip install zappa flask awscli`
- Clone the content of my repository in a separate folder of your environment
<center>
<img src="{{ site.baseurl }}/img/posts/zappa_messenger/pictures/environnment.png" />
</center>
- Replace the `access_token` in the application.py file by the access token of your application
- Think to a `verify_token`

## Step 3 : Deploy the application on AWS
- Use the command `zappa init` in your project folder
- In this tutorial you can accept the default parameter that zappa offer you to complete the process but if you want to add some specifications go for it
- Check if in your project folder there is a zappa_settings.json file
- Your application is now initialize so we can deploy the app with the command `zappa deploy dev`
- Let's the deployment begin (chill out).
<center><img src="https://media.giphy.com/media/xEGqih6o0meyY/giphy.gif"></center>

## Step 4 : Finish to complete the Facebook application setup
On the Messenger settings page you have to setup the webhooks
- Copy paste the url of your app on the callback url field
- Add the `verify_token` that you thinked before
- Verify and save
- Subscribe the webhook to your Facebook page events
<center>
<img src="{{ site.baseurl }}/img/posts/zappa_messenger/pictures/subscribe_webhook.png" />
</center>

## Step 5 : Update you application
- To finish you need to update the `verify_token` variable on the application.py files by the token that you create previously
- Update the app with the command `zappa update dev`

## Step 6 : Test it
<center>
<img src="{{ site.baseurl }}/img/posts/zappa_messenger/pictures/test_bot.png" />
</center>

# Feedback
So with this approach you can have a Messenger chatbot that you can deploy easily without a server with a very limited cost.

I hope that this article that present my approach to deploy a chatbot will help you and if you have any remarks on my works contact me.

# Resources
+ [Gareth Dwyer course ](https://www.codementor.io/garethdwyer/building-a-telegram-bot-using-python-part-1-goi5fncay)
- [Telegram weather bot](https://github.com/jeanmidevacc/weatherforecast_telegrambot)
- [Messenger bot with Python](https://github.com/jeanmidevacc/messenger-bot-python-flask-zappa-amazon)
- [Facebook Messenger Bot Tutorial: Step-by-Step Instructions for Building a Basic Facebook Chat Bot](https://blog.hartleybrody.com/fb-messenger-bot/) by Hartley Brody
- [First step with Zappa Flask and Python 3](https://andrich.blog/2017/02/12/first-steps-with-aws-lambda-zappa-flask-and-python/) by Oliver Andrich
- [Zappa](https://www.zappa.io/)
