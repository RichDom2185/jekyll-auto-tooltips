---
layout: demo
tooltips:
  - name: apple
    content: |
      A round, red fruit.

      _Example:_ Alice said, "Oh, an apple. I **love** apples!"
---
<!-- markdownlint-disable-file first-line-h1 -->
<!-- markdownlint-disable-file blanks-around-fences -->
<!-- markdownlint-disable-file blanks-around-headers -->

> {{ site.description }}
{: style="font-size: 1.5em" }

For the full guide (usages, etc.), view the project on [GitHub]({{ site.github.repository_url }}).

<!-- markdownlint-disable-next-line blanks-around-headers -->
## Table of Contents
{: .no_toc}

* toc
{:toc}

## What it does

It can automatically create tooltips for you, from a customizable set of tooltip data:

Original Markdown code:

```markdown
* An [[ :apple ]]
```

Result:

* An [[ apple ]]

### Terminology

* `key`: the key used to search the dictionary/dataset of labels
* `entry`: the data to be shown inside the tooltip preview
* `label`: the dotted-underlined tooltip label. If unspecified, it is equal to the key

### Custom labels

<!-- markdownlint-disable-next-line no-inline-html -->
To specify a different label for the tooltip than the key, map the key to a label using a colon (<kbd>:</kbd>).

Keys are case sensitive, which means custom labels can be useful, especially for plural forms and start-of-sentence labels.

```markdown
* An [[ :apple ]]
* Two [[ :apple:apples ]]
* [[ :apple:Apple ]], not to be confused with the computer company, Apple Inc.
```

Result:

* An [[ apple ]]
* Two [[ apple:apples ]]
* [[ apple:Apple ]], not to be confused with the computer company, Apple Inc.

### Keys with no entries

When there is no tooltip entry to be found for the corresponding key, it leaves the syntax untouched:

```markdown
* A [[ banana ]]
```

Result:

* A [[ banana ]]

### Preventing tooltip generation

In order to prevent a tooltip for being generated for a specific key (e.g. "apple"), we explicitly say that "apple" is a label, not a key. In order to do so, map an empty key ("") to the label to prevent the tooltip generation. This is equivalent to prefixing the "apple" key with a colon.

This has actually been used in this page in order to prevent the tooltips from being generated in the Markdown code examples.

```markdown
* An [[ ::apple ]]
```

Result:

* An [[ :apple ]]

## Creating tooltips data

Simply add to the YAML frontmatter in the page(s) you want your tooltips to be available in. This also means you are free to have different entries for different keys in different pages. Full Markdown is supported, which means you can style the tooltip entries to your liking.

```yaml
tooltips:
  - name: apple
    content: |
      A round, red fruit.

      _Example:_ Alice said, "Oh, an apple. I **love** apples!"
  # ... and so on
```

## Extension: Glossary generation

Since the tooltips generated also include links to a specific reference/fragment of the page, it can also be useful to automatically generate glossaries. You can easily generate a glossary that links to each tooltip, just like the one at the [bottom of this page](#glossary).

---

## Glossary

This glossary is automatically generated from the available tooltip entries on this page, and so is the quick reference table below:

{% assign groups = page.tooltips | group_by_exp: "item", "item.name | truncate: 1, ''" %}

<!-- markdownlint-disable no-inline-html -->
<table>
<thead>
  <td align="center"><strong>Quick Reference</strong></td>
</thead>
{% tablerow entry in page.tooltips %}
<a href="#glossary-{{ entry.name | slugify }}">{{ entry.name }}</a>
{% endtablerow %}
</table>
<!-- markdownlint-enable no-inline-html -->

{% for group in groups %}
### {{ group.name | upcase }}
{: .no_toc #glossary-{{ group.name | downcase }}}

<!-- markdownlint-disable-next-line no-inline-html -->
<div markdown="1" class="glossary">
{% for entry in group.items %}
#### {{ entry.name | title_case }}
{: .no_toc #glossary-{{ entry.name | slugify }}}

<!-- markdownlint-disable-next-line no-inline-html -->
<div markdown="1" class="glossary-body">
{{ entry.content }}
</div>
{% endfor %}
</div>

{% endfor %}
