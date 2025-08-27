# Actividad-5-final-curso-django-27-agosto
proyecto final
¡Por supuesto! A continuación, te presento una **versión única, completa, funcional y coherente** del proyecto, combinando ambas respuestas en una guía paso a paso **totalmente autónoma**, con todos los archivos, estructura, configuraciones y explicaciones necesarias.

Este proyecto está listo para copiar, pegar y ejecutar. Es **100% funcional**, sin faltar ningún archivo, y sigue buenas prácticas de desarrollo Django.

---

# 🎵 **Práctica 4: Aplicación Web Musical con Django (Proyecto Completo y Funcional)**

> **¡Bienvenido!** En esta práctica construiremos una aplicación web completa para gestionar **artistas y sus canciones**, usando solo **Django, Python, HTML y CSS puro**.  
>  
> ✅ Sin Bootstrap  
> ✅ Sin `crispy_forms`  
> ✅ Con diseño moderno y responsive  
> ✅ Con estructura profesional  
> ✅ Ideal para estudiantes principiantes

---

## 🧰 **Tecnologías y Requisitos**

- Python 3.8+
- Django
- Pillow (para imágenes)
- Sistema: Windows, macOS o Linux
- Editor: VS Code recomendado

---

## 📁 **Estructura Final del Proyecto**

```
Curso_Django/
└── Artista/
    ├── .venv/                     # Entorno virtual
    ├── backend_artista/           # Configuración del proyecto
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
    │   ├── templates/
    │   │   ├── base.html
    │   │   ├── listar_artistas.html
    │   │   ├── detalle_artista.html
    │   │   ├── formulario_artista.html
    │   │   └── confirmar_borrar.html
    │   └── static/
    │       └── css/
    │           └── styles.css
    ├── media/                     # Imágenes subidas (creado automáticamente)
    ├── manage.py
    └── requirements.txt
```

---

## 🔧 **Paso 1: Crear el Proyecto (Terminal)**

```bash
# 1. Crear carpetas
mkdir Curso_Django
cd Curso_Django
mkdir Artista
cd Artista

# 2. Abrir en VS Code
code .

# 3. Crear entorno virtual
python -m venv .venv

# 4. Activar entorno (Windows)
.\.venv\Scripts\activate

# 5. Instalar dependencias
pip install django pillow

# 6. Guardar dependencias
pip freeze > requirements.txt

# 7. Crear proyecto y app
django-admin startproject backend_artista .
python manage.py startapp app_artista

# 8. Crear carpetas necesarias
mkdir app_artista/templates
mkdir app_artista/static/css
mkdir media
```

---

## ⚙️ **Paso 2: Configuración del Proyecto**

### ✅ `backend_artista/settings.py`

```python
import os
from pathlib import Path

BASE_DIR = Path(__file__).resolve().parent.parent

SECRET_KEY = 'django-insecure-ajustame-en-produccion'
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
STATICFILES_DIRS = [BASE_DIR / 'app_artista/static']
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

## 🗃️ **Paso 3: Modelos y Formularios**

### ✅ `app_artista/models.py`

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

## 🎨 **Paso 5: Plantillas HTML**

### ✅ `app_artista/templates/base.html`

```html
{% load static %}
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block titulo %}Melodías{% endblock %}</title>
    <link rel="stylesheet" href="{% static 'css/styles.css' %}">
</head>
<body>
    <div class="wrapper">
        <header class="main-header">
            <h1><a href="{% url 'app_artista:listar_artistas' %}">🎵 Melodías</a></h1>
            <nav class="main-nav">
                <a href="{% url 'app_artista:listar_artistas' %}">📋 Artistas</a>
                <a href="{% url 'app_artista:crear_artista' %}">➕ Nuevo</a>
            </nav>
        </header>

        <main class="main-content">
            {% block contenido %}{% endblock %}
        </main>

        <footer class="main-footer">
            <p>© 2025 Melodías - Aplicación educativa de Django</p>
        </footer>
    </div>
</body>
</html>
```

### ✅ `app_artista/templates/listar_artistas.html`

```html
{% extends 'base.html' %}
{% block titulo %}Artistas | Melodías{% endblock %}

{% block contenido %}
<div class="page-header">
    <h2>Artistas Registrados</h2>
    <a href="{% url 'app_artista:crear_artista' %}" class="btn btn-primary">+ Agregar Artista</a>
