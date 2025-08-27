¡Claro! A continuación, te presento una **versión completamente reconfigurada** del proyecto, donde **eliminamos el archivo `styles.css`** y en su lugar **usamos Bootstrap desde la red (CDN)** para lograr un diseño moderno, atractivo y responsive, **sin perder ninguna funcionalidad ni instrucción paso a paso**.

Todo sigue siendo **100% funcional**, sin modificar `models.py`, `admin.py`, `forms.py` ni `views.py`, y ahora con una interfaz basada en **Bootstrap 5 desde CDN**, ideal para estudiantes que quieren un buen diseño sin complicaciones.

---

# 🎵 **Práctica 4: Aplicación Web Musical con Django + Bootstrap (CDN)**

> **¡Bienvenido!** En esta práctica construiremos una aplicación web para gestionar **artistas y sus canciones**, usando:
>
> ✅ Django y Python  
> ✅ HTML y Bootstrap 5 (desde CDN, sin instalación)  
> ✅ Estructura profesional  
> ✅ Diseño moderno y responsive  
> ✅ Sin archivos CSS locales  
> ✅ Ideal para principiantes

---

## 🧰 **Tecnologías y Requisitos**

- Python 3.8+
- Django
- Pillow (para imágenes)
- Conexión a internet (para cargar Bootstrap desde CDN)
- Editor: VS Code recomendado

---

## 📁 **Estructura Final del Proyecto**

```
Curso_Django/
└── Artista/
    ├── .venv/
    ├── backend_artista/
    │   ├── __init__.py
    │   ├── settings.py
    │   ├── urls.py
    │   └── wsgi.py
    ├── app_artista/
    │   ├── __init__.py
    │   ├── models.py
    │   ├── admin.py
    │   ├── forms.py
    │   ├── views.py
    │   ├── urls.py
    │   └── templates/  # Solo plantillas HTML
    ├── media/          # Imágenes subidas
    ├── manage.py
    └── requirements.txt
```

> 🔹 **Nota:** Ya no necesitamos la carpeta `static/css/` porque usaremos Bootstrap desde internet.

---

## 🔧 **Paso 1: Crear el Proyecto (Terminal)**

```bash
mkdir Curso_Django
cd Curso_Django
mkdir Artista
cd Artista

code .

python -m venv .venv
.\.venv\Scripts\activate

pip install django pillow
pip freeze > requirements.txt

django-admin startproject backend_artista .
python manage.py startapp app_artista

mkdir app_artista/templates
mkdir media
```

---

## ⚙️ **Paso 2: Configuración del Proyecto**

### ✅ `backend_artista/settings.py` (sin cambios importantes)

```python
import os
from pathlib import Path

BASE_DIR = Path(__file__).resolve().parent.parent

SECRET_KEY = 'django-insecure-tu-clave-aqui'
DEBUG = True
ALLOWED_HOSTS = []

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_artista',
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

ROOT_URLCONF = 'backend_artista.urls'

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'app_artista/templates'],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

WSGI_APPLICATION = 'backend_artista.wsgi.application'

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

AUTH_PASSWORD_VALIDATORS = []

LANGUAGE_CODE = 'es-es'
TIME_ZONE = 'UTC'
USE_I18N = True
USE_TZ = True

STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / 'app_artista/static']  # Aunque no usemos CSS, puede servir luego
STATIC_ROOT = BASE_DIR / 'staticfiles'

MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'

DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'
```

---

### ✅ `backend_artista/urls.py`

```python
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_artista.urls')),
]

if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

---

## 🗃️ **Paso 3: Modelos, Formularios y Admin**

### ✅ `app_artista/models.py` (sin cambios)

```python
from django.db import models

class Artista(models.Model):
    nombre = models.CharField(max_length=100, help_text="Nombre del artista o banda")
    nacionalidad = models.CharField(max_length=100)
    foto_artista = models.ImageField(upload_to='img_artistas/', blank=True, null=True)

    def __str__(self):
        return self.nombre

    class Meta:
        verbose_name = "Artista"
        verbose_name_plural = "Artistas"

class Cancion(models.Model):
    titulo = models.CharField(max_length=100)
    artista = models.ForeignKey(Artista, on_delete=models.CASCADE, related_name='canciones')

    def __str__(self):
        return f"{self.titulo} - {self.artista.nombre}"

    class Meta:
        verbose_name = "Canción"
        verbose_name_plural = "Canciones"
