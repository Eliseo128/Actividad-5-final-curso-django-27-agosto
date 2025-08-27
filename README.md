# Actividad-5-final-curso-django-27-agosto
proyecto final
¡Perfecto! Tu guía original ya es muy completa, clara y bien estructurada. Ahora, como solicitaste, **vamos a mejorar esta propuesta sin modificar** los archivos `models.py`, `admin.py`, `forms.py` ni `views.py`, **manteniendo su lógica intacta**, pero **optimizando la estructura del proyecto desde el inicio**, **mejorando la organización de carpetas y archivos**, y **reforzando el diseño y experiencia visual sin usar Bootstrap ni `crispy_forms`**, tal como indicaste.

El objetivo es **hacer el proyecto más profesional, atractivo y escalable**, manteniendo la simplicidad para estudiantes principiantes, pero con una estructura que prepare para proyectos reales.

---

## ✅ **Mejora Propuesta: Estructura Profesional + Diseño Atractivo + Claridad Pedagógica**

---

### 📘 **Práctica 4: Django con Python – Aplicación Web Musical Profesional (Versión Mejorada)**

> **¡Bienvenidos!** En esta práctica construiremos una aplicación web profesional para gestionar artistas y sus canciones. Aprenderemos a organizar bien el proyecto desde el inicio, manejar relaciones entre modelos, y crear una interfaz visualmente atractiva con HTML y CSS puro.  
>  
> **Este proyecto no usa frameworks de diseño (como Bootstrap) ni librerías de formularios (como `crispy_forms`).** Todo es **HTML semántico y CSS moderno**, ideal para aprender buenas prácticas.

---

### 🎯 **Objetivos de Aprendizaje (Actualizados)**

- ✅ Crear un entorno virtual y estructurar un proyecto Django profesional.
- ✅ Organizar correctamente carpetas estáticas, de plantillas y de medios.
- ✅ Trabajar con relaciones `ForeignKey` (uno a muchos) entre modelos.
- ✅ Implementar CRUD completo para `Artista` y mostrar sus `Canciones`.
- ✅ Usar herencia de plantillas (`{% extends %}`), bloques y variables.
- ✅ Gestionar subida de imágenes con `Pillow` y `MEDIA_ROOT`.
- ✅ Aplicar CSS moderno para un diseño limpio, responsive y atractivo.
- ✅ Aprender buenas prácticas de nomenclatura y estructura.

---

## 🧱 **Paso 0: Estructura de Carpetas y Archivos (Desde el Inicio)**

Vamos a definir la estructura completa **antes de empezar**, para que todo esté organizado desde el principio.

```
Curso_Django/
└── Artista/
    ├── .venv/                     # Entorno virtual (creado con venv)
    ├── backend_artista/           # Carpeta del proyecto Django
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
    │   ├── templates/             # Plantillas específicas de la app
    │   │   ├── base.html
    │   │   ├── listar_artistas.html
    │   │   ├── detalle_artista.html
    │   │   ├── formulario_artista.html
    │   │   └── confirmar_borrar.html
    │   └── static/                # Estilos y recursos estáticos
    │       └── css/
    │           └── styles.css
    ├── media/                     # Imágenes subidas por usuarios (se crea al correr el proyecto)
    ├── manage.py
    └── requirements.txt           # Lista de dependencias
```

> ✅ **Ventaja:** Esta estructura es limpia, escalable y sigue las mejores prácticas de Django.

---

## ⚙️ **Paso 1: Preparación del Entorno (Mejorado)**

### ✅ **1.1. Crear la estructura desde la terminal**

```bash
# 1. Crear carpetas principales
mkdir Curso_Django
cd Curso_Django
mkdir Artista
cd Artista

# 2. Abrir en VS Code
code .

# 3. Crear entorno virtual y activarlo
python -m venv .venv
.\.venv\Scripts\activate

# 4. Instalar dependencias
pip install django pillow

# 5. Guardar dependencias en requirements.txt
pip freeze > requirements.txt

# 6. Crear proyecto y app
django-admin startproject backend_artista .
python manage.py startapp app_artista
```

> ✅ **Tip:** `requirements.txt` permite a otros replicar el entorno fácilmente con `pip install -r requirements.txt`.

---

## 📁 **Paso 2: Organización de Carpetas (Antes de Configurar)**

### ✅ **2.1. Crear carpetas necesarias**

```bash
# Desde la terminal, en la raíz del proyecto (donde está manage.py)
mkdir app_artista/templates
mkdir app_artista/static/css
mkdir media
```

> ✅ Esto asegura que las rutas de `STATICFILES_DIRS` y `MEDIA_ROOT` funcionen desde el inicio.

---

## ⚙️ **Paso 3: Configuración del Proyecto (Mejorada y Clara)**

### ✅ `backend_artista/settings.py`

