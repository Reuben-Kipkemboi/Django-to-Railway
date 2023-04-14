## Django Web application with PostreSQL database deployment to  Railway.app
- Deployment of a django application with postgres database to railway.app

#### By Reuben Kipkemboi

## Table of Content

+ [Description](#description)
+ [Installations and prerequisites](#installations-and-prerequisites)
+ [Technology Used](#technologies-used)
+ [License](#license)
+ [Authors Info](#authors-info)

## Description
- Django is a popular web framework used for building web applications, and with the rise of cloud hosting services, it has become easier to deploy and manage these applications. One such service is Railway.app, which provides a platform for deploying and hosting web applications quickly and easily. In this article, we will discuss how to deploy a Django app to Railway.app, including the steps to set up the environment, configure the app, and deploy it using the Railway CLI tool. We will also cover some tips and best practices for deploying Django apps to Railway.app.

## Installations and prerequisites
- Make sure your web app is ready for deployment.For django web app here are the common requirements that should be in your app environment.


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

