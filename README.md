

# 🧑‍💼 Django User Management API

A RESTful User Management system built using Django and Django REST Framework (DRF). This project supports full CRUD operations for managing users, including email uniqueness validation and scalable routing architecture.

---

## ✅ Step-by-Step Implementation Guide

### 1. 🚀 Project Initialization
- Create a Django project named `UserManagement`:
  ```bash
  django-admin startproject UserManagement


* Inside the project directory, create a Django app named `users`:

  ```bash
  python manage.py startapp users
  ```

---

### 2. 📦 Install Required Packages

```bash
python -m venv venv
source venv/bin/activate       # Mac/Linux
venv\Scripts\activate          # Windows

pip install django djangorestframework
```

---

### 3. ⚙️ Configure Django Settings

In `UserManagement/settings.py`, update `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    ...
    'rest_framework',
    'users',
]
```

---

### 4. 👤 User Model Setup

You can use Django’s built-in `User` model (`django.contrib.auth.models.User`) which includes:

* `id`, `username`, `email`, `first_name`, `last_name`, `date_joined`

---

### 5. 🧩 Create DRF Serializer

In `users/serializers.py`:

```python
from django.contrib.auth.models import User
from rest_framework import serializers

class UserSerializer(serializers.ModelSerializer):
    class Meta:
        model = User
        fields = ['id', 'username', 'email', 'first_name', 'last_name', 'date_joined']

    def validate_email(self, value):
        if User.objects.filter(email=value).exists():
            raise serializers.ValidationError("Email must be unique.")
        return value
```

---

### 6. 🔧 Create Views with ViewSets

In `users/views.py`:

```python
from django.contrib.auth.models import User
from rest_framework import viewsets
from .serializers import UserSerializer

class UserViewSet(viewsets.ModelViewSet):
    queryset = User.objects.all().order_by('-date_joined')
    serializer_class = UserSerializer
```

---

### 7. 🔌 Set Up Routers and URLs

In `users/urls.py`:

```python
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from .views import UserViewSet

router = DefaultRouter()
router.register(r'users', UserViewSet)

urlpatterns = router.urls
```

In `UserManagement/urls.py`:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('users.urls')),
]
```

---

### 8. 🛠️ Run Migrations

```bash
python manage.py makemigrations
python manage.py migrate
```

---

### 9. 🧪 Test API Endpoints

Use **Postman**, **cURL**, or DRF’s **Browsable API** to test:

| Action        | Method | Endpoint           |
| ------------- | ------ | ------------------ |
| Create user   | POST   | `/api/users/`      |
| List users    | GET    | `/api/users/`      |
| Retrieve user | GET    | `/api/users/<id>/` |
| Update user   | PUT    | `/api/users/<id>/` |
| Delete user   | DELETE | `/api/users/<id>/` |

---

## 🎯 Key Features

* Full CRUD API for user management
* Unique email validation
* Clean, scalable DRF architecture
* Uses `ModelViewSet` and `DefaultRouter` for DRY code
* Easily integrable with frontend/mobile clients

---

## 📁 Project Structure

```
UserManagement/
├── manage.py
├── users/
│   ├── serializers.py
│   ├── views.py
│   ├── urls.py
│   └── ...
├── UserManagement/
│   ├── settings.py
│   ├── urls.py
│   └── ...
├── db.sqlite3
└── requirements.txt
```

---

## 🧪 Sample Output

```json
POST /api/users/
{
    "username": "john",
    "email": "john@example.com",
    "first_name": "John",
    "last_name": "Doe"
}
```

Response:

```json
{
    "id": 1,
    "username": "john",
    "email": "john@example.com",
    "first_name": "John",
    "last_name": "Doe",
    "date_joined": "2025-07-19T12:34:56Z"
}
```

---

## ⚡ Optional: gRPC Integration

This project optionally includes a gRPC request via a **Django management command**:

1. Install `grpcio` and `protobuf`:

   ```bash
   pip install grpcio grpcio-tools
   ```

2. Example usage (from terminal):

   ```bash
   python manage.py grpc_demo
   ```

> 📘 Documentation for this script will be available in `users/management/commands/grpc_demo.py`.

---

## 📦 Installation (Quick Setup)

```bash
git clone https://github.com/deepak21SCSE1420019/UserManagement.git
cd UserManagement
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python manage.py migrate
python manage.py runserver
```