```python
import os
from pathlib import Path

BASE_DIR = Path(__file__).resolve().parent.parent

SECRET_KEY = 'tu-clave-secreta-aqui'  # Cambiar en producción
DEBUG = True
ALLOWED_HOSTS = []

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_artista',  # Nuestra app
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
        'DIRS': [BASE_DIR / 'app_artista/templates'],  # Ruta explícita a plantillas
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

# ✅ Archivos estáticos (CSS, JS)
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / 'app_artista/static']  # Donde se guardan los archivos durante desarrollo
STATIC_ROOT = BASE_DIR / 'staticfiles'  # Usado en producción (con collectstatic)

# ✅ Archivos media (imágenes subidas)
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'  # Carpeta donde se guardan las imágenes subidas

DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'
```

> ✅ **Mejoras:**
> - `TEMPLATES['DIRS']` apunta explícitamente a `app_artista/templates`.
> - Se usan rutas con `Path` para mayor claridad.
> - Se define `STATIC_ROOT` para preparar producción.

---

## 🔗 **Paso 4: URLs del Proyecto (Claras y Escalables)**

### ✅ `backend_artista/urls.py`

```python
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_artista.urls')),  # Todas las rutas de la app
]

# Solo en desarrollo: servir archivos media
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

---

## 📂 **Paso 5: URLs de la Aplicación (Ordenadas y con nombres descriptivos)**

### ✅ `app_artista/urls.py`

```python
from django.urls import path
from . import views

app_name = 'app_artista'  # Nombre de espacio para evitar conflictos

urlpatterns = [
    path('', views.listar_artistas, name='listar_artistas'),
    path('artista/<int:artista_id>/', views.detalle_artista, name='detalle_artista'),
    path('crear/', views.crear_artista, name='crear_artista'),
    path('editar/<int:artista_id>/', views.editar_artista, name='editar_artista'),
    path('borrar/<int:artista_id>/', views.borrar_artista, name='borrar_artista'),
]
```

> ✅ **Ventaja:** Uso de `app_name` evita conflictos si hay más apps.

---

## 🎨 **Paso 6: Plantillas (Mejoradas con HTML Semántico y CSS Moderno)**

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
    <link rel="icon" href="{% static 'img/favicon.ico' %}" type="image/x-icon">
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
            {% block contenido %}
            {% endblock %}
        </main>

        <footer class="main-footer">
            <p>© 2025 Melodías - Aplicación educativa de Django</p>
        </footer>
    </div>
</body>
</html>
```

> ✅ **Mejoras:**
> - Uso de `app_name` en URLs: `{% url 'app_artista:listar_artistas' %}`.
> - Etiquetas semánticas: `<header>`, `<main>`, `<footer>`.
> - Favicon opcional (puedes agregar un `favicon.ico` en `static/img/`).

---

### ✅ `listar_artistas.html`

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

---

### ✅ `detalle_artista.html`

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

---

## 🎨 **Paso 7: Estilos CSS (Moderno, Responsive y Atractivo)**

### ✅ `app_artista/static/css/styles.css`

```css
/* Reset básico */
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

/* Header */
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

/* Main */
.main-content {
    flex: 1;
    padding: 2rem;
    max-width: 1200px;
    margin: 0 auto;
    width: 100%;
}

/* Página principal */
.page-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 2rem;
    flex-wrap: wrap;
    gap: 1rem;
}

/* Grid de artistas */
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

/* Detalle de artista */
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

/* Botones */
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

/* Footer */
.main-footer {
    text-align: center;
    padding: 1.5rem;
    background: white;
    color: #718096;
    font-size: 0.9rem;
    margin-top: auto;
}

/* Responsive */
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

> ✅ **Diseño moderno:** gradientes, sombras, hover, grid, responsive.
> ✅ **Sin Bootstrap:** todo con CSS puro.
> ✅ **Accesible:** etiquetas semánticas, contraste de colores.

---

## ✅ **Paso Final: Ejecutar y Probar**

```bash
python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

Ve a `http://127.0.0.1:8000/` y disfruta de tu aplicación profesional.

---

## 🎉 **¡Proyecto Mejorado y Listo para el Mundo Real!**

### ✅ **Lo que hemos mejorado:**

| Elemento | Mejora |
|--------|--------|
| **Estructura** | Definida desde el inicio, clara y profesional |
| **Configuración** | Más clara, con `Path`, `STATIC_ROOT`, `TEMPLATES['DIRS']` |
| **Plantillas** | Con `app_name`, más semánticas y con bloques útiles |
| **Diseño** | Moderno, responsive, con CSS puro y sin librerías externas |
| **Organización** | Carpetas creadas antes, mejor flujo de trabajo |
| **Pedagogía** | Guía paso a paso, ideal para estudiantes |

---

## 📌 **Consejo Final para Estudiantes**

> "Este proyecto es una base sólida. Puedes extenderlo fácilmente: añade géneros, álbumes, o incluso un reproductor básico con audio. ¡La clave es entender cada paso!"

---

✅ **¡Listo!** Has creado una aplicación Django profesional, visualmente atractiva, bien estructurada y sin librerías externas. Ideal para aprender y mostrar en tu portafolio.
