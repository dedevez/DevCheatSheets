# Managing Media Files in a Django Project

Media files in Django refer to user-uploaded files such as profile pictures, documents, or any other file type uploaded by users during runtime. This cheat sheet provides a comprehensive guide for setting up, managing, and securing media files in Django.

---

## Table of Contents

1. [What Are Media Files?](#what-are-media-files)
2. [Setting Up Media Files](#setting-up-media-files)
3. [Serving Media Files During Development](#serving-media-files-during-development)
4. [Using Media Files in Templates](#using-media-files-in-templates)
5. [Best Practices for Media Files](#best-practices-for-media-files)
6. [Security Considerations](#security-considerations)
7. [Resources](#resources)

---

## What Are Media Files?

- Media files are files uploaded by users during runtime.
- Examples:
  - Profile pictures
  - Uploaded documents
  - Videos or audio files
  - Attachments in forms

Django handles media files differently during development and production.

---

## Setting Up Media Files

### Add Configuration to `settings.py`

Add the following configuration in your `settings.py` file:

```python
# URL to access media files
MEDIA_URL = '/media/'

# Path to store media files
MEDIA_ROOT = BASE_DIR / 'media'
```

### Create a `media/` Directory

1. In your project’s root directory (where `manage.py` is located), create a folder named `media`.
2. Ensure this directory is writable by the server for uploading files.

---

## Serving Media Files During Development

During development, Django can serve media files directly. Update your project’s `urls.py` file to include the following:

```python
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    # Your other URL patterns
]

# Serve media files in development
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

> **Note**: Django does not serve media files in production. Use a web server (e.g., Nginx, Apache) or cloud storage.

---

## Using Media Files in Templates

To use media files in your HTML templates, you need to load the `media` template tag.

### Example:

1. In your model, define a file field:

   ```python
   from django.db import models

   class Profile(models.Model):
       name = models.CharField(max_length=100)
       profile_picture = models.ImageField(upload_to='profile_pictures/')
   ```

   - The `upload_to` argument specifies the directory within `MEDIA_ROOT` where the file will be saved.

2. In your view, pass the model instance to the template:

   ```python
   from django.shortcuts import render
   from .models import Profile

   def profile_view(request):
       profile = Profile.objects.first()
       return render(request, 'profile.html', {'profile': profile})
   ```

3. In your template, use the `{{ model.field.url }}` attribute to access the file URL:
   ```html
   {% load static %}
   <img src="{{ profile.profile_picture.url }}" alt="{{ profile.name }}" />
   ```

### Explanation:

- **`upload_to`**: Defines the subdirectory within `MEDIA_ROOT` for saving files.
- **`.url`**: Provides the accessible URL for the uploaded file.

---

## Best Practices for Media Files

1. **Organize Files**:

   - Use the `upload_to` argument to organize files by type or category.
   - Example:
     ```python
     upload_to='uploads/documents/'
     ```

2. **Set File Size Limits**:

   - Validate file size during upload:

     ```python
     from django.core.exceptions import ValidationError

     def validate_file_size(value):
         max_size = 5 * 1024 * 1024  # 5MB
         if value.size > max_size:
             raise ValidationError('File size exceeds the 5MB limit.')

     class Document(models.Model):
         file = models.FileField(upload_to='documents/', validators=[validate_file_size])
     ```

3. **Restrict File Types**:

   - Use validators to limit accepted file types:

     ```python
     from django.core.validators import FileExtensionValidator

     class Document(models.Model):
         file = models.FileField(
             upload_to='documents/',
             validators=[FileExtensionValidator(allowed_extensions=['pdf', 'docx'])]
         )
     ```

4. **Use Cloud Storage in Production**:
   - Integrate cloud storage services like AWS S3, Google Cloud Storage, or Azure for scalability and security.

---

## Security Considerations

1. **Prevent Directory Traversal Attacks**:

   - Always use the `.url` attribute to access files instead of constructing file paths manually.

2. **Validate File Content**:

   - Use libraries like `python-magic` to validate file MIME types for added security.

3. **Use Secure Storage**:

   - Restrict permissions on the `media/` directory to prevent unauthorized access.

4. **Serve Private Media Files**:

   - For sensitive files, use Django's `FileResponse` to serve files instead of exposing them via URLs.

   ```python
   from django.http import FileResponse

   def private_file_view(request, filename):
       file_path = MEDIA_ROOT / 'private' / filename
       return FileResponse(open(file_path, 'rb'))
   ```

---

## Resources

- [Django Media Files Documentation](https://docs.djangoproject.com/en/stable/topics/files/)
- [Django Validators](https://docs.djangoproject.com/en/stable/ref/validators/)
- [Boto3 for AWS S3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)