```

### ✅ `app_artista/admin.py`

```python
from django.contrib import admin
from .models import Artista, Cancion

admin.site.register(Artista)
admin.site.register(Cancion)
```

### ✅ `app_artista/forms.py`

```python
from django import forms
from .models import Artista

class ArtistaForm(forms.ModelForm):
    class Meta:
        model = Artista
        fields = ['nombre', 'nacionalidad', 'foto_artista']
```

---

## 🧠 **Paso 4: Vistas y URLs**

### ✅ `app_artista/views.py`

```python
from django.shortcuts import render, get_object_or_404, redirect
from .models import Artista
from .forms import ArtistaForm

def listar_artistas(request):
    artistas = Artista.objects.all()
    return render(request, 'listar_artistas.html', {'artistas': artistas})

def detalle_artista(request, artista_id):
    artista = get_object_or_404(Artista, id=artista_id)
    return render(request, 'detalle_artista.html', {'artista': artista})

def crear_artista(request):
    if request.method == 'POST':
        form = ArtistaForm(request.POST, request.FILES)
        if form.is_valid():
            form.save()
            return redirect('app_artista:listar_artistas')
    else:
        form = ArtistaForm()
    return render(request, 'formulario_artista.html', {'form': form, 'titulo': 'Crear Artista'})

def editar_artista(request, artista_id):
    artista = get_object_or_404(Artista, id=artista_id)
    if request.method == 'POST':
        form = ArtistaForm(request.POST, request.FILES, instance=artista)
        if form.is_valid():
            form.save()
            return redirect('app_artista:detalle_artista', artista_id=artista.id)
    else:
        form = ArtistaForm(instance=artista)
    return render(request, 'formulario_artista.html', {'form': form, 'titulo': 'Editar Artista'})

def borrar_artista(request, artista_id):
    artista = get_object_or_404(Artista, id=artista_id)
    if request.method == 'POST':
        artista.delete()
        return redirect('app_artista:listar_artistas')
    return render(request, 'confirmar_borrar.html', {'artista': artista})
```

### ✅ `app_artista/urls.py`

```python
from django.urls import path
from . import views

app_name = 'app_artista'

urlpatterns = [
    path('', views.listar_artistas, name='listar_artistas'),
    path('artista/<int:artista_id>/', views.detalle_artista, name='detalle_artista'),
    path('crear/', views.crear_artista, name='crear_artista'),
    path('editar/<int:artista_id>/', views.editar_artista, name='editar_artista'),
    path('borrar/<int:artista_id>/', views.borrar_artista, name='borrar_artista'),
]
```

---

## 🎨 **Paso 5: Plantillas con Bootstrap 5 (CDN)**

> 🔹 **No usamos ningún archivo CSS local. Todo el diseño viene de Bootstrap desde internet.**

### ✅ `app_artista/templates/base.html`

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block titulo %}Melodías{% endblock %}</title>
    <!-- Bootstrap 5 CSS desde CDN -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.10.5/font/bootstrap-icons.css" rel="stylesheet">
    <style>
        body {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding-top: 70px;
        }
        .card {
            transition: transform 0.3s;
        }
        .card:hover {
            transform: translateY(-5px);
        }
        .artist-img {
            height: 200px;
            object-fit: cover;
        }
        .btn i { margin-right: 5px; }
    </style>
</head>
<body>
    <!-- Navbar -->
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark fixed-top">
        <div class="container">
            <a class="navbar-brand" href="{% url 'app_artista:listar_artistas' %}">
                <i class="bi bi-music-note-beamed"></i> Melodías
            </a>
            <div class="navbar-nav ms-auto">
                <a class="nav-link" href="{% url 'app_artista:listar_artistas' %}">📋 Artistas</a>
                <a class="nav-link" href="{% url 'app_artista:crear_artista' %}">➕ Nuevo</a>
            </div>
        </div>
    </nav>

    <!-- Contenido -->
    <div class="container mt-4">
        {% block contenido %}
        {% endblock %}
    </div>

    <!-- Footer -->
    <footer class="text-center text-white py-3 mt-5 bg-dark bg-opacity-25">
        <p class="mb-0">© 2025 Melodías - Aplicación educativa de Django</p>
    </footer>

    <!-- Bootstrap JS (opcional, para interactividad) -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

---

### ✅ `app_artista/templates/listar_artistas.html`

```html
{% extends 'base.html' %}
{% block titulo %}Artistas | Melodías{% endblock %}