</div>

<ul class="artist-grid">
    {% for artista in artistas %}
    <li class="artist-card">
        <a href="{% url 'app_artista:detalle_artista' artista.id %}">
            {% if artista.foto_artista %}
                <img src="{{ artista.foto_artista.url }}" alt="Foto de {{ artista.nombre }}" class="artist-img">
            {% else %}
                <div class="artist-placeholder">🎵</div>
            {% endif %}
            <h3>{{ artista.nombre }}</h3>
            <p class="artist-nationality">{{ artista.nacionalidad }}</p>
        </a>
    </li>
    {% empty %}
    <li class="no-artists">No hay artistas registrados.</li>
    {% endfor %}
</ul>
{% endblock %}
```

### ✅ `app_artista/templates/detalle_artista.html`

```html
{% extends 'base.html' %}
{% block titulo %}{{ artista.nombre }} | Melodías{% endblock %}

{% block contenido %}
<article class="artist-detail">
    <div class="artist-banner">
        {% if artista.foto_artista %}
            <img src="{{ artista.foto_artista.url }}" alt="Foto de {{ artista.nombre }}" class="detail-photo">
        {% else %}
            <div class="detail-placeholder">👤</div>
        {% endif %}
        <div class="artist-info">
            <h1>{{ artista.nombre }}</h1>
            <h3>{{ artista.nacionalidad }}</h3>
        </div>
    </div>

    <div class="detail-actions">
        <a href="{% url 'app_artista:editar_artista' artista.id %}" class="btn btn-secondary">✏️ Editar</a>
        <a href="{% url 'app_artista:borrar_artista' artista.id %}" class="btn btn-danger">🗑️ Borrar</a>
    </div>

    <section class="songs-section">
        <h2>Canciones</h2>
        {% if artista.canciones.all %}
        <ul class="song-list">
            {% for cancion in artista.canciones.all %}
            <li>{{ cancion.titulo }}</li>
            {% endfor %}
        </ul>
        {% else %}
        <p class="empty-message">Este artista no tiene canciones registradas.</p>
        {% endif %}
    </section>
</article>
{% endblock %}
```

### ✅ `app_artista/templates/formulario_artista.html`

```html
{% extends 'base.html' %}
{% block titulo %}{{ titulo }} | Melodías{% endblock %}

{% block contenido %}
<h2>{{ titulo }}</h2>
<form method="post" enctype="multipart/form-data" class="form-styled">
    {% csrf_token %}
    {{ form.as_p }}
    <div class="actions">
        <button type="submit" class="btn btn-primary">Guardar</button>
        <a href="{% url 'app_artista:listar_artistas' %}" class="btn">Cancelar</a>
    </div>
</form>
{% endblock %}
```

### ✅ `app_artista/templates/confirmar_borrar.html`

```html
{% extends 'base.html' %}
{% block titulo %}Eliminar Artista | Melodías{% endblock %}

{% block contenido %}
<h2>¿Estás seguro de que quieres borrar a "{{ artista.nombre }}"?</h2>
<p>Esta acción no se puede deshacer.</p>
<form method="post">
    {% csrf_token %}
    <div class="actions">
        <button type="submit" class="btn btn-danger">Sí, Borrar</button>
        <a href="{% url 'app_artista:detalle_artista' artista.id %}" class="btn">Cancelar</a>
    </div>
