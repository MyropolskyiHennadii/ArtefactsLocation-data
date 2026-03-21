# How the Database Was Built

## Overview

The ArtefactsLocation database was built by systematically crawling Wikipedia categories 
related to architectural styles and extracting structured data from article infoboxes. 
The goal was to obtain architectural objects clearly assigned to specific architectural 
categories with quality descriptions from Wikipedia itself.

## Data Collection Method

### Step 1: Category Crawling

The process starts with a Wikipedia category, for example **"Gothic architecture of France"**.

The program recursively traverses:
- The category itself
- All subcategories
- Down to the "end" articles (individual buildings/objects)

Each discovered article is assigned to a general architectural category 
(e.g., "Gothic Architecture").

By systematically crawling countries and styles/categories, the full database was assembled.

### Step 2: Data Extraction from Infoboxes

For each Wikipedia article, the following data is extracted from the article's **infobox**:

- **Object name** (in the article's language)
- **Geographical coordinates** (latitude/longitude)
- **Architect(s)** / author(s)
- **Dates** (construction, reconstruction, destruction, etc.)
- **Image** reference

### Step 3: Multilingual Synonyms

For each article, the program collects **synonym pages** — the same object's Wikipedia 
articles in other supported languages (currently 10-12 languages). This provides:
- Object names in multiple languages
- Wikipedia references in each language

### Step 4: Date Normalization

Dates from infoboxes require semi-manual normalization. For example:

| Infobox text                   | Normalized result                |
|--------------------------------|----------------------------------|
| "created 1154 rebuilt 1678"    | creation completed: 1154-1678    |
| "built between XII-XVI"        | creation completed: XII-XVI      |

### Step 5: Quality Control (Visual Inspection)

After each batch of 500-1000 records, results are visually inspected to catch:
- **Miscategorized articles** (e.g., films or books appearing under architectural categories)
- **Non-architectural objects**
- **Data extraction errors**

## Why Wikipedia, Not Wikidata?

The decision to crawl Wikipedia directly (rather than using Wikidata) was intentional:

1. **Clear category assignment** — Wikipedia categories provide well-structured 
   architectural classification (style, period, country)
2. **Richer data** — Wikipedia articles often contain more structured data in infoboxes 
   than the corresponding Wikidata items
3. **Quality descriptions** — Wikipedia provides meaningful article text, not just 
   external links
4. **More complete coverage** — An architect's "list of works" article on Wikipedia 
   often links to more buildings than the corresponding Wikipedia category or 
   Wikidata contains

## Ongoing Maintenance

The database is actively maintained. Each table has `created`, `modified`, and `reviewed` 
fields for tracking data freshness.

### Annual Review Process

A full review is performed approximately once a year (takes 7-10 days of machine time). 
The typical error rate does not exceed **0.3%** of the database size.

The review covers:

| Task | Description |
|------|-------------|
| **New articles** | Crawl categories again to discover newly created Wikipedia articles |
| **New synonyms** | Check if existing objects gained new Wikipedia pages in supported languages |
| **Duplicate detection** | Two independent pages (in different languages) about the same object may get merged by Wikipedia editors. Detect and remove the resulting duplicates |
| **Page renames** | Track articles that changed their title (important for extracting summaries) |
| **Deleted pages** | Detect and handle articles that no longer exist |
| **Data updates** | Pick up changes to infobox data (dates, architects, coordinates) |

### Semi-Manual Additions (Recent)

Recently, the database has been expanded through semi-manual work:
- Adding objects from non-Wikipedia sources (currently limited to specific cities)
- Expanding coverage by working through architects' lists of works on Wikipedia
- These manual additions account for less than **0.02%** of the database volume

## Database Timeline

- **2021** — Initial database construction by crawling Wikipedia categories worldwide
- **2021-2025** — Annual reviews and updates
- **2025-2026** — Migration to Java 21, public release on GitHub and Zenodo
- **Last full review** — March 2026 (immediately before public release)

## Technical Implementation

The crawling and maintenance tools are written in Java. The data model is available at:
- **[ArtefactsLocation-model](https://github.com/MyropolskyiHennadii/ArtefactsLocation-model)** — Java/Hibernate entity model

## Statistics

- **~200,000** architectural objects
- **11** supported languages
- **99%** of objects have a direct Wikipedia page link
- **Annual error rate:** < 0.3%
- **Manual additions:** < 0.02% of total volume
