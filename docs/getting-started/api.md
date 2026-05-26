---
title: API Overview | OhMyUniversity!
description: Overview of the OhMyUniversity! API layer, which aggregates and normalizes data from MIM, CINECA, and European Data Portal into a unified interface.
head:
  - - meta
    - property: og:title
      content: API Overview | OhMyUniversity!
  - - meta
    - property: og:description
      content: Discover how the OhMyUniversity API layer unifies multiple educational data sources into a consistent and normalized interface.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/getting-started/api
  - - meta
    - name: keywords
      content: ohmyuniversity api, university data aggregation api, cineca mim european data portal, unified education api, student data normalization, university api gateway
  - - meta
    - name: twitter:title
      content: API Overview | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Overview of the OhMyUniversity! API layer aggregating MIM, CINECA, and European Data Portal data.
---

# OhMyUniversity! - API Overview

The API layer is responsible for aggregating and normalizing data from multiple external sources such as MIM, CINECA, and the European Data Portal.

### Key characteristics

- Unified interface over multiple data sources
- No direct user authentication required for public data
- Data is primarily file-based (CSV/ZIP) or preprocessed into structured responses
- Focus on normalization and consistency across datasets

### Example usage

```bash
curl https://api.example.com/universities
```

### Documentation

Full API documentation is available in the `/api/` section of the repository.
