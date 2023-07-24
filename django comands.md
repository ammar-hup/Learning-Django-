# Django Commands

Here are some useful commands and tips for working with Django.

To check the installed Django version:

```
py -m django --version
```

To create a new Django project:

```
django-admin startproject mysite
```

To start the server:

```
py manage.py runserver
```

To create a new app:

```
py manage.py startapp polls
```

## Templates

To create a new template:

1. First, create a folder named 'templates' and make a folder with the page name in it
2. Then, make an `index.html` file in it 
3. Go to the main project settings file and import the os library (`import os`)
4. Go to `DIRS` section and add the HTML templates folder path to it (`os.path.join(BASE_DIR,'templates')`)
5. Go to the views file in the page and use a function to return the template:

```
def index(request):
    return render(request,'mainpage/index.html')
```

To make a base template file and inherit it in other HTML files:

1. Make a `base.html` file in templates folder
2. Write your HTML and CSS in it 
3. Go to the `index.html` file
4. Extend the `base.html` file to use it in your file by writing: `{% extends 'file path' %}` 
5. In our case, it will be like that: `{% extends 'base.html' %}`
6. To write HTML code in the file that has the extend line (`{% extends 'base.html' %}`), you need to use the block element 
7. The block element has an opening and closing tag, like this:

```
{% block (block name) %}
	content
{% endblock %}
```

8. Then, you should go to the `base.html` (the parent file) and add the two blocks (start and end):

```
{% block content %}
{% endblock content %}
```

If you want to make a footer or a navbar and use it in all pages of your project:

1. Make a folder for all the parts in your project (head, body, footer)
2. Make a `footer.html` file in the `parts` folder
3. Go to the `base.html` file and use the include line (like the block element before): `{% include 'file path' %}` the file that we make in parts folder
4. In this case, it will be like this: `{% include 'parts\footer.html' %}`

## HTML and Blocks

To use `if`, `elif`, `else`, `for` with HTML by blocks:

```
{% if name == 'ammar' %}
	<h1> hi <h1>
{% elif name == 'ammar' %}
	<h1> no <h1>
{% else %}
	<h1> not found <h1>
{% endif %}

{% for x in name %}
	<p>{{x}}<p>
{% endfor %}
```

## Static Files

To add images, CSS, and JS files to your project by static files:

1. Make a folder named 'static' in your project
2. Make a folder for each file you will add to your project (CSS, JS, images)
3. Add the file to its folder 
4. Go to `settings.py` file in the project to activate the static files
5. Go to the `STATIC_URL` section and add the static folder path that you have created
6. Write this line before the `STATIC_URL`: `STATIC_ROOT = os.path.join(BASE_DIR,'static')` that will put the file beside the project folder in the hierarchy
7. Write this line after the `STATIC_URL`: `STATICFILES_DIR = [ os.path.join(BASE_DIR,'project/static') ]` that will identify it with the folder path
8. Run the command `py manage.py collectstatic`
9. That will create a new static folder in your hierarchy beside the project, and it will have a folder named `admin`

To enable these files to your HTML:

1. Go to your HTML file (`base.html` for example) and write these two lines:

```
{% load static %}
```

2. Then, add the file that you want (image/CSS/JS) by its tag in HTML (for example, here we add CSS file):

```
<link rel="stylesheet" href="{% static 'css\style.css' %}">
```

If you want to add an image, you should first put the image in the `static/images` folder in the project, then use:

```
{% load static %}
{% block content %}
<img src="{% static 'image/male.png' %}" alt="not found!">
{% endblock content %}
```

## Models and Database

To work with models (database):

1. Make a new app which you will use this data in2. Connect it to your main app by using the `settings.py` file 
3. Make a new file in your new app called `urls.py` and connect it with the main app `urls.py` by the include method
4. Go to your `models.py` file then start to specify the data into a class
5. We will use the `models` which is imported by Django and it has all fields we need
6. The code will be something like this:

```
class Product(models.Model):
    name = models.CharField(max_length=50)    # the product name
    content = models.TextField()              # the product content
    price = models.DecimalField(max_digits=6,decimal_places=2)    # product price
    image = models.ImageField(upload_to='photos/%y/%m/%d')        # product image
    active = models.BooleanField(default=True)                  # product status (active / not active)
```

7. For the image part, we will split the pics into folders named (year, month, day), and that will be helpful and increase our website speed. We will use (`upload_to='folder name </photo> /%y/%m/%d'`).

## Admin Panel

To make an administration account and manage your products:

1. Run `py manage.py createsuperuser`
2. Then, choose a name, email, and password
3. Run the server and go to `/admin`