{% block contenido %}
<div class="d-flex justify-content-between align-items-center mb-4">
    <h2 class="text-white">Artistas Registrados</h2>
    <a href="{% url 'app_artista:crear_artista' %}" class="btn btn-primary">
        <i class="bi bi-plus-circle"></i> Agregar Artista
    </a>
</div>

<div class="row g-4">
    {% for artista in artistas %}
    <div class="col-md-6 col-lg-4">
        <div class="card shadow-sm h-100">
            {% if artista.foto_artista %}
                <img src="{{ artista.foto_artista.url }}" class="card-img-top artist-img" alt="Foto de {{ artista.nombre }}">
            {% else %}
                <div class="card-img-top d-flex align-items-center justify-content-center bg-light artist-img">
                    <i class="bi bi-person-fill" style="font-size: 5rem; color: #6c757d;"></i>
                </div>
            {% endif %}
            <div class="card-body">
                <h5 class="card-title">{{ artista.nombre }}</h5>
                <p class="card-text text-muted"><i class="bi bi-globe"></i> {{ artista.nacionalidad }}</p>
                <a href="{% url 'app_artista:detalle_artista' artista.id %}" class="btn btn-outline-primary w-100">
                    Ver Detalles
                </a>
            </div>
        </div>
    </div>
    {% empty %}
    <div class="col-12">
        <div class="alert alert-info text-center">
            <i class="bi bi-emoji-frown"></i> No hay artistas registrados.
        </div>
    </div>
    {% endfor %}
</div>
{% endblock %}
```

---

### ✅ `app_artista/templates/detalle_artista.html`

```html
{% extends 'base.html' %}
{% block titulo %}{{ artista.nombre }} | Melodías{% endblock %}

{% block contenido %}
<div class="row g-5">
    <!-- Información del artista -->
    <div class="col-lg-4">
        <div class="card shadow-sm text-center">
            {% if artista.foto_artista %}
                <img src="{{ artista.foto_artista.url }}" class="card-img-top rounded-circle mt-3" style="height: 180px; object-fit: cover;" alt="Foto de {{ artista.nombre }}">
            {% else %}
                <div class="rounded-circle mx-auto mt-3 d-flex align-items-center justify-content-center bg-light" style="width: 180px; height: 180px;">
                    <i class="bi bi-person-circle" style="font-size: 5rem; color: #6c757d;"></i>
                </div>
            {% endif %}
            <div class="card-body">
                <h3>{{ artista.nombre }}</h3>
                <p class="text-muted"><strong>Nacionalidad:</strong> {{ artista.nacionalidad }}</p>
                <div class="d-grid gap-2">
                    <a href="{% url 'app_artista:editar_artista' artista.id %}" class="btn btn-warning">
                        <i class="bi bi-pencil"></i> Editar
                    </a>
                    <a href="{% url 'app_artista:borrar_artista' artista.id %}" class="btn btn-danger">
                        <i class="bi bi-trash"></i> Borrar
                    </a>
                </div>
            </div>
        </div>
    </div>

    <!-- Canciones -->
    <div class="col-lg-8">
        <div class="card shadow-sm">
            <div class="card-header bg-primary text-white">
                <h4><i class="bi bi-music-note-list"></i> Canciones</h4>
            </div>
            <div class="card-body">
                {% if artista.canciones.all %}
                <ul class="list-group">
                    {% for cancion in artista.canciones.all %}
                    <li class="list-group-item d-flex justify-content-between align-items-center">
                        {{ cancion.titulo }}
                        <span class="badge bg-secondary rounded-pill">{{ forloop.counter }}</span>
                    </li>
                    {% endfor %}
                </ul>
                {% else %}
                <div class="alert alert-light text-center mb-0">
                    <i class="bi bi-music-note"></i> Este artista no tiene canciones registradas.
                </div>
                {% endif %}
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

---

### ✅ `app_artista/templates/formulario_artista.html`

