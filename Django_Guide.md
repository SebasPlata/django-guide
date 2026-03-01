# Guía Completa para Crear y Configurar un Proyecto en Django

# 

# Tipo de presentación: Documento técnico en Markdown publicado en repositorio GitHub público.

# 

# 0\. Creación de un ambiente virtual para trabajar en Django

# Paso 1: Instalar Python

# 

# Descargar Python desde:

# https://www.python.org/downloads/

# 

# Verificar instalación:

# 

# python --version

# Paso 2: Crear entorno virtual

# python -m venv venv

# 

# Activar entorno virtual (Windows):

# 

# venv\\Scripts\\activate

# 

# Si todo está correcto, aparecerá (venv) en la terminal.

# 

# Paso 3: Instalar Django y librerías necesarias

# pip install django

# pip install django-extensions

# 

# Guardar dependencias del proyecto:

# 

# pip freeze > requirements.txt

# 1\. Pasos para crear un proyecto en Django

# Paso 1: Crear el proyecto

# django-admin startproject config .

# 

# Se crean los siguientes archivos principales:

# 

# manage.py

# 

# Carpeta config/

# 

# Paso 2: Configurar la base de datos

# 

# En config/settings.py buscar la sección DATABASES.

# 

# Ejemplo con SQLite (por defecto):

# 

# DATABASES = {

# &nbsp;   'default': {

# &nbsp;       'ENGINE': 'django.db.backends.sqlite3',

# &nbsp;       'NAME': BASE\_DIR / 'db.sqlite3',

# &nbsp;   }

# }

# 

# Ejemplo con SQL Server:

# 

# DATABASES = {

# &nbsp;   'default': {

# &nbsp;       'ENGINE': 'mssql',

# &nbsp;       'NAME': 'nombre\_db',

# &nbsp;       'USER': 'usuario',

# &nbsp;       'PASSWORD': 'password',

# &nbsp;       'HOST': 'localhost',

# &nbsp;   }

# }

# Paso 3: Ejecutar migraciones iniciales

# python manage.py migrate

# 

# Esto crea las tablas internas de Django.

# 

# 2\. Pasos para crear una app en Django

# Paso 1: Crear la app

# python manage.py startapp articles

# Paso 2: Registrar la app en settings.py

# 

# En config/settings.py, agregar en INSTALLED\_APPS:

# 

# 'articles',

# 'django\_extensions',

# Paso 3: Crear un modelo (models.py)

# 

# En articles/models.py:

# 

# from django.db import models

# 

# class Article(models.Model):

# &nbsp;   title = models.CharField(max\_length=200)

# &nbsp;   content = models.TextField()

# Paso 4: Crear migraciones del modelo

# python manage.py makemigrations

# python manage.py migrate

# 3\. Repaso sobre la estructura de archivos y fólderes

# 

# Estructura general del proyecto:

# 

# project/

# │

# ├── manage.py

# ├── config/

# │   ├── settings.py

# │   ├── urls.py

# │

# ├── articles/

# │   ├── models.py

# │   ├── views.py

# │   ├── forms.py

# │   ├── urls.py

# │   ├── templates/

# │   └── migrations/

# │

# ├── requirements.txt

# └── Django\_Guide.md

# 

# Descripción:

# 

# manage.py: ejecuta comandos administrativos.

# 

# settings.py: configuración general del proyecto.

# 

# models.py: definición de entidades.

# 

# views.py: lógica que responde a solicitudes HTTP.

# 

# urls.py: enrutamiento del sistema.

# 

# templates/: archivos HTML que se muestran al usuario.

# 

# 4\. Pasos para probar cualquier modificación en Django

# 

# Si se modifica un modelo:

# 

# python manage.py makemigrations

# python manage.py migrate

# 

# Para correr el servidor:

# 

# python manage.py runserver

# 

# Abrir en navegador:

# 

# http://127.0.0.1:8000/

# 

# Si hay errores:

# 

# Revisar consola.

# 

# Verificar base de datos.

# 

# Confirmar migraciones aplicadas.

# 

# 5\. Pasos para habilitar una URL que muestre todas las entidades en una página

# Paso 1: Crear vista en views.py

# from django.shortcuts import render

# from .models import Article

# 

# def article\_list(request):

# &nbsp;   articles = Article.objects.all()

# &nbsp;   return render(request, 'article\_template.html', {'articles': articles})

# Paso 2: Crear urls.py en la app

# 

# En articles/urls.py:

# 

# from django.urls import path

# from . import views

# 

# urlpatterns = \[

# &nbsp;   path('', views.article\_list, name='article\_list'),

# ]

# Paso 3: Conectar rutas en config/urls.py

# from django.urls import path, include

# 

# urlpatterns = \[

# &nbsp;   path('', include('articles.urls')),

# ]

# Paso 4: Crear template article\_template.html

# 

# En articles/templates/article\_template.html:

# 

# <h1>Lista de Artículos</h1>

# 

# {% for article in articles %}

# &nbsp;   <h2>{{ article.title }}</h2>

# &nbsp;   <p>{{ article.content }}</p>

# {% endfor %}

# 6\. Pasos para habilitar una URL que genere una nueva entidad

# Paso 1: Crear formulario en forms.py

# from django import forms

# from .models import Article

# 

# class ArticleForm(forms.ModelForm):

# &nbsp;   class Meta:

# &nbsp;       model = Article

# &nbsp;       fields = \['title', 'content']

# Paso 2: Crear vista para crear entidad

# 

# En views.py:

# 

# from .forms import ArticleForm

# from django.shortcuts import redirect

# 

# def article\_create(request):

# &nbsp;   if request.method == 'POST':

# &nbsp;       form = ArticleForm(request.POST)

# &nbsp;       if form.is\_valid():

# &nbsp;           form.save()

# &nbsp;           return redirect('article\_list')

# &nbsp;   else:

# &nbsp;       form = ArticleForm()

# 

# &nbsp;   return render(request, 'article\_form.html', {'form': form})

# Paso 3: Agregar ruta en urls.py

# path('create/', views.article\_create, name='article\_create'),

# Paso 4: Crear template article\_form.html

# <h1>Nuevo Artículo</h1>

# 

# <form method="POST">

# &nbsp;   {% csrf\_token %}

# &nbsp;   {{ form.as\_p }}

# &nbsp;   <button type="submit">Guardar</button>

# </form>

# Referencias

# 

# Documentación Oficial Django: https://docs.djangoproject.com/

# 

# Documentación Oficial Python: https://docs.python.org/

