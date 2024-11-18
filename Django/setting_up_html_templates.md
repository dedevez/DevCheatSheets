# Setting Up HTML Templates in a Django Project

Django uses its **template engine** to render HTML pages dynamically. This guide provides a step-by-step process for setting up HTML templates, including creating a `base.html`, extending it in other templates, and setting up reusable components like headers and footers.

---

## Step 1: Set Up the `templates/` Directory

For projects where templates are shared across multiple apps, it’s recommended to place the `templates/` directory in the project’s root directory. This ensures that all apps can access the shared templates easily.

### Create the Directory

1. In your project’s root directory (where `manage.py` is located), create a folder named `templates`.
2. Inside `templates/`, organize templates by app name to avoid conflicts:

```
templates/
    base.html
    blog/
        home.html
```

### Update `settings.py`

Tell Django where to find the templates by modifying the `TEMPLATES` setting in `settings.py`:

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'],  # Add this line
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
```

> **Note:** `APP_DIRS: True` enables Django to look for app-specific templates in their `templates/` directories.

---

### When to Use the Project Root `templates/` Directory

- Use this method if you have shared templates that need to be accessed by multiple apps (e.g., `base.html`, a shared `header.html`).
- Ideal for larger projects with multiple apps.

### When to Use App-Specific `templates/` Directories

- Place the `templates/` folder in an app directory if the templates are specific to that app.
- This keeps the app self-contained and modular.
- Example: If you have an app named `blog` and all templates are used exclusively by `blog`, place the `templates/` directory in the `blog/` directory.

---

## Step 2: Create a `base.html` Template

The `base.html` template serves as the foundation for other templates. Define common layout elements like the `<head>`, navigation bar, and footer here.

### Example `base.html`:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>{% block title %}My Website{% endblock %}</title>
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" />
  </head>
  <body>
    <header>
      <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <div class="container-fluid">
          <a class="navbar-brand" href="#">My Website</a>
        </div>
      </nav>
    </header>

    <main class="container my-5">
      {% block content %}
      <!-- Content goes here -->
      {% endblock %}
    </main>

    <footer class="text-center py-4">
      <p>&copy; 2024 My Website. All rights reserved.</p>
    </footer>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js"></script>
  </body>
</html>
```

### Explanation:

- **`{% block title %}`**: Allows child templates to override the `<title>` tag.
- **`{% block content %}`**: Placeholder for child templates to insert their content.
- Includes Bootstrap for responsive design.
- Defines a header and footer that will appear on all pages.

---

## Step 3: Extend `base.html` in Other Templates

To create a new template that uses the `base.html` structure, extend it using the `{% extends %}` tag.

### Example `home.html`:

```html
{% extends 'base.html' %} {% block title %}Home Page{% endblock %} {% block
content %}
<h1>Welcome to My Website</h1>
<p>This is the home page of the website.</p>
{% endblock %}
```

### Explanation:

- **`{% extends 'base.html' %}`**: Inherits the structure and styles from `base.html`.
- **`{% block title %}`**: Customizes the `<title>` tag for this page.
- **`{% block content %}`**: Adds specific content for the home page.

---

## Step 4: Reusable Components (Header, Footer, etc.)

If you want to reuse components like headers or footers across multiple templates, split them into separate files and include them using `{% include %}`.

### Example: Create `header.html`

```html
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">My Website</a>
  </div>
</nav>
```

### Example: Create `footer.html`

```html
<footer class="text-center py-4">
  <p>&copy; 2024 My Website. All rights reserved.</p>
</footer>
```

### Include Components in `base.html`

Replace the header and footer sections in `base.html` with:

```html
{% include 'header.html' %} {% block content %}{% endblock %} {% include
'footer.html' %}
```

---

## Step 5: Test the Templates

1. Update your view to render the `home.html` template.

### Example `views.py`:

```python
from django.shortcuts import render

def home(request):
    return render(request, 'home.html')
```

2. Update your app's `urls.py` file:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home'),
]
```

3. Run the development server:

```bash
python manage.py runserver
```

4. Visit `http://127.0.0.1:8000/` in your browser to see the rendered `home.html` template.

---

## Summary

1. Create a `templates/` directory in the project’s root for shared templates or within an app directory for app-specific templates.
2. Define a `base.html` template for the common structure of your site.
3. Use `{% block %}` tags to insert dynamic content in child templates.
4. Split reusable components (e.g., headers, footers) into separate files and include them with `{% include %}`.
5. Test your templates by connecting them to views and URLs.

This approach ensures flexibility and reusability across your Django project.
