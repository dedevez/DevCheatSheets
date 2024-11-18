# Setting Up a PostgreSQL Database in a Django Project (Using `python-dotenv`)

This cheat sheet provides a step-by-step guide for configuring PostgreSQL as the database for a Django project. It also covers best practices for security and deployment.

---

## Table of Contents

1. [Install PostgreSQL](#install-postgresql)
2. [Set Up a PostgreSQL Database](#set-up-a-postgresql-database)
3. [Install Required Packages](#install-required-packages)
4. [Configure `.env` File](#configure-env-file)
5. [Update Django Settings](#update-django-settings)
6. [Apply Migrations](#apply-migrations)
7. [Best Practices for Security and Maintenance](#best-practices-for-security-and-maintenance)
8. [Resources](#resources)

---

## Install PostgreSQL

### On Linux

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```

### On macOS

Use Homebrew:

```bash
brew update
brew install postgresql
```

### On Windows

1. Download the installer from [PostgreSQL Downloads](https://www.postgresql.org/download/).
2. Run the installer and follow the prompts.

---

## Set Up a PostgreSQL Database

### Access the PostgreSQL Command Line Interface

Log in as the default PostgreSQL user (`postgres`):

```bash
sudo -i -u postgres psql
```

### Create a Database

```sql
CREATE DATABASE mydatabase;
```

### Create a User

```sql
CREATE USER myuser WITH PASSWORD 'mypassword';
```

### Grant Privileges to the User

```sql
GRANT ALL PRIVILEGES ON DATABASE mydatabase TO myuser;
```

### Exit `psql`

```sql
\q
```

---

## Install Required Packages

Django requires the `psycopg2` package to interact with PostgreSQL. Additionally, install `python-dotenv` to manage environment variables.

```bash
pip install psycopg2-binary python-dotenv
```

> **Note**: Use `psycopg2` (without `-binary`) in production for better performance.

---

## Configure `.env` File

Create a `.env` file in the root directory of your Django project (next to `manage.py`) and add the database configuration:

```env
DATABASE_NAME=mydatabase
DATABASE_USER=myuser
DATABASE_PASSWORD=mypassword
DATABASE_HOST=localhost
DATABASE_PORT=5432
```

> **Tip**: Do not commit your `.env` file to version control. Add it to `.gitignore`:

```bash
echo ".env" >> .gitignore
```

---

## Update Django Settings

Modify the `settings.py` file to load environment variables using `python-dotenv` and configure the PostgreSQL database.

### Import and Load Environment Variables

At the top of `settings.py`:

```python
from dotenv import load_dotenv
import os

load_dotenv()  # Load environment variables from .env
```

### Update `DATABASES` Configuration

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.getenv('DATABASE_NAME'),
        'USER': os.getenv('DATABASE_USER'),
        'PASSWORD': os.getenv('DATABASE_PASSWORD'),
        'HOST': os.getenv('DATABASE_HOST', 'localhost'),
        'PORT': os.getenv('DATABASE_PORT', '5432'),
    }
}
```

---

## Apply Migrations

Run the following commands to apply migrations and create database tables:

1. Generate migration files:

   ```bash
   python manage.py makemigrations
   ```

2. Apply migrations:

   ```bash
   python manage.py migrate
   ```

3. Create a superuser:

   ```bash
   python manage.py createsuperuser
   ```

4. Test the setup by running the development server:
   ```bash
   python manage.py runserver
   ```

---

## Best Practices for Security and Maintenance

### Secure Your PostgreSQL Instance

1. Use strong passwords for database users.
2. Limit access to the database to trusted IP addresses using PostgreSQL’s `pg_hba.conf` file.
3. Use TLS/SSL for connections in production.

### Backup Your Database

Use `pg_dump` to back up your database:

```bash
pg_dump -U myuser mydatabase > backup.sql
```

### Environment Variables Best Practices

- Use `.env` for local development but rely on system environment variables for production.
- Automate loading of environment variables in production using tools like `dotenv` or a deployment pipeline.

---

## Resources

- [Django Database Settings](https://docs.djangoproject.com/en/stable/ref/settings/#databases)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [pgAdmin](https://www.pgadmin.org/)
- [python-dotenv Documentation](https://pypi.org/project/python-dotenv/)
