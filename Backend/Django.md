# Django Cheat Sheet - Beginner to Pro

## Table of Contents
- [Setup & Installation](#setup--installation)
- [Project Structure](#project-structure)
- [Settings](#settings)
- [URLs & Routing](#urls--routing)
- [Views](#views)
- [Models](#models)
- [QuerySets & ORM](#querysets--orm)
- [Forms](#forms)
- [Templates](#templates)
- [Admin Interface](#admin-interface)
- [Authentication](#authentication)
- [Middleware](#middleware)
- [Signals](#signals)
- [Static Files](#static-files)
- [Media Files](#media-files)
- [Django REST Framework](#django-rest-framework)
- [Testing](#testing)
- [Deployment](#deployment)
- [Best Practices](#best-practices)

---

## Setup & Installation

### Installation
```bash
# Install Django
pip install django

# Check version
python -m django --version

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install Django in virtual environment
pip install django

# Install with specific version
pip install django==4.2.0
```

### Create Project
```bash
# Create new project
django-admin startproject myproject

# Create project in current directory
django-admin startproject myproject .

# Create app
python manage.py startapp myapp

# Run development server
python manage.py runserver

# Run on specific port
python manage.py runserver 8080

# Run on specific host and port
python manage.py runserver 0.0.0.0:8000
```

### Essential Commands
```bash
# Database migrations
python manage.py makemigrations
python manage.py migrate

# Create superuser
python manage.py createsuperuser

# Collect static files
python manage.py collectstatic

# Run shell
python manage.py shell

# Run tests
python manage.py test

# Check for issues
python manage.py check
```

---

## Project Structure

### Basic Structure
```
myproject/
├── myproject/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
├── myapp/
│   ├── migrations/
│   │   └── __init__.py
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
├── templates/
├── static/
├── media/
├── manage.py
└── requirements.txt
```

---

## Settings

### Basic Settings
```python
# myproject/settings.py

# Debug mode (never use True in production)
DEBUG = True

# Allowed hosts
ALLOWED_HOSTS = ['localhost', '127.0.0.1']

# Installed apps
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'myapp',  # Your app
]

# Middleware
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

# Root URL configuration
ROOT_URLCONF = 'myproject.urls'

# Templates
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'],
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

# Database
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

# Language and timezone
LANGUAGE_CODE = 'en-us'
TIME_ZONE = 'UTC'
USE_I18N = True
USE_TZ = True

# Static files
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / 'static']
STATIC_ROOT = BASE_DIR / 'staticfiles'

# Media files
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'
```

### Database Configurations
```python
# PostgreSQL
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'mydatabase',
        'USER': 'myuser',
        'PASSWORD': 'mypassword',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}

# MySQL
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'mydatabase',
        'USER': 'myuser',
        'PASSWORD': 'mypassword',
        'HOST': 'localhost',
        'PORT': '3306',
    }
}

# Environment variables
import os
from pathlib import Path

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.environ.get('DB_NAME'),
        'USER': os.environ.get('DB_USER'),
        'PASSWORD': os.environ.get('DB_PASSWORD'),
        'HOST': os.environ.get('DB_HOST'),
        'PORT': os.environ.get('DB_PORT'),
    }
}
```

---

## URLs & Routing

### Project URLs
```python
# myproject/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('myapp.urls')),
    path('api/', include('api.urls')),
]
```

### App URLs
```python
# myapp/urls.py
from django.urls import path
from . import views

app_name = 'myapp'

urlpatterns = [
    # Basic path
    path('', views.index, name='index'),
    path('about/', views.about, name='about'),
    
    # Path with parameter
    path('post/<int:pk>/', views.post_detail, name='post_detail'),
    path('user/<str:username>/', views.user_profile, name='user_profile'),
    
    # Path with slug
    path('article/<slug:slug>/', views.article_detail, name='article_detail'),
    
    # Multiple parameters
    path('post/<int:year>/<int:month>/<slug:slug>/', views.post_archive, name='post_archive'),
    
    # Regex path
    from django.urls import re_path
    re_path(r'^articles/(?P<year>[0-9]{4})/$', views.year_archive),
]
```

### URL Patterns
```python
# Path converters
# <int:pk>     - Matches positive integers
# <str:name>   - Matches any non-empty string, excluding '/'
# <slug:slug>  - Matches slug strings (letters, numbers, hyphens, underscores)
# <uuid:id>    - Matches UUID
# <path:path>  - Matches any non-empty string, including '/'

# URL reversing
from django.urls import reverse
from django.shortcuts import redirect

# In views
def my_view(request):
    url = reverse('myapp:post_detail', kwargs={'pk': 1})
    return redirect('myapp:index')

# In templates
# {% url 'myapp:post_detail' pk=post.id %}
```

---

## Views

### Function-Based Views (FBV)
```python
# myapp/views.py
from django.shortcuts import render, redirect, get_object_or_404
from django.http import HttpResponse, JsonResponse
from .models import Post

# Basic view
def index(request):
    return HttpResponse("Hello, World!")

# Render template
def home(request):
    context = {'message': 'Welcome'}
    return render(request, 'home.html', context)

# Query database
def post_list(request):
    posts = Post.objects.all()
    return render(request, 'posts/list.html', {'posts': posts})

# Detail view
def post_detail(request, pk):
    post = get_object_or_404(Post, pk=pk)
    return render(request, 'posts/detail.html', {'post': post})

# Handle POST request
def create_post(request):
    if request.method == 'POST':
        title = request.POST.get('title')
        content = request.POST.get('content')
        Post.objects.create(title=title, content=content)
        return redirect('post_list')
    return render(request, 'posts/create.html')

# JSON response
def api_posts(request):
    posts = Post.objects.all().values('id', 'title', 'content')
    return JsonResponse(list(posts), safe=False)
```

### Class-Based Views (CBV)
```python
from django.views import View
from django.views.generic import (
    ListView, DetailView, CreateView, UpdateView, DeleteView
)
from django.urls import reverse_lazy

# Generic View
class PostListView(ListView):
    model = Post
    template_name = 'posts/list.html'
    context_object_name = 'posts'
    paginate_by = 10
    
    def get_queryset(self):
        return Post.objects.filter(published=True)
    
    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context['title'] = 'All Posts'
        return context

# Detail View
class PostDetailView(DetailView):
    model = Post
    template_name = 'posts/detail.html'
    context_object_name = 'post'

# Create View
class PostCreateView(CreateView):
    model = Post
    fields = ['title', 'content']
    template_name = 'posts/create.html'
    success_url = reverse_lazy('post_list')
    
    def form_valid(self, form):
        form.instance.author = self.request.user
        return super().form_valid(form)

# Update View
class PostUpdateView(UpdateView):
    model = Post
    fields = ['title', 'content']
    template_name = 'posts/update.html'
    success_url = reverse_lazy('post_list')

# Delete View
class PostDeleteView(DeleteView):
    model = Post
    template_name = 'posts/delete.html'
    success_url = reverse_lazy('post_list')

# Custom View
class MyCustomView(View):
    def get(self, request):
        return render(request, 'template.html')
    
    def post(self, request):
        # Handle POST
        return redirect('success')

# URLs for CBVs
# path('posts/', PostListView.as_view(), name='post_list'),
```

### View Decorators
```python
from django.contrib.auth.decorators import login_required, permission_required
from django.views.decorators.http import require_http_methods, require_POST
from django.views.decorators.cache import cache_page

# Login required
@login_required
def profile(request):
    return render(request, 'profile.html')

# Permission required
@permission_required('myapp.can_publish')
def publish_post(request):
    return render(request, 'publish.html')

# HTTP methods
@require_http_methods(["GET", "POST"])
def my_view(request):
    pass

@require_POST
def submit_form(request):
    pass

# Cache
@cache_page(60 * 15)  # Cache for 15 minutes
def my_view(request):
    return render(request, 'template.html')

# Multiple decorators
@login_required
@permission_required('myapp.can_edit')
def edit_post(request, pk):
    post = get_object_or_404(Post, pk=pk)
    return render(request, 'edit.html', {'post': post})
```

---

## Models

### Basic Model
```python
# myapp/models.py
from django.db import models
from django.contrib.auth.models import User

class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    author = models.ForeignKey(User, on_delete=models.CASCADE)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    published = models.BooleanField(default=False)
    
    class Meta:
        ordering = ['-created_at']
        verbose_name = 'Post'
        verbose_name_plural = 'Posts'
    
    def __str__(self):
        return self.title
    
    def get_absolute_url(self):
        from django.urls import reverse
        return reverse('post_detail', kwargs={'pk': self.pk})
```

### Field Types
```python
from django.db import models

class Example(models.Model):
    # Text fields
    char_field = models.CharField(max_length=100)
    text_field = models.TextField()
    email_field = models.EmailField()
    url_field = models.URLField()
    slug_field = models.SlugField()
    
    # Numeric fields
    integer_field = models.IntegerField()
    positive_int = models.PositiveIntegerField()
    decimal_field = models.DecimalField(max_digits=10, decimal_places=2)
    float_field = models.FloatField()
    
    # Boolean
    boolean_field = models.BooleanField(default=False)
    
    # Date and time
    date_field = models.DateField()
    time_field = models.TimeField()
    datetime_field = models.DateTimeField()
    auto_now_add = models.DateTimeField(auto_now_add=True)
    auto_now = models.DateTimeField(auto_now=True)
    
    # File fields
    file_field = models.FileField(upload_to='files/')
    image_field = models.ImageField(upload_to='images/')
    
    # Choice field
    STATUS_CHOICES = [
        ('draft', 'Draft'),
        ('published', 'Published'),
        ('archived', 'Archived'),
    ]
    status = models.CharField(max_length=20, choices=STATUS_CHOICES, default='draft')
    
    # JSON field (Django 3.1+)
    json_field = models.JSONField(default=dict)
    
    # UUID field
    import uuid
    id = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
```

### Field Options
```python
class Post(models.Model):
    title = models.CharField(
        max_length=200,
        unique=True,           # Must be unique
        blank=False,           # Required in forms
        null=False,            # Cannot be NULL in database
        default='Untitled',    # Default value
        help_text='Post title',
        verbose_name='Title',
        db_index=True,         # Create database index
        editable=True,         # Show in forms
    )
    
    # Optional field (can be blank and null)
    subtitle = models.CharField(max_length=200, blank=True, null=True)
```

### Relationships
```python
# One-to-Many (ForeignKey)
class Post(models.Model):
    author = models.ForeignKey(
        User,
        on_delete=models.CASCADE,  # Delete posts when user is deleted
        related_name='posts'        # Access via user.posts.all()
    )

# on_delete options:
# CASCADE - Delete related objects
# PROTECT - Prevent deletion
# SET_NULL - Set to NULL (requires null=True)
# SET_DEFAULT - Set to default value
# DO_NOTHING - Do nothing (can cause database errors)

# One-to-One
class Profile(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    bio = models.TextField()

# Many-to-Many
class Post(models.Model):
    tags = models.ManyToManyField('Tag', related_name='posts')

class Tag(models.Model):
    name = models.CharField(max_length=50)

# Many-to-Many with through model
class Post(models.Model):
    tags = models.ManyToManyField('Tag', through='PostTag')

class Tag(models.Model):
    name = models.CharField(max_length=50)

class PostTag(models.Model):
    post = models.ForeignKey(Post, on_delete=models.CASCADE)
    tag = models.ForeignKey(Tag, on_delete=models.CASCADE)
    created_at = models.DateTimeField(auto_now_add=True)
```

### Model Methods
```python
class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    
    # Instance method
    def is_recent(self):
        from django.utils import timezone
        from datetime import timedelta
        return self.created_at >= timezone.now() - timedelta(days=7)
    
    # Property
    @property
    def word_count(self):
        return len(self.content.split())
    
    # Class method
    @classmethod
    def published_posts(cls):
        return cls.objects.filter(published=True)
    
    # Save override
    def save(self, *args, **kwargs):
        # Custom logic before save
        self.slug = slugify(self.title)
        super().save(*args, **kwargs)
    
    # Delete override
    def delete(self, *args, **kwargs):
        # Custom logic before delete
        super().delete(*args, **kwargs)
```

### Model Meta Options
```python
class Post(models.Model):
    # Fields...
    
    class Meta:
        ordering = ['-created_at']                    # Default ordering
        verbose_name = 'Post'                         # Singular name
        verbose_name_plural = 'Posts'                 # Plural name
        db_table = 'blog_posts'                       # Custom table name
        unique_together = [['author', 'title']]       # Unique combination
        indexes = [                                    # Database indexes
            models.Index(fields=['created_at']),
            models.Index(fields=['author', 'created_at']),
        ]
        permissions = [                                # Custom permissions
            ('can_publish', 'Can publish posts'),
        ]
        abstract = False                               # Abstract model
        proxy = False                                  # Proxy model
```

---

## QuerySets & ORM

### Basic Queries
```python
from myapp.models import Post

# Get all objects
posts = Post.objects.all()

# Get single object
post = Post.objects.get(pk=1)
post = Post.objects.get(title='My Post')

# Get or 404
from django.shortcuts import get_object_or_404
post = get_object_or_404(Post, pk=1)

# Filter
posts = Post.objects.filter(published=True)
posts = Post.objects.filter(author__username='john')

# Exclude
posts = Post.objects.exclude(published=False)

# Get first/last
first = Post.objects.first()
last = Post.objects.last()

# Order by
posts = Post.objects.order_by('-created_at')
posts = Post.objects.order_by('author', '-created_at')

# Limit results
posts = Post.objects.all()[:5]  # First 5
posts = Post.objects.all()[5:10]  # Offset 5, limit 5

# Count
count = Post.objects.count()
count = Post.objects.filter(published=True).count()

# Exists
exists = Post.objects.filter(title='Test').exists()
```

### Advanced Queries
```python
from django.db.models import Q, F

# OR queries
posts = Post.objects.filter(Q(published=True) | Q(author=request.user))

# AND queries
posts = Post.objects.filter(Q(published=True) & Q(created_at__year=2024))

# NOT queries
posts = Post.objects.filter(~Q(published=True))

# F expressions (field comparisons)
posts = Post.objects.filter(likes__gt=F('views'))

# Annotations
from django.db.models import Count, Avg, Sum
posts = Post.objects.annotate(comment_count=Count('comments'))
posts = Post.objects.annotate(avg_rating=Avg('ratings__score'))

# Aggregations
from django.db.models import Count, Avg, Max, Min, Sum
total_posts = Post.objects.aggregate(total=Count('id'))
avg_likes = Post.objects.aggregate(avg=Avg('likes'))

# Select related (ForeignKey, OneToOne)
posts = Post.objects.select_related('author')
for post in posts:
    print(post.author.username)  # No additional query

# Prefetch related (ManyToMany, reverse ForeignKey)
posts = Post.objects.prefetch_related('tags')
for post in posts:
    for tag in post.tags.all():  # No additional queries
        print(tag.name)

# Only/Defer
posts = Post.objects.only('title', 'created_at')  # Load only these fields
posts = Post.objects.defer('content')  # Load all except this field

# Values/Values list
posts = Post.objects.values('title', 'author__username')
posts = Post.objects.values_list('title', flat=True)  # List of titles

# Distinct
posts = Post.objects.filter(published=True).distinct()
```

### Lookups
```python
# Exact match
Post.objects.filter(title='My Post')
Post.objects.filter(title__exact='My Post')

# Case-insensitive
Post.objects.filter(title__iexact='my post')

# Contains
Post.objects.filter(title__contains='Django')
Post.objects.filter(title__icontains='django')  # Case-insensitive

# Starts/Ends with
Post.objects.filter(title__startswith='How to')
Post.objects.filter(title__endswith='Django')

# Greater than / Less than
Post.objects.filter(likes__gt=100)
Post.objects.filter(likes__gte=100)  # Greater than or equal
Post.objects.filter(likes__lt=100)
Post.objects.filter(likes__lte=100)  # Less than or equal

# In list
Post.objects.filter(id__in=[1, 2, 3])

# Date queries
from datetime import date
Post.objects.filter(created_at__date=date.today())
Post.objects.filter(created_at__year=2024)
Post.objects.filter(created_at__month=1)
Post.objects.filter(created_at__day=15)

# Range
from datetime import datetime
Post.objects.filter(created_at__range=(start_date, end_date))

# Is null
Post.objects.filter(published_at__isnull=True)

# Regex
Post.objects.filter(title__regex=r'^(An?|The) +')
Post.objects.filter(title__iregex=r'^(an?|the) +')  # Case-insensitive
```

### Create, Update, Delete
```python
# Create
post = Post.objects.create(
    title='My Post',
    content='Content here',
    author=request.user
)

# Or
post = Post(title='My Post', content='Content')
post.save()

# Get or create
post, created = Post.objects.get_or_create(
    title='My Post',
    defaults={'content': 'Default content'}
)

# Update or create
post, created = Post.objects.update_or_create(
    title='My Post',
    defaults={'content': 'Updated content'}
)

# Update single object
post = Post.objects.get(pk=1)
post.title = 'New Title'
post.save()

# Bulk update
Post.objects.filter(published=False).update(published=True)

# Delete single object
post = Post.objects.get(pk=1)
post.delete()

# Bulk delete
Post.objects.filter(published=False).delete()

# Delete all
Post.objects.all().delete()
```

### Transactions
```python
from django.db import transaction

# Atomic decorator
@transaction.atomic
def create_post_with_tags(data):
    post = Post.objects.create(**data)
    for tag_name in data['tags']:
        tag, _ = Tag.objects.get_or_create(name=tag_name)
        post.tags.add(tag)
    return post

# Context manager
def my_view(request):
    with transaction.atomic():
        post = Post.objects.create(title='Test')
        post.tags.add(tag)
```

---

## Forms

### Django Forms
```python
# myapp/forms.py
from django import forms
from .models import Post

# Basic form
class ContactForm(forms.Form):
    name = forms.CharField(max_length=100)
    email = forms.EmailField()
    message = forms.CharField(widget=forms.Textarea)
    
    def clean_email(self):
        email = self.cleaned_data['email']
        if not email.endswith('@example.com'):
            raise forms.ValidationError('Email must be from example.com')
        return email

# Model form
class PostForm(forms.ModelForm):
    class Meta:
        model = Post
        fields = ['title', 'content', 'published']
        # Or exclude fields
        # exclude = ['author', 'created_at']
        
        widgets = {
            'content': forms.Textarea(attrs={'rows': 10}),
        }
        
        labels = {
            'title': 'Post Title',
        }
        
        help_texts = {
            'title': 'Enter a descriptive title',
        }
    
    def clean_title(self):
        title = self.cleaned_data['title']
        if Post.objects.filter(title=title).exists():
            raise forms.ValidationError('Title already exists')
        return title

# Using forms in views
def create_post(request):
    if request.method == 'POST':
        form = PostForm(request.POST)
        if form.is_valid():
            post = form.save(commit=False)
            post.author = request.user
            post.save()
            return redirect('post_detail', pk=post.pk)
    else:
        form = PostForm()
    
    return render(request, 'posts/create.html', {'form': form})

# Update form
def update_post(request, pk):
    post = get_object_or_404(Post, pk=pk)
    
    if request.method == 'POST':
        form = PostForm(request.POST, instance=post)
        if form.is_valid():
            form.save()
            return redirect('post_detail', pk=post.pk)
    else:
        form = PostForm(instance=post)
    
    return render(request, 'posts/update.html', {'form': form})
```

### Form Fields
```python
from django import forms

class ExampleForm(forms.Form):
    # Text fields
    char_field = forms.CharField(max_length=100)
    text_field = forms.CharField(widget=forms.Textarea)
    email_field = forms.EmailField()
    url_field = forms.URLField()
    slug_field = forms.SlugField()
    
    # Numeric fields
    integer_field = forms.IntegerField()
    decimal_field = forms.DecimalField(max_digits=10, decimal_places=2)
    float_field = forms.FloatField()
    
    # Boolean
    boolean_field = forms.BooleanField(required=False)
    
    # Date and time
    date_field = forms.DateField()
    time_field = forms.TimeField()
    datetime_field = forms.DateTimeField()
    
    # Choice field
    CHOICES = [
        ('option1', 'Option 1'),
        ('option2', 'Option 2'),
    ]
    choice_field = forms.ChoiceField(choices=CHOICES)
    multiple_choice = forms.MultipleChoiceField(choices=CHOICES)
    
    # File fields
    file_field = forms.FileField()
    image_field = forms.ImageField()
    
    # Hidden field
    hidden_field = forms.CharField(widget=forms.HiddenInput())
```

### Form Validation
```python
class PostForm(forms.ModelForm):
    class Meta:
        model = Post
        fields = ['title', 'content']
    
    # Field-specific validation
    def clean_title(self):
        title = self.cleaned_data['title']
        if len(title) < 10:
            raise forms.ValidationError('Title must be at least 10 characters')
        return title
    
    # Form-wide validation
    def clean(self):
        cleaned_data = super().clean()
        title = cleaned_data.get('title')
        content = cleaned_data.get('content')
        
        if title and content and title in content:
            raise forms.ValidationError('Title should not appear in content')
        
        return cleaned_data
```

---

## Templates

### Template Syntax
```django
<!-- templates/base.html -->
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}My Site{% endblock %}</title>
</head>
<body>
    {% block content %}{% endblock %}
</body>
</html>

<!-- templates/home.html -->
{% extends 'base.html' %}

{% block title %}Home - {{ block.super }}{% endblock %}

{% block content %}
    <h1>{{ title }}</h1>
    
    <!-- Variables -->
    <p>{{ user.username }}</p>
    <p>{{ post.title|title }}</p>
    
    <!-- If statement -->
    {% if user.is_authenticated %}
        <p>Welcome, {{ user.username }}!</p>
    {% else %}
        <p>Please log in.</p>
    {% endif %}
    
    <!-- For loop -->
    {% for post in posts %}
        <div>
            <h2>{{ post.title }}</h2>
            <p>{{ post.content }}</p>
        </div>
    {% empty %}
        <p>No posts found.</p>
    {% endfor %}
    
    <!-- For loop helpers -->
    {% for post in posts %}
        {{ forloop.counter }} - {{ post.title }}
        {% if forloop.first %}First!{% endif %}
        {% if forloop.last %}Last!{% endif %}
    {% endfor %}
    
    <!-- URL reverse -->
    <a href="{% url 'post_detail' pk=post.pk %}">View Post</a>
    
    <!-- Include -->
    {% include 'partials/header.html' %}
    
    <!-- With -->
    {% with total=posts|length %}
        Total posts: {{ total }}
    {% endwith %}
    
    <!-- Comments -->
    {# This is a comment #}
    {% comment %}
        Multi-line
        comment
    {% endcomment %}
{% endblock %}
```

### Template Filters
```django
<!-- String filters -->
{{ value|lower }}
{{ value|upper }}
{{ value|title }}
{{ value|capfirst }}
{{ value|length }}
{{ value|truncatewords:30 }}
{{ value|truncatechars:50 }}
{{ value|slugify }}
{{ value|linebreaks }}
{{ value|safe }}  <!-- Mark as safe HTML -->
{{ value|escape }}

<!-- Number filters -->
{{ value|add:5 }}
{{ value|divisibleby:3 }}

<!-- Date filters -->
{{ value|date:"Y-m-d" }}
{{ value|time:"H:i" }}
{{ value|timesince }}
{{ value|timeuntil }}

<!-- List filters -->
{{ list|first }}
{{ list|last }}
{{ list|join:", " }}
{{ list|length }}

<!-- Default -->
{{ value|default:"Nothing" }}
{{ value|default_if_none:"N/A" }}

<!-- Chain filters -->
{{ post.title|lower|truncatewords:10 }}
```

### Template Tags
```django
<!-- Load custom tags -->
{% load static %}
{% load custom_tags %}

<!-- Static files -->
<link rel="stylesheet" href="{% static 'css/style.css' %}">
<img src="{% static 'images/logo.png' %}" alt="Logo">

<!-- CSRF token -->
<form method="post">
    {% csrf_token %}
    <!-- form fields -->
</form>

<!-- Now (current date/time) -->
{% now "Y-m-d H:i" %}

<!-- Spaceless -->
{% spaceless %}
    <p>
        <a href="#">Link</a>
    </p>
{% endspaceless %}

<!-- Autoescape -->
{% autoescape off %}
    {{ html_content }}
{% endautoescape %}
```

### Custom Template Tags
```python
# myapp/templatetags/custom_tags.py
from django import template

register = template.Library()

# Simple tag
@register.simple_tag
def multiply(a, b):
    return a * b

# Usage: {% multiply 5 10 %}

# Inclusion tag
@register.inclusion_tag('tags/post_card.html')
def show_post(post):
    return {'post': post}

# Usage: {% show_post post %}

# Filter
@register.filter
def add_exclamation(value):
    return f"{value}!"

# Usage: {{ post.title|add_exclamation }}
```

---

## Admin Interface

### Basic Admin
```python
# myapp/admin.py
from django.contrib import admin
from .models import Post

# Simple registration
admin.site.register(Post)

# With ModelAdmin
@admin.register(Post)
class PostAdmin(admin.ModelAdmin):
    list_display = ['title', 'author', 'created_at', 'published']
    list_filter = ['published', 'created_at']
    search_fields = ['title', 'content']
    prepopulated_fields = {'slug': ('title',)}
    date_hierarchy = 'created_at'
    ordering = ['-created_at']
    list_per_page = 20
    
    fieldsets = (
        ('Basic Information', {
            'fields': ('title', 'content', 'author')
        }),
        ('Settings', {
            'fields': ('published', 'slug'),
            'classes': ('collapse',)
        }),
    )
    
    # Custom actions
    actions = ['make_published']
    
    def make_published(self, request, queryset):
        queryset.update(published=True)
    make_published.short_description = 'Mark selected posts as published'
```

### Inline Admin
```python
from django.contrib import admin

class CommentInline(admin.TabularInline):
    model = Comment
    extra = 1

@admin.register(Post)
class PostAdmin(admin.ModelAdmin):
    inlines = [CommentInline]
    list_display = ['title', 'author']
```

---

## Authentication

### User Authentication
```python
from django.contrib.auth import authenticate, login, logout
from django.contrib.auth.decorators import login_required
from django.contrib.auth.models import User

# Login view
def login_view(request):
    if request.method == 'POST':
        username = request.POST['username']
        password = request.POST['password']
        user = authenticate(request, username=username, password=password)
        
        if user is not None:
            login(request, user)
            return redirect('home')
        else:
            messages.error(request, 'Invalid credentials')
    
    return render(request, 'login.html')

# Logout view
def logout_view(request):
    logout(request)
    return redirect('home')

# Register view
def register_view(request):
    if request.method == 'POST':
        username = request.POST['username']
        email = request.POST['email']
        password = request.POST['password']
        
        user = User.objects.create_user(username, email, password)
        login(request, user)
        return redirect('home')
    
    return render(request, 'register.html')

# Protected view
@login_required
def profile_view(request):
    return render(request, 'profile.html')
```

### Custom User Model
```python
# myapp/models.py
from django.contrib.auth.models import AbstractUser

class CustomUser(AbstractUser):
    bio = models.TextField(blank=True)
    birth_date = models.DateField(null=True, blank=True)

# settings.py
AUTH_USER_MODEL = 'myapp.CustomUser'
```

---

## Middleware

### Custom Middleware
```python
# myapp/middleware.py

class SimpleMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response
    
    def __call__(self, request):
        # Code executed for each request before view
        print(f"Request: {request.path}")
        
        response = self.get_response(request)
        
        # Code executed for each request/response after view
        print(f"Response: {response.status_code}")
        
        return response

# settings.py
MIDDLEWARE = [
    # ...
    'myapp.middleware.SimpleMiddleware',
]
```

---

## Signals

### Using Signals
```python
# myapp/signals.py
from django.db.models.signals import post_save, pre_save, post_delete
from django.dispatch import receiver
from django.contrib.auth.models import User
from .models import Profile

@receiver(post_save, sender=User)
def create_user_profile(sender, instance, created, **kwargs):
    if created:
        Profile.objects.create(user=instance)

@receiver(post_save, sender=User)
def save_user_profile(sender, instance, **kwargs):
    instance.profile.save()

# myapp/apps.py
class MyappConfig(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = 'myapp'
    
    def ready(self):
        import myapp.signals
```

---

## Static Files

### Configuration
```python
# settings.py
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / 'static']
STATIC_ROOT = BASE_DIR / 'staticfiles'

# In templates
{% load static %}
<link rel="stylesheet" href="{% static 'css/style.css' %}">
<script src="{% static 'js/script.js' %}"></script>
<img src="{% static 'images/logo.png' %}" alt="Logo">

# Collect static files
# python manage.py collectstatic
```

---

## Media Files

### Configuration
```python
# settings.py
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'

# urls.py
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    # your urls
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)

# In templates
<img src="{{ post.image.url }}" alt="{{ post.title }}">
```

---

## Django REST Framework

### Installation & Setup
```bash
pip install djangorestframework

# settings.py
INSTALLED_APPS = [
    # ...
    'rest_framework',
]

REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 10,
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.TokenAuthentication',
    ],
}
```

### Serializers
```python
# myapp/serializers.py
from rest_framework import serializers
from .models import Post

class PostSerializer(serializers.ModelSerializer):
    author_name = serializers.CharField(source='author.username', read_only=True)
    
    class Meta:
        model = Post
        fields = ['id', 'title', 'content', 'author', 'author_name', 'created_at']
        read_only_fields = ['id', 'created_at']
    
    def validate_title(self, value):
        if len(value) < 10:
            raise serializers.ValidationError('Title too short')
        return value
```

### API Views
```python
# myapp/views.py
from rest_framework import generics, viewsets
from rest_framework.response import Response
from rest_framework.decorators import api_view
from .models import Post
from .serializers import PostSerializer

# Function-based view
@api_view(['GET', 'POST'])
def post_list(request):
    if request.method == 'GET':
        posts = Post.objects.all()
        serializer = PostSerializer(posts, many=True)
        return Response(serializer.data)
    
    elif request.method == 'POST':
        serializer = PostSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save(author=request.user)
            return Response(serializer.data, status=201)
        return Response(serializer.errors, status=400)

# Generic views
class PostListCreateView(generics.ListCreateAPIView):
    queryset = Post.objects.all()
    serializer_class = PostSerializer
    
    def perform_create(self, serializer):
        serializer.save(author=self.request.user)

class PostDetailView(generics.RetrieveUpdateDestroyAPIView):
    queryset = Post.objects.all()
    serializer_class = PostSerializer

# ViewSets
class PostViewSet(viewsets.ModelViewSet):
    queryset = Post.objects.all()
    serializer_class = PostSerializer
    
    def get_queryset(self):
        queryset = super().get_queryset()
        if not self.request.user.is_staff:
            queryset = queryset.filter(published=True)
        return queryset

# URLs
from rest_framework.routers import DefaultRouter

router = DefaultRouter()
router.register(r'posts', PostViewSet)

urlpatterns = [
    path('api/', include(router.urls)),
]
```

---

## Testing

### Basic Tests
```python
# myapp/tests.py
from django.test import TestCase, Client
from django.contrib.auth.models import User
from .models import Post

class PostModelTest(TestCase):
    def setUp(self):
        self.user = User.objects.create_user('testuser', 'test@test.com', 'password')
        self.post = Post.objects.create(
            title='Test Post',
            content='Test content',
            author=self.user
        )
    
    def test_post_creation(self):
        self.assertEqual(self.post.title, 'Test Post')
        self.assertEqual(str(self.post), 'Test Post')
    
    def test_post_list_view(self):
        client = Client()
        response = client.get('/posts/')
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, 'Test Post')
    
    def test_post_detail_view(self):
        client = Client()
        response = client.get(f'/posts/{self.post.pk}/')
        self.assertEqual(response.status_code, 200)

# Run tests
# python manage.py test
```

---

## Deployment

### Production Settings
```python
# settings.py
DEBUG = False
ALLOWED_HOSTS = ['yourdomain.com', 'www.yourdomain.com']

# Security settings
SECURE_SSL_REDIRECT = True
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True
SECURE_BROWSER_XSS_FILTER = True
SECURE_CONTENT_TYPE_NOSNIFF = True
X_FRAME_OPTIONS = 'DENY'

# Static files
STATIC_ROOT = BASE_DIR / 'staticfiles'

# Database (use environment variables)
import os
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.environ.get('DB_NAME'),
        'USER': os.environ.get('DB_USER'),
        'PASSWORD': os.environ.get('DB_PASSWORD'),
        'HOST': os.environ.get('DB_HOST'),
        'PORT': os.environ.get('DB_PORT'),
    }
}
```

---

## Best Practices

### Project Organization
```python
# ✅ Good: Separate settings
myproject/
├── settings/
│   ├── __init__.py
│   ├── base.py
│   ├── development.py
│   └── production.py

# development.py
from .base import *
DEBUG = True

# production.py
from .base import *
DEBUG = False

# ✅ Good: Use environment variables
import os
SECRET_KEY = os.environ.get('SECRET_KEY')
DEBUG = os.environ.get('DEBUG', 'False') == 'True'
```

### Query Optimization
```python
# ✅ Good: Use select_related
posts = Post.objects.select_related('author').all()

# ✅ Good: Use prefetch_related
posts = Post.objects.prefetch_related('tags').all()

# ❌ Bad: N+1 queries
posts = Post.objects.all()
for post in posts:
    print(post.author.username)  # Extra query for each post
```

### Security
```python
# ✅ Good: Validate user input
from django.core.validators import validate_email

def register(request):
    email = request.POST.get('email')
    try:
        validate_email(email)
    except ValidationError:
        messages.error(request, 'Invalid email')

# ✅ Good: Use CSRF protection
# Always include {% csrf_token %} in forms

# ✅ Good: Hash passwords
from django.contrib.auth.hashers import make_password
password = make_password('plaintext_password')
```

---

**Pro Tip:** Master the Django ORM for efficient database queries, use class-based views for reusable code, leverage Django's built-in authentication and admin, and always use migrations for database schema changes!