---
description: Adding and displaying simple enrichments.
---

# Basic enrichments

## In this section

* Element eIds.
* Adding enrichments to parts of the document.
* Showing enrichments in the document gutter.

## Element eIds

One of the big benefits of Akoma Ntoso XML is that we can treat a document as data, not just text. This is possible because every element in an AKN document has a well-formed, unique ID called an **eId**. These eIds are also included in the HTML form of the document, as both the `id` and the `data-eid` attributes.

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption><p>The XML eId is included as both the id and data-eid elements in the corresponding HTML.</p></figcaption></figure>

We can associate extra data with an element's eId, and then show it along with the document.

## Enriching a document

In this section, we'll build some interactivity that allows the user to comment on any item in the document, using this eId attributes. We will show the comments in the gutter to the right of the document.

Let's add the Law Widgets [\<la-gutter>](https://github.com/laws-africa/law-widgets/blob/main/core/src/components/gutter/readme.md) widget to the page, and a floating button to add a comment. We use the `.la-akoma-ntoso-with-gutter` helper class to provide styles and spacing for gutter items.

Modify your `expression_detail.html` file:

{% code title="expression_detail.html" %}
```html
<!-- ... -->

    <div style="flex: 3">
      <la-decorate-terms popup-definitions link-terms></la-decorate-terms>
      <la-decorate-internal-refs popups flag></la-decorate-internal-refs>

      <div class="la-akoma-ntoso-with-gutter">
        <!-- use la-akoma-ntoso law widget to apply styles -->
        <la-akoma-ntoso frbr-expression-uri="{{ expression.frbr_uri }}">
          {{ expression.content|safe }}
        </la-akoma-ntoso>

        <la-gutter id="gutter"></la-gutter>
      </div>
    </div>

    <button id="btn-comment" style="position: fixed; top: 10px; right: 10px;">Add comment...</button>

```
{% endcode %}

Now we're going to add some javascript to support the following functionality:

1. The user selects text anywhere in the document
2. The user clicks on the "Add comment..." button
3. The user is prompted to type in their comment
4. We get the best eId for the selection
5. We create a new `<la-gutter-item>` widget that has the comment text, and add it to the gutter.

Add this script block to the bottom of `expression_detail.html`:

{% code title="expression_detail.html" %}
```html
<!-- ... -->

  <script>
    // add a comment on the selected text
    document.getElementById('btn-comment').addEventListener('click', (e) => {
      const sel = document.getSelection();
      if (sel && !sel.isCollapsed) {
        let node = sel.getRangeAt(0).commonAncestorContainer;
        // go from a text node to an element
        if (node.nodeType !== document.ELEMENT_NODE) {
          node = node.parentElement;
        }
        // get the nearest element with an eId
        node = node.closest('[data-eid]');
        if (node) {
          const eId = node.getAttribute('data-eid');
          const comment = prompt(`What's your comment on this section? (#${eId})`);
          if (comment) {
            item = document.createElement('la-gutter-item');
            item.setAttribute('anchor', `#${eId}`);
            item.innerText = comment;
            document.getElementById('gutter').appendChild(item);
          }
        }
      }
    });
  </script>
</body>
```
{% endcode %}

The user can now link a comment to any portion of the document. This example is very simple, but you can see how you can link arbitrary data to the various portions of the document.

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

Remember that the Table of Contents JSON object has the titles and eIds of all the hierarchical elements in the document. You can use this to build an interface to help your editors to annotate different parts of the document.

The `<la-gutter>` widget makes it easy to show a comment or other information alongside a part of the document. Add a `<la-gutter-item>` element to the `<la-gutter>` and set its `anchor` attribute to the eId of the targeted element.&#x20;

The gutter positions the gutter item at the correct height of its matching anchor. It will do its best to lay out other gutter items so that they don't overlap.

Clicking on a gutter item activates it, and forces it to be shown alongside its anchor. Surrounding gutter items will move out the way.

Gutter items can be anything: text, images, buttons, or cards. How you style them is up to you.
