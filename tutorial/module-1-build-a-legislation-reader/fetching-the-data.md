---
description: Fetching and storing data from the Laws.Africa Content API.
---

# Fetching the data

## In this section

* Working with the Laws.Africa Content API
* Creating a management command to fetch data from the Content API

## Using the Laws.Africa Content API

If you haven't already, follow the instructions at the [Laws.Africa Developer Guide](https://developers.laws.africa/quick-start) for setting up an account and getting your API token.

Visit [https://api.laws.africa/v3/akn/za-cpt/.json](https://api.laws.africa/v3/akn/za-cpt/.json) to see the data that we'll be ingesting. Log into your Laws.Africa if necessary.

The [https://api.laws.africa/v3/akn/za-cpt/.json](https://api.laws.africa/v3/akn/za-cpt/.json) endpoint returns a list of all the by-laws for the City of Cape Town, in South Africa. Each **work** is a by-law.

The items in the `results` array are the **most recent expression** of each by-law. Each item in the array has the complete information of both the expression and the work.

### Multiple points-in-time

Legislation changes over time (also called "points in time"), and may also be available in different languages. The content API returns the most recent (latest) available version, in the country's default language (English in this example).

Other points-in-time (**expressions**) may also be available. These are listed in the  `points_in_time` attribute. It contains the dates and expressions available at those dates. There will only be different expressions at the same date if there are different languages. Each entry includes a URL with the full details of that particular expression.

* Each `point_in_time`'s `expression` has its own `url`, to which you can append `.json` to fetch the JSON details of the expression.
* If you don't append `.json`, it will return the XML of the expression.

## Create a management command

Rather than fetching and saving data on the command line, let's write a [Django management command](https://docs.djangoproject.com/en/3.2/howto/custom-management-commands/) that we can run from inside the app to fetch the data from the Laws.Africa Content API.

This command should do the following:

* Start at the [https://api.laws.africa/v3/akn/za-cpt/.json](https://api.laws.africa/v3/akn/za-cpt/.json) endpoint and work with the `results` list.
  * The API token needs to be provided each time an API call is made.
  * The results may be paginated when getting all works in a place, so it's important to check for `next` in the response.
* For each result, create a `Work` object in the database using the data in the result.
  * In the example below, we use the `update_or_create` method, in case a `Work` object with the given FRBR URI already exists in the database. This allows us to run the command multiple times. You may wish to simply use `create`.
* Next, create the relevant `Expression` objects, with the related work being the one that has already been created for the current result.
  * A work can have multiple expressions, or no expressions.
  * You need to look at the list of `expressions` inside each entry in the `points_in_time` list to get the expression details.
  * The metadata for each expression is listed in the place's `results`, but the content of each expression and its table of contents require separate API calls:
    * For the HTML content, append `.html` to the `url` for the expression.
    * For the table of contents, append `/toc.json` to the `url` for the expression.

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

Create the `management/commands/` directories inside `reader`:

```
reader/
    management/
        __init__.py
        commands/
            __init__.py
            ingest_capetown_bylaws.py
```

{% code title="ingest_capetown_bylaws.py" %}
```python
import requests

from django.core.management.base import BaseCommand

from reader.models import Work, Expression


class Command(BaseCommand):
    help = 'Ingest Cape Town By-laws'
    api_url = 'https://api.laws.africa/v3/akn/za-cpt/.json'
    api_token = None

    def add_arguments(self, parser):
        parser.add_argument('api_token', type=str)

    def handle(self, *args, **options):
        self.api_token = options['api_token']
        url = self.api_url

        # handle paginated results
        while url:
            resp = self.call_url_with_token(url).json()

            for result in resp.get('results', {}):
                self.create_work_and_expressions(result)

            url = resp['next']  # this is always present, but may be null

    def create_work_and_expressions(self, data):
        # for each result, create the Work object
        self.stdout.write(self.style.NOTICE(f"Creating or updating a Work for {data['frbr_uri']}"))
        work, new = Work.objects.update_or_create(
            frbr_uri=data['frbr_uri'],
            defaults={
                'title': data['title'],
                'metadata': data
            }
        )
        self.stdout.write(self.style.NOTICE(f'    Work {"created" if new else "updated"}: {work}'))

        # for each work, create the relevant Expression objects
        for date in data['points_in_time']:
            for expression in date['expressions']:
                self.create_expression(expression, work)

    def create_expression(self, data, work):
        self.stdout.write(self.style.NOTICE(f"Creating or updating an Expression for {data['expression_frbr_uri']}"))
        expression, new = Expression.objects.update_or_create(
            work=work,
            frbr_uri=data['expression_frbr_uri'],
            defaults={
                'title': data['title'],
                'language_code': data['language'],
                'date': data['expression_date'],
                'content': self.call_url_with_token(f"{data['url']}.html").content.decode('utf-8'),
                'toc_json': self.call_url_with_token(f"{data['url']}/toc.json").json(),
            }
        )
        self.stdout.write(self.style.NOTICE(f'    Expression {"created" if new else "updated"}: {expression}'))

    def call_url_with_token(self, url):
        self.stdout.write(self.style.NOTICE(f'Making a call to: {url}'))
        resp = requests.get(url, headers={'Authorization': f'token {self.api_token}'})
        resp.raise_for_status()
        self.stdout.write(self.style.NOTICE(f'    Response: {resp.status_code}'))
        return resp

```
{% endcode %}

This command takes your API key as a parameter and stores the content from the API in the database:

```bash
python manage.py ingest_capetown_bylaws <YOUR_AUTH_TOKEN>
```

