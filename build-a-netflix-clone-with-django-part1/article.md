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


## Design our database
We have created our project and added our main app. Now let's design our database according to our features. Remember, each model is a python class and inherits from the internal Django class `django.db.models.Model`.

- Open the `models.py` file that is inside `netflix` directory.

- We have users that will register and watch movies. By default, Django have what we call `contribs` which are pre-made modules to ease our work. From those contribs we have a default `User` model. You can customize the default Django `User` model as described [here](https://docs.djangoproject.com/en/4.0/topics/auth/customizing/). But, in this tutorial, we will just use the default one.


### Movie models
- We will have movies to watch. Let's create a model class to represent a movie.

```python
class Movie(models.Model):
    """Movie model class."""

```

That means, we will have a table named `Movie` in our database.

Each movie will have a name, let's add it:

```python
CHARS_MAX_LENGTH: int = 150

class Movie(models.Model):
    """Movie model class."""

    name = models.CharField(max_length=CHARS_MAX_LENGTH, blank=True)
```
The name column should be a string that why we used the Django model field named `CharField`. We've decided to limit the name length to 150 chars.
With the line `CHARS_MAX_LENGTH: int = 150` we define a constant to avoid using magic numbers in our code that we will use.
With line `name = models.CharField(max_length=CHARS_MAX_LENGTH, blank=True)`. we ask Django to add a column named `name` into our `movies` table and each record of movie should not have a name with more than 150 chars. `blank=True` means that, when we want to create or modify a record of the movie, we don't need to provide the name value. You'll understand more in the following. 

- Each movie could have a description, let's add it with this line:

`description = models.TextField(blank=True, null=True)`

For it we use `TextField` instead of `CharField` as the description length is not predictable, but we know movies names are not that long.
Note `null=True` means that we can create a Movie without specifying a value for the description. If `null=True` was not specified like for `name`, trying to create a movie without providing `description` will raise an exception. 
We might need to keep the creation date of the movie, let do it:
`date_created = models.DateTimeField(auto_now_add=True)`.

Not that we used `DateTimeField` because it's a date and we set attribute `auto_now_add` to `True` so the creation date will automatically be set by Django during object creation.

Our `models.py` should look like this now:


```python
from django.utils import timezone
from django.db import models


class Movie(models.Model):
    """Movie model class."""

    name = models.CharField(max_length=CHARS_MAX_LENGTH, blank=True)
    description = models.TextField(blank=True, null=True)
    date_created = models.DateTimeField(default=timezone.now)


```

- Each movie should have a category. It's better to store categories in the database as well so we can manage them. Let's create a Category model before the Movie model with `name` and `description` (the category creation date is not really useful):

```python
class Category(models.Model):
    """Category model class."""

    name = models.CharField(max_length=CHARS_MAX_LENGTH, blank=True)
    description = models.TextField(blank=True, null=True)
```

- We will suppose for this tutorial that a movie could have only one category, so that defines a OneToMany relationship. In Django it's represented by `django.db.models.ForeignKey`. Let's add it:
`category = models.ForeignKey(
        Category,
        models.CASCADE
    )`

The attribute `models.CASCADE` mean that if the category is deleted, all its movies should be deleted to avoid data invalid values (For example having some movie with non-existant category).

- Each movie will have one or more tags. It's better to store tags as well in the database. Let's create a Tag model before the Movie model with `name` and `description` (the category creation date is not really useful):

```python
class Tag(models.Model):
    """Tag model class."""

    name = models.CharField(max_length=CHARS_MAX_LENGTH, blank=True)
    description = models.TextField(blank=True, null=True)
```

- We will suppose for this tutorial that a movie could have only one category, so that defines a ManyToMany relationship. In django it's represented by `django.db.models.ManyToManyField`. Let's add it:
`category = models.ManyToManyField(Tag)`.

Note: The attribute `models.CASCADE` mean that if a tag is deleted, all it's related movies will also be deleted to avoid data invalid values (For example having some movies with non-existant tag).

- We might need some statistics about each movie like the number of people who watched it. Let's add it:
`watch_count = models.IntegerField(default=0)`.
Here, as it is a number, we used `IntegerField` and  `default = 0` means that the default value is `0`. And plus, as we defined a default value, even if we don't provide that attribute, django won't raise an exception, but set the attribute to `0`.

- Naturally, we need to store the movie video file. For that 
Nice there we go. We have all the minimal information for our database. 



### Managing static files
Before that, we will explain how Django manages files. By django serve all files related to the website (html, css, js, images) to the value of `STATIC_URL` parameter in `settings.py` file. And the default value is `/static/`. 
```python
STATIC_URL = 'static/'
``` 
You can change it with another value, but I recommend you to keep it only if you have a good reason to change it.

During development, Django recommends storing those files are stored in a folder called `static` inside each app.


### Managing media files
Now the files that are uploaded into the projects (like our movies videos) are served to the value of `MEDIA_URL` parameter in `settings.py`. That setting is not there by default, but it recommended value is `/media/` so we have to add it (we could add it right after `STATIC_URL`): 
```python
MEDIA_URL = 'media/'
```
By default, media files are not stored in the database but in a folder defined by the settings `MEDIA_ROOT`. That setting also was not there so we have to define it.

 ```python
import os # Add this at the top of the settings.py file
...
...
STATIC_URL = 'static/'
MEDIA_URL = 'media/' 
MEDIA_ROOT = os.path.join(BASE_DIR, 'media/') # Add this line too
```.
That means, our movies will be uploaded into a folder named `media` at the root of the directory. We used `os.path.join` and `BASE_DIR` (defined at the top of the file) to get the full path of the folder in the system. That folder didn't exists, so let's create it:
Make sure to be at the root of the project and run:
`mkdir media`.

- The last part to finalize the setup of static and media files is to add their values to the path. Open `urls.py` file like this:
```python
from django.contrib import admin
from django.urls import path
from django.conf import settings            # Add this line
from django.conf.urls.static import static  # Add this line

urlpatterns = [
    path('admin/', admin.site.urls),
]

# Add the lines below
urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

We have finished to set up static and media files for our project.

We can then use `django.db.models.FileField` to store the link to our movie (media) like this: 
```python
file = models.FileField(upload_to='movies/')
```.

- A last thing, we need a preview picture for our movie. As it's a picture, Let use `django.db.models.ImageField` like this: 
```python
preview_image = models.ImageField(upload_to='preview_images/')
```.

Our final `Movie` class file looks like this:
```python
class Movie(models.Model):
    """Movie model class."""

    name = models.CharField(max_length=CHARS_MAX_LENGTH, blank=True)
    description = models.TextField(blank=True, null=True)
    categories = models.ForeignKey(Category, on_delete=models.CASCADE)
    tags = models.ManyToManyField(Tag)
    watch_count = models.IntegerField(default=0)
    file = models.FileField(upload_to='movies/')
    preview_image = models.ImageField(upload_to='preview_images/')
    date_created = models.DateTimeField(default=timezone.now)

```

There we go.


## Django Migrations
We have finished to set up our basic database. Now we need to create what we call `migrations`. Migrations are Python files containing the definition and history of changes in your models. To create migration we should run the command:
`python manage.py makemigrations`. Note that we have used the project command tool `manage.py`.

After running the command, we should get this error:


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/cgnnjoclual9lztjx9r9.png)
It's because it's a module required by `ImageField` to work that is not yet installed in our environment. So we can add it to our `requirements.txt` file like this:
```
django==4.0.2
Pillow==9.0.1 # Add this line into requirements.txt
```.
Then run `pip install -r requirements.txt` again (it will only install missing modules, in our case `pillow`).

Then run again: `python manage.py makemigrations`.
You should have this result:
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ma098waurm8pu2onsk9c.png).

That will generate some files (migrations) into `migrations` folder under `netflix` module. Those files contain descriptions of our models that will be used to physically create and/update our database.


## Migrate database
To physically create our database, we need to specify the database type in the settings. If you open the `settings.py` file, you'll see:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```
`'ENGINE': 'django.db.backends.sqlite3'` means our database is `SQLite` and `'NAME': BASE_DIR / 'db.sqlite3',` is named `db.sqlite3` and should be at the root of the project. Django support multiple others backend like `MysQL` that we will see in Part5 of this course. For the time being we will stick to the defaults settings.

Now that we see that our database was already set up but understood which setting handle it, let's populate our `db.sqlite3` database with our models throughout migrations. Run `python manage.py migrate`. You'll see a message like this:


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6m3yuzkbmdbe42pt11r6.png)


## The model Manager
Each Django model has something called Manager and comes with a default one named `objects`. It allows us to perform queries to the database from our model. For example, basic queries we can do on the `Movie` model are:
- **Create a movie***: it's achieved with `Movie.objects.create(name="Avatar")`
- **Get all movies***: it's achieved with `Movie.objects.all()`.
