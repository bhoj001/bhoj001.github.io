---
layout: post
title: "Implementing github hook to run jenkins job on a local machine"
description: "Jenkins, github hook with python"
tags: [python, design-pattern]
# link: http://mademistakes.com  
published: true
authors: 'Bhoj Bahadur Karki'
name: 'Bhoj Bahadur Karki'
share: true
---

## Background
In this article you will see how you can implement github hook and run your jenkin's job running on you local computer. As you already know in order for github hook to trigger you need a valid internet ip address or domain name. You can not use localhost:8080( default port for jenkin's job) on github hoook. 

### What is jenkins and github hook?
In the world of continuous integration and continuous delivery(CI/CD) git and jenkins are important tools. Jenkins is a tool to automate jobs and run or build your program based on various configuration such as run it periodically on fix time, or run it using github hook or be it triggered using a callback url from remote machine, etc. This makes jenkins suitable for building your program and testing it. Git helps to integrate our code and thus helps various developer to collaborate on a single project. This fulfills the CI part of CI/CD portion. Jenkins helps in pulling your code from version control system and build them using Jenkin's jobs.

### How to set up jenkins?
Setting up jenkins is simple you need to have java installed, and you can download jenkins from jenkins website([Downlaod Jenkins](https://www.jenkins.io/doc/book/installing/) ). Jenkins supports windows, Linux, and Mac, you can download for your operating system. Once you download and install it it automatically runs in port 8080 i.e localhost:8080. 
After installation you need to get the secrete key from mention location when you type localhost:8080 and enter it in the box after that you need to create a user and login via that user.

## Creating a job to be triggered:
To create a job follow these steps:
1.  login to the jenkins web interface using localhost:8080/
2.  create jenkins job using create new item
3.  select free style porject for creting job and save it.
<br/>   
<img src="/images/my_post_images/2020_08_3_new_item.png" />
<br/>
4. add a description and save if for now. You will have to come back later to change these configuration.

After this steps let's setup github to support hook trigger.
Follow these steps to create github.
1. create a github repository (you will need to have github account)
2. Let's add a index.html file in it.
3. make a clone of it in your local machine and link it to remote repository.

For now this is all we have to do. 
Let's come to the important stuff. How are we going to link github to our localhost:8080 as our jenkins is hosted in local machine. Welcome to the world of ngrok. The ngrok is an awesome tool it let's you expose you local machine port to the internet through it's server.

## Setup ngrok in local machine.
Follow these steps to setup
1. download ngrok for your machine. [Download ngrok](https://ngrok.com/download)
2. Create an ngrok account you need an api to identify your machine and run ngrok properly.
3. Login to ngrok and get the secrete key
4. Go to the directory where ngrok is download and unzip it and then run these commands(if you are in linux you have to unzip ngrok)
   1. unzip /path/to/ngrok.zip (For unzipping ngrok, after that go inside the unzip foloder)
   2. ./ngrok authtoken <  your secrete key here >
   3.  ./ngrok help
   4.  ./ngrok http 8080

After this you will get an ngrok url link which maps to your localhost:8080 port. 
Some thing like this
https://a5131490f792.ngrok.io -> http://localhost:8080   
This links updates everytime you run it. If you run another time you will get a different link like this
<img src="/images/my_post_images/2020-08-31_ngrok.png">    
You can now use this ngrok link( https://a5131490f792.ngrok.io  ) and add it in github for web hooks.

## Finishing github part.
You have added a repo and pushed to the github now got to settings and click on webhooks section.
Once you are there you click on add web hook
and link the ngrook in you github webhooks like this
<br/>
<img src="/images/my_post_images/2020-08-31_github_webhook.png"/>

After that click on webhooks and it will be saved. By default github will tirggers this on every push request to the github.
Now back to jenkins, let's configure githubhooks
## Finishing jenkins part
Follow these steps
1. go to your job previously created
2. go to configuration
3. Go to Source code manage management and link your github repot to jenkins. Let's see my java program which is a public repo being attacted to jenkins
<img src="/images/my_post_images/from%202020-08-31_link_github.png"/>
4. Go to Build triggers and check __GitHub hook trigger for GITScm polling__
<img src="/images/my_post_images/2020-08-31_build_tirggers.png" />
5. When ever there is push to the repo this hook will be triggered and your reposetory code will be downloaded. 


That's it !  Now everytime you make push to your github repo this jenkins job will be automatically triggered and pull code from github. You can add scripts to build you project and many more configuration.


### Conclusion
In conclusion, in this article you saw how to link your jenkins running on local machine to be triggered using github hook. 