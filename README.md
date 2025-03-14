# Usul Data

This repo contains all the data used for usul.ai

## Guides

1. Accessing localized content:
   Entities like books, authors, etc.. don't have a direct "name" property. Instead, they have a "localizedNames" property, which contains a list of localized names.
   Each localized name has a "locale" property, which is a two letter language code, and "text" or "texts".

2. Accessing book content:
   All books have a "versions" property, which contains different editions of the book. Each version has a "value" property. You can access the book's content by fetching this url:

```
https://assets.usul.ai/book-content/{version.source}/{version.value}.json
```

Example:

```
https://assets.usul.ai/book-content/turath/12081.json
```
