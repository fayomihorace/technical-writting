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


