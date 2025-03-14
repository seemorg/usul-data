# Usul Data

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

A robust and comprehensive multilingual dataset providing structured Islamic texts, author biographies, and geographical information that powers [Usul.ai](https://usul.ai).

## Overview

This repository contains the complete structured data that drives the Usul.ai platform:

- **Books**: Extensive collection of classical and contemporary Islamic texts with multilingual metadata, content versioning, and cover images
- **Authors**: Detailed biographical information about Islamic scholars with dates, transliterations, and descriptions in multiple languages
- **Genres**: Comprehensive categorization system for Islamic literature (tafsir, creeds, jurisprudence, etc.)
- **Locations**: Historical and modern geographical data related to Islamic scholarship, with multilingual city names
- **Regions**: Broader geographical and cultural regions with historical context and significance

All data is provided in a consistent, well-structured JSON format with comprehensive multilingual support across 14 languages: Arabic, English, Persian, Urdu, Hindi, French, Turkish, Spanish, Malay, Russian, Bengali, Hausa, Somali, and Pashto.

## Data Sources

A significant portion of this dataset is built upon the following scholarly projects:

1. **OpenITI Project**: Many of the classical texts in this repository are based on the [OpenITI](https://openiti.org/) corpus, which provides machine-readable versions of classical Arabic texts. Visit their GitHub repository at [https://github.com/OpenITI](https://github.com/OpenITI).

2. **Turath.io**: This repository incorporates texts and metadata from [Turath.io](https://app.turath.io/), a comprehensive digital library of classical Islamic works that includes data from the Shamela collection.

These invaluable resources have been enhanced with additional metadata, translations, and structural improvements to create a comprehensive multilingual dataset.

## Data Structure

### Multilingual Content System

All entities in the dataset follow a consistent localization pattern designed for maximum accessibility:

```json
"primaryNameTranslations": [
  {
    "locale": "ar",
    "text": "أبو زيد الأنصاري"
  },
  {
    "locale": "en",
    "text": "Abu Zayd Ansari"
  },
  // Additional languages...
]
```

For entities with multiple names, alternate forms, or variations:

```json
"otherNameTranslations": [
  {
    "locale": "en",
    "texts": ["Alternative Name 1", "Alternative Name 2"]
  }
]
```

Biographical, descriptive, and overview content follows the same pattern:

```json
"bioTranslations": [
  {
    "locale": "en",
    "text": "Biographical information in English..."
  },
  {
    "locale": "ar",
    "text": "المعلومات السيرة الذاتية باللغة العربية..."
  }
]
```

### Book Versioning and Content Access

Books in the Usul.ai ecosystem support multiple versions and editions, accessible through:

```json
"versions": [
  {
    "id": "jt7T0Pggh0",
    "value": "1379FarajJubran.YawmiyyatAdamWaHawa.Hindawi075181646-ara1",
    "source": "openiti",
    "aiSupported": true,
    "keywordSupported": true,
    "publicationDetails": {}
  }
]
```

Access the complete book content via the Usul.ai content API:

```
https://assets.usul.ai/book-content/{version.source}/{version.value}.json
```

Example:
```
https://assets.usul.ai/book-content/turath/12081.json
```

## Usage Examples

### Finding Authors by Time Period

<details>
<summary><strong>JavaScript</strong></summary>

```javascript
const authors = require('./authors.json');

// Find authors from the 4th century Hijri (roughly 10th century CE)
const fourthCenturyAuthors = authors.filter(author => 
  author.year >= 300 && author.year < 400
);

// Sort authors by year
const sortedAuthors = fourthCenturyAuthors.sort((a, b) => a.year - b.year);
```
</details>

<details>
<summary><strong>Python</strong></summary>

```python
import json

# Load authors from file
with open('authors.json', 'r', encoding='utf-8') as file:
    authors = json.load(file)

# Find authors from the 4th century Hijri (roughly 10th century CE)
fourth_century_authors = [author for author in authors if author.get('year') and 300 <= author['year'] < 400]

# Sort authors by year
sorted_authors = sorted(fourth_century_authors, key=lambda x: x['year'])
```
</details>

### Accessing Book Information in Multiple Languages

<details>
<summary><strong>JavaScript</strong></summary>

```javascript
const books = require('./books.json');

function getBookTitles(bookId, languages = ['ar', 'en', 'fr']) {
  const book = books.find(b => b.id === bookId);
  if (!book) return null;
  
  return languages.reduce((titles, lang) => {
    const translation = book.primaryNameTranslations.find(t => t.locale === lang);
    if (translation) titles[lang] = translation.text;
    return titles;
  }, {});
}

// Example: Get a book's title in multiple languages
const bookTitles = getBookTitles('1379FarajJubran.YawmiyyatAdamWaHawa');
console.log(bookTitles); 
// Output: { ar: 'يوميات آدم وحواء', en: 'Yawmiyyat Adam Wa Hawa', fr: 'Journal d'Adam et Ève' }
```
</details>

<details>
<summary><strong>Python</strong></summary>

```python
import json

# Load books from file
with open('books.json', 'r', encoding='utf-8') as file:
    books = json.load(file)

def get_book_titles(book_id, languages=['ar', 'en', 'fr']):
    """Get a book's title in multiple languages"""
    book = next((b for b in books if b['id'] == book_id), None)
    if not book:
        return None
    
    titles = {}
    for lang in languages:
        translation = next((t for t in book['primaryNameTranslations'] if t['locale'] == lang), None)
        if translation:
            titles[lang] = translation['text']
    return titles

# Example: Get a book's title in multiple languages
book_titles = get_book_titles('1379FarajJubran.YawmiyyatAdamWaHawa')
print(book_titles)
# Output: { 'ar': 'يوميات آدم وحواء', 'en': 'Yawmiyyat Adam Wa Hawa', 'fr': 'Journal d'Adam et Ève' }
```
</details>

### Looking Up Geographical Information

<details>
<summary><strong>JavaScript</strong></summary>

```javascript
const locations = require('./locations.json');
const regions = require('./regions.json');

// Find all locations in a specific region
function getLocationsByRegion(regionId) {
  return locations.filter(location => location.regionId === regionId)
    .map(location => {
      // Get the English name of each location
      const englishName = location.cityNameTranslations.find(t => t.locale === 'en')?.text;
      return { id: location.id, name: englishName, type: location.type };
    });
}

// Example: Get all locations in Andalus
const andalusLocations = getLocationsByRegion('andalus');
```
</details>

<details>
<summary><strong>Python</strong></summary>

```python
import json

# Load data files
with open('locations.json', 'r', encoding='utf-8') as file:
    locations = json.load(file)
    
with open('regions.json', 'r', encoding='utf-8') as file:
    regions = json.load(file)

def get_locations_by_region(region_id):
    """Find all locations in a specific region"""
    region_locations = [loc for loc in locations if loc.get('regionId') == region_id]
    
    result = []
    for location in region_locations:
        # Get the English name of each location
        english_translation = next((t for t in location.get('cityNameTranslations', []) 
                                  if t.get('locale') == 'en'), None)
        english_name = english_translation.get('text') if english_translation else None
        
        result.append({
            'id': location.get('id'),
            'name': english_name,
            'type': location.get('type')
        })
    
    return result

# Example: Get all locations in Andalus
andalus_locations = get_locations_by_region('andalus')
```
</details>

## Data Updates

The Usul.ai platform is continuously expanding with new content. This repository is regularly updated to reflect the latest additions to the platform. To get the most current data:

```bash
git pull origin main
```

## Contributing

Contributions to enhance or correct the dataset are welcome. If you're interested in:

- Adding new books or authors
- Improving translations
- Correcting metadata
- Enhancing geographical information

Please either:
- Submit a pull request with your changes
- Email us at contribute@usul.ai with your contribution

## License

This dataset is available under the MIT License. You are free to use, modify, and distribute it for both personal and commercial projects, as long as you include the original license and copyright notice.

## Acknowledgments

- This data is maintained by the [Usul.ai](https://usul.ai) team
- The OpenITI project ([https://openiti.org/](https://openiti.org/)) has provided the foundation for many of the classical texts included in this dataset
- Turath.io and the Shamela digital library have contributed significantly to the Arabic textual content
- Special thanks to all the scholars, translators, and contributors who have helped build this comprehensive dataset
- Built with appreciation for the rich tradition of Islamic scholarship spanning centuries and continents