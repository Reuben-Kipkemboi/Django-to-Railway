## Django Web application with PostgreSQL database deployment to  Railway.app
- Deployment of a django application with postgres database to railway.app

**Our Road map**
![Project Roadmap](/Images/railway_app.webp)

## Table of Content

+ [Description](#description)
+ [Installations and prerequisites](#installations-and-prerequisites)
+ [Deployment](#deployment)
+ [References](#references)
+ [License](#license)
+ [Authors Info](#authors-info)

## Description
- Django is a popular web framework used for building web applications, and with the rise of cloud hosting services, it has become easier to deploy and manage these applications. One such service is Railway.app, which provides a platform for deploying and hosting web applications quickly and easily. In this article, we will discuss how to deploy a Django app to Railway.app, including the steps to set up the environment, configure the app, and deploy it using the Railway CLI tool. We will also cover some tips and best practices for deploying Django apps to Railway.app.

## Installations and prerequisites
- Make sure your web app is ready for deployment.For django web app here are the common requirements that should be in your app environment.
when I talk of ready, I mean have your application on a github repository.If not worry less.
1. First go to [Github](https://github.com/)  and create your account.
2. Once you are logged in, click the + link in the top toolbar and select New repository.
* Fill in all the fields on this form. While these are not compulsory, they are strongly recommended.
* Enter a new repository name and description. For example, you might use the name "django_test" and description "First web_app deployed to railway.app".
* Choose Python in the Add .gitignore selection list.
* Choose your preferred license in the Add license selection list.
* Check Initialize this repository with a README.
3. Press Create repository.
4. Now push your local application to the repository or clone the repository to your local computer and start pushing code remotely.
5. Open a command prompt/terminal and use the add command to add all files to git. This adds the files which aren't ignored by the .gitignore file to the "staging area".

let's start commit process:

1. `git add -A`or `git add . `or `git add *`
2. Use the status command to check that all files you are about to commit are correct (you want to include source files, not binaries, temporary files etc.). 

* `git status`

3. When you're satisfied, commit the files to your local repo.

* ` git commit -m "st:project set-up - django-test web app deployment"`

- N/B *look at this [Repository](https://github.com/Reuben-Kipkemboi/git-conventions) for more git commit conventions.You can clone it to use later*

4. At this point, the remote repo has not been changed. The last step is to synchronize (push) your local repo up to the remote GitHub repo using the following command:

* `git push origin main` or create an upstream by using the command `git push -u origin master/main`

Briefly, this is it;

```
git init #Initialize git
git remote add origin <new-github-repo-url> #Add remote origin, if you cloned the repo you will not add origin since it's already added.
git add .
git commit -m "st:project set-up - django-test web app deployment"
git push -u origin main

```



 <mark>DEBUG.</mark> This should be set as False in production (DEBUG = False). This stops the sensitive/confidential debug trace and variable information from being displayed.
 ```
 # SECURITY WARNING: don't run with debug turned on in production!
# DEBUG = True
so `DEBUG = FALSE`

 ```

<mark>SECRET_KEY.</mark>This is a large random value used for CSRF protection, etc. It is important that the key used in production is not in source control or accessible outside the production server. 

<mark>Secret key.</mark> should be stored in a `.env file`.
sample secret key.

```
# SECURITY WARNING: keep the secret key used in production secret!
# SECRET_KEY = "cg#p$g+j9tax!#a3cup@1$8obt2_+&k3q+pmu)5%asj6yjpkag"
import os
SECRET_KEY = os.environ.get('DJANGO_SECRET_KEY', 'cg#p$g+j9tax!#a3cup@1$8obt2_+&k3q+pmu)5%asj6yjpkag')

```

### How Railway Works.

- Each web application runs in its own isolated and independent virtualized container. Railway must be able to set up the appropriate environment and dependencies, as well as understand how your application is launched, in order to execute it. We provide this information in a number of text files for Django apps:

- **runtime.txt**: states the programming language and version to use.
requirements.txt: lists the Python dependencies needed for your site, including Django.
- **Procfile**: A list of processes to be executed to start the web application. For Django this will usually be the Gunicorn web application server (with a .wsgi script).
- **wsgi.py**: [WSGI](https://wsgi.readthedocs.io/en/latest/what.html) configuration to call our Django application in the Railway environment.

## Deployment.
### Install your requirements from the requirements.txt file

*  ` pip install -r requirements.txt` or `pipenv install -r requirements.txt`
### Environment variables 

-If you are using a different .env file.

### Database

* PostgreSQL database for local development and in production as well.

```
psql -U postgres
(type your password!)
create database <database-name> with owner postgres;
\q (quit)

```

For production, Use the `dj_database_url` utility to authenticate the Postgres database. 

Test my project on localhost.

```
python3 manage.py makemigrations
python3 manage.py migrate
python3 manage.py createsuperuser
python3 manage.py runserver

```
Go to `http://127.0.0.1:8000/` on your browser.

### The Procfile!

* `web: gunicorn config.wsgi`

### runtime.txt 
* Which has to include the Python version.e.g..
* `python-3.x.x`

### Commit to github

```
git init #Initialize git
git remote add origin <new-github-repo-url> #Add remote origin, if you cloned the repo you will not add origin since it's already added.
git add .
git commit -m "st:project set-up - django-test web app deployment"
git push -u origin main

```
### Deploy to railway

1. Register and login to Railway (I logged in with my Github.)
2. On Railway interface: Dashboard → +New →
**Creating project in Railway Dashboard**
![Creating project in Railway Dashboard, the Linux mascot](/Images/new_project.png)

3. Deploy from GitHub repo → Add variables:

* You can add here other environment variables required for your project.

*In Railway interface (Your project → Settings), you can add a custom domain name*.

Then, don’t forget to update the settings.py with the hostname!

`ALLOWED_HOSTS = [ “<your-domain-name>.up.railway.app”, ]`
If you forms in your Django app protected by CSRF tokens, add the CSRF_TRUSTED_ORIGINS with the url of the Railway project to the bottom of the settings.py file. Without this, you might have issues later.

`CSRF_TRUSTED_ORIGINS = [ „https://<your-domain-name>.up.railway.app” ]`

*Commit the changes and push them into your GitHub repository!*

```
git status #listing the changes

git add .

git commit -m "ft:deployment - deploy to railway.app"

git push #works if you had added upstream. else git push origin main/master

```
* At the end of this process, you may have to wait a few seconds before the page loads. While the page is loading, you may get a misleading “Bad Request (400)”. However, if you get “Server Error (500)”, it may be a database

### PostgreSQL
- On Railway interface: Dashboard → Your Project → +New → Database
![Database in Railway Dashboard](/Images/railway_project_add_database.png)

- On Railway interface: Dashboard → Your Project → +New → Database → PostgreSQL
![PostgreSQL Database in Railway Dashboard](/Images/railway_project_add_database_select_type.png)



- Have the `DATABASE_URL` in your app variables.

#### Migration
- Migrate my Django models into the production mode as well.
Let's use the railway CLI
*check hoe to install railway cli for your operating system*

* For ubuntu users *

`npm i -g @railway/cli`

*You need to have => 16.x version of Node.js installed.*

1. Railway authentication
`railway login`

2. Select your current project.

`railway link → select the current project!`

3. After that use well-known Python-Django commands for migrating models to the database.

```
railway run python manage.py migrate
railway run python manage.py createsuperuser

```
Project working :joy:

https://<your-domain-name>.up.railway.app


## References

+ [mdn web_docs](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Deployment)

+ [Medium](https://medium.com/@grbeno/deploying-django-project-from-heroku-to-railway-8ce65bf5c44)

+ [Devto community](https://dev.to/mr_destructive/django-postgresql-deployment-on-railway-app-d54)

+ [Railway](https://docs.railway.app/develop/cli)

## License

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Authors Info

Reuben Kipkemboi

<div id="site-badges" align="start">
  <a href="https://www.linkedin.com/in/reuben-kipkemboi/">
    <img src="https://img.shields.io/badge/LinkedIn-blue?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn"/>
  </a>
  <a href="https://mail.google.com/mail">
    <img src="https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white" alt="Gmail"/>
  </a>
  <a href="https://twitter.com/_RotichReuben">
    <img src="https://img.shields.io/badge/Twitter-blue?style=for-the-badge&logo=twitter&logoColor=white" alt="Twitter"/>
  </a>
  <a href="https://github.com/Reuben-Kipkemboi/Reuben-Kipkemboi">
  <img src="https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge&logo=github&logoColor=blue" alt= "Github" />
  </a>

  
</div>

[![made-with-Markdown](https://img.shields.io/badge/Made%20with-Markdown-1f425f.svg)](http://commonmark.org)

<p align = "center">
    &copy; 2023 @Reuben Kipkemboi.

</p>










