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


## Django Shell: play with our data
To quickly test our models and the manager, Django provides a very useful tool called **Django Shell**.
- Open a terminal on VSCode (by default it will be opened ad the root of the project).
- Run: `python man shell`. A new command prompt will be shown.
- Import our `Movie` models: `from netflix.models import Movie`
- Create a Movie named `Dracula3`: Type `Movie.objects.create(name="Dracula3")` and press `enter` to execute just that line. We should see this `<Movie: Movie object (1)>`. Now we can ask Django to display us the name of the movie instead of `object (1)`. To do that by adding `__str__` method to our model class like this:
```python
class Movie(models.Model):
    """Movie model class."""

    # Add this function
    def __str__(self):
        return self.name
...
```
Add the function to `Category` and `Tag` models in order to display their names as well.
- List all Movie to be sure that `Dracula3` has been created: type `Movie.objects.all()` and press `enter`. Now we should see the name of the movie displayed.
- We can also create a movie in another way. Type
`new_movie = Movie(name="Harry potter3")` and press `enter`.
Then after type
`new_movie.save()` and press `enter`.
- You can check that the `watch_count` of our movie is `0`. Run `new_movie.watch_count`. It should display: `0`.
- We can update the count to any number. Run `new_movie.watch_count = 10` Press `enter` and after run `new_movie.save()` to save the changes.
Then print again the `watch_count` by running `new_movie.watch_count`: you should see `10`. 
- You can show the number of movies in the database. Run `Movie.objects.count()`. Should print  `2`.
- You can remove a movie: run `new_movie.delete()`.
Then run `Movie.objects.count()` to print the count again. It should display `1`.
- You can exit the Django shell by running: `exit()`. 

So those are basics operation on our model performed on `Django shell` so you'll start taking hands, We will see more in the coming parts of this course.


## Django ORM
Django models are class abstractions that represents our database, Even if we perform operations using either the default model manager (`Movie.objects.create()`) or on the model instance `new_movie.save()`, all those operations are translated in background into SQL queries by Django ORM (Object-Relational Mapper). Django's ORM is just a pythonical way to create SQL to queries, manipulate our database and get results in a pythonic fashion. Awesome right !!!


## Register Admin
Now that you know how to manipulate your data on the shell, let's see how to manipulate it through a graphical interface.
Django allows us to easily administrate our database with a powerful tool called **Django Admin**. Basically, it provided an administration interfaces where we can easily list, create, modify, view, and delete items of our database. And to set up the admin features is easy like eating a cake.
Reminder, it's the contrib `'django.contrib.admin'` that allows provide us that powerful feature.

Open the `admin.py` file under `netflix` folder. And add those lines:
```python
from netflix.models import Movie
from netflix.models import Category
from netflix.models import Tag

admin.site.register(Movie)
admin.site.register(Category)
admin.site.register(Tag)
```
That's all.

Note: The admin interface is customizable but we won't cover this in this tutorial.

## Create a superuser
To manage our admin site we need to create a super user. It's a user with enough privilege to access the administration interface. By default, a created user is not a superuser. We will dive deep into Django authentication and roles in the next parts of this tutorial course.
To create a superuer run in a porject temrinal in VScode:
- `python manage.py createsuperuser`. It will ask to enter some values. Let use **admin** for **username**, **admin@admin.com** for the **email**, and type **admin**, for the password.
It will print some warnings: 
```
The password is too similar to the username.
This password is too short. It must contain at least 8 characters.
This password is too common.
Bypass password validation and create a user anyway? [y/N]:
```
Press `Y` and continue, we will come back later on that in the next parts of the tutorial.

There we Go.

## Start development server and add some data
Django comes with an internal light development server. To start it, open a new terminal in the project inside VScode as usual and run:
`python manage.py runserver`. You'll see this message:
```
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).
February 27, 2022 - 02:28:29
Django version 4.0.2, using settings 'django_netflix_clone.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```
That means, the server has started and is available at host `http://127.0.0.1:8000/`. You can specify your own port with `python manage.py runserver 0.0.0.0:<your_port>`.



### Login to the Admin interface and play with data
- Open your favorite browser (me I use chrome).
- Type the development URL, you should see this page

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/g67t2647fe3ibeypvdk9.png)

- Open the admin page: got to `http://127.0.0.1:8000/admin/` on the browser. It looks like this:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/e23380s9g54cpnmaxvh1.png)
-It's the login page. Enter the credentials for the super user (username = "admin", password = "admin") and validate the form.
- You'll be redirected to the admin dashboard presented below:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pc5udrkrd9cfdgwn0ns7.png)

We will come back later on `User` and `Group`. Let's play with our `Media`, `Category`, and `Tag`. Click on `Category` and add some categories. The interface is intuitive. You have the `Add` button at the top right corner.

- Add Tags with the name `Trending`, `Popular`. Set the description you want.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ap1d9gummk7ehkf7w9wg.png)

- Add categories named `Action` , `Adventure`, `Romance`, `Thriller`. 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/rcho5ucgfnzgpctmpra1.png)

- Add **Spider-Man: No Way Home** movie:
    - Add `Spider-Man: No Way Home` for the name and description
    - Select the category
    - Select tags (To select multiple tags as a movie could have more that one tag, keep this pressed down and select the tags with right mouse clicks)
    - Download the image from the internet to your computer and add it to the `preview-image` field. 

Currently, the movie page looks like this:
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/rjs0n9io8acokck6qlxe.png)
We can a bit customize Django admin to allow super users to see the `preview_image` and `description` of a movie.
- Open `admin.py` file
- Remove line `admin.site.register(Movie)` And add these code:
```python
from django.utils.html import format_html # Add this line
...
...
# add the lines below
class MovieAdmin(admin.ModelAdmin):

    def preview(self, movie):
        """Render preview image as html image."""

        return format_html(
            f'<img style="height: 200px" src="/media/{movie.preview_image}" />'
        )

    def video(self, movie):
        """Render movie video as html image."""

        return format_html(
            '''
            <video width="320" height="240" controls>
                <source src="%s" type="video/mp4">
                Your browser does not support the video tag.
            </video>''' % movie.file
        )

    preview.short_description = 'Movie image'
    video.short_description = 'Movie video'
    list_display = ['name', 'preview', 'video', 'description']

admin.site.register(Movie, MovieAdmin)
```

- We used the Django class `django.contrib.admin.ModelAdmin` to customize how our Movie will be displayed. We used the django formatter `django.utils.html.format_html` to render the `preview_image` and `file` video as HTML.
- With `list_display = ['name', 'preview', 'video', 'description']` we ask django to display the computed `preview` and `video` and also the `name` and `description` attributes of each `Movie` record of the database.

And this is the result:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/mavmd6ae8sz1no13ad1h.png)

NB: The full source code is available here:  [https://github.com/fayomihorace/django-netflix-clone](https://github.com/fayomihorace/django-netflix-clone)


### Congratulations.
We are at the end of this first part.


# Summary
In this tutorial you've learned
- what is Django
- how to create a Django project
- how to create an app and link it to the project
- the basics Django files architectures
- the basics Django settings
- what is a model and how to create models
- How to use the basic Django model fields
- what is Django ORM
- what is a Django Model manager and how to perform basic database operations on them
- How to use Django Shell to easily play with our models
- what are migrations, and how to create them
- How to start a Django development server
- what are statics files and media files and how to set up them
- How to register your models with Django admin and create a superuser
- How to manage our database using the Django admin interface.


If you faced any blocker or error while following this tutorial, please drop a comment, I'll reply to help you as soon as possible. Excited to have you in part 2 of this tutorial-course. 
