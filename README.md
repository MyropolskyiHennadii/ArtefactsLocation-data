# ArtefactsLocation-data

A comprehensive database of 200,000+ architectural and cultural heritage objects worldwide, with geographical coordinates, multilingual support, and rich metadata.

## Overview

This database contains curated information about architectural heritage from around the world, sourced primarily from Wikipedia and other open cultural heritage databases. Each object includes:

- Geographical coordinates (longitude/latitude)
- Names in multiple languages
- Wikipedia references
- Architectural styles and periods
- Historical events (construction, destruction, etc.)
- Author/architect information
- Images and copyright information

## Database Statistics

- **200,000+** architectural objects
- **Worldwide coverage** with geographical coordinates
- **Multilingual support** - names and descriptions in multiple languages
- **Rich categorization** - architectural styles, temporal periods, subjects
- **Well-defined authors** - architects with Wikipedia references
- **Historical events** - construction dates, significant modifications

## Download

**Latest Release:** [v1.0.0](../../releases/latest)

Download `artefacts-database.zip` (27 MB) containing complete MySQL dump files.

## Database Schema

The database consists of the following main tables:

### Core Tables
- **artefacts** - Main table with architectural objects
- **artefacts_locations** - GPS coordinates (partitioned by longitude)
- **artefacts_images** - Image references and copyright
- **artefacts_authors** - Authors/architects attribution
- **artefacts_events** - Historical events
- **artefacts_synonyms** - Names in different languages

### Classification Tables
- **categories** - Architectural styles, temporal periods
- **categories_synonyms** - Category names in multiple languages
- **artefacts_categories** - Many-to-many relationship
- **themas** (subjects) - High-level grouping (Architecture, Sculpture, etc.)

### Author Tables
- **web_authors** - Well-defined authors with Wikipedia pages
- **web_authors_synonyms** - Author names in multiple languages

## Installation

### Requirements
- MySQL 5.7+ or MariaDB 10.3+
- Approximately 500 MB disk space

### Import Instructions

1. **Create database:**
   ```sql
   CREATE DATABASE artefacts_and_locations CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
2. **Extract the zip file:**
unzip artefacts-database.zip
3. **Import SQL files:**
mysql -u your_username -p artefacts_and_locations < artefacts.sql
mysql -u your_username -p artefacts_and_locations < artefacts_locations.sql
mysql -u your_username -p artefacts_and_locations < categories.sql
# ... import all other tables ...
4. **Verify import:**

USE artefacts_locations;
SELECT COUNT(*) FROM artefacts;
-- Should return ~200,000

### Sample Queries ###
Find artefacts near a location
```sql
use artefacts_and_locations;
SELECT a.artefacts_name, a.web_reference_wiki, 
       al.latitude, al.longitude
FROM artefacts a
JOIN artefacts_locations al ON a.id_artefacts = al.id_artefacts
WHERE al.latitude BETWEEN 48.8 AND 48.9
  AND al.longitude BETWEEN 2.2 AND 2.4
LIMIT 10;

### Data Model ### 
For detailed information about the data model and Java/Hibernate entities, see:
ArtefactsLocation-model - [Java model library](https://github.com/MyropolskyiHennadii/ArtefactsLocation-model)

### Usage with Java ###
This database is designed to work with the Hibernate ORM model library (install ArtefactsLocation-model before in your local repository):
<dependency>
    <groupId>myropolskyi.locations</groupId>
    <artifactId>ArtefactsLocation-model</artifactId>
    <version>2.2.2</version>
</dependency>

### Data Sources ### 
Data is curated from:
Wikipedia (primary source)
Open cultural heritage databases
Public domain architectural records

All data is from free and open sources.

### License ### 
This database is licensed under Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0).

You are free to:

✅ Use for any purpose (including commercial)

✅ Share and redistribute

✅ Modify and build upon

Under these terms:

⚠️ Attribution - Give appropriate credit

⚠️ ShareAlike - Derivatives must use the same license (keep it free)

Full license: https://creativecommons.org/licenses/by-sa/4.0/

### Related Projects ### 
[LookAroundArchitecture] (https://apalladio.org/architecture-around/)

[Android mobile application] (https://play.google.com/store/apps/details?id=myropolskyi.android.locations&pcampaignid=web_share)

### Contributing ### 
This database represents years of curation work. Contributions are welcome!
If you find errors or want to add new objects:
Fork this repository
Make your changes
Submit a pull request with documentation

### Future Maintenance ### 
This project is being made public to ensure long-term preservation of this cultural heritage data. If you're interested in maintaining or extending this database, please contact the original author.

### Contact ### 
Original Author: Hennadii Myropolskyi
Email:miropolskij@gmail.com

For questions, suggestions, or collaboration opportunities.

### Citation ### 
If you use this database in research or publications, please cite:
Myropolskyi, H. (2026). ArtefactsLocation Database: A Comprehensive Collection 
of Architectural Heritage Objects. GitHub. 
[https://github.com/MyropolskyiHennadii/ArtefactsLocation-data](https://github.com/MyropolskyiHennadii/ArtefactsLocation-data)

Last Updated: March 2026
Database Version: 1.0.0
Total Objects: 200,000+