</form>
{% endblock %}
```

---

## 🎨 **Paso 6: Estilos CSS**

### ✅ `app_artista/static/css/styles.css`

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: #333;
    line-height: 1.6;
}

.wrapper {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
}

.main-header {
    background: white;
    padding: 1rem 2rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    border-bottom: 1px solid #eee;
}

.main-header h1 a {
    color: #4a5568;
    text-decoration: none;
    font-size: 1.8rem;
    font-weight: 700;
}

.main-nav a {
    margin-left: 1.5rem;
    color: #4a5568;
    text-decoration: none;
    font-weight: 500;
    transition: color 0.3s;
}

.main-nav a:hover {
    color: #667eea;
}

.main-content {
    flex: 1;
    padding: 2rem;
    max-width: 1200px;
    margin: 0 auto;
    width: 100%;
}

.page-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 2rem;
    flex-wrap: wrap;
    gap: 1rem;
}

.artist-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 1.5rem;
    list-style: none;
}

.artist-card {
    background: white;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 4px 15px rgba(0,0,0,0.1);
    transition: transform 0.3s;
}

.artist-card:hover {
    transform: translateY(-5px);
}

.artist-card a {
    text-decoration: none;
    color: inherit;
    display: block;
}

.artist-img, .artist-placeholder {
    width: 100%;
    height: 200px;
    object-fit: cover;
}

.artist-placeholder {
    background: #e2e8f0;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 4rem;
}

.artist-card h3 {
    padding: 1rem;
    font-size: 1.2rem;
    color: #2d3748;
}

.artist-nationality {
    color: #718096;
    font-size: 0.9rem;
    padding: 0 1rem 1rem;
}

.artist-detail {
    background: white;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 4px 15px rgba(0,0,0,0.1);
    padding: 2rem;
}

.artist-banner {
    display: flex;
    gap: 2rem;
    margin-bottom: 2rem;
    align-items: center;
}

.detail-photo {
    width: 180px;
    height: 180px;
    border-radius: 50%;
    object-fit: cover;
    border: 5px solid #e2e8f0;
}

.detail-placeholder {
    width: 180px;
    height: 180px;
    background: #e2e8f0;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 4rem;
}

.artist-info h1 {
    font-size: 2.5rem;
    color: #2d3748;
}

.detail-actions {
    margin-bottom: 2rem;
    display: flex;
    gap: 1rem;
}

.song-list {
    list-style: none;
    counter-reset: song-counter;
}

.song-list li {
    counter-increment: song-counter;
    padding: 0.8rem 1rem;
    background: #f7fafc;
    border-left: 4px solid #667eea;
    margin-bottom: 0.5rem;
    font-weight: 500;
}

.song-list li::before {
    content: counter(song-counter) ". ";
    color: #667eea;
    font-weight: bold;
}

.btn {
    padding: 0.7rem 1.5rem;
    border: none;
    border-radius: 8px;
    font-weight: 600;
    cursor: pointer;
    transition: background 0.3s;
    text-decoration: none;
    display: inline-block;
}

.btn-primary {
    background: #667eea;
    color: white;
}

.btn-primary:hover { background: #5a67d8; }

.btn-secondary {
    background: #718096;
    color: white;
}

.btn-secondary:hover { background: #4a5568; }

.btn-danger {
    background: #e53e3e;
    color: white;
}

.btn-danger:hover { background: #c53030; }

.main-footer {
    text-align: center;
    padding: 1.5rem;
    background: white;
    color: #718096;
    font-size: 0.9rem;
    margin-top: auto;
}

@media (max-width: 768px) {
    .artist-banner {
        flex-direction: column;
        text-align: center;
    }
    .main-header {
        flex-direction: column;
        gap: 0.5rem;
    }
    .page-header {
        flex-direction: column;
        align-items: stretch;
    }
    .detail-actions {
        justify-content: center;
    }
}
```

---

## 🚀 **Paso 7: Ejecutar el Proyecto**

```bash
# 1. Activar entorno
.\.venv\Scripts\activate

# 2. Migraciones
python manage.py makemigrations
python manage.py migrate

# 3. Crear superusuario
python manage.py createsuperuser

# 4. Correr servidor
python manage.py runserver
```

---

## 🌐 **Acceder a la Aplicación**

- **Sitio web:** `http://127.0.0.1:8000/`
- **Panel de admin:** `http://127.0.0.1:8000/admin/`

---

## ✅ **Funcionalidades Completas**

- ✅ Listar artistas
- ✅ Ver detalles y canciones
- ✅ Crear, editar y borrar artistas
- ✅ Subir fotos
- ✅ Diseño moderno y responsive
- ✅ Sin librerías externas de diseño

---

## 📦 **Listo para Usar**

Este proyecto es **completo, funcional y listo para ejecutar**. Puedes:

- Copiar y pegar los archivos.
- Ejecutar los comandos.
- Probar en tu navegador.

---

## 🎁 **Consejo Final**

> Usa este proyecto como base para futuros desarrollos: añade álbumes, géneros, reproductor de audio, o incluso un sistema de usuarios.

---

✅ **¡Felicitaciones!** Has completado una aplicación web profesional con Django, desde cero, con estructura clara, diseño atractivo y sin dependencias innecesarias.

¿Quieres que te genere un archivo ZIP descargable con todo el proyecto listo?
