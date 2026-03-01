# Guía Completa para Crear y Configurar un Proyecto en Django

Tipo de presentación: Documento técnico en Markdown publicado en repositorio GitHub público.

---

# 0. Creación de un ambiente virtual para trabajar en Django

## Paso 1: Instalar Python

Descargar Python desde:
https://www.python.org/downloads/

Verificar instalación:

```bash
python --version
Paso 2: Crear entorno virtual
python -m venv venv

Activar entorno virtual (Windows):

venv\Scripts\activate

Si todo está bien, aparecerá (venv) en la terminal.

Paso 3: Instalar Django y librerías necesarias
pip install django
pip install django-extensions

Guardar dependencias del proyecto:

pip freeze > requirements.txt
1. Pasos para crear un proyecto en Django
Paso 1: Crear el proyecto
django-admin startproject config .

Se crean:

manage.py

Carpeta config/

Paso 2: Configurar la base de datos

En config/settings.py buscar la sección DATABASES.

Ejemplo con SQLite (por defecto):

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

Si se utiliza SQL Server, se debe instalar el conector correspondiente y configurar:

DATABASES = {
    'default': {
        'ENGINE': 'mssql',
        'NAME': 'nombre_db',
        'USER': 'usuario',
        'PASSWORD': 'password',
        'HOST': 'localhost',
    }
}
Paso 3: Ejecutar migraciones iniciales
python manage.py migrate

Esto crea las tablas internas de Django.

2. Pasos para crear una app en Django
Paso 1: Crear la app
python manage.py startapp articles
Paso 2: Registrar la app en settings.py

En config/settings.py, agregar en INSTALLED_APPS:

'articles',
'django_extensions',
Paso 3: Crear un modelo (models.py)

En articles/models.py:

from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
Paso 4: Crear migraciones del modelo
python manage.py makemigrations
python manage.py migrate
3. Repaso sobre la estructura de archivos y fólderes

Estructura general:

project/
│
├── manage.py
├── config/
│   ├── settings.py
│   ├── urls.py
│
├── articles/
│   ├── models.py
│   ├── views.py
│   ├── forms.py
│   ├── urls.py
│   ├── templates/
│   └── migrations/
│
├── requirements.txt
└── Django_Guide.md

Descripción:

manage.py: ejecuta comandos administrativos.

settings.py: configuración general del proyecto.

models.py: definición de entidades.

views.py: lógica que responde a las solicitudes.

urls.py: rutas del sistema.

templates/: archivos HTML.

4. Pasos para probar cualquier modificación en Django

Si se modifica un modelo:

python manage.py makemigrations
python manage.py migrate

Para correr el servidor:

python manage.py runserver

Abrir en navegador:

http://127.0.0.1:8000/

Si hay errores:

Revisar consola.

Verificar base de datos.

Confirmar migraciones aplicadas.

5. Pasos para habilitar una URL que muestre todas las entidades en una página
Paso 1: Crear vista en views.py
from django.shortcuts import render
from .models import Article

def article_list(request):
    articles = Article.objects.all()
    return render(request, 'article_template.html', {'articles': articles})
Paso 2: Crear urls.py en la app

En articles/urls.py:

from django.urls import path
from . import views

urlpatterns = [
    path('', views.article_list, name='article_list'),
]
Paso 3: Conectar rutas en config/urls.py
from django.urls import path, include

urlpatterns = [
    path('', include('articles.urls')),
]
Paso 4: Crear template article_template.html

En articles/templates/article_template.html:

<h1>Lista de Artículos</h1>

{% for article in articles %}
    <h2>{{ article.title }}</h2>
    <p>{{ article.content }}</p>
{% endfor %}
6. Pasos para habilitar una URL que genere una nueva entidad
Paso 1: Crear formulario en forms.py
from django import forms
from .models import Article

class ArticleForm(forms.ModelForm):
    class Meta:
        model = Article
        fields = ['title', 'content']
Paso 2: Crear vista para crear entidad

En views.py:

from .forms import ArticleForm
from django.shortcuts import redirect

def article_create(request):
    if request.method == 'POST':
        form = ArticleForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('article_list')
    else:
        form = ArticleForm()

    return render(request, 'article_form.html', {'form': form})
Paso 3: Agregar ruta en urls.py
path('create/', views.article_create, name='article_create'),
Paso 4: Crear template article_form.html
<h1>Nuevo Artículo</h1>

<form method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Guardar</button>
</form>
Referencias

Documentación Oficial Django: https://docs.djangoproject.com/

Documentación Oficial Python: https://docs.python.org/

Documento elaborado con fines académicos.