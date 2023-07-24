py -m django --version

django-admin startproject mysite(proj name)

py manage.py runserver

py manage.py startapp polls


/////////////////////////////////////////////

models -> views <- templates 

DTL (django template language)

*creating a template*

1.firat we create a folder named 'templates' and make a folder with the page name in it then
we make an index.html in it 

2.we go to the main progect sittings file and import the os library (import os)

3.we go to 'DIRS' section and add the html templates folder path to it
os.path.join(BASE_DIR,'templates')

4.then we go to views file in our page and use the function to return the template
def index(request):
    return render(request,'mainpage/index.html')

//////////////////////////////////////////////

*make a base template file and inherent it in our other html files*

1.make a base.html file in templates folder
2.write ur html and css in it 
3.go to the index.html file
4.extend the base.html file to use it in ur file by writing :{% extends 'file path' %} 
5. in our case it will be like that : {% extends 'base.html' %}
6. to write html code in the file that have extend line ({% extends 'base.html' %})
u need to use the block element 
7.the block elemnt have an open and close tags
8.its like that 
{% block (block name) %}
	content
{% endblock %}
9.then u should go to the base.html (the parent file)
and add the 2 blocks(start and end)
{% block content %}
{% endblock content %}

if we want to make a footer or a navbar and use it in all pages of our project

1.we make a folder for all the parts in our project(head,body,footer)
2.we make a footer.html file in the (parts) folder
3.we go to the base.html file and make the include line (like the block element before)
4.{% include 'file path' %} the file that we make in parts folder
5.in this case it will be like this {% include 'parts\footer.html' %}

//////////////////////////////////////////////

*use (if elif else , for) with html by blocks 

{% if name == 'ammar' %}
	<h1> hi <h1>
{% elif name == 'ammar' %}
	<h1> no <h1>
{% else %}
	<h1> not found <h1>
{% endif %}
--------------------------------
{% for x in name %}
	<p>{{x}}<p>
{% endfor %}

////////////////////////////////////////////////

*add images , css, js files to ur project by static files

1. make a folder named 'static' in ur project
2. make a folder for each file u will add to ur project (css,js,images)
3. add the file to its folder 
4. go to sittings file in the project to activate the static files
5. go to STATIC_URL section and add the static folder path that u have created
6. write this line before the STATIC_URL -> STATIC_ROOT = os.path.join(BASE_DIR,'static') that will put    	the file beside the project folder in the hierarchy
7. write this line after the STATIC_URL -> STATICFILES_DIR = [ os.path.join(BASE_DIR,'project/static') ] 	that will identefy it with the folder path
8. run the command "py manage.py collectstatic"
9. that will creat a new static folder in ur hierarchy besides the project 
	and it will have a folder named admin

/////////////////////////////////////

*to enable these files to ur html 

1. go to ur html file (base.html for example) and write these 2 lines
	{% load static %} 
the u add the file that u want (image/css/js) by its tag in html (for exaple here we add css file)
	<link rel="stylesheet" href="{% static 'css\style.css' %}">
2. if u want to add an image u should first put the image in the static/images folder in the project
	then use  : {% load static %} 
	{% block content %}
	<img src="{% static 'image/male.png' %}" alt="not found!">
	{% endblock content %}

///////////////////////////////////////////////

*if we want to make a responsive navber

1. go to the previous navbar.html file in parts folder that u made
2. we will make a span with a name of link and in the link we will put 
	the name of the page that we made in the urls file
	<span><a href="{% url 'index' %}">Home</a></span>

//////////////////////////////////////////////

* we will work with models now (database)

1.we should make a new app which we will use these data in it
2.connect it to ur main app by using 'sittings.py' file 
3.make a new file in ur new app called urls.py and connect it with the main app urls.py by include method
4.go to ur 'models.py' file then start to specify the data into a class
5.we will use the 'models' which imported by django and it have all fields we need
6.the code will be somthing like this :
----------------------------------------------
 class Product(models.Model):
    name = models.CharField(max_length=50)	#the product name
    content = models.TextField()			#the product content
    price = models.DecimalField(max_digits=6,decimal_places=2)	#product price
    image = models.ImageField(upload_to='photos/%y/%m/%d')		#product image
    active = models.BooleanField(default=True)				#product status (active / not active)
------------------------------------------------------------------
7.for the image part we will split the pics into folders nammed (year,month,day)
  and that will be helpful and increase our website speed
  we will use (upload_to='folder name <'photo' fo example> /%y/%m/%d')

////////////////////////////////////////////////////////

* to make an administration account and manage ur products

1. run 'py manage.py createsuperuser'
2. then u choose name and email and password
3. run the server and go to /admin then log in
4. u need to but the product class u have created to the database
5. use 'py manage.py makemigrations'
6. then use 'py manage.py migrate'
7. now run the servver and u can edit/add/remove a product from the admin page

/////////////////////////////////////////////////////////////////////////////////

* show the products u added in my website

1.go to the templates folder and make a new folder named 'products'
2.we make 2 html files one for product and the other for all products (product.html,products.html)
3.we go to the 'views' file in product folder and make 2 methods (product , products)
---------------------------------------------------------------------------------------
   def product(request):
      return render(request, 'products/product.html')

   def products(request):
      return render(request, 'products/products.html',{'productsName' : Product.objects.all()})
-----------------------------------------------------------------------------------------------
5.then we go to the products.html file and make a block content for coade and make a for loop 
  on the products to view it 
-----------------------------------------------------------------------------------------------
	{% block content %}

	{% for x in productsName %}
   	 	<h1>{{x.name}}</h1>
    		<h5>{{x.content}}</h5>
    		<span>{{x.price}} $</span>
	{% endfor %}

	{% endblock content %}
-----------------------------------------------------------------------------------------------






