---
description: Fetching enrichments from the Laws.Africa Enrichments API.
---

# Advanced enrichments

## In this section

* Working with the Laws.Africa Enrichments API
* Displaying API-based enrichments in a document

We'll be using the [Cape Town Liquor Trading by-law](https://openbylaws.org.za/akn/za-cpt/act/by-law/2014/control-undertakings-liquor/eng@2014-12-12) for these examples, which has the FRBR URI `/akn/za-cpt/act/by-law/2014/control-undertakings-liquor`.

## Enrichments API

The Laws.Africa Enrichments API provides access to the enrichment datasets. The API endpoint is [https://api.laws.africa/v3/enrichment-datasets/](https://api.laws.africa/v3/enrichment-datasets/).

An **enrichment** is additional data that is associated with an expression, but is not included in the text of the document. Laws.Africa groups common enrichments together as an **enrichment dataset**. Each enrichment dataset has a **taxonomy tree** that is used to "tag" (enrich) elements.

In this example we will use the "Tutorial" enrichment dataset provided by the Laws.Africa API.

* Name: Tutorial Enrichment Dataset
* Taxonomy tree:
  * Tutorial
    * Red
    * Green
    * Blue

An enrichment is linked to a single provision (element) in a work, identified with the provision's **eId**.&#x20;

For example, we can enrich Section 4(1) of the Cape Town Liquor Trading by-law with the taxonomy tag "Red":

* work: `/akn/za-cpt/act/by-law/2014/control-undertakings-liquor`
* provision: `sec_4__subsec_1`
* taxonomy topic: `red`

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption><p>Visualisation of section 4(1) enriched with "Red"</p></figcaption></figure>

{% hint style="info" %}
The enrichment is linked to a work, not an expression. The same enrichment applies to all expressions of the work, including different language and date versions.
{% endhint %}

## Storing enrichments

We need to model enrichments to store them in our Django app's database.

{% code title="models.py" %}
```python
# ...

class EnrichmentDataset(models.Model):
    name = models.CharField(max_length=512, unique=True)
    taxonomy_tree = models.JSONField()


class ProvisionEnrichment(models.Model):
    dataset = models.ForeignKey(EnrichmentDataset, related_name='enrichments', on_delete=models.CASCADE)
    work = models.ForeignKey(Work, related_name='enrichments', on_delete=models.CASCADE)
    provision_id = models.CharField(max_length=512)
    taxonomy_topic = models.CharField(max_length=1024)
```
{% endcode %}

Now create new database migrations and run them to update the database:

```sh
python manage.py makemigrations
python manage.py migrate
```

Now let's create a new management command to fetch the Tutorial Dataset from the Enrichments API endpoint [https://api.laws.africa/v3/enrichment-datasets/1.json](https://api.laws.africa/v3/enrichment-datasets/1.json).

Create a new file `reader/management/commands/ingest_enrichments.py`:

{% code title="ingest_enrichments.py" %}
```python
import requests

from django.core.management.base import BaseCommand

from reader.models import Work, Expression, EnrichmentDataset, ProvisionEnrichment


class Command(BaseCommand):
    help = 'Ingest Enrichments'
    api_url = 'https://api.laws.africa/v3/enrichment-datasets/1.json'
    api_token = None

    def add_arguments(self, parser):
        parser.add_argument('api_token', type=str)

    def handle(self, *args, **options):
        self.api_token = options['api_token']
        resp = self.call_url_with_token(self.api_url).json()

        self.stdout.write(self.style.NOTICE(f"Creating or updating enrichment dataset {resp['name']}"))
        dataset, new = EnrichmentDataset.objects.update_or_create(
            name=resp['name'],
            taxonomy_tree=resp['taxonomy']
        )

        # clear out existing enrichments
        dataset.enrichments.all().delete()

        # for each enrichment, create the relevant object
        for enrichment in resp['enrichments']:
            work = Work.objects.filter(frbr_uri=enrichment["work"]).first()
            if work:
                self.stdout.write(self.style.NOTICE(f"    Enrichment created for {enrichment['work']} -- {enrichment['provision_id']}"))
                ProvisionEnrichment.objects.create(
                    work=work,
                    dataset=dataset,
                    provision_id=enrichment["provision_id"],
                    taxonomy_topic=enrichment["taxonomy_topic"]
                )
            else:
                self.stdout.write(self.style.WARNING(f"Work does not exist: {enrichment['work']}"))

    def call_url_with_token(self, url):
        self.stdout.write(self.style.NOTICE(f'Making a call to: {url}'))
        resp = requests.get(url, headers={'Authorization': f'token {self.api_token}'})
        resp.raise_for_status()
        self.stdout.write(self.style.NOTICE(f'    Response: {resp.status_code}'))
        return resp

```
{% endcode %}

You will need to provide it with your API token:

```bash
python manage.py ingest_enrichments TOKEN
```

## Displaying enrichments

We're going to show the enrichments in the gutter, just like we did for the basic enrichments. This time, however, we'll load them when the page loads.

Let's update the `expression_detail.html` file to replace the existing `la-gutter` as follows:

<pre class="language-markup" data-title="expression_detail.html"><code class="lang-markup"><strong># ...
</strong><strong>
</strong><strong>        &#x3C;la-gutter id="gutter">
</strong>          {% for enrichment in expression.work.enrichments.all %}
            &#x3C;la-gutter-item anchor="#{{ enrichment.provision_id }}">{{ enrichment.taxonomy_topic }}&#x3C;/la-gutter-item>
          {% endfor %}
        &#x3C;/la-gutter>

</code></pre>

Now the enrichments (if any) will show in the gutter when the page loads.

The tutorial enrichment dataset only has enrichments for the FRBR URI `/akn/za-cpt/act/by-law/2014/control-undertakings-liquor`.

Try visiting [http://localhost/expression/akn/za-cpt/act/by-law/2014/control-undertakings-liquor/eng@2014-12-12](http://localhost/expression/akn/za-cpt/act/by-law/2014/control-undertakings-liquor/eng@2014-12-12) to view the enrichments in your local dataset.

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption><p>Showing enrichments pulled from the enrichments dataset.</p></figcaption></figure>
