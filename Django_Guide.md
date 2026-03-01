# Guía Completa para Crear y Configurar un Proyecto en Django

> Tipo de presentación: Documento técnico en Markdown publicado en
> repositorio GitHub público.

------------------------------------------------------------------------

## 0. Creación de un ambiente virtual para trabajar en Django

### Paso 1: Instalar Python

Descargar Python desde: https://www.python.org/downloads/

Verificar instalación:

``` bash
python --version
```

### Paso 2: Crear entorno virtual

``` bash
python -m venv venv
```

Activar entorno virtual (Windows):

``` bash
venv\Scripts\activate
```

Si todo está correcto, aparecerá `(venv)` en la terminal.

### Paso 3: Instalar Django y librerías necesarias

``` bash
pip install django
pip install django-extensions
```

Guardar dependencias del proyecto:

``` bash
pip freeze > requirements.txt
```

------------------------------------------------------------------------

## 1. Pasos para crear un proyecto en Django

### Paso 1: Crear el proyecto

``` bash
django-admin startproject config .
```

Se crean los siguientes archivos principales:

-   `manage.py`
-   Carpeta `config/`

### Paso 2: Configurar la base de datos

En `config/settings.py` buscar la sección `DATABASES`.

#### Ejemplo con SQLite (por defecto)

``` python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

#### Ejemplo con SQL Server

``` python
DATABASES = {
    'default': {
        'ENGINE': 'mssql',
        'NAME': 'nombre_db',
        'USER': 'usuario',
        'PASSWORD': 'password',
        'HOST': 'localhost',
    }
}
```

### Paso 3: Ejecutar migraciones iniciales

``` bash
python manage.py migrate
```

------------------------------------------------------------------------

## 2. Pasos para crear una app en Django

### Paso 1: Crear la app

``` bash
python manage.py startapp articles
```

### Paso 2: Registrar la app en settings.py

Agregar en `INSTALLED_APPS`:

``` python
'articles',
'django_extensions',
```

### Paso 3: Crear un modelo (models.py)

``` python
from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
```

### Paso 4: Crear migraciones

``` bash
python manage.py makemigrations
python manage.py migrate
```

------------------------------------------------------------------------

## Referencias

-   Documentación Oficial Django: https://docs.djangoproject.com/
-   Documentación Oficial Python: https://docs.python.org/

Documento elaborado con fines académicos.
