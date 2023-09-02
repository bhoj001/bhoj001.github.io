---
layout: post
title: "Deploying django app on VPS using nginx,gunicorn, supervisor "
description: "Deploying django app on VPS"
# tags: [sample post, link post]
# link: http://mademistakes.com  
published: true
authors: 'Bhoj Karki'
name: 'Bhoj Karki'
share: true
---
We have used:
<br/>
python version: python3 
<br/>
machine: linux
<br/>
django version: django-2.x


## Project Structure

Lets assume that you have a django project and the folder structure looks like this:
<figure class="first">
	<img src="/images/my_post_images/django_app_structure.png">
	<figcaption>Figure: Django app folder structure</figcaption>
</figure>

## About structure
<span class="custom_text_color_blue">mysite/</span> is our django actual project
<br/>
<br/>
<span class="custom_text_color_blue">*config/*</span> folder has config.json file which contains all our configuration file.
<br/>
<br/>
<span class="custom_text_color_blue">*mysite/settings/*</span> folder contains main setting of our application.
Setting is seprated in base.py, development.py and production.py.
If we want to run in development mode we need to pass the settings file.
base.py is common settings file and development.py is for development mode.
For running app in development we do: <span class="custom_text_color_blue">python3 manage.py runserver --settings=mysite.settings.development.</span>
<br/>
<br/>
<span class="custom_text_color_blue">*Requiremnts/*</span> folder has our requirement files 
base.txt-- all pacakges in our app
development.txt -- packages only used in development(e.g django-debug-toolbar)
production.txt -- package used in production environment but not in development.
<br/>
<br/>
<span class="custom_text_color_blue">*mysite/*</span> contains main djago apps.
<br/>
<br/>
<span class="custom_text_color_blue">*mysite_apps/*</span> contains all our apps(note this folder is a python package)


## Requirements

nginx: Nginx is used as a server to serve static files in our project to the user. 
gunicorn: This is an used as an application server to serve our application. In development mode we can do 
__python3 manage.py runserver__ and it will run the app. If we want to run the app using gunicorn we need to configure gunicorn in our application. We will see the configuration later.
supervisor: This tool is used to run gunicorn. Setting supervisor will help our application to restart in case of any problem. Supervisor is configured to handle running of gunicorn.

## What is the flow of application:
user request ---> nginx server---> gunicorn(served by supervisor)--->our django app

## Steps:
1. Install requirements
2. Setup virtualenvironment for our application
3. Clone the project from github or other version control system
4. Install requirements of our application(i.e. packages used by our app)
5. Setting up postgres database
6. Run migration and test application by running it
7. Configure gunicorn
8. Configure nginx
9. Configure supervisord (or simply supervisor)
10. Test everything is running fine


## 1. Install requirements:
Here we assume that we are using python3 for our application and we are deploying on linux machine.
We need to install python3 using pip, virtualenv. 
Enough talk lets dive into it:
Log into the vps machine using the credentials given by your vps provide. Once you are in we go through following steps:
First lets update our machine and install python3 and virtualenv:	
{% highlight Ruby %}
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install python3-pip
sudo pip3 install virtualenv
{%endhighlight%}

## 2. Setup virtualenvironment for our application:
Lets create a virutal environment and start the virtualenv
{% highlight python %}

virtualenv /Projects/venv
source /Projects/venv/bin/activate

{%endhighlight%}

## 3. Clone the project from github or other version control system
Now we clone our project from github:


{% highlight python %}

git clone http://github.com/[path to your project]/mysite.git

{%endhighlight%}

## 4. Install requirements of our application(i.e. packages used by our app)
Once the app is downloaded and virtualenv is running lets install the requirements. All our project dependent packages are in base.txt file and we use production.txt file to install from base.txt and also other packages from production.txt. We go to folder where manage.py is and run following command:

{% highlight python %}
pip install -r requirements/production.txt
{%endhighlight%}

## 5. Setting Up postgres database

1. first go to postgres as:
{% highlight postgres %}
sudo su - postgres
{% endhighlight %}

2. Then create database and user and set password in user:
{% highlight postgres %}
postgres=# CREATE DATABASE db_name;
postgres=# CREATE USER db_user;
postgres=# ALTER USER db_user PASSWORD 'myPassword';
{% endhighlight %}

3. Grant permission: 
{% highlight postgres %}
postgres=# GRANT ALL PRIVILEGES ON DATABASE db_name TO db_user;
{% endhighlight %}

4. After granting permission log out from psql using ‘\q’ symbolizing quite. Then change the postgres user to root user as:
{% highlight postgres %}
postgres@server:~$ su - root
{% endhighlight %}


## 6. Run migration and test application by running it

{% highlight python %}
cd mysite
./manage.py migrate --settings=mysite.settings.production
./manage.py runserver --settings=mysite.settings.production
{% endhighlight %}

After testing everything stop the server.

## 7. Configure gunicorn
Lets install gunicorn:
{% highlight python %}
pip install gunicorn
{%endhighlight%}

After installing gunicorn lets run our application using gunicorn for testing

{% highlight python %}
DJANGO_SETTINGS_MODULE=mysite.settings.prduction  gunicorn mysite.wsgi
{%endhighlight%}

Note: This is similar to saying __./manage.py runserver --settings=mysite.settings.production__ in development.
<br>
<br>
We can also pass workers to gunicorn using workers parameter as:
{% highlight python %}
DJANGO_SETTINGS_MODULE=mysite.settings.prduction  gunicorn --workers 3 mysite.wsgi
{%endhighlight%}

Once we verify everything is working fine we move on to configuring nginx.

## 8. Configure nginx
Lets install nginx:

{% highlight python %}
sudo apt-get install nginx
{%endhighlight%}

After installation lets configure nginx file in /etc/nginx/sites-available/
Lets create a file name mysite in /etc/nginx/sites-available/mysite  and add following configuation:
{% highlight Ruby %}
server{
	listen 80;
	server_name ipaddress(e.g.x.x.x.x);
	
	location /static {
		alias /var/www/html/Projects_static_root;
		expires 30d;
	}
	
	location /media {
	alias /var/www/html/Projects_media_root;
	expires 30d;
	}

	location / {
		include proxy_params;
		proxy_pass http://127.0.0.1:8000;
	}
	
	}
{%endhighlight%}

Here in place  ipaddress(e.g.x.x.x.x) use the ipaddress provided by your host provider.

Now, check if our configuration file was correctly written and start our nginx:
{% highlight python %}
sudo nginx -t
sudo service nginx restart
{%endhighlight%}

## 9. Configure supervisord (or simply supervisor)
Installation:
{% highlight shell %}
sudo apt-get install supervisor
{%endhighlight%}

Now, let's create mysite.conf in /etc/supervisor/conf.d/ folder:
{% highlight vim %}
sudo vim /etc/supervisor/conf.d/mysite.conf
{%endhighlight%}

and configure our project:

{% highlight RUBY %}
[program:mysite]
command=/root/Projects/venv/bin/gunicorn --workers 3 --bind 127.0.0.1:8000 mysite.wsgi:application
environment=DJANGO_SETTINGS_MODULE=mysite.settings.production
directory=/root/Projects/mysite
autostart=true
autorestart=true
stderr_logfile=/var/log/mysite.err.log
stdout_logfile=/var/log/mysite.out.log
user=root
group=www-data
programs:mysite
{%endhighlight%}

Now lets start our app uing supervisor
{% highlight shell %}
service supervisor stop
service supervisor restart
{%endhighlight%}

Now our app should be running on the ipaddress configured by you check for further details.