# Setting Up a Django Project

Django is a high-level Python web framework that encourages rapid development and clean, pragmatic design. This guide provides a step-by-step approach to setting up a Django project, from installation to running the development server.

---

## Why Use Django?

- **Batteries-Included Framework**: Comes with built-in features for database handling, authentication, URL routing, and more.
- **Scalability**: Suitable for both small projects and large, enterprise-level applications.
- **Community and Documentation**: Django has extensive documentation and a large community for support.

---

## Step 1: Install Django

Before starting, ensure you have Python installed on your system. Check your Python version:

```bash
python --version
```

If Python is not installed, download it from [python.org](https://www.python.org/downloads/).

### Install Django via pip

Create a virtual environment (recommended) and install Django:

```bash
pip install django
```

Check the Django version to confirm installation:

```bash
django-admin --version
```

---

## Step 2: Start a New Django Project

Navigate to your desired directory and run the following command to create a new Django project:

```bash
django-admin startproject <project_name>
```

Replace `<project_name>` with the name of your project (e.g., `myproject`).

### Directory Structure

The `startproject` command creates the following structure:

```
myproject/
    manage.py
    myproject/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```

- `manage.py`: Command-line utility for interacting with your project.
- `settings.py`: Configuration for your project.
- `urls.py`: URL declarations for your project.
- `asgi.py` and `wsgi.py`: Entry points for ASGI and WSGI web servers.

---

## Step 3: Run the Development Server

Navigate into the project directory and start the server:

```bash
cd <project_name>
python manage.py runserver
```

The output will display a URL (e.g., `http://127.0.0.1:8000/`). Open this in your browser to see the default Django welcome page.

### Tips:

- Use `python manage.py runserver 0.0.0.0:8000` to make the server accessible on your local network.

---

## Step 4: Create a Django App

Django projects are organized into smaller units called **apps**. Each app handles a specific function (e.g., user authentication, blog posts).

Run the following command to create an app:

```bash
python manage.py startapp <app_name>
```

Replace `<app_name>` with the name of your app (e.g., `blog`). This creates a new directory structure for the app:

```
blog/
    migrations/
        __init__.py
    __init__.py
    admin.py
    apps.py
    models.py
    tests.py
    views.py
```

- `models.py`: Define your database models here.
- `views.py`: Define your application logic here.

---

## Step 5: Add the App to `INSTALLED_APPS`

Open the `settings.py` file and add your app to the `INSTALLED_APPS` list:

```python
INSTALLED_APPS = [
    ...,
    'blog',
]
```

This step ensures that Django recognizes the app.

---

## Step 6: Create Database Migrations

Django uses an ORM (Object-Relational Mapper) to manage database interactions. To apply changes to the database, you must create and run migrations.

### Create Migrations:

```bash
python manage.py makemigrations
```

### Apply Migrations:

```bash
python manage.py migrate
```

This sets up the database schema based on your app's `models.py`.

---

## Step 7: Superuser and Admin Panel

Django includes an admin interface for managing data. To access it, you must create a superuser account:

```bash
python manage.py createsuperuser
```

Provide a username, email, and password when prompted.

Access the admin panel at `http://127.0.0.1:8000/admin/` using the superuser credentials.

---

## Step 8: Useful Tips

- **Debug Mode:** Ensure `DEBUG = True` in `settings.py` during development. Set it to `False` for production.
- **Static Files:** Run `python manage.py collectstatic` to gather static files for production.
- **Environment Variables:** Use libraries like `python-decouple` to manage sensitive settings (e.g., secret keys).

---

### Resources:

- [Django Official Documentation](https://docs.djangoproject.com/)
- [Django Admin Customization](https://docs.djangoproject.com/en/stable/ref/contrib/admin/)
- [Django Deployment Checklist](https://docs.djangoproject.com/en/stable/howto/deployment/checklist/)
