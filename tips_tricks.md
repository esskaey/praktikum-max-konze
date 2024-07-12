# Tips and tricks


## User

### Create new user

```shell
>> python manage.py shell
```
```python
# shell
from django.contrib.auth.models import User
usr = User.objects.create_user("username")
```
### Delete user

```python
python manage.py shell

from django.contrib.auth.models import User

usr = User.objects.filter(username="username")
usr.delete()
```


### Set user password
```python
python manage.py shell

from django.contrib.auth.models import User

usr = User.objects.get(username='your username')
usr.set_password('raw password')
usr.save()
```

## Reload updated styles for the webpage

- When the style sheets updates are not immediately reflected on the website page. Press "Ctrl+F5" for reloading style sheets

## Dos and donts with models and db actions to avoid inconsistencies
 - Dont delete migration(s) as it may make the data model and database inconsistent with each other

 - Dont remove (DROP) complete tables from the database without reciprocating it first at the datamodel side ( App.models.__modelname__). If required remove rows (TRUNCATE) from the table instead

 - When moving data from one field to another field in a model, use the following procedure to avoid data loss

        1. Add the new field to the model affected, with null=True
        2. Perform 'makemigrations' and 'migrate' to prepare the database table
        3. move the data manually or using user functions from one field to another
        4. verify data accuracy of the move accuracy
        5. remove the old field from model and perform migrations as in step 2.

 - Procedure for switching between different databases

## Running a selected test

In order to run a selected test case(s), then run the following command

```bash
# example: python manage.py test apps.content_managment.tests.views.partials.test_partials
python manage.py test {PATH_TO_TESTCASE}
```