```html
{% extends 'base.html' %}
{% block titulo %}{{ titulo }} | Melodías{% endblock %}

{% block contenido %}
<div class="row justify-content-center">
    <div class="col-lg-8">
        <div class="card shadow-sm">
            <div class="card-header bg-success text-white">
                <h4><i class="bi bi-pencil-square"></i> {{ titulo }}</h4>
            </div>
            <div class="card-body">
                <form method="post" enctype="multipart/form-data">
                    {% csrf_token %}
                    <div class="mb-3">
                        <label for="{{ form.nombre.id_for_label }}" class="form-label">Nombre del artista</label>
                        {{ form.nombre }}
                        {% if form.nombre.errors %}
                            <div class="text-danger">{{ form.nombre.errors }}</div>
                        {% endif %}
                    </div>

                    <div class="mb-3">
                        <label for="{{ form.nacionalidad.id_for_label }}" class="form-label">Nacionalidad</label>
                        {{ form.nacionalidad }}
                        {% if form.nacionalidad.errors %}
                            <div class="text-danger">{{ form.nacionalidad.errors }}</div>
                        {% endif %}
                    </div>

                    <div class="mb-3">
                        <label for="{{ form.foto_artista.id_for_label }}" class="form-label">Foto del artista</label>
                        {{ form.foto_artista }}
                        {% if form.foto_artista.errors %}
                            <div class="text-danger">{{ form.foto_artista.errors }}</div>
                        {% endif %}
                    </div>

                    <div class="d-grid gap-2 d-md-flex justify-content-md-between">
                        <button type="submit" class="btn btn-success">
                            <i class="bi bi-save"></i> Guardar
                        </button>
                        <a href="{% url 'app_artista:listar_artistas' %}" class="btn btn-secondary">
                            <i class="bi bi-x-circle"></i> Cancelar
                        </a>
                    </div>
                </form>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

---

### ✅ `app_artista/templates/confirmar_borrar.html`

```html
{% extends 'base.html' %}
{% block titulo %}Eliminar Artista | Melodías{% endblock %}

{% block contenido %}
<div class="row justify-content-center">
    <div class="col-lg-6">
        <div class="card shadow-sm border-danger">
            <div class="card-header bg-danger text-white">
                <h4><i class="bi bi-exclamation-triangle"></i> Confirmar Eliminación</h4>
            </div>
            <div class="card-body text-center">
                <p class="lead">¿Estás seguro de que quieres borrar al artista?</p>
                <h3 class="text-danger">{{ artista.nombre }}</h3>
                <p class="text-muted">Esta acción no se puede deshacer.</p>
                <form method="post">
                    {% csrf_token %}
                    <div class="d-grid gap-2 d-md-flex justify-content-md-center">
                        <button type="submit" class="btn btn-danger btn-lg">
                            <i class="bi bi-trash"></i> Sí, Borrar
                        </button>
                        <a href="{% url 'app_artista:detalle_artista' artista.id %}" class="btn btn-secondary btn-lg">
                            <i class="bi bi-arrow-left"></i> Cancelar
                        </a>
                    </div>
                </form>
            </div>
        </div>
    </div>
</div>
{% endblock %}
```

---

## 🚀 **Paso 6: Ejecutar el Proyecto**

```bash
.\.venv\Scripts\activate
python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

---

## 🌐 **Acceder a la Aplicación**

- **Sitio web:** [http://127.0.0.1:8000/](http://127.0.0.1:8000/)
- **Panel de admin:** [http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/)

---

## ✅ **Ventajas de esta Versión**

| Característica | Beneficio |
|----------------|----------|
| ✅ Bootstrap 5 (CDN) | Diseño moderno sin archivos CSS locales |
| ✅ Íconos con Bootstrap Icons | Interfaz visualmente atractiva |
| ✅ Responsive | Se ve bien en móviles y escritorio |
| ✅ Sin `crispy_forms` | Todo con HTML y Django |
| ✅ Mismo flujo de trabajo | Ideal para estudiantes |

---

## 🎉 **¡Proyecto Completo y Funcional!**

Has creado una **aplicación web profesional** con:
- Gestión completa de artistas y canciones
- Subida de imágenes
- CRUD completo
- Diseño moderno con Bootstrap desde internet
- Sin dependencias locales de diseño

Perfecto para aprender, enseñar o incluir en tu portafolio.

---

¿Quieres que te genere un ZIP con esta versión lista para descargar?
