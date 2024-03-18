---
description: Adjusting content styles based on enrichment information.
---

# Advanced interactivity

## In this section

* Use enrichment information to dynamically adjust the styles for displayed content.

## Highlighting tagged provisions

Our final piece of interactivity will add an option to highlight provisions that have been tagged with a particular enrichment.

We're going to build the following:

* A dropdown that lets the user choose one of the enrichments.
* When an item is chosen, dim all the text of the document, except those portions that have the chosen enrichment.

Add a new button beneath the `btn-comment` button in `expression_detail.html`:

{% code title="expression_detail.html" %}
```markup
# ... 
    <button id="btn-comment" style="position: fixed; top: 10px; right: 10px;">Add comment...</button>
    <select id="select-enrichments" style="position: fixed; top: 40px; right: 10px;">xx</select>
```
{% endcode %}

Add a new javascript block to populate the dropdown from the items in the gutter, and handles the interactivity when an item is chosen.

{% code title="expression_detail.html" %}
```html
<script>
  const select = document.getElementById('select-enrichments');

  // add a mutation observer to take action when items are added to the gutter
  function updateHighlightOptions () {
    // empty the options
    while (select.firstChild) select.removeChild(select.firstChild);
    // add the empty option
    const option = document.createElement('option');
    option.innerText = "(off)";
    option.value = "";
    select.appendChild(option);

    // unique text of gutter items
    const items = [...new Set([...document.querySelectorAll('la-gutter-item')].map((item) => item.innerText))];
    items.sort();

    // add the items as options to the dropdown
    for (const item of items) {
      const option = document.createElement('option');
      option.innerText = item;
      option.value = item;
      select.appendChild(option);
    }
  }

  const observer = new MutationObserver(() => {
    window.setTimeout(updateHighlightOptions, 500);
  });
  observer.observe(document.getElementById('gutter'), { childList: true });
  updateHighlightOptions();

  // when the select changes, highlight the provisions that match by adding a class to those provisions
  select.addEventListener('change', (e) => {
    const doc = document.getElementsByTagName('la-akoma-ntoso')[0];

    // remove any existing highlights
    for (const provision of document.querySelectorAll('.highlight')) {
      provision.classList.remove('highlight');
    }

    const selected = e.target.value;
    if (selected) {
      // turn on highlighting
      doc.classList.add('highlighting');

      // mark each highlighted element
      for (const item of document.querySelectorAll('la-gutter-item')) {
        if (item.innerText === selected) {
          const anchor = item.getAttribute('anchor');
          const provision = doc.querySelector(`[id="${anchor.slice(1)}"]`);
          if (provision) {
            provision.classList.add('highlight');
          }
        }
      }
    } else {
      // turn off highlighting
      doc.classList.remove('highlighting');
    }
  });
</script>
```
{% endcode %}

Finally, let's add the necessary styling to handle the new `.highlight` and `.highlighting` classes.

Add this block in the `<head>` tag of `expression_detail.html`.

{% code title="expression_detail.html" %}
```markup
  <style>
    .highlighting {
      color: lightgrey;
    }

    .highlighting .highlight {
      color: black;
      background-color: palegreen;
    }
  </style>
```
{% endcode %}

Load the page and choose an item from the dropdown:

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>
