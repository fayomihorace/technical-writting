# Build a Netflix clone with Django - Part 1 (Complete beginner course)

![Django and Netflix](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/g4qht1jfclf5qdz799mo.png)

Hello, welcome to the first part of this tutorial-course in which you'll learn and understand the basics of Django by building a clone of the popular website [Netflix](https://www.netflix.com/).
The tutorial is divided into 5 parts as follows:

1. Part 1: **Setup the project, create the database, and manage it with Django Admin**.
2. Part 2: **Add authentication features**.
3. Part 3: **Add Netflix basics features (List, search, filter, watch movies)**.
4. Part 4: **Unit test your Django project**.
5. Part 5: **Deploy your Netflix clone to Netlify (and add it to your portfolio 😌😉)**.


This tutorial assumes that you have at least basics knowledge of:

- Python3 programming language 
- Linux
- Html and CSS
- Mysql.

-------------------------

## Presentation of Django
Django is one of the most popular Frameworks to build full-stack websites and also APIs. It follows the MVT (Model, views, and template) architecture.

- What are **Models**: The models are python object representations of our database. For example, the class *User* represents **the user's table in our database** and an instance of that *User* class is a record in the table *users*. Then, each attribute of the user class is a column of the table **users*** and the value of each attribute for a specific instance of **User** class, is the value of the column related to that attribute. Don't worry if it sounds weird now, you'll understand better in deep before the end of this tutorial when we will play with models.

- What are **Templates**: To make it simple, *templates* are what end-users see (the interface made by HTML and CSS, button, images, text, animations, etc...).

- What are **Views**: *Views* are the listeners to users' actions on the *templates*. User action might trigger a request to a *View*. The view will perform an operation related to handling user requests, if required it will interact with the database (throughout *models*) and return a response to the user if needed. The response could be another template or html page, or a Json response or even another type of response.

- In addition, an important thing to know for the beginning, is that a Django project is divided into small reusable pieces (python modules) called **apps**.


## Django installation
To install Django on your system, I advise you to:

1. work on [a virtual environnemt](https://docs.python.org/3/library/venv.html) or a conda workspace. But first, make sure python is installed on your computer.

2. Create a `requirements.txt` file that will contain the list of python libraries and their versions required by our project to work well on any computer. For now, we just need `Django` so our `requirements.txt` will just contain one line like this:

```
Django==4.0.2
```
At the time of this article, the latest stable version of Django is `4.0.2`.

3. Then run `pip install -r requirements.txt` or  `python -m pip install requirements.txt` (or it could also be `python3` instead of just `python` or `pip3` instead of `pip`). That will install all the packages listed in the file with the specified versions.


## Create the Django project
Let's use **django_netflix_clone** for the project name. (I used underscore because it improves the readability.

- Then run: `django-admin startproject django_netflix_clone`.

Note: `django-admin` is the Django command-line utility. It’s used for common operations in the Django development phase as we will see in the following.

A new folder named `django_netflix_clone` will be created. 
I advise you to move the `requirements.txt` file into `django_netflix_clone` folder, so it will be inside the project.
- Run: `mv requirements.txt django_netflix_clone`.

The `django_netflix_clone` project architecture should looks like this now:

![django_netflix_clone project strcucture](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/sgoepl3q0s27o7lgbb53.png)

You'll notice that Django has created some files and a default folder with the same name as the project `django_netflix_clone`.
At the root of the project we should have the following:
- **manage.py** file: it is a Python file helper used to execute commands related to the project. We will see its uses as we go.

- **django_netflix_clone** folder. We said that a Django project is made of apps. That folder is the default `app` automatically created by Django. It contains some auto-generated files as well. Let's explain a bit them:

- **wsgi.py** (Web Server Gateway Interface): When you want to deploy your Django to production in a real server, this file is used as the entrypoint, to allow the application server to connect to your Django project.
- **asgi.py** (Asynchronous Server Gateway Interface): To make it simple, it plays the same role as **wsgi.py**, but is for asynchronous web servers and applications. So it's used in the place of **wsgi.py** when your project supports asynchronous tasks.
- **settings.py**: It's the project configuration file. All the apps and projects settings are defined on it.
The important settings here are:
    - `SECRET_KEY`: it's is used to provide a cryptographic signing to the project. It is mostly used to sign session cookies (we will talk about sessions and cookies later in this course). If one were to have this key, they would be able to modify the cookies sent by the application.
    - `INSTALLED_APPS`: it's an array that contains the list of apps in the project. There are default apps added by Django like `django.contrib.admin` to manage administration (we will cover it later in this tutorial), `django.contrib.auth` to manage authentication, etc... (The apps names a self-explanatory)
    - `MIDDLEWARE`: Middlewares are functions that are executed before each request. Defaults middlewares are added by django like `django.middleware.security.SecurityMiddleware` to verify the security of request.

We will explain others settings as we go.

- **urls.py**: This file describes all the paths that could be accessed in the project. By default it contains one path (`admin`):

```
urlpatterns = [
    path('admin/', admin.site.urls),
]
```
At this step, we can say that the allowed URLs on our app are `http://my_project_url` and `http://my_project_url/admin/`

Then enter the project:
- `cd django_netflix_clone`
- Open the project in a code editor (Me I use VSCode or visual studio code)
`code .`

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hwvo6xvf9eqxurp1dyxb.png)


## Create our working app
The default app is meant to be a centralizes app. Let's create another app named `netflix` to handle our `Netflix` features. 
- On VS code open a new terminal by clicking `Terminal -> New terminal` from the top horizontal bar menu. It should open a terminal at the root project directory.
- Then type this command: `django-admin startapp netflix`.
A new folder named `netflix` will be created:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/20lzu36fvbsx04i6l8q3.png)
- open the `settings.py` file and add `netflix` app into `INSTALLED_APPS` setting as described above:
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/en6c7aqtj5je3jecb6i7.png)
This will register our `netflix` app to the project. Without that, the app is created but not linked to the project.


We won't explain all the files and folders that are newly created under `netflix` app folder now. Let's just explain `views.py` and `models.py`:
- **views.py**: it is the file that will contain our views described at the top. Each view is a class, and we will cover them in deep later in the course.
- **models.py**: It's the file that contains our models. As described above, in Django our database is represented by Django classes.
