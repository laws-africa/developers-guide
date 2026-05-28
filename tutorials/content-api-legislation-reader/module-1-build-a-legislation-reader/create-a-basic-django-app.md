# Create a basic Django app

## In this section

* Setup a Python environment
* Create a basic Django app

## Create a basic Django app

We will build our example application using Django. There is nothing special about using Django, you can easily build an equivalent application using another language and framework.

Follow the first steps for setting up a Django app using the [Django tutorial](https://docs.djangoproject.com/en/3.2/intro/tutorial01/), using `legislation` and `reader` for your project and app, respectively.

{% hint style="warning" %}
Ignore the instruction to create a `urls.py` file in your `reader` app; we'll cover these in [expression-detail-page.md](expression-detail-page.md "mention").
{% endhint %}

Here is a summary of the commands to run:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install django==3.2
pip install requests
django-admin startproject legislation
cd legislation

python manage.py startapp reader
# see Create database models
python manage.py makemigrations reader
python manage.py migrate

# see Fetching the data
python manage.py ingest_capetown_bylaws <YOUR_AUTH_TOKEN>
python manage.py runserver
```

We'll use the default settings, adding `legislation` and `reader` to the the list of installed apps:

{% code title="settings.py" %}
```python
INSTALLED_APPS = [
    'legislation',
    'reader',
    'django.contrib.admin',
    ...
]
```
{% endcode %}

Instead of creating objects manually to play with, as they do in the [Django tutorial](https://docs.djangoproject.com/en/3.2/intro/tutorial02/#playing-with-the-api), we'll ingest some live data from the publicly available Indigo API in [fetching-the-data.md](fetching-the-data.md "mention").

We won't cover the [Django Admin](https://docs.djangoproject.com/en/3.2/intro/tutorial02/#introducing-the-django-admin) in this tutorial, but you might find it useful.

