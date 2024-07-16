# Creating a Custom User Model in Django

Django allows for the extension of the default user model, enabling the customization of user-related data. This guide outlines the steps to create a custom user model.

## Dump Data (Create Backup), But Only Wanted Apps

1. Preview data in terminal/console:
    ```shell
    python manage.py dumpdata
    ```
    - This will show all data in the database.
    - To filter, add the app name:
        ```shell
        python manage.py dumpdata app_name
        ```


2. Create backup, write to file in project root:
    ```shell
    python manage.py dumpdata > backup.json
    ```

## Create the Custom User Model

1. Create a new app:
    ```shell
    python manage.py startapp user
    ```

2. Register the new app in `settings.py`.

3. Register UserModel in `settings.py` (at the bottom):
    ```python
    AUTH_USER_MODEL = "user.User" # nameOfApp.nameOfModel
    ```

4. Create the user model in `models.py`:
    ```python
    from django.contrib.auth.models import AbstractUser
    from django.db import models

    class User(AbstractUser):
        pass
    ```

5. Remove old user imports from models, replace with new user model:
    ```python
    from django.contrib.auth import get_user_model
    User = get_user_model()
    ```

6. Delete old SQLite database.

7. Delete old migrations.

8. Make migrations and migrate. If `user.user` table shows up in IDE database, all is good.

9. (Optional) Change `auth.user` to `user.user` in `backup.json`. (This is only necessary if you want to import the old data.)

10. Import database backup:
    ```shell
    python manage.py loaddata backup.json
    ```

### Optional

- Register UserTable in admin:
    ```python
    from django.contrib import admin
    from django.contrib.auth import get_user_model
    from django.contrib.auth.admin import UserAdmin

    User = get_user_model()
    admin.site.register(User, UserAdmin)
    ```

## Expanding/Modifying UserModel

1. Overwrite `AbstractUser` with email as login in User Model:
    ```python
    from django.contrib.auth.models import AbstractUser
    from django.db import models

    class User(AbstractUser):
        email = models.EmailField(unique=True)
        USERNAME_FIELD = "email"
        REQUIRED_FIELDS = [] # Overwrite required fields
    ```


2. Fix Admin Registration:
    ```python
    from django.contrib import admin
    from django.contrib.auth.admin import UserAdmin
    from .models import User

    class CustomUserAdmin(UserAdmin):
        readonly_fields = ('date_joined',)

        add_fieldsets = (
            (None, {
                'classes': ('wide',),
                'fields': ('email', 'username', 'password1', 'password2')}
            ),
        )

        fieldsets = (
            (None, {'fields': ('email', 'username', 'password')}),
            ('Personal info', {'fields': ('first_name', 'last_name')}),
            ('Permissions', {'fields': ('is_active', 'is_staff', 'is_superuser', 'user_permissions')}),
            ('Important dates', {'fields': ('last_login', 'date_joined')}),
            ('Groups', {'fields': ('groups',)}),
        )

    admin.site.register(User, CustomUserAdmin)
    ```