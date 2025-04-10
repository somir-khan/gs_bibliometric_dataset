# Google Scholar's Bibliometric Dataset

This repository contains a bibliometric dataset constructed from Google Scholar profiles. The dataset includes detailed information such as citation metrics, publication details, and computed analytical fields. Due to privacy concerns, a series of de-identification steps have been applied before public release. The detailed data collection, preprocessing steps, and utilization of this dataset can be found in the manuscript "Curbing the Ramifications of Authorship Abuse in Science (https://arxiv.org/abs/2504.02769)"

## Table of Contents

- [Dataset Overview](#dataset-overview)
- [Database Schema](#database-schema)
- [De‑Identification Procedures](#de-identification-procedures)
  - [Scholar ID Replacement](#scholar-id-replacement)
  - [Author Name Hashing](#author-name-hashing)
  - [Profile Link and Related Fields](#profile-link-and-related-fields)
  - [Publication URL Replacement](#publication-url-replacement)

## Dataset Overview

This dataset is created for bibliometric analysis and includes three primary tables:

1. **authors**  
   Contains scholar profiles with metrics like h-index, i10-index, total citations, and research areas.

2. **publications**  
   Stores publication details, including titles, citation counts, publication dates, and a comma‑separated list of author names. The `author_names` field is used in fuzzy matching algorithms (prior to hashing).

3. **publications_computed_fields**  
   Holds computed metrics for each publication (e.g., author position and total number of authors), derived from the publications table.

## Database Schema

Below is a summary of the schema:

- **authors**  
  - **scholar_id**: Unique identifier for the scholar (Primary Key).  
  - **name**: Scholar's name (hashed during de‑identification).  
  - **profile_link**: URL of the scholar profile (set to NULL).  
  - **field**, **sub_field**: Research fields (retained for analysis).  
  - Other metrics: page_number, page_rank, h_index, i10_index, total_publications, total_citations, page_url, created_at, bibliometrics_added_at.

- **publications**  
  - **scholar_id**: Foreign key linking to `authors`.  
  - **title**: Publication title.  
  - **publication_url**: Unique URL for the publication (replaced with a new UUID).  
  - **author_names**: Comma‑separated list of author names (hashed during de‑identification).  
  - Other publication details: citations, publication_date, publication_type, is_processed, failed_attempt, pub_added_at, details_added_at.

- **publications_computed_fields**  
  - **publication_url**: Foreign key matching the publications table (updated accordingly).  
  - **author_position**, **total_authors**, **author_present**, **publication_year**, **scholar_id**.

## De‑Identification Procedures

To protect privacy while retaining analytical integrity, the following de‑identification steps were applied:

### Scholar ID Replacement
`scholar_id` in the `authors` table is replaced with a new UUID while maintaining referential integrity in other tables.
### Author Name Hashing
Author names are hashed in the authors and publications table.
### Profile Link and Related Fields
  - In the `authors` table: `profile_link`, `page_number`, `page_rank`, `page_url` these fields were replaced with empty space ('')

### Publication URL Replacement

`publication_url` in the `publications` table is replaced with a new UUID while maintaining referential integrity in the `publications_computed_fields` table.
