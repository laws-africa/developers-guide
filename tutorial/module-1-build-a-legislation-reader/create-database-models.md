---
description: Create models for storing data in the database.
---

# Create database models

## In this section

* Create the Django models for storing data in the database

We're going to create a **Work** model and an **Expression** model. Both models are uniquely identified by their respective **FRBR URIs**.

{% hint style="info" %}
See [introductory-concepts.md](introductory-concepts.md "mention") for background on **Works**, **Expressions** and **FRBR URI**s.
{% endhint %}

We'll store all the raw metadata in a single `metadata` JSON field. We'll also extract certain fields from that metadata and store that information separately, so we can query it more easily.

{% code title="models.py" %}
```python
from django.db import models


class Work(models.Model):
    frbr_uri = models.CharField(max_length=512, unique=True)
    title = models.CharField(max_length=512)
    metadata = models.JSONField()
        
    class Meta:
        ordering = ['title']

    def __str__(self):
        return f'{self.title} ({self.frbr_uri})'


class Expression(models.Model):
    # the expression inherits most of its metadata from its work
    work = models.ForeignKey(Work, related_name='expressions', on_delete=models.PROTECT)
    # the expression's FRBR URI is more specific than its work's
    frbr_uri = models.CharField(max_length=512, unique=True)
    # the expression's title may differ from its work's
    title = models.CharField(max_length=512)
    # the expression has a language, date, and content, which its work doesn't have
    language_code = models.CharField(max_length=3)
    date = models.DateField()
    content = models.TextField(null=True, blank=True)
    toc_json = models.JSONField()

    class Meta:
        # latest first
        ordering = ['-date']

    def __str__(self):
        return f'{self.title} ({self.frbr_uri})'

```
{% endcode %}

{% hint style="info" %}
Note:

* The `frbr_uri` on both `Work` and `Expression` must be unique.
* One work can have multiple expressions, hence the ForeignKey from `Expression` to `Work`.
* Expressions require a work, so `PROTECT` any expressions if a work is deleted.
* Only key pieces of metadata needed for the legislation reader app have been specified on the `Work` model. The `metadata` field is used for storing the rest.
{% endhint %}

You'll need to [make and run migrations](https://docs.djangoproject.com/en/3.2/intro/tutorial02/#activating-models) after adding these models.

```sh
python manage.py makemigrations
python manage.py migrate
